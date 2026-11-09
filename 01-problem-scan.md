# Bài cá nhân — Quét cơ hội và Quick Problem Cards

## Phase 1 — SCAN

| # | Đơn vị trong bối cảnh lab | Lens | Bài toán cụ thể |
|---|---|---|---|
| 1 | Xanh SM | Tốn thời gian; stakeholder pain | Điều phối viên phải đọc mô tả sự cố pin, xác nhận thông tin rồi soạn tin hướng dẫn cho tài xế. |
| 2 | Vinhomes | Lặp lại; AI-upgrade | CSKH đọc phản ánh cư dân dạng tự do rồi chuyển đúng đội điện, nước hoặc vệ sinh. |
| 3 | Vinpearl | Tốn thời gian | Nhân viên nhập yêu cầu đoàn từ email vào biểu mẫu để kiểm tra phòng. |
| 4 | VinFast | Lặp lại | Nhân viên đối chiếu mã phiên sạc và số tiền giữa hai bảng dữ liệu, đánh dấu chênh lệch. |
| 5 | Xanh SM | Stakeholder pain | Nhân viên tổng hợp ghi chú hủy chuyến thành nhóm nguyên nhân để tìm vấn đề điểm đón. |
| 6 | VinUni | AI-upgrade | Trợ giảng diễn giải lỗi code thành gợi ý cho sinh viên, không tự sửa điểm. |

Dùng đủ 4 lenses ở Phase 1; rubric I1 nhắc 3 lenses nhưng worksheet mô tả 4, nên chọn yêu cầu bao phủ rộng hơn.

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

> Thời gian và mục tiêu trong các card là giả định cần xác minh.

### Card 1 — Xanh SM: nháp phản hồi sự cố pin

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Soạn nháp phản hồi sự cố pin              │
│ cho tài xế Xanh SM nhanh và có kiểm soát.                   │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ) —        │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên và tài xế.              │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận báo cáo ──> 2. Xác nhận pin/vị trí                │
│   ──> 3. Tra cứu ──> 4. Soạn, kiểm tra tin                  │
│   ──> 5. Bàn giao và xác nhận tiếp nhận.                    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4                   │
│ (⏱ 9 phút/lượt: tra cứu 5 phút, soạn tin 4 phút).           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 4:            │
│ đọc mô tả, soạn nháp; rule kiểm tra, người duyệt.           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Thời gian nhận báo cáo đến nháp được duyệt:               │
│   15 phút ──> ≤5 phút; nháp đạt rubric ≥95%;                │
│   0 lần tự gửi trong kiểm thử.                              │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

**Dữ liệu cần:** 30 tình huống giả lập cho vòng đầu; sau đó 100 log được phép sử dụng, đã ẩn danh và gán nhãn.

**Ranh giới/fallback:** chỉ sinh nháp, không truy cập API xe/trạm và không điều cứu hộ. Pin hoặc vị trí chưa xác minh → chuyển người; thiếu dữ liệu → không chỉ đường.

### Card 2 — Vinhomes: phân loại phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Phân loại phản ánh cư dân                 │
│ Vinhomes để chuyển đúng đội xử lý.                          │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ) —        │
│                                                             │
│ Ai đang đau (Actor)? CSKH và đội kỹ thuật tòa nhà.          │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận phản ánh ──> 2. Đọc, hỏi bổ sung                  │
│   ──> 3. Chọn đội xử lý ──> 4. Lập phiếu bàn giao.          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3                   │
│ (⏱ 4 phút/phiếu): đọc và phân loại mô tả đa ý.              │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3:             │
│ tóm tắt, đề xuất loại; CSKH duyệt, rule định tuyến.         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Thời gian phân loại: 4 phút ──> ≤1 phút/phiếu;            │
│   macro-F1 ≥0,90 trên 100 phiếu ẩn danh;                    │
│   100% ca nghi khẩn cấp được người review.                  │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

**Dữ liệu cần:** taxonomy đã thống nhất, danh sách đội nhận, 100 phiếu được gán nhãn.

**Rủi ro/fallback:** sai mức khẩn cấp; phiếu thiếu thông tin hoặc liên quan an toàn chuyển trực tiếp cho nhân viên theo SOP.

### Card 3 — VinFast: đối chiếu hóa đơn phiên sạc

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Đối chiếu hóa đơn phiên sạc               │
│ VinFast để phát hiện sai lệch giữa hai bảng dữ liệu.        │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ) —        │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên đối soát.                    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Xuất dữ liệu ──> 2. Ghép mã phiên sạc                  │
│   ──> 3. So số tiền ──> 4. Lập danh sách sai lệch.          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3                   │
│ (⏱ 40 phút/lô 200 dòng): ghép mã và so số tiền.             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Chưa cần AI;          │
│ bước 2-3 dùng rule: join, kiểm tra trùng, so số tiền.       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Thời gian/lô 200 dòng: 60 phút ──> ≤10 phút;              │
│   phát hiện 100% sai lệch cài trong dữ liệu giả lập;        │
│   0 lần tự ghi sổ/thanh toán.                               │
│                                                             │
│ Quick Architecture: [x] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```


