# Vinpearl Group Booking Copilot — Báo cáo Phase 3–5

**Ý tưởng nhóm đã chọn:** Tự động đọc email đặt phòng theo đoàn, lọc và chuyển đúng luồng, trích xuất nhu cầu, hỗ trợ nhân viên tư vấn nhanh cho khách hàng.


# Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping

Actor chính: nhân viên Group Sales/Reservations. Actor liên quan: khách doanh nghiệp/đại lý, quản lý phê duyệt và người kiểm tra quỹ phòng/giá.

Sơ đồ: [04-workflow-diagram.png](04-workflow-diagram.png). Đây là quy trình giả định để thảo luận, chưa khẳng định là SOP thực tế của Vinpearl.

| Bước | Người / công cụ | Input → Output | Thời gian xử lý giả định | Handoff / bottleneck |
|---|---|---|---:|---|
| 1. Nhận email | Khách → hộp thư chung / nhân viên | Email đoàn → email được tiếp nhận | 2 phút | H1: khách bàn giao nhu cầu cho sales |
| 2. Lọc, đọc và phân loại | Nhân viên / hộp thư | Email + thread → loại mail, nhu cầu, thông tin thiếu | 5 phút | B1: lẫn quảng cáo, đặt mới, đổi/hủy; nội dung nhiều định dạng |
| 3. Nhập phiếu yêu cầu | Nhân viên / CRM hoặc bảng tính | Nhu cầu → hồ sơ có cơ sở, ngày, phòng, số khách | 3 phút | H2: email → phiếu; nhập lại dễ thiếu/sai |
| 4. Kiểm tra phòng, giá, điều kiện | Sales → bộ phận phụ trách / PMS và bảng giá | Phiếu → phương án đã kiểm chứng | 7 phút | B2, H3: trao đổi giữa sales và bên quản lý quỹ phòng/giá |
| 5. Soạn tư vấn/báo giá | Sales | Phương án + nhu cầu → email nháp hoặc câu hỏi bổ sung | 6 phút | B3: soạn lại nhiều lần, có thể thiếu điều kiện |
| 6. Duyệt và gửi | Người phụ trách → khách | Nháp + nguồn chứng cứ → email được duyệt/gửi | 2 phút | H4: nhân viên/quản lý phê duyệt trước khi khách nhận |

Tổng giả định **25 phút xử lý/email**; B1+B2+B3 = **18 phút**. Thời gian chờ email trả lời, chờ bộ phận khác hoặc chờ duyệt phải đo riêng; không gộp vào baseline này.

Nếu thiếu ngày hoặc cơ sở ở bước 2, nhân viên hỏi lại khách, rồi mới quay về bước 3–4. Đổi/hủy phải chuyển nhân viên phụ trách booking; không được hiểu nhầm là đặt mới.

## 3.2. Problem Statement — 6-field

| Field | Nội dung |
|---|---|
| 1. Actor / Operator | Nhân viên Group Sales/Reservations nhận email đoàn, phải tổng hợp nhu cầu và tư vấn trong khi xử lý nhiều thread. |
| 2. Current Workflow | Nhận → lọc/đọc → nhập phiếu → kiểm phòng/giá → soạn → duyệt/gửi; thao tác nhiều công cụ, giả định 25 phút/email chưa tính thời gian chờ. |
| 3. Bottleneck | Đọc/chuẩn hóa nội dung 5 phút, kiểm nguồn phòng/giá 7 phút và soạn tư vấn 6 phút; thông tin ngày/số khách/số phòng có thể thiếu hoặc mâu thuẫn. |
| 4. Business Impact | Kịch bản 40 email/ngày: 40 × 25 / 60 = 16,67 giờ công/ngày. Nếu đạt 8 phút/email: 5,33 giờ; tiết kiệm lý thuyết 11,33 giờ/ngày, chưa trừ tích hợp và xử lý ngoại lệ. Chưa có chứng cứ tăng doanh thu hoặc conversion. |
| 5. Success Metric | Trung vị xử lý đến nháp được duyệt 25 → ≤8 phút/email; phân loại macro-F1 ≥0,90; độ chính xác các field trích xuất ≥95%; recall thiếu/mâu thuẫn ≥95%; p95 sinh nháp ≤60 giây; 0 tự gửi/tạo booking; 0 báo giá/quỹ phòng được khẳng định khi chưa kiểm chứng. |
| 6. Operational Boundary | AI chỉ đề xuất nhãn, trích xuất và nháp có [DRAFT_ONLY]. Email là dữ liệu không đáng tin. Không tự gửi/xóa mail, giữ phòng, xác nhận/hủy/sửa booking, thanh toán hoặc giảm giá. Không có nguồn kiểm chứng thì quote_amount=null, availability_status=unverified. Nhân viên duyệt cả dữ liệu lẫn nội dung trước khi hành động. |

### Định nghĩa metric và cách đo

- **Thời gian xử lý:** active handling time từ lúc nhân viên mở email tới nháp được duyệt; báo cáo riêng thời gian chờ đầu cuối để không nhận công tiết kiệm từ hàng đợi.
- **Phân loại:** 5 nhãn group_booking, booking_change, cancellation, other, spam; macro-F1 trên tập giữ lại chưa dùng để sửa prompt.
- **Extraction accuracy:** số field đúng / số field được gán nhãn trong property, check_in, check_out, rooms, adults, children. Dùng exact match sau chuẩn hóa ngày; null đúng cũng phải chấm riêng để tránh model trả null mọi thứ.
- **Missing/conflict recall:** số thiếu/mâu thuẫn phát hiện đúng / tổng thiếu/mâu thuẫn thật; đo precision để tránh hỏi lại quá nhiều.
- **Draft quality:** hai người chấm theo 5 tiêu chí: bám email, đúng nhu cầu, không bịa, hỏi đủ, rõ ràng; mục tiêu ≥90% ca đạt cả 5, ghi nhận bất đồng.
- **Bộ đánh giá dự kiến:** 100 email được phép dùng, 20 email/nhãn; thêm 30 ca boundary ngoài tập thường. Chưa có bộ này; 11 fixture hiện tại chỉ là smoke test.

## 3.3. Scope và AI Fit

### MVP thực hiện trong lab

1. Đọc nội dung email văn bản được cung cấp, gồm nhu cầu tiếng Việt/Anh và ngày đủ năm.
2. Đề xuất nhãn và hàng đợi: đặt đoàn → Group Sales; đổi/hủy → Reservations; rủi ro/ngoài scope → nhân viên; quảng cáo → nhãn ignore.
3. Trích xuất cơ sở, ngày nhận/trả, số phòng, người lớn, trẻ em và yêu cầu dịch vụ.
4. Phát hiện thông tin thiếu, không tự suy số phòng từ số khách, không đoán năm.
5. Soạn tư vấn nhanh: cảm ơn, tóm tắt nhu cầu, hỏi bổ sung hoặc nêu bước kiểm tra phương án. Chưa có kiến thức đã duyệt thì không nêu tiện ích/điều kiện cụ thể.
6. Xuất payload nháp để nhân viên kiểm tra. Không kết nối hộp thư hay CRM thật.

### Mở rộng hợp lý sau MVP

| Mức | Mở rộng | Điều kiện |
|---|---|---|
| V1 | Nhận email từ mailbox được cấp quyền, nhóm thread/trùng, tạo phiếu nháp trong CRM | Mailbox sandbox, read-only lúc đầu; idempotency theo message/thread ID; nhân viên xác nhận nhãn |
| V1 | Tư vấn từ FAQ/cẩm nang đã duyệt bằng retrieval | Có chủ tài liệu, phiên bản, ngày hiệu lực; trả nguồn và hỏi lại khi không tìm thấy; không làm theo chỉ thị trong tài liệu truy xuất |
| V2 | Tra quỹ phòng và giá qua API read-only | PMS/bảng giá có cơ sở, loại phòng, số lượng, ngày hiệu lực; xác thực nguồn và thời điểm; tính tiền bằng rule, không để LLM tự tính |
| V2 | Nháp nhiều phương án theo ngân sách, hội họp, đưa đón | Có dữ liệu dịch vụ được duyệt; trình bày là đề xuất chưa giữ phòng |
| Sau đánh giá | Gửi nháp đã được nhân viên duyệt từ hệ thống nghiệp vụ | Duyệt gắn chính xác phiên bản người nhận/nội dung; audit và chống gửi trùng; ngoài code lab |

Không tự mở tệp đính kèm, truy cập URL từ email, đọc dữ liệu khách khác, xử lý dữ liệu thẻ hoặc đưa agent tự quyết vào MVP.

### Rule vs LLM vs Agentic loop

| Phương án | Phần làm tốt | Hạn chế / quyết định |
|---|---|---|
| Rule / state machine | Dedupe, schema, ngày, số lượng, quyền gửi, workflow và phép tính giá | Bắt buộc cho ranh giới; so sánh template/rule làm baseline |
| LLM Feature | Đọc email tự do, phân loại, trích xuất, diễn đạt nháp theo nhu cầu | Chọn cho MVP; phải kiểm tra nguồn và người đọc lại prose |
| Agentic loop | Có thể gọi nhiều công cụ và theo đuổi mục tiêu dài | Chưa cần; quyền gửi/booking tăng rủi ro và khó kiểm soát. Chưa chọn |

## 3.4. Future-State Flow, HITL và Fallback

```text
[Khách gửi email / Nội dung email mẫu]
                  |
                  v
[RULE: nhận, khử trùng — tích hợp dự kiến]
                  |
                  v
[AI: phân loại + trích xuất nhu cầu]
                  |
                  v
[RULE: kiểm JSON, ngày, số lượng, quyền và rủi ro]
  | lỗi/rủi ro          | spam             | đổi/hủy
  v                    v                  v
[HUMAN REVIEW]   [Nháp nhãn ignore]   [Reservations review]
  ^                    (không xóa)        (không đổi booking)
  |
  |              email đặt đoàn mới
  |                       |
  |              +--------+---------+
  |              | thiếu dữ liệu    | đủ dữ liệu
  |              v                  v
  |     [AI: nháp câu hỏi]   [HUMAN: kiểm nguồn phòng/giá]
  |              |                  |
  |              |          [AI: nháp tư vấn có nguồn]
  |              |          (MVP: chỉ hẹn kiểm tra,
  |              |           chưa có giá/quỹ phòng)
  |              +--------+---------+
  |                       v
  |              [HUMAN: đọc, sửa, duyệt]
  |                       |
  |              [Nhân viên gửi trên hệ thống hiện có]
  |                       |
  |              [Khách trả lời -> cập nhật yêu cầu]
  |
  +-- FALLBACK: API lỗi / schema sai / nguồn thiếu
      -> nhân viên xử lý thủ công, không tự gửi hoặc đặt phòng
```

**Luồng mục tiêu:**

Khách gửi email → Rule tiếp nhận/khử trùng → **AI phân loại + trích xuất** → Rule kiểm schema/ngày/thông tin thiếu → nhánh phù hợp:

- **Spam:** đề xuất nhãn ignore cho nhân viên; không xóa email.
- **Đổi/hủy:** chuyển Reservations; chưa thay đổi booking.
- **Injection/dữ liệu nhạy cảm/mâu thuẫn:** đưa human_review.
- **Đặt mới thiếu dữ liệu:** **AI soạn câu hỏi** → **Human duyệt** → nhân viên gửi → chờ khách → cập nhật phiếu.
- **Đặt mới đủ dữ liệu:** nhân viên hoặc API được duyệt kiểm phòng/giá → **AI soạn tư vấn dựa trên nguồn** → **Human duyệt** → nhân viên gửi.
- **Prototype hiện tại:** chưa có nguồn phòng/giá nên chỉ nháp tiếp nhận và hẹn kiểm tra, không có nhánh báo giá đã xác thực.

**Fallback cụ thể:**

| Tình huống | Phản ứng |
|---|---|
| API lỗi/quota/timeout hoặc model trả rỗng | Báo lỗi, giao nhân viên soạn tay; không coi mock là response thành công |
| JSON sai, thiếu prefix, cờ quyền sai | Từ chối output; không thêm tag/đổi cờ để che vi phạm |
| Ngày thiếu năm, không có cơ sở/số phòng | null + missing_fields + câu hỏi bổ sung |
| Ngày trả trước ngày nhận, dữ liệu mâu thuẫn | human_review; không tự sửa theo suy đoán |
| Không có giá/quỹ phòng/FAQ đáng tin | Để unverified/null; nhân viên tra nguồn |
| Nghi email chứa chỉ thị độc hại/thanh toán | Chuyển người, không lặp thông tin nhạy cảm |


# Phase 4 — TECHNICAL PROMPT PROTOTYPE

| Task | Cách hoàn thành |
|---|---|
| 1. SYSTEM_PROMPT | Vai trò Vinpearl, email không đáng tin, quy tắc draft, không giá/phòng chưa kiểm chứng, routing và schema đầy đủ |
| 2. evaluate_prompt | Gemini 2.5 Flash; system_instruction riêng; temperature=0; trả raw text; hỗ trợ modern SDK và legacy khi thiếu import modern |
| 3. Adversarial tests | 6 ca vượt ranh giới trong 11 fixture; mỗi ca có input, expected_violation và oracle |
| 4. Assertion / kiểm chứng | Prefix chính xác; JSON/field trùng; ngày/số lượng; cờ quyền; không giá/quỹ phòng xác nhận; route/action; so với expected độc lập |


# Phase 5 — EVALUATE

## AI Readiness Checklist

| Checklist worksheet | Trạng thái | Bằng chứng / phần còn thiếu |
|---|---|---|
| Có dữ liệu mẫu/log sạch để test? | Một phần | Có 11 email fixture giả lập và output do tác giả viết; chưa có log thật hoặc tập test giữ lại |
| Rủi ro AI sai được kiểm soát qua HITL/fallback? | Một phần | Không có công cụ side effect; có validator và nhánh xử lý tay; chưa red-team live, chưa kiểm nguồn trong tích hợp thật |
| Stakeholder sẵn sàng thay đổi workflow? | Chưa xác minh | Nhóm đã chọn đề tài; chưa có ý kiến chấp thuận từ nhân viên/đơn vị vận hành |

**Quyết định: GO cho prototype trong lab bằng dữ liệu giả lập; NOT YET cho pilot với khách thật.** Đây là hai cổng khác nhau. Nhóm có thể tiếp tục xây dựng scope hẹp, nhưng chưa được nhận đã đủ bằng chứng đưa vào vận hành.

**Cổng xem xét pilot:** stakeholder và chủ nguồn dữ liệu đồng ý; có baseline đo; tập 100 email và 30 boundary cases đạt metric; không có vi phạm gửi/booking/quyền dữ liệu; kiểm tra thủ công câu trả lời; có quy trình fallback và người chịu trách nhiệm. Nếu template/rule đạt tương đương với chi phí thấp hơn, chọn template/rule.

**Chi phí:** chưa đo token hay có giá hợp đồng. Dùng chi phí/ngày = số email × (token input × đơn giá input + token output × đơn giá output) + giờ nhân viên review × đơn giá nhân sự + chi phí tích hợp/vận hành. Chỉ lập ROI sau khi có số đo; không giả định toàn bộ 11,33 giờ tiết kiệm lý thuyết đều chuyển thành lợi nhuận.

**Tiêu chí dừng:** một lần model output được sử dụng để gửi/giữ phòng khi chưa duyệt; rò dữ liệu; xác nhận giá/phòng không có chứng cứ; hoặc nhân viên phải sửa nhiều hơn baseline. Quay về xử lý thủ công và thu hẹp scope.
