# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |Vinhomes                          |Tính lặp lại, Thời gian xử lý, Khó khăn của các bên liên quan |AI phân loại và điều phối yêu cầu bảo trì của cư dân: Tự động phân tích phản ánh về điện, nước, thang máy, vệ sinh; trích xuất vị trí, phân loại sự cố và đề xuất đội kỹ thuật phụ trách. |
| 2 |Xanh SM                           |Tính lặp lại, Thời gian xử lý, Khó khăn của các bên liên quan  |AI phân loại khiếu nại sau chuyến đi: Phân tích phản ánh của khách hàng về tài xế, giá cước, hành trình và phương tiện; xác định mức độ ưu tiên và đề xuất bộ phận xử lý. |
| 3 |Vinmec                            |Tính lặp lại, Thời gian xử lý, Tiềm năng ứng dụng AI |AI phân loại yêu cầu đặt lịch khám: Phân tích yêu cầu của bệnh nhân, xác định chuyên khoa, trích xuất thông tin cần thiết và phát hiện dữ liệu còn thiếu để hỗ trợ nhân viên tiếp nhận. |
| 4 |VinFast                           |Tính lặp lại, Thời gian xử lý, Tiềm năng ứng dụng AI |AI hỗ trợ chẩn đoán lỗi xe điện: Phân tích DTC, ECU logs và mô tả triệu chứng, tổng hợp thông tin lỗi, đề xuất nguyên nhân nghi vấn và hướng kiểm tra cho kỹ thuật viên. |
| 5 |VinFast                           |Tính lặp lại, Thời gian xử lý, Tiềm năng ứng dụng AI |AI phát hiện bất thường trong dữ liệu kiểm tra pin EV: Phân tích điện áp, nhiệt độ và nội trở cell pin để phát hiện các mẫu bất thường, cảnh báo và đề xuất kiểm tra bổ sung. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                   │
│                                                                         │
│ Bài toán (1 câu): Xây dựng AI hỗ trợ phân loại yêu cầu bảo trì,          
   trích xuất thông tin sự cố và đề xuất bộ phận xử lý.                   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [X] Vinhomes              
                     [ ] Vinmec   [ ] Khác (Ghi rõ)________               │
│                                                                         │
│ Ai đang đau (Actor)? Nhân viên tiếp nhận và điều phối yêu cầu bảo trì   │
│                                                                         │
│ Workflow thủ công hiện tại (3-5 bước):                                  │
│    1. Cư dân gửi yêu cầu qua ứng dụng, hotline hoặc lễ tân.             
|    2. Nhân viên tiếp nhận và đọc nội dung phản ánh.                     
|    3. Xác định loại sự cố, vị trí và mức độ ưu tiên.                    
|    4. Tìm bộ phận/kỹ thuật viên phù hợp và chuyển yêu cầu.
|    5. Theo dõi trạng thái xử lý.                                        │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? 
|  - Bước 3–4: Phân loại sự cố và xác định bộ phận phụ trách.
(⏱ 5 phút/lượt)                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? 
  - Phân tích nội dung phản ánh bằng ngôn ngữ tự nhiên.
  - Trích xuất vị trí, loại sự cố và mức độ khẩn cấp.
  - Đề xuất bộ phận phụ trách để nhân viên xác nhận.                      │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)? ______________________
  - Giảm thời gian phân loại từ 5 phút xuống dưới 1 phút/yêu cầu.
  - Độ chính xác phân loại sự cố ≥ 95%.
  - Tỷ lệ đề xuất sai bộ phận phụ trách dưới 5%.
  - Không bỏ sót sự cố nghiêm trọng trong tập kiểm thử. │
│                                                                         │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent
  - Rule: Kiểm tra từ khóa và nhận diện tình huống khẩn cấp.
  - LLM: Phân tích phản ánh, trích xuất thông tin và phân loại.
  - Human-in-the-loop: Nhân viên xác nhận trước khi điều phối.            |
└─────────────────────────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                   │
│                                                                         │
│ Bài toán (1 câu): Xây dựng AI hỗ trợ phân loại khiếu nại, 
   trích xuất thông tin chuyến đi và đề xuất hướng xử lý.                 │
│ Công ty thành viên: [ ] VinFast  [X] Xanh SM  [ ] Vinhomes              
                     [ ] Vinmec   [ ] Khác (Ghi rõ)________               │
│                                                                         │
│ Ai đang đau (Actor)? Nhân viên chăm sóc khách hàng                      │
│                                                                         │
│ Workflow thủ công hiện tại (3-5 bước):                                  │
    1. Khách hàng gửi khiếu nại qua ứng dụng hoặc kênh hỗ trợ.             
    2. Nhân viên tiếp nhận và đọc nội dung khiếu nại.                     
    3. Xác định nhóm vấn đề: tài xế, giá cước, phương tiện, 
    thái độ phục vụ hoặc thất lạc đồ.                    
    4. Kiểm tra thông tin chuyến đi và dữ liệu liên quan.
    5. Đề xuất hướng xử lý và soạn phản hồi cho khách hàng.               │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? 
|  - Bước 3–5: Phân loại, xác minh thông tin và chuẩn bị phản hồi.
(⏱ 10 phút/lượt)                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? 
  - Đọc và phân loại nội dung khiếu nại.
  - Trích xuất mã chuyến đi, thời gian và loại vấn đề.
  - Tóm tắt khiếu nại và đề xuất hướng xử lý.
  - Soạn phản hồi ban đầu để nhân viên kiểm tra.                          │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)? ______________________
  Giảm thời gian xử lý ban đầu từ 10 phút xuống dưới 2 phút/khiếu nại.
   Độ chính xác phân loại khiếu nại ≥ 95%.
   Tỷ lệ phải chỉnh sửa đáng kể nội dung AI đề xuất dưới 10%.
   100% phản hồi được nhân viên kiểm duyệt trước khi gửi trong giai đoạn 
   thử nghiệm.                                                            │
│                                                                         │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent
  - Rule: Kiểm tra mã chuyến đi, định dạng thông tin và điều kiện nghiệp vụ.
  - LLM: Phân loại, tóm tắt khiếu nại và đề xuất phản hồi.
  - Human-in-the-loop: Nhân viên quyết định hướng xử lý và phê duyệt phản 
  hồi.                                                                    |
└─────────────────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                   │
│                                                                         │
│ Bài toán (1 câu): Xây dựng AI hỗ trợ phân loại nhu cầu khám, trích xuất 
   thông tin bệnh nhân và đề xuất chuyên khoa phù hợp.                    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes              
                     [X] Vinmec   [ ] Khác (Ghi rõ)________               │
│                                                                         │
│ Ai đang đau (Actor)? Nhân viên tiếp nhận và đặt lịch khám.              │
│                                                                         │
│ Workflow thủ công hiện tại (3-5 bước):                                  │
    1. Bệnh nhân liên hệ qua hotline, website hoặc ứng dụng để đặt lịch.
    2. Nhân viên tiếp nhận thông tin và nhu cầu khám.
    3. Xác định chuyên khoa phù hợp dựa trên nhu cầu được mô tả.
    4. Kiểm tra lịch bác sĩ và khung giờ còn trống.
    5. Xác nhận lịch khám với bệnh nhân.                                  │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? 
|  - Bước 3–4: Xác định chuyên khoa và tìm kiếm lịch khám phù hợp.
(⏱ 7 phút/lượt)                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? 
  - Phân tích nhu cầu khám bằng ngôn ngữ tự nhiên.
  - Trích xuất thông tin về chuyên khoa, thời gian và cơ sở khám.
  - Đề xuất chuyên khoa phù hợp để nhân viên xác nhận.
  - Gợi ý khung giờ còn trống thông qua hệ thống lịch khám.               │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)? ______________________
  - Giảm thời gian tiếp nhận và đề xuất lịch từ 7 phút xuống dưới 
      2 phút/yêu cầu.                                                     |
  - Độ chính xác phân loại chuyên khoa ≥ 95%.
  - Tỷ lệ thông tin đặt lịch thiếu hoặc sai dưới 5%.
  - 100% trường hợp có dấu hiệu cấp cứu hoặc triệu chứng nguy hiểm được
   chuyển cho nhân viên y tế đánh giá.                                    │
│                                                                         │
│ Quick Architecture: [ ] No AI  [X] Rule  [X] LLM  [ ] Agent
  - Rule: Kiểm tra thông tin bắt buộc, lịch trống và điều kiện đặt lịch.
  - LLM: Phân tích nhu cầu khám và đề xuất chuyên khoa.
  - Human-in-the-loop: Nhân viên xác nhận chuyên khoa và lịch khám trước 
  khi đặt chính thức.                                                     |
└─────────────────────────────────────────────────────────────────────────┘
```
> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

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

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [ ] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
