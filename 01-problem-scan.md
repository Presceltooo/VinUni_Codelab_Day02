🔍 Phase 1 — SCAN (Cá nhân, 20 min)
Hãy sử dụng 4 Lenses dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại ít nhất 5 bài toán/bottleneck thực tế.

4 Lenses tìm bài toán AI cho Vingroup:
Lặp lại (Repetitive): Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
Tốn thời gian (Time-consuming): Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
AI có thể tốt hơn (AI-upgrade): Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
Pain từ người khác (Stakeholder Pain): Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).
Tip

🤖 AI Prompts — Partner brainstorm: Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng: "Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."

📝 List bài toán của tôi:
#	Subsidiary (VinFast/Xanh SM...)	Lens	Mô tả ngắn bài toán
| # | Subsidiary  | Lens | Mô tả ngắn bài toán |
|---|-------------|------|---------------------|
| 1 | Xanh SM| Repetitive| Điều phối khẩn cấp xe cứu hộ/sạc pin lưu động cho tài xế taxi điện gặp sự cố hết pin nguy hiểm 5%.|
| 2 | VinFast|Time-consuming | Đối soát và phát hiện bất thường hàng nghìn nhật ký (logs) giao dịch sạc điện hàng ngày giữa trạm sạc, ngân hàng và app VinFast.|
| 3 | Vinhomes| Time-consuming| Phân loại, định tuyến và soạn phản hồi tự động cho các phản ánh/khiếu nại 1-star của cư dân trên ứng dụng Vinhomes Resident.|
| 4 | Vinpearl| AI-upgrade| Trợ lý ảo tư vấn, lập hành trình du lịch tối ưu và đặt trọn gói (combo phòng + vé VinWonders) theo ngữ cảnh cá nhân hóa của khách hàng.|
| 5 | Vinmec| Stakeholder Pain| Tóm tắt tiền sử bệnh án và các kết quả xét nghiệm qua nhiều đợt khám để hỗ trợ bác sĩ đưa ra quyết định lâm sàng nhanh chóng.|		
🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)
Chọn top 3 bài toán từ danh sách trên và hoàn thiện 3 Quick Problem Cards dưới đây (10 phút/card).
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                     │
│                                                             │
│ Bài toán (1 câu): Tự động hóa quá trình tiếp nhận, đánh giá vị trí/mức pin nguy cấp ($< 5\%$) và điều phối xe sạc lưu động cho tài xế Xanh SM. │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên tổng đài (tốn thời gian xử lý) & Tài xế Xanh SM (nguy cơ chết máy giữa đường). │
│                                                             │
│ Workflow thủ công hiện tại:                              │
│   1. Tài xế báo về tổng đài ──> 2. Điều phối viên hỏi % pin & vị trí ──> 3. Đo khoảng cách tới trạm sạc gần nhất ──> 4. Gọi xe sạc lưu động hoặc hướng dẫn trạm sạc. │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 5–8 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4 (Phân tích mức pin, chặn điều hướng trạm sạc xa $> 5\text{km}$ và tự động tạo JSON command gọi xe sạc). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                     │
│   Giảm thời gian xử lý yêu cầu sạc khẩn cấp từ 8 min ──> under 1 min; 100% tài xế $<5\%$ pin không bị chỉ dẫn nhầm ra trạm sạc xa $> 5\text{km}$. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                     │
│                                                             │
│ Bài toán (1 câu): Phân loại tự động các ticket khiếu nại của cư dân Vinhomes và soạn thảo câu trả lời mẫu cho Ban Quản Lý (BQL). │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Ban Quản Lý Vinhomes (quá tải vì hàng trăm ý kiến/đánh giá 1-star mỗi ngày). │
│                                                             │
│ Workflow thủ công hiện tại:                              │
│   1. Cư dân gửi feedback trên App ──> 2. BQL đọc & phân loại thủ công ──> 3. Chuyển cho bộ phận chuyên trách (Kỹ thuật/Vệ sinh) ──> 4. Soạn tin nhắn phản hồi cư dân. │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ 10–15 phút/ticket) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 4 (Tự động đọc sentiment, gán nhãn phòng ban và sinh sẵn dự thảo phản hồi [DRAFT_ONLY]). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                     │
│   Giảm thời gian phản hồi cư dân từ 12 min ──> under 2 min; tăng tỷ lệ hài lòng xử lý khiếu nại lên $95\%$. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                     │
│                                                             │
│ Bài toán (1 câu): Tự động tổng hợp dữ liệu lịch sử khám bệnh và các chỉ số xét nghiệm bất thường của bệnh nhân để hỗ trợ bác sĩ Vinmec trước buổi khám. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ khám bệnh tại Vinmec (tốn nhiều thời gian tra cứu hồ sơ cũ làm giảm thời gian tương tác với bệnh nhân). │
│                                                             │
│ Workflow thủ công hiện tại:                              │
│   1. Mở hệ thống EHR/EMR ──> 2. Đọc từng hồ sơ bệnh án cũ & file PDF xét nghiệm ──> 3. Ghi chép/tổng hợp các chỉ số bất thường ──> 4. Đưa ra chẩn đoán mới. │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 7–10 phút/bệnh nhân) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 (RAG/LLM trích xuất tự động tóm tắt tiền sử y khoa và highlight chỉ số bất thường ngay khi bấm mở hồ sơ). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                     │
│   Giảm thời gian đọc & tóm tắt hồ sơ bệnh án từ 8 min ──> under 1.5 min/bệnh nhân. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘