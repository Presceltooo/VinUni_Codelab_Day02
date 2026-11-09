# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> **Lưu ý về số liệu:** Mọi con số về thời gian và mục tiêu trong file này là **giả định để lập khung bài toán**. Chưa có khảo sát thực địa hay dữ liệu nội bộ của Vingroup. Các số này cần được kiểm chứng trước khi dùng để ra quyết định.

---

## Phase 1 — SCAN

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** | AI có thể tốt hơn | Khách mô tả lỗi xe bằng lời tự nhiên, tổng đài viên phải tự chọn nhóm lỗi. Chọn sai thì xưởng chuẩn bị sai và khách phải hẹn lại. Khoảng 6 phút/lượt (giả định). |
| 2 | **Vinpearl** | Tốn thời gian | Email đặt phòng theo đoàn của công ty du lịch không theo mẫu, nhân viên sales phải đọc rồi nhập tay vào hệ thống. Khoảng 15 phút/email (giả định). |
| 3 | **Vinhomes** | Lặp lại | Cư dân nộp hồ sơ đăng ký thi công nội thất. Ban quản lý đối chiếu tay từng giấy tờ, và hồ sơ thiếu phải gửi đi gửi lại nhiều lần. |
| 4 | **Vinpearl** | Pain từ người khác | Phàn nàn nặng của khách nằm rải rác trên Booking.com, Agoda, Google Maps, nên quản lý khách sạn biết muộn. Khoảng 30 phút/ngày để đọc và lọc (giả định). |
| 5 | **Vinmec** | Pain từ người khác | Khách không biết nên đặt lịch khám chuyên khoa nào, nên tổng đài phải hỏi lại và có lượt phải chuyển khoa khi đã đến viện. |
| 6 | **VinWonders** | Lặp lại | Nhân viên trực chat trả lời lặp lại các câu hỏi về giá vé, giờ mở cửa và lịch trò chơi theo từng khu. |

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

### Card #3 — Vinpearl: Phát hiện phàn nàn khẩn cấp từ review

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Phát hiện sớm phàn nàn khẩn cấp trong review trên │
│ Booking, Agoda, Google Maps rồi báo đúng quản lý khách sạn. │
│ Công ty thành viên: [x] Khác: Vinpearl                      │
│                                                             │
│ Ai đang đau? Quản lý khách sạn, nhân viên CSKH, khách hàng  │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Mở từng trang review ──> 2. Đọc, lọc phàn nàn nặng     │
│   ──> 3. Tổng hợp gửi quản lý ──> 4. Soạn phản hồi          │
│                                                             │
│ Bước tốn nhất? Bước 1-2 (⏱ 30 phút/ngày, giả định)          │
│ AI hỗ trợ ở bước nào? Bước 2-3 (phân loại mức độ, tóm tắt)  │
│                                                             │
│ Metric: Thời gian quản lý biết phàn nàn nặng từ 1 ngày      │
│ ──> dưới 2 giờ; bắt được 90% phàn nàn nặng                  │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```
