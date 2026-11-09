# 03 — AI Log & Reflection

Trong bài lab này, tôi dùng Claude Code làm trợ lý chính và kiểm thử prototype trên các model Gemini. Bản nháp của `01-problem-scan.md`, phần code và nhật ký này chủ yếu do AI viết theo yêu cầu của tôi. Tôi đọc lại, chọn hướng đi, yêu cầu sửa và tự chạy để kiểm tra.

**AI đã giúp gì.** AI tóm tắt đề, phân biệt phần cá nhân với phần nhóm, giải thích adversarial test, autograder và cách push code. AI cũng đề xuất các bài toán để tôi chọn ra 3 bài làm Quick Card.

**AI đã sai ở đâu.** Bản đầu dùng bài toán Xanh SM giống bài mẫu, một số mô tả chưa rõ, và các con số thời gian chỉ là giả định. Cấu hình thinking AI viết gây lỗi `400` trên `gemini-3.8-flash`, và việc AI tự chạy thử nhiều lần đã dùng hết hạn mức miễn phí 20 request. AI cũng từng hiểu nhầm ý tôi và không giữ dòng model như đã nói khi hoàn tác. Gemini thì vượt qua cả 3 test, nhưng vẫn suy diễn nhỏ, ví dụ viết "khách hàng thông báo xe đã sạc đầy" dù đầu vào không nói ai báo.

**Tôi đã sửa thế nào.** Tôi yêu cầu bỏ bài toán trùng bài mẫu, viết lại mô tả ngắn gọn và ghi rõ số liệu là giả định. Với code, tôi yêu cầu viết lại `SYSTEM_PROMPT` bằng tiếng Anh, ngắn gọn nhưng đủ ranh giới, bỏ cấu hình thinking và đổi sang `gemini-3.1-flash-lite`. Tôi cũng không cho AI tự chạy chương trình nữa. Khi tôi tự chạy, cả 3 test đều đạt và phần code được 5/5 trên autograder.

**Điều rút ra.** Dấu ✅ chỉ kiểm tra chuỗi ký tự, nên vẫn phải đọc nội dung đầu ra. Số liệu AI đưa ra nghe hợp lý chưa chắc đã đúng, và tôi vẫn phải là người hiểu đề và kiểm soát những gì AI được làm.
