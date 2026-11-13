# 32. Luồng design mới của Quantpet (mockup v1, 11/11/2026)

Mockup 18 màn, chép luồng "giao việc cho agent" của TreadFi (docs/31) rồi thêm pet và backtest. Màu và chữ là token brand thật, pet là ảnh giữ chỗ, mọi số là ví dụ. Chữ trên màn bằng tiếng Anh.

- Mockup bấm thử: https://claude.ai/artifact/Dia8BBvZcpeCiscTygKXaM
- Ảnh từng màn: `docs/assets/ui-flow/quantpet-mock/shots/`
- Sơ đồ màn nối màn (ảnh thật): `docs/assets/ui-flow/quantpet-mock/screen-flow-2026-11-11.html`

## 1. Năm luồng

Luồng 1 đến 3 dùng chung đoạn cuối: **M4 hỏi → M5 backtest → M6 xác nhận → M6b pet chạy → M7 Portfolio**. M6 là chỗ duy nhất tiền đi.

| # | Luồng | Màn |
|---|---|---|
| 1 | Chạy pet có sẵn | M1 kệ pet → M2 hồ sơ pet → M3 chat → đoạn chung |
| 2 | Copy pet trên Market | P2 Market → P2b listing → P2c chat với bản copy → đoạn chung |
| 3 | Tạo pet riêng ở Studio | P3a mô tả → P3b chọn ability → P3c pet nở → P3d giao việc đầu → đoạn chung |
| 4 | Quản lý pet đã tạo (Breeder) | P4: listing, người copy, thu nhập |
| 5 | Cổng venue | P1: nối key sàn. M4 tự đẩy sang P1 nếu sàn chưa nối, xong quay lại |

Dừng pet: M7 → M8 (tạm dừng giữ vị thế, hoặc dừng và đóng).

## 2. Từng màn và backend cần trả gì

Cột cuối là gợi ý từ những gì màn hiển thị, để đối chiếu với thiết kế backend, chưa phải spec.

| Màn | User thấy và làm gì | Backend cần |
|---|---|---|
| M1 Pet shelf | Kệ 10 pet có sẵn, lọc theo loại | Danh sách pet: loại, mô tả ngắn, số backtest tóm tắt, quyền (trade hay read-only) |
| M2 Pet detail | Pet làm gì, backtest, giới hạn mặc định, phí | Hồ sơ pet + kết quả backtest đã lưu + limit mặc định |
| M3 Chat | Nói chuyện với pet, pet hiện các bước nó đang làm ("Read funding...", "Read balance...") | Agent chat có lịch sử, gọi tool đọc (funding, số dư sàn), trả từng bước suy nghĩ |
| M4 Question card | Pet hỏi 3 câu: số tiền, sàn/cặp, stop loss (kèm số $ mất tối đa) | Agent sinh câu hỏi có cấu trúc, kiểm số dư; sàn chưa nối thì chuyển P1 |
| M5 Backtest card | Kết quả backtest 30 ngày ngay trong chat | Chạy backtest theo tham số vừa chọn, trả số tóm tắt |
| M6 Confirm card | Tóm tắt lệnh: số tiền, đòn bẩy, notional, stop, phí. Nút Start | Bản nháp lệnh có kiểm rủi ro; chỉ đặt lệnh khi user bấm |
| M6b Pet started | Lệnh đã khớp từng chân, pet báo đang chạy | Đặt lệnh nhiều chân, báo khớp, xác nhận với sàn |
| M7 Portfolio | Kanban pet đang chạy + bảng vị thế, PnL hôm nay | Trạng thái từng task, vị thế, PnL, funding đã nhận |
| M8 Stop pet | Chọn tạm dừng (giữ vị thế) hoặc dừng và đóng, thấy số tiền nhận lại | Pause/resume task, đóng vị thế, ước tính phí đóng |
| P1 Venue access | Nối key sàn (Lighter...), xem quyền của key | Lưu key mã hoá, kiểm quyền key |
| P2 Market | Danh sách pet người khác tạo | Listing: tác giả, volume, số người copy |
| P2b Listing | Chi tiết một listing, nút Copy | Thông số chiến lược, phí copy |
| P2c Chat with copy | Bản copy chạy bằng key và limit của user, tác giả không thấy tài khoản | Clone cấu hình sang user, chia phí copy (ví dụ 1 bps: 70% tác giả, 30% Quantpet) |
| P3a Describe | User mô tả chiến lược bằng lời | Agent dịch mô tả thành cấu hình chiến lược |
| P3b Abilities | Chọn khả năng của pet (đọc, trade, sàn nào) | Danh sách ability = tool pet được dùng |
| P3c Pet is born | Pet mới nở, có tên và ảnh | Tạo pet, sinh ảnh/biến thể |
| P3d First task | Chat giao việc đầu tiên | Giống M3, pet level 0 chưa chạy thật |
| P4 Breeder | Pet mình tạo, người copy, thu nhập | Thống kê copy và phí theo tác giả |

## 3. Chưa chốt

- Tên "Breeder" hay "Creator" cho màn P4.
- Có thêm tab "My pets" không (chưa vẽ).
- M4 hỏi 3 câu trên một card hay mỗi card một câu.
