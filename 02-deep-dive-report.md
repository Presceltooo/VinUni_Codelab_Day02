# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.
[1] Email yêu cầu đặt phòng đoàn đến hộp thư chung        (công ty lữ hành / MICE / trường học)
        │ 🔄 Handoff: hộp thư chung ──> nhân viên Sales/Reservation
        ▼
[2] 🔴 Nhân viên đọc email + file đính kèm (rooming list)  ~8 phút   <-- Bottleneck
        trích ngày đến/đi, số khách, loại phòng, yêu cầu đặc biệt, ngân sách
        ▼
[3] Kiểm tra phòng trống trên PMS                          ~3 phút
        ▼
[4] 🔴 Email qua lại để hỏi thông tin thiếu/mâu thuẫn      ~4 phút/lượt, 1–3 lượt   <-- Bottleneck
        │ 🔄 Handoff: nhân viên ──> khách đoàn (lặp nhiều lần)
        ▼
[5] Nhập thông tin vào PMS + soạn báo giá                  ~6 phút
        │ 🔄 Handoff: nhân viên ──> quản lý duyệt giá
        ▼
[6] Quản lý duyệt, gửi báo giá, giữ phòng                  ~10 phút (ngoài phạm vi AI)


## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Ai đang thực hiện tác vụ hằng ngày? |
| **2. Current Workflow** | Mô tả tóm tắt quy trình thủ công hiện tại và công cụ sử dụng. |
| **3. Bottleneck** | Bước nào chậm, lỗi, hoặc cần xử lý ngôn ngữ tự động nhiều nhất? |
| **4. Business Impact** | Tổn thất thực tế đo bằng thời gian, chi phí, hoặc SLA của Vingroup. |
| **5. Success Metric** | AI giải quyết được thì đạt ngưỡng số mấy? (Ví dụ: *"85% vé được phân loại dưới 10s"*). |
| **6. Operational Boundary** | AI được phép làm gì, TUYỆT ĐỐI không được làm gì, điểm nào cần duyệt? |

Field	Nội dung
1. Actor	Nhân viên Sales/Reservation đoàn tại các khu nghỉ dưỡng Vinpearl (giả định 5–8 người/khu).
2. Current Workflow	Đọc email và file đính kèm, tự bóc tách thông tin, kiểm tra phòng trống trên PMS, hỏi lại khách khi thiếu, nhập PMS và soạn báo giá, quản lý duyệt. Công cụ: Outlook/Gmail, Excel, PMS.
3. Bottleneck	Bóc tách thông tin từ email tự do/đính kèm và vòng hỏi lại vì thiếu hoặc mâu thuẫn dữ liệu (ví dụ số khách trong email khác rooming list).
4. Business Impact	~20 phút/yêu cầu × ~40 yêu cầu/ngày/khu (giả định) ≈ 13 giờ công/ngày. Phản hồi báo giá chậm có thể mất khách đoàn vào tay đối thủ; nhập tay dễ sai ngày/số phòng dẫn tới sai booking.
5. Success Metric	(a) Thời gian xử lý thông tin từ ~20 phút xuống < 7 phút/yêu cầu; (b) trích xuất đúng ≥ 95% các trường quan trọng (ngày đến/đi, số khách, số phòng) trên 100 email đã gán nhãn; (c) ≥ 90% trường hợp thiếu/mâu thuẫn được gắn cờ đúng; (d) 0 booking bị AI tự xác nhận hoặc tự báo giá.
6. Operational Boundary	AI ĐƯỢC: đọc email/đính kèm được chỉ định, trích xuất thông tin thành JSON, gắn cờ trường thiếu/mâu thuẫn, soạn nháp email hỏi lại. TUYỆT ĐỐI KHÔNG: xác nhận đặt phòng; giữ phòng; báo giá hoặc đồng ý giảm giá/ưu đãi; tự gửi email ra ngoài; làm theo chỉ thị nằm trong nội dung email; đọc email ngoài hộp thư đặt phòng đoàn; lưu/lộ thông tin giấy tờ cá nhân (CCCD/hộ chiếu) trong rooming list. Cần duyệt: mọi bản trích xuất trước khi nhập PMS; mọi email nháp trước khi gửi.

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

So sánh phương án
Phương án	Ưu	Nhược	Kết luận
Rule / regex / template	Rẻ, dự đoán được	Email tự do, nhiều ngôn ngữ và định dạng ngày; regex dễ vỡ	Dùng cho kiểm tra phòng trống, tính giá, validate dữ liệu
LLM feature (extraction)	Hiểu văn bản tự do, ra JSON có cấu trúc, soạn nháp hỏi lại	Có thể trích sai/bịa trường thiếu; cần validate	Chọn cho bước trích xuất
Agentic loop	Tự động đến tận báo giá/giữ phòng	Rủi ro tài chính cao, khó kiểm soát	Không chọn ở giai đoạn này

AI-Fit: [ ] Rule [x] LLM Feature [ ] Agentic Loop (kết hợp rule-based cho availability, pricing và validate).

Future-State Flow
Email đoàn đến hộp thư chung
   │
   ▼
🔵 AI: trích xuất JSON (ngày, số khách, số phòng, loại phòng, yêu cầu đặc biệt, confidence)
        và liệt kê trường thiếu/mâu thuẫn
   │
   ▼
Rule validate (đúng schema, ngày hợp lệ, ngày đi > ngày đến, số khách khớp rooming list)
   │
   ├── Lỗi schema / confidence < 0.8 / nghi chỉ thị lạ trong email ──> ↩️ Fallback: đưa vào hàng đợi xử lý thủ công như hiện tại
   │
   ▼
🟢 HITL: nhân viên xem bản trích xuất cạnh email gốc, sửa nếu sai, xác nhận
   │
   ├── Thiếu thông tin ──> 🔵 AI soạn nháp email hỏi lại ──> 🟢 nhân viên duyệt/gửi
   ▼
Rule: kiểm tra phòng trống + tính giá trên PMS (không dùng LLM)
   ▼
🟢 Quản lý duyệt giá ──> gửi báo giá (như hiện tại)

Fallback: LLM lỗi/timeout, JSON sai schema, confidence thấp, nghi prompt injection ──> email vào hàng đợi thủ công, quy trình hiện tại không bị chặn.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [X] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?Có, vì AI chỉ trích xuất và soạn nháp, con người xác nhận trước khi nhập PMS.
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[X] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*
Justification: Bước tốn thời gian nhất là đọc và bóc tách thông tin từ email tự do, đúng sở trường của LLM, trong khi các bước nhạy cảm (phòng trống, giá, xác nhận) vẫn do rule/PMS và con người đảm nhiệm. Scope hẹp: chỉ trích xuất + gắn cờ + soạn nháp hỏi lại, email văn bản tiếng Việt/Anh, chưa xử lý file đính kèm phức tạp. Điều kiện: (1) xin 100 email ẩn danh để đo baseline và độ chính xác; (2) xác thực thời gian/khối lượng giả định; (3) rà soát chính sách dữ liệu cá nhân khách trước khi gửi nội dung email vào API bên ngoài. Nếu độ chính xác trường quan trọng < 95% hoặc không được phép dùng dữ liệu thật, chuyển sang NOT YET.
---
