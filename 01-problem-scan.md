# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> **Lưu ý về số liệu:** Mọi con số về thời gian và mục tiêu trong file này là **giả định để lập khung bài toán**. Chưa có khảo sát thực địa hay dữ liệu nội bộ của Vingroup. Các số này cần được kiểm chứng trước khi dùng để ra quyết định.

---

## Phase 1 — SCAN

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | AI có thể tốt hơn | Khách mô tả lỗi xe bằng tiếng Việt đời thường (ví dụ *"qua gờ giảm tốc kêu cụp cụp ở bánh trước"*). Tổng đài khó quy về đúng nhóm lỗi kỹ thuật ban đầu. |
| 2 | **Vinpearl** | Tốn thời gian | Email đặt phòng theo đoàn từ công ty lữ hành có cấu trúc rất khác nhau. Nhân viên đọc tay rồi nhập lại để kiểm tra quỹ phòng. |
| 3 | **Vinhomes** | Lặp lại | Cư dân nộp hồ sơ đăng ký thi công nội thất. Ban quản lý đối chiếu tay từng giấy tờ, và hồ sơ thiếu phải gửi đi gửi lại nhiều lần. |
| 4 | **Vinpearl** | Pain từ người khác | Phàn nàn khẩn cấp nằm rải rác trên Booking.com, Agoda, Google Maps nên quản lý biết muộn. |
| 5 | **Vinmec** | Pain từ người khác | Khách không biết nên đặt lịch khám chuyên khoa nào, nên tổng đài phải hỏi lại và có lượt phải chuyển khoa khi đã đến viện. |
| 6 | **VinWonders** | Lặp lại | Nhân viên trực chat trả lời lặp lại các câu hỏi về giá vé, giờ mở cửa và lịch trò chơi theo từng khu. |

Bài toán 5 thuộc lĩnh vực y tế, gợi ý sai có thể làm chậm việc khám của bệnh nhân. Bài toán này cần ranh giới rất chặt và có nhân viên y tế duyệt, nên không đưa vào top 3 ở vòng này.

Bài toán 6 phần lớn là tra cứu thông tin cố định, nên **FAQ hoặc rule-based có thể đã đủ** mà không cần LLM.

---

## Phase 2 — QUICK-ASSESS

### Card #1 — VinFast: Phân loại lỗi xe từ mô tả tiếng Việt

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Quy mô tả lỗi tiếng Việt của khách về nhóm lỗi    │
│ kỹ thuật ban đầu để chuyển đúng xưởng dịch vụ.              │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Tổng đài viên, cố vấn dịch vụ, khách hàng      │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Nghe khách mô tả ──> 2. Hỏi thêm theo kinh nghiệm      │
│   ──> 3. Chọn nhóm lỗi ──> 4. Tạo phiếu, chuyển xưởng       │
│                                                             │
│ Bước tốn nhất? Bước 2-3 (⏱ 6 phút/lượt, giả định)           │
│ AI hỗ trợ ở bước nào? Bước 2-3 (gợi ý câu hỏi, nhóm lỗi)    │
│                                                             │
│ Metric: Top-3 đúng nhóm lỗi đạt 90%, đo trên phiếu đã đóng  │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — Vinpearl: Đọc email đặt phòng theo đoàn

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Trích xuất ngày, số phòng, loại phòng từ email    │
│ đoàn không theo mẫu để kiểm tra quỹ phòng và draft booking. │
│ Công ty thành viên: [x] Khác: Vinpearl                      │
│                                                             │
│ Ai đang đau? Nhân viên sales khách đoàn, công ty lữ hành    │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Đọc email ──> 2. Nhập yêu cầu vào PMS                  │
│   ──> 3. Kiểm tra quỹ phòng ──> 4. Soạn phản hồi            │
│                                                             │
│ Bước tốn nhất? Bước 1-2 (⏱ 15 phút/email, giả định)         │
│ AI hỗ trợ ở bước nào? Bước 1-2 và draft bước 4              │
│                                                             │
│ Metric: 95% trường trích xuất đúng, xử lý dưới 3 phút/email │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

### Card #3 — Vinhomes: Kiểm tra hồ sơ đăng ký thi công nội thất

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Kiểm tra hồ sơ đăng ký thi công nội thất của cư   │
│ dân xem đủ giấy tờ chưa, báo lại phần thiếu.                │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân, nhân viên ban quản lý tòa nhà          │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Nhận hồ sơ ──> 2. Đối chiếu từng giấy tờ               │
│   ──> 3. Báo phần thiếu ──> 4. Chuyển kỹ thuật duyệt        │
│                                                             │
│ Bước tốn nhất? Bước 2-3 (⏱ 20 phút/hồ sơ, giả định)         │
│ AI hỗ trợ ở bước nào? Bước 2-3 (đọc giấy tờ, nháp phản hồi) │
│                                                             │
│ Metric: Giảm từ 20 phút/hồ sơ ──> dưới 5 phút;              │
│ 95% hồ sơ thiếu giấy tờ được phát hiện đúng                 │
│                                                             │
│ Quick Architecture: [x] Rule + LLM Feature                  │
└─────────────────────────────────────────────────────────────┘
```
