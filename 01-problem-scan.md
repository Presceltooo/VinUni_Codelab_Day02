# 01 — Problem Scan & Quick Problem Cards (Cá nhân)

**Học viên:** AI Product Engineer  
**Đơn vị:** Vin Smart Future — Vingroup  
**Dự án:** AI Product Scoping Lab 02  

---

## 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội qua 4 Lăng kính (4 Lenses)

Quét qua hoạt động vận hành của các công ty thành viên Vingroup để tìm kiếm các bài toán tốn thời gian, lặp lại và rò rỉ hiệu suất có thể giải quyết bằng AI:

| # | Đơn vị (Subsidiary) | Lăng kính (Lens) | Mô tả ngắn bài toán vận hành |
|---|---------------------|------------------|------------------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian (Time-consuming) | Điều phối viên xử lý thủ công các cuộc gọi sự cố cạn pin/hỏng sạc giữa đường từ tài xế taxi điện, mất 15-20 phút/lượt tra cứu trạm và hướng dẫn. |
| 2 | **Xanh SM (GSM)** | Tốn thời gian (Time-consuming) | Bộ phận QA/Vận hành nghe và tóm tắt thủ công hàng trăm file ghi âm cuộc gọi khiếu nại/hủy cuốc giờ cao điểm để tìm pattern lỗi hệ thống. |
| 3 | **Xanh SM (GSM)** | Lặp lại (Repetitive) | So khớp và tái phân bổ cuốc xe thủ công khi khách hàng đổi lộ trình/điểm đến giữa chừng, gây trễ hẹn và xung đột cuốc tiếp theo của tài xế. |
| 4 | **VinFast** | Lặp lại (Repetitive) | So khớp thủ công hóa đơn sạc pin và đối chiếu dữ liệu giao dịch sạc giữa hệ thống trụ sạc VinFast và các đối tác nhượng quyền hằng tuần. |
| 5 | **Vinhomes** | AI-upgrade | Hệ thống phân loại khiếu nại và route tự động các phản hồi của cư dân trên App Vinhomes Resident (CSKH phản hồi rập khuôn, trễ SLA đến 12 tiếng). |
| 6 | **Vinmec** | Pain từ người khác (Stakeholder Pain) | Bác sĩ lâm sàng quá tải vì phải mất 20–30 phút/bệnh nhân để tóm tắt hồ sơ bệnh án và lịch sử dùng thuốc khi làm thủ tục xuất viện. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Tập trung chuyên sâu Xanh SM)

Chọn lọc top 3 bài toán trọng điểm từ khối vận hành **Xanh SM (GSM)** để đánh giá sơ bộ:

### 📋 Quick Problem Card #1: Xanh SM — Điều phối sự cố sạc pin thực địa

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Bài toán (1 câu): Tài xế taxi Xanh SM báo cạn pin hoặc lỗi sạc giữa đường, │
│ cần điều phối khẩn cấp trạm sạc trống hoặc xe sạc lưu động.                 │
│ Công ty thành viên: [x] Xanh SM (GSM)   [x] VinFast (Trạm sạc)              │
│                                                                             │
│ Ai đang đau (Actor)? Tài xế (chờ lâu, stress), Điều phối viên (quá tải)     │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Nhận cuộc gọi khẩn cấp từ tài xế (2 phút)                              │
│   → 2. Mở dashboard tra cứu GPS xe thực địa (2 phút)                        │
│   → 3. Tra cứu trạm sạc VinFast còn trụ trống & công suất phù hợp (5 phút)  │
│   → 4. Soạn thảo tin nhắn SMS chỉ dẫn chi tiết gửi app tài xế (5 phút)      │
│   → 5. Liên hệ đội xe sạc di động nếu pin dưới 5% (1 phút)                  │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & Bước 4 (⏱ 10 phút/lượt)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & Bước 4 (Tự động hóa          │
│ lấy tọa độ, lọc trạm trống và draft tin nhắn điều phối kèm chỉ dẫn).        │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt.                 │
│ Tỉ lệ chỉ đúng trạm còn trụ trống đạt trên 98%.                             │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule kiểm tra pin < 5%)        │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📋 Quick Problem Card #2: Xanh SM — Tóm tắt & Phân loại lý do hủy cuốc giờ cao điểm

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Bài toán (1 câu): Phân tích và trích xuất lý do gốc rễ khiến khách hàng     │
│ hủy cuốc xe giờ cao điểm từ cuộc gọi ghi âm và ghi chú của tài xế.          │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)? Đội ngũ Quản lý Vận hành & Trưởng ca Điều vận          │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Lọc danh sách các cuốc xe bị khách hủy trên hệ thống (3 phút)          │
│   → 2. Nghe lại file ghi âm cuộc gọi CSKH hoặc đọc log chat (10 phút)        │
│   → 3. Ghi chép và phân loại thủ công lý do vào bảng tính Excel (7 phút)    │
│   → 4. Tổng hợp báo cáo tuần gửi Ban Giám đốc Vận hành (5 phút)             │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & Bước 3 (⏱ 17 phút/cuốc hủy)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & Bước 3 (AI Audio Speech-     │
│ to-Text kết hợp LLM tóm tắt đoạn hội thoại và tự động gắn nhãn nguyên nhân).│
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ Rút ngắn thời gian phân tích từ 25 phút/ca ──> dưới 2 phút.                 │
│ Tăng độ bao phủ phân tích cuốc hủy từ 10% mẫu ngẫu nhiên ──> 100% cuốc hủy. │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Audio Transcript + Text Classification)│
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📋 Quick Problem Card #3: Xanh SM — Tái điều phối cuốc xe khi khách đổi lộ trình

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                       │
│                                                                             │
│ Bài toán (1 câu): Khách hàng thay đổi điểm đến đột xuất giữa chuyến đi,     │
│ hệ thống cần tính toán lại lộ trình, chi phí và tái phân bổ cuốc xe kế tiếp.│
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)? Tài xế (bị động), Khách đón tiếp theo (chờ lâu)       │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Tài xế hoặc khách gọi tổng đài báo đổi điểm trả khách (2 phút)         │
│   → 2. Điều phối viên tra cứu thủ công tình trạng giao thông mới (3 phút)   │
│   → 3. Tính toán lại cước phí phát sinh và giải thích cho khách (5 phút)    │
│   → 4. Hủy ghép đôi cuốc đón tiếp theo và tìm xe thay thế (5 phút)          │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & Bước 4 (⏱ 10 phút/lượt)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & Bước 4 (LLM hỗ trợ tạo giải  │
│ thích cước phí minh bạch và gợi ý phương án điều xe kế tiếp thông minh).     │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ Giảm thời gian xử lý thay đổi lộ trình từ 15 phút ──> dưới 2 phút.          │
│ Giảm tỉ lệ khách hàng tiếp theo hủy xe vì chờ đổi chuyến xuống dưới 5%.      │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule-based Routing API)        │
└─────────────────────────────────────────────────────────────────────────────┘
```
