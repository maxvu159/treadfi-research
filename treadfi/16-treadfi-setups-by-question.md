# Đọc hiểu bot TreadFi bằng 5 câu hỏi của quant (3/11/2026)

> Mục đích: một phương pháp để Max đọc bất kỳ bot farm nào (TreadFi, Vooi, Vesper, bot của Duy) và hiểu mỗi tham số sinh ra để làm gì. Tham số lấy từ `11-treadfi-research.md` (docs.tread.fi, đọc 2/11). Giả định của Max: founder TreadFi là ex-quant nên thiết kế là chuẩn tốt để học. Bằng chứng ủng hộ: danh sách algo (Impact Minimization, Implementation Shortfall, Target Participation Rate, VWAP, TWAP, Iceberg) là bộ algo chuẩn của desk thực thi tổ chức, không phải bot retail tự nghĩ.

## Giới hạn của việc học TreadFi

TreadFi là OEMS: nó dạy **thực thi rẻ và an toàn** (đặt lệnh sao cho ít phí, ít impact, không cháy). Nó không dạy **farm ở đâu, cặp nào, giờ nào thì cost per point thấp**. Phần sau TreadFi để user tự đoán (Mid spread 0 là "volume machine" nhưng cost per 1M từ $150 đến $270 là user tự đo). Quantpets học phần 1 từ TreadFi, phần 2 phải tự chạy baseline mà đo.

## 5 câu hỏi

Mọi tham số của mọi bot trả lời một trong 5 câu. Đọc bot mới thì xếp tham số vào 5 ô này.

| # | Câu | Vì sao quant hỏi |
|---|---|---|
| 1 | Neo giá ở đâu? | Lệnh limit cần một giá gốc để cộng trừ spread. Chọn sai neo là mua đỉnh bán đáy |
| 2 | Bao nhiêu, nhanh cỡ nào? | Size lớn và nhanh thì impact giá, size nhỏ và chậm thì không đủ volume. Luôn là đánh đổi |
| 3 | Cách neo bao xa, nghiêng bên nào? | Gần thì khớp nhiều lãi ít, xa thì ngược lại. Nghiêng là có quan điểm hướng |
| 4 | Dừng khi nào? | Mọi bot phải có điều kiện thoát trước khi vào, không thì cháy |
| 5 | Tạm dừng hoặc reset khi nào? | Thị trường đổi chế độ (ngang sang trend) thì bot phải tự biết, không thì kẹt một chân |

---

## Bot 1. Market Maker (MM)

Mục đích: đặt lệnh limit mua và bán đồng thời quanh một giá neo, làm mới liên tục. Dùng để farm volume maker, farm point, bắt rebate. Mặc định chỉ maker ("never crosses the spread").

### Câu 1: Neo giá ở đâu (6 mode)

| Mode | Neo | Dùng khi | Rủi ro |
|---|---|---|---|
| Mid (mặc định) | Mid order book hiện tại | Farm volume, point. Docs: "fills matter more than per-trade PnL" | Giá rơi nhanh thì mua cao bán thấp. Spread 0 đến 2 bps khớp nhiều, lãi không |
| Grid | Giá khớp cuối; không mua cao hơn giá đã bán, không bán thấp hơn giá đã mua | Đi ngang. PnL spread không âm, chỉ mất phí | Trend mạnh thì kẹt một chân |
| RGrid | VWAP phần fill gần nhất; mua trên trung bình khi lên, bán dưới khi xuống; có taker | Trend | Đi ngang thì kẹt. Passive Only tắt |
| DGrid | Mỗi 30 giây đo variance ratio của proxy (BTC, XAUT, S&P 500, CL) và realized move, tự chuyển Grid hoặc RGrid (ngưỡng 1.15 và 1.25), tự chọn spread tới 50 bps | Cặp vừa ngang vừa trend trong ngày. Cộng đồng gọi "endgame" (4/2026) | Proxy không tương quan thì chọn sai |
| Blend | Trung bình có trọng số mid sàn đang quote và mid sàn lớn (0 đến 100%, mặc định 50%) | Sàn mỏng, giá lệch sàn lớn | Ví dụ trong docs không khớp công thức |
| Signal | Mid dịch theo RSI cặp tín hiệu (RSI 70 dịch xuống 20 bps, RSI 30 dịch lên, tối đa 50 bps) | Muốn nghiêng ngược vùng quá mua bán | Mất feed thì không đặt lệnh |

Bài học: một bot MM thật sự chỉ khác nhau ở **neo giá**. Farm thì Mid. Muốn không lỗ spread thì Grid. Không muốn canh thì DGrid.

### Câu 2: Bao nhiêu, nhanh cỡ nào

| Tham số | Giá trị | Ý nghĩa |
|---|---|---|
| Margin hoặc notional | tuỳ | Tiền cọc giao cho bot; notional chia 50/50 hai bên |
| Đòn bẩy | tuỳ sàn | Notional = margin × đòn bẩy. Docs cảnh báo trên 20x sàn thanh lý trước khi stop kịp chạy |
| POV preset | Aggressive 0.1 × volume 24h, Normal 0.05 ×, Passive 0.01 × | Tốc độ bơm volume so với thị trường. Đây là tham số TWAP: "mình chiếm bao nhiêu % volume của cặp" |
| Thời lượng | tối thiểu theo Aggressive, tối đa 10 × Passive, Passive chặn 24 giờ | Thời lượng và POV ràng nhau: POV cao thì xong nhanh |
| Clip | nội bộ | Mỗi lệnh con bao lớn |

Bài học: quant không nói "đặt 1,000 USD", mà nói "chiếm 5% volume trong 4 giờ". POV là cách nghĩ về size của người thực thi.

### Câu 3: Cách neo bao xa, nghiêng bên nào

| Tham số | Giá trị | Ý nghĩa |
|---|---|---|
| Spread | -50 đến +50 bps | Âm = quote bên trong mid (khớp chắc, lỗ spread), dương = ngoài mid (khớp ít, lãi spread). Preset docs: Grid -1 bps cho đa số; OI thấp Grid +5 bps; farm point maker Mid trên 10 bps |
| Directional bias | -1 đến +1 | Nghiêng size về một bên. -1 chỉ bán, +1 chỉ mua, 0 cân |
| Blend weight | 0 đến 100% | Chỉ cho Blend |

Bài học: spread âm là thứ chỉ người farm mới dùng; quant kiếm tiền thì spread dương. Quantpets farm nên mặc định quanh 0 đến -1 bps như cộng đồng.

### Câu 4: Dừng khi nào

| Tham số | Giá trị | Ý nghĩa |
|---|---|---|
| Stop loss % margin | mặc định 10% | Lỗ bao nhiêu % tiền cọc thì dừng. DGrid: đo realized ở pha Grid, unrealized ở pha RGrid |
| Take profit % | tuỳ | Lãi đủ thì dừng |
| Thời lượng | như trên | Hết giờ thì dừng |
| Lịch chạy | giờ | Tránh mở cửa Mỹ, FOMC, CPI (preset docs) |

### Câu 5: Tạm dừng hoặc reset khi nào

| Cơ chế | Giá trị | Ý nghĩa |
|---|---|---|
| Exposure pause | tự động | Lệch một phía thì ngừng quote bên đó, cân lại thì tiếp. Paused là trạng thái an toàn, không phải lỗi |
| Grid reset | 5, 12.5, 25, 50, 100 bps | Giá chạy xa X bps thì chân chậm nhảy về mid. Setting @aaalex: reset 5 bps, spread 1 bps, stop 5%, US500 20x |
| DGrid soft reset | khoảng 4 × spread | Tự động |
| Huỷ bot | | Tự đặt lệnh đóng phần dư về net 0 |

---

## Bot 2. Delta Neutral (DN)

Mục đích: long một chân, short một chân cùng lúc, có thể khác sàn khác cặp. Ăn chênh funding, hoặc farm hai sàn một lúc mà không chịu rủi ro giá.

| Câu | Tham số | Giá trị | Ý nghĩa |
|---|---|---|---|
| 1 | Execution mode | TWAP cho cả hai chân | Hai chân khớp dần song song để không lệch |
| 2 | Notional tổng | chia 50/50 | |
| 2 | Đòn bẩy | tuỳ | Thấp hơn MM vì rủi ro là thanh lý một chân, không phải hướng giá |
| 2 | Tài khoản và cặp mỗi chân | | Khác sàn (farm hai sàn), khác cặp (pair trade) |
| 3 | (không có spread) | | DN không quote, nó vào vị thế |
| 4 | Stop loss | 50%, 75% (mặc định), 90%, Uncapped | Tính trên margin một chân. Mặc định 75% là rất rộng vì hedge rồi |
| 4 | Thời lượng | tối đa 1 ngày | Ép user xem lại mỗi ngày |
| 5 | Unbalanced | net exposure trên 1% | Cảnh báo, không tự đóng |
| 5 | Wind Down | nút | Đóng cả hai chân cùng lúc |

Bài học: DN có ít tham số hơn MM vì không có "neo" và "spread". Toàn bộ rủi ro DN nằm ở câu 5: hai chân lệch nhau. TreadFi chỉ cảnh báo; MöB tự đóng khi lệch 5%; Vooi đóng khi một chân gần thanh lý. Đây là chỗ Quantpets phải hơn TreadFi.

---

## Bot 3. Execution algos (lệnh một chiều)

Mục đích: mua hoặc bán một lượng lớn mà không đẩy giá. Người farm ít dùng (5 tweet), nhưng đây là phần lộ rõ nhất gốc quant của TreadFi.

| Câu | Tham số | Ý nghĩa |
|---|---|---|
| 1 | Limit price, dynamic limit price | Giá trần hoặc sàn cho cả lệnh, có thể trượt theo thị trường |
| 1 | Trajectory: TWAP, VWAP, Target Time, IS | Phân bổ size theo thời gian đều (TWAP), theo volume lịch sử (VWAP), theo mốc giờ, hoặc tối thiểu chênh so với giá lúc quyết định (Implementation Shortfall) |
| 2 | Size, urgency, duration, max clip size | Urgency là nút tổng: cao = nhanh = impact nhiều |
| 2 | Strategy: Impact Minimization, Time Constant, Target Participation Rate, Market Maker, Aggressive Maker, Aggressive Taker, Market, Limit, IOC, Iceberg | Thang từ thụ động nhất (chỉ maker, chờ) tới chủ động nhất (market). Iceberg giấu size |
| 3 | Passive only, discretion, alpha tilt | Passive only = không bao giờ taker. Discretion = được vượt limit bao nhiêu. Alpha tilt = nghiêng tốc độ theo tín hiệu |
| 4 | Exit conditions, reduce only | Reduce only = chỉ được giảm vị thế, không mở ngược |
| 5 | OOL pause (out of limit), order conditions, scale orders | Giá ra ngoài limit thì tạm dừng; điều kiện kích hoạt; chia thang giá |

Bài học: đây là từ điển thuật ngữ thực thi. Hiểu Impact Minimization, IS, POV, passive only, reduce only là đọc được mọi docs OEMS khác. Quantpets v0 chỉ cần `twap_execute` với passive_only và reduce_only.

---

## Bot 4. Multi, Spread, Basis, Portfolio, Chained

Mục đích: một lệnh lên nhiều sàn; spread hai chân; basis spot với perp; chuỗi lệnh.

| Câu | Tham số | Ý nghĩa |
|---|---|---|
| 3 | Dynamic limit spread | Chênh hai chân tối đa chấp nhận |
| 5 | Exposure tolerance | Hai chân lệch bao nhiêu thì dừng |
| 3 | Directional bias | Như MM |

Bài học: cùng 5 câu, ít tham số hơn. Không cần cho Quantpets v0.

---

## Bot 5. Campaigns (DMM tập thể)

Nhiều user gộp MM bot làm market maker cho một token. Tham số: slot thời gian, preset khoá (mode, spread, bias), boost. Thưởng theo uptime quote. Bài học cho Quantpets: **preset khoá** là cách cho người mới chạy MM mà không cần hiểu 5 câu; đúng mô hình Low risk / Volume / High risk của Quantpets.

## Bot 6. Agents và custom agents

Agent không có tham số mới. Nó điền 5 câu trên cho user bằng chat, rồi hiện thẻ Propose Start. Custom agent = prompt + điều kiện bật tắt (ADX, RSI báo sideway thì chạy MM, tin đỏ thì dừng). Giới hạn 24 giờ mỗi task, 25 task MM đồng thời. Bài học: agent là lớp điền form, không phải lớp mới.

---

## Cách dùng phương pháp này cho Quantpets

1. Lấy bot của Duy và Bean (baseline Volume Lighter, DN HL/Ondo), xếp tham số vào 5 câu. Ô nào trống là thiếu (dự đoán: câu 5 của DN trống, câu 2 không có POV).
2. Làm tương tự với Vooi (4 goal) và Vesper (10 mode): sẽ thấy Vooi gần như chỉ có câu 2, 4, 5 vì DN; Vesper khoá câu 1 và 3 theo sàn.
3. Preset Quantpets = điền sẵn 5 câu cho từng mục tiêu. Ví dụ Volume Lighter: neo Mid, POV Normal, spread -1 bps, stop 10% margin, 24 giờ, exposure pause bật.
4. Số cụ thể trong preset (spread mấy bps, hold mấy giờ) lấy từ baseline strat, không lấy từ TreadFi.

Nguồn: `11-treadfi-research.md` (docs.tread.fi: market-maker-bot, delta-neutral-bot, algo orders, agents, campaigns), `14a-q1-bot-farm-sheets.md`, `15-tool-layer-scorecard.md`.
