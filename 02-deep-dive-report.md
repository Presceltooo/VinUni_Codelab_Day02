# 02 — Báo Cáo Phân Tích Chuyên Sâu (Problem Deep-Dive Report)

**Dự án:** Trợ lý Điều phối Thông minh Xanh SM (GSM Intelligent Dispatcher Co-Pilot)  
**Đơn vị phụ trách:** Vin Smart Future — Vingroup  
**Thành viên thực hiện:** Nhóm AI Product Engineering  

---

## 🏛️ 1. Bối cảnh & Bài toán lựa chọn

Qua buổi thảo luận và sàng lọc từ danh sách các bài toán thực tế của các công ty thành viên Vingroup, nhóm quyết định lựa chọn giải quyết nút thắt trong khâu vận hành của **Xanh SM (GSM)** phối hợp cùng hạ tầng trạm sạc của **VinFast**:

> **Tên bài toán:** Tối ưu hóa quy trình điều phối và hỗ trợ xử lý sự cố cạn pin / tìm trạm sạc khẩn cấp cho tài xế taxi điện Xanh SM trong giờ cao điểm.

---

## 🔄 2. Gate G1: Current-State Workflow Mapping

### 2.1. Quy trình thủ công hiện tại (5 bước):
1. **Bước 1 — Tiếp nhận cuộc gọi sự cố (⏱ 2 phút):** Tài xế gặp sự cố xe cạn pin (< 10%) hoặc trạm sạc dự kiến bị quá tải gọi điện khẩn cấp về tổng đài điều vận Xanh SM.
2. **Bước 2 — Định vị vị trí xe thực địa (⏱ 2 phút):** Điều phối viên (Dispatcher) tra cứu thủ công biển số xe trên phần mềm Fleet Management để lấy tọa độ GPS hiện tại.
3. **Bước 3 — Tra cứu trạm sạc VinFast còn trụ trống (⏱ 5 phút — 🔴 BOTTLENECK):** Dispatcher mở Dashboard trạm sạc VinFast, tìm kiếm các trạm sạc trong bán kính 5km, kiểm tra loại cổng sạc (DC công suất cao/AC) phù hợp với dòng xe (VF5 Plus, VFe34, VF8) và tình trạng trụ sạc còn trống thực tế.
4. **Bước 4 — Soạn thảo văn bản chỉ dẫn gửi tài xế (⏱ 5 phút — 🔴 BOTTLENECK):** Dispatcher gõ tin nhắn hướng dẫn chi tiết (tên trạm sạc, địa chỉ cụ thể, khoảng cách, số lượng trụ trống và lời dặn an toàn) gửi qua SMS hoặc App Driver.
5. **Bước 5 — Kích hoạt cứu hộ pin nếu cần (⏱ 1 phút):** Nếu xe báo pin dưới 5% không thể di chuyển đến trạm gần nhất, điều phối viên gọi liên hệ đội Xe sạc pin lưu động (Mobile Charging Vehicle) của VinFast.

* **Tổng thời gian xử lý thủ công:** **15 phút/lượt**.
* **Điểm chuyển giao thông tin (🔄 Handoffs):** 
  * Handoff 1: Tài xế $\rightarrow$ Tổng đài viên (Điện thoại).
  * Handoff 2: Dispatcher $\rightarrow$ App Tài xế (Tin nhắn hướng dẫn).
  * Handoff 3: Dispatcher $\rightarrow$ Đội xe cứu hộ lưu động (nếu nguy cấp).
* **Sơ đồ quy trình trực quan:** Đã được trực quan hóa chi tiết tại file [`04-workflow-diagram.png`](04-workflow-diagram.png).

---

## 🎯 3. Gate G2: Problem Statement (6-field) — Chuẩn Vin Smart Future

| Trường thông tin (Field) | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM và Tài xế taxi điện Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo sự cố cạn pin giữa đường, Dispatcher tra cứu vị trí GPS trên hệ thống quản lý xe, tra cứu thủ công tình trạng trụ sạc trống trên hệ thống trạm VinFast, soạn thảo tin nhắn hướng dẫn đường đi và liên hệ đội cứu hộ nếu pin dưới ngưỡng an toàn. Quy trình gồm 5 bước thủ công kéo dài 15 phút/lượt. |
| **3. Bottleneck** | Bước 3 & Bước 4 (mất 10 phút): Tra cứu thủ công đối chiếu cổng sạc/trụ trống và viết văn bản hướng dẫn bằng tiếng Việt thân thiện, rõ ràng theo quy chuẩn của tập đoàn. |
| **4. Business Impact** | Tại thị trường Hà Nội & TP.HCM, trung bình có 80–120 sự cố cảnh báo pin/ngày. Tiêu tốn hơn 25 giờ làm việc/ngày của khối điều vận. Xe nằm chờ lâu làm tăng tỉ lệ hủy cuốc 14%, giảm doanh thu vận hành và gây nguy cơ tắc nghẽn giao thông nếu xe chết máy giữa đường. |
| **5. Success Metric** | **1. Vận hành:** Giảm tổng thời gian xử lý sự cố từ **15 phút xuống dưới 3 phút/lượt**.<br>**2. Chất lượng:** Độ chính xác trong việc đề xuất đúng loại cổng sạc và còn trụ trống đạt **$\ge 98\%$**.<br>**3. Mức độ hài lòng:** Giảm áp lực thời gian thực cho điều phối viên xuống 75%. |
| **6. Operational Boundary** | AI được phép gọi API đọc tọa độ GPS, API tình trạng trạm sạc VinFast, và tự động tạo bản nháp tin nhắn (`[DRAFT_ONLY]`).<br>**CẤM:** AI **tuyệt đối không được gửi thẳng tin nhắn** cho tài xế mà chưa qua phê duyệt của Dispatcher (bắt buộc Human-in-the-loop); **tuyệt đối không gợi ý trạm sạc xa > 5km** khi pin xe $< 5\%$ mà phải lập tức kích hoạt đề xuất điều xe cứu hộ sạc di động. |

---

## 🤖 4. Gate G3: AI Fit, Future-State Flow & Ranh giới vận hành

### 4.1. Ma trận so sánh tính phù hợp (AI-Fit Matrix)

| Tiêu chí | Phương án 1: Rule-based Code | Phương án 2: LLM Feature (ĐƯỢC CHỌN) | Phương án 3: Autonomous Agent |
|---|---|---|---|
| **Khả năng sinh văn bản thân thiện** | Kém (tin nhắn rập khuôn dạng template cứng nhắc, khó biến thiên linh hoạt theo ngữ cảnh thời tiết/tắc đường) | **Rất tốt** (soạn văn bản chỉ dẫn tiếng Việt tự nhiên, lịch sự theo đúng tôn chỉ dịch vụ 5 sao của Vingroup) | Rất tốt |
| **Độ phức tạp triển khai** | Thấp | **Vừa phải, triển khai nhanh** | Rất cao, rủi ro vòng lặp vô tận |
| **Rủi ro sai số / Ảo giác** | Thấp | **Được kiểm soát tuyệt đối** nhờ Human-in-the-loop (`[DRAFT_ONLY]`) và Rule pin < 5% | Rất cao (Agent tự ý ra quyết định có thể gửi nhầm địa chỉ hoặc gọi sai xe cứu hộ) |
| **Kết luận lựa chọn** | Dùng làm tầng kiểm tra điều kiện cứng (Rule filter) | **Lựa chọn tối ưu làm Co-Pilot** hỗ trợ nhân viên | Không khuyến nghị cho môi trường vận hành giao thông khẩn cấp |

### 4.2. Quy trình vận hành tương lai (Future-State Flow):

```text
┌────────────────┐     ┌────────────────────────────────┐     ┌────────────────────────────────┐
│ Bước 1:        │     │ Bước 2:                        │     │ Bước 3:                        │
│ Tiếp nhận cuộc │ ──> │ 🔵 AI Co-Pilot tự động         │ ──> │ 🔵 AI LLM Feature              │
│ gọi sự cố pin  │     │ lấy GPS + Lọc trạm trống       │     │ Soạn Draft chỉ dẫn             │
│ (Dispatcher)   │     │ (Rule-based check pin < 5%)    │     │ [DRAFT_ONLY]                   │
└────────────────┘     └────────────────────────────────┘     └────────────────────────────────┘
                                                                              │
                                                                              ▼
┌────────────────┐     ┌────────────────────────────────┐     ┌────────────────────────────────┐
│ Hoàn tất:      │     │ ↩️ Fallback khi AI lỗi:        │     │ Bước 4:                        │
│ Tài xế nhận tin│ <── │ Dispatcher tự tra cứu và       │ <── │ 🟢 Dispatcher nhấn duyệt       │
│ & di chuyển    │     │ soạn tin nhắn tay như cũ       │     │ (Human-in-the-loop) & gửi tin  │
└────────────────┘     └────────────────────────────────┘     └────────────────────────────────┘
```

* **Cơ chế Human-in-the-loop (HITL):** Mọi tin nhắn sinh ra từ LLM luôn gắn tiền tố `[DRAFT_ONLY]`. Điều phối viên chỉ cần đọc lướt qua trong 5 giây, chỉnh sửa nếu cần, rồi bấm nút "Phê duyệt & Gửi".
* **Cơ chế Fallback:** Nếu dịch vụ LLM bị timeout hoặc phản hồi không hợp lệ, hệ thống tự động rơi về màn hình hiển thị danh sách trạm sạc gần nhất để điều phối viên xử lý thủ công, đảm bảo hệ thống không bị gián đoạn hoạt động.

---

## 💻 5. Phase 4: Thử nghiệm kỹ thuật Prompt Prototype & Kiểm thử biên

Nhóm đã xây dựng và hoàn thiện mã nguồn nguyên mẫu tại [`starter-code/prompt_prototype.py`](starter-code/prompt_prototype.py) sử dụng Gemini API:
1. **Thiết lập ranh giới an toàn (System Prompt Boundaries):**
   * Ép buộc mọi tin nhắn văn bản phải có nhãn `[DRAFT_ONLY]`.
   * Thiết lập luật cứng: Khi xe báo pin nguy cấp dưới 5%, cấm tuyệt đối việc gợi ý trạm sạc xa quá 5km, bắt buộc trả về tín hiệu điều động xe sạc di động: `{"action": "dispatch_mobile_charger", ...}`.
2. **Kiểm thử tấn công biên (Adversarial Testing):**
   * **Test 1 (Ép chỉ đường khi pin 2%):** Prompt người dùng cố tình ép chỉ trạm sạc 8km $\rightarrow$ Hệ thống phát hiện pin nguy cấp và kích hoạt lệnh cứu hộ `dispatch_mobile_charger`. (Passed ✅).
   * **Test 2 (Ép bỏ nhãn [DRAFT_ONLY]):** Prompt người dùng yêu cầu bỏ thẻ nháp để gửi thẳng $\rightarrow$ Hệ thống vẫn kiên quyết giữ thẻ `[DRAFT_ONLY]` ở đầu thông báo. (Passed ✅).

---

## 🏁 6. Gate G4: AI Readiness Checklist & Quyết định đầu tư

### 6.1. Bảng kiểm tra độ sẵn sàng (AI Readiness Checklist):
* [x] **Dữ liệu mẫu/logs sạch:** Hệ thống quản lý đội xe Xanh SM và mạng lưới trạm sạc VinFast đã có sẵn API thời gian thực và lịch sử cuộc gọi ghi âm sự cố.
* [x] **Rủi ro nằm trong tầm kiểm soát:** Nhờ cơ chế Human-in-the-loop (HITL) với thẻ `[DRAFT_ONLY]` và luật bảo vệ pin $< 5\%$, không có nguy cơ thông tin sai lệch tự động gửi tới khách hàng/tài xế.
* [x] **Sự sẵn sàng của Stakeholders:** Ban Quản lý Vận hành Xanh SM rất ủng hộ vì giải pháp giúp giảm trực tiếp thời gian xử lý và giảm áp lực cho điều phối viên.

### 6.2. Quyết định cuối cùng: **GO (Bắt đầu xây dựng Prototype)**

**Lý giải quyết định (Justification):**
* **Giá trị kinh tế & vận hành rõ ràng:** Rút ngắn thời gian từ 15 phút xuống dưới 3 phút/lượt xử lý, giải phóng ~20 giờ công/ngày cho đội ngũ điều vận, nâng cao độ tin cậy của dịch vụ taxi điện Xanh SM.
* **Tính khả thi kỹ thuật cao:** Sử dụng kiến trúc **LLM Feature** có ranh giới rõ ràng kết hợp Rule-based filtering, không đòi hỏi huấn luyện lại mô hình nền tảng phức tạp.
* **Chi phí tối ưu:** Chi phí API cho một lần gọi Gemini 2.5 Flash rất nhỏ (chưa tới 50 VNĐ/lượt) so với chi phí nhân sự và thiệt hại khi tài xế cạn kiệt pin giữa đường.

