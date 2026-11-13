# TreadFi: nghiên cứu cho QuantPets

Ngày: 02/11/2026. Nguồn: tread.fi, docs.tread.fi, blog tread.fi, 272 tweet từ 15 truy vấn X (196 tweet có nhắc TreadFi, từ 11/2025 đến 02/11/2026), 40 bài gần nhất của @tread_fi (15,316 follower).

Mọi số liệu lấy từ tweet của user được đánh dấu "chưa xác minh". Số liệu từ docs hoặc blog chính thức là tự công bố, chưa có kiểm toán.

## Kết luận

- Farming volume và điểm là cách dùng thật của phần lớn người dùng retail TreadFi. Độ tin cậy: cao. Trong 196 tweet có nhắc TreadFi, 123 tweet nói về point, airdrop hoặc farm; 96 nói về volume; chỉ 5 nói về TWAP, VWAP hay thuật toán khớp lệnh. Chính TreadFi cũng viết blog "Become a Hyperliquid Whale for Pennies on the Dollar" (14/10/2025) để bán bot MM cho mùa điểm Hyperliquid.
- TreadFi đóng gói hai bot chính: Market Maker (6 chế độ giá tham chiếu) và Delta Neutral. Cộng đồng farm dùng MM bot nhiều nhất (55 tweet), DN bot thứ hai (32 tweet), và từ 08/2026 chuyển sang "custom agent" bọc ngoài MM bot để chạy 24/7 theo điều kiện (38 tweet nhắc agent). Độ tin cậy: cao.
- Chi phí farm qua TreadFi quanh 150 đến 270 USD cho mỗi 1,000,000 USD volume trên HIP-3 (builder fee 1 đến 2.5 bps cộng phí maker sàn cộng PnL âm nhỏ). Số từ tweet, chưa xác minh.
- Lighter chưa được tích hợp tính đến 02/11/2026. Docs liệt kê 18 sàn, không có Lighter. User đòi Lighter từ 11/2025 đến nay. Độ tin cậy: cao.
- Với sản phẩm agent farm điểm, tool layer kiểu TreadFi là đúng hướng: agent chọn và tham số hoá bot, bot giữ các bất biến an toàn (chỉ lệnh maker, stop loss theo % margin, tự tạm dừng khi lệch exposure, đóng reduce-only). Bằng chứng: TreadFi tự xây Agents và custom agents trên cùng bộ bot này, không cho agent đặt lệnh thô. Độ tin cậy: trung bình đến cao.

## 1. Cách dùng thật

### 1.1 TreadFi tự giới thiệu thế nào

Trang chủ tread.fi: "Algorithmic Trading Terminal for Digital Assets", "Institutional grade OEMS", "Supercharge your limit order book trading with execution algorithms". Bio X: "Algorithmic trading terminal. Agentic trading platform. Institutional grade OEMS." Mặt ngoài nhắm tổ chức. QRT (Qube Research & Technologies) đầu tư vào TreadFi, công bố 01/10/2026. Kalshi perps và prediction markets tích hợp 08/09/2026.

Nhưng blog và chương trình điểm nói chuyện khác. Blog 14/10/2025 viết thẳng: "turn your Hyperliquid account into a volume-generating machine for the next points season", "Stop watching the leaderboards from the bottom. Be at the summit." Chương trình điểm Season 1 (18/11/2025 đến 18/05/2026): 100,000 điểm mỗi tuần, chia theo "adjusted volume", phải có hơn 100,000 USD volume mỗi tuần mới được tính. Tức là TreadFi chấm điểm người dùng bằng volume, và boost điểm theo sàn (RiseX 10x, Dreamcash 5x, XYZ 2x theo tweet user).

Sau TGE (16/09/2026), các TIP tiếp tục xoay quanh volume:

- TIP-1: giảm builder fee theo volume 14 ngày và lượng TREAD nắm giữ (1,000 TREAD giảm 10%, 1,000,000 TREAD giảm 100%).
- TIP-2 "Tread Vault": mọi airdrop, builder reward, rebate TreadFi nhận từ các sàn sẽ chia lại cho user theo phí đã trả qua TreadFi trên sàn đó. Đây là lý do user tiếp tục farm qua TreadFi sau TGE (tweet @OG_Branxi 14/10/2026 kể đã tiết kiệm 1,000 USD phí, 731 USD trong đó trên HL Entropy và Kinetiq).
- TIP-3: marketplace custom agent, sponsor khoá TREAD để giữ slot, creator và sponsor chia phí.
- TIP-4: sàn perp muốn tích hợp phải thắng đấu giá bằng TREAD.

### 1.2 Số liệu

Chính thức (tự công bố):

- Blog "A Year of Tread" (19/10/2026): 12.37 tỷ USD volume trên Hyperliquid trong 365 ngày (09/10/2025 đến 08/10/2026), 40.26 triệu fill, 5,108 ví lần đầu, trung bình 33.89 triệu USD mỗi ngày. Ngày lớn nhất 03/03/2026: 186.20 triệu USD, trong đó USA500 (cash, một thị trường HIP-3) chiếm 68.24%. Q1/2026: 5.16 tỷ USD, 42% cả năm. Tháng 01/2026 cao nhất: 1.92 tỷ USD.
- Tweet @tread_fi 09/10/2026: "100B+ in volume" (gồm cả CEX, toàn lịch sử).
- Tweet @tread_fi 06/09/2026: tỉ lệ lệnh theo kênh: Terminal 88.6%, Agents 10.5%, MCP 0.9%.
- Builder fee: 2.5 bps gốc, xuống 1 bps khi volume 14 ngày trên 100 triệu USD.

Từ cộng đồng (chưa xác minh):

- @OG_Branxi (4,224 follower) 18/05/2026: Season 1 kéo dài 27 tuần, 24.5 tỷ USD volume. TreadFi là builder số 2 trên Hyperliquid theo volume 30 ngày (9.7 tỷ USD tổng, 2 triệu USD doanh thu), builder số 1 trên Extended (1.07 tỷ), Pacifica (800 triệu), Paradex (5.9 tỷ), Nado (2.17 tỷ).
- @muarmemuar (10,222 follower) 28/04/2026, dẫn DefiLlama: doanh thu tổng 3.3 triệu USD, volume perp 30 ngày 1.47 tỷ USD, 805 user 30 ngày. Lưu ý: 805 user tạo 1.47 tỷ USD, tức khoảng 1.8 triệu USD mỗi user mỗi tháng. Đây là dấu hiệu rõ của farm bằng bot, không phải trader tay.
- @philarmonia0x 30/08/2026, dẫn DefiLlama: hơn 20 tỷ USD volume, 4 triệu USD phí.
- DefiLlama (qua kết quả search, trang trả 403 khi fetch trực tiếp): volume perp 30 ngày gần đây 573.02 triệu USD, nguồn doanh thu là Hyperliquid và Extended. Chưa xác minh.
- @Tom_Degen68 (5,546 follower) 18/09/2026: hơn 100 tỷ USD routed, hơn 30,000 trader.
- @sportytechworld (1,391 follower) 03/09/2026: khoảng 230 custom agent trên marketplace.
- Airdrop TREAD: 15.2% supply (nhiều tweet 09 đến 10/2026). Tread Foundation đốt 3,023,113.03 TREAD chưa claim (3.02% supply) ngày 17/09/2026 (tweet chính thức).

### 1.3 User nói gì trên X

Mẫu tweet tiêu biểu, sắp theo thời gian:

| Ngày | Tài khoản (follower) | Nội dung chính |
|---|---|---|
| 19/11/2025 | @onchainmonk (8,558) | "Monk has become a Market Maker first time using tread.fi... make large volumes on Hyperliquid + tradexyz + Pacifica in hours + earn Tread points". Cần code mời để mở bot. |
| 21/11/2025 | @Zen_Trades1 (19,923) | Farm HIP-3 bằng MM bot: 1.09 triệu USD volume với 544.68 USD phí, "better than anything I've tried manually". Bot chỉ đặt limit nên không phải wash trade. |
| 10/12/2025 | @mystcapital | "Stop farming perp dexes manually." HL taker tốn 450 USD mỗi 1 triệu USD, nên dùng MM bot. |
| 21/01/2026 | @aaalex | Tuần 3: 69.2 điểm, 1.6 triệu USD volume, 2.9 triệu adjusted volume, vốn khởi điểm 160 USD, chủ yếu trên Paradex. |
| 22/02/2026 | @irlbogush | Nado: 500,000 USD volume với 300 USD vốn bằng MM bot. |
| 09/05/2026 | @OG_Branxi (4,224) | 1 triệu USD đầu tiên trên RiseX bằng MM bot. Chi phí 170 USD rồi tăng lên 270 USD mỗi 1 triệu. Chế độ tốt nhất: DGrid, nhì RGrid. Không dùng quá 50% margin cho bot. |
| 09/06/2026 | @0xBellinili | Ondo: PnL âm 45 USD, thưởng 103 USD, lãi ròng 58 USD cộng điểm Ondo. |
| 16/09/2026 | @HyperliquidEco (2,544) | Hơn 250 triệu USD volume trên HL, Extended, Nado, Ondo. Trả 30,000 USD phí, nhận lại 10,000 USD bribe, đã rút 20,000 USD hoà vốn, còn hơn 80,000 TREAD. |
| 19/09/2026 | @Lady_Farming | Ý kiến ngược: "treadfi is too costly", dùng vesper_mm tốn 90 USD cho 1.2 triệu volume. |
| 03/10/2026 | @OG_Branxi | Gần 4 triệu USD trên Entropy qua TreadFi trong 2 tuần, khoảng 200 USD mỗi 1 triệu nhờ rebate Entropy. Luận điểm "3 trong 1": Tread S2, Entropy S0, Hyperliquid S3. |
| 04/10/2026 | @devilonnn (370) | Setting cụ thể MM bot trên Entropy: DRAM, 25x, POV Normal, giá tham chiếu Mid, bias Neutral, spread 0 bps, stop loss 10%. 200,000 USD volume tốn khoảng 25 USD. |
| 05/10/2026 | @edwardmorra_btc (62,311) | Farm Extended, Pacifica, Nado qua API TreadFi nhiều tháng. Dùng DN bot để ăn chênh funding giữa 2 sàn. |
| 20/10/2026 | @OG_Branxi | MM bot TreadFi tốn khoảng 210 USD mỗi 1 triệu (Tread S2). |
| 25/10/2026 | @sportytechworld (1,391) | Custom agent của mình làm khoảng 60 triệu USD volume trong 30 ngày, agent volume cao nhất marketplace. |
| 29/10/2026 | @HyperliquidEco | "MM'ing some HIP4 volume for a potential Hyperliquid airdrop. 3k in 30 min with even a small profit." |
| 31/10/2026 | @sportytechworld | Agent quote 10 cặp Ondo trong 5 bps 24/7 để ăn 100,000 USD liquidity reward. Tạm dừng khi biến động, không quote lúc mở cửa Mỹ, tin đỏ, FOMC. |
| 02/11/2026 | @louisdives | Trả lời hỏi phí: "additional fee on tread 2.5bps... 250 USD for every 1m volume". |

Người dùng Việt: @laivietnam91 (829 follower) 01/10/2026 tóm tắt TIP-2 và kế hoạch: hold 10,000 TREAD để giảm 25% phí, trade tối thiểu 1 triệu USD mỗi tháng, chạy MM bot trên Entropy qua Agent hoặc DN bot giữa Entropy và TradeXYZ qua Agent vì Agent cho đặt điều kiện nâng cao chạy 24/7.

Có một nhóm nhỏ dùng khác: @OG_Branxi test Desk Trader agent với TrueNorth (07/2026), @Dhruv_Sniper tạo custom agent theo chỉ báo CCI, @0xBellinili thử Mid cộng directional để có PnL dương. Nhưng ngay cả nhóm này cũng nhắc điểm trong cùng bài.

### 1.4 Lighter

Docs tread.fi (đọc 02/11/2026) liệt kê 18 sàn: Arcus, Aster, Aftermath, Binance, Bitget, Bybit, Deribit, Extended, OKX, Gate, Hyperliquid, Nado, Ondo, Pacifica, Paradex, RISEx, Perpl, cộng Kalshi và Polymarket qua prediction markets. Không có Lighter. Tweet @onchainmonk 20/11/2025 ghi "Lighter (soon)"; @mystcapital 30/11/2025 "Need Lighter, Extended and Variational support soon". Extended đã lên 10/12/2025, Lighter và Variational vẫn chưa. Truy vấn "treadfi lighter" trên X chỉ ra các bài so sánh airdrop, không có bài nào farm Lighter qua TreadFi. Lighter có chương trình Partner Attribution cho frontend bên thứ ba (docs.lighter.xyz/integrations), nên rào cản có thể là TIP-4 (sàn phải đấu giá bằng TREAD) hơn là kỹ thuật. Chưa xác minh lý do.

### 1.5 Kết luận câu 1

Farming volume và điểm là cách dùng chính. Độ tin cậy cao, dựa trên ba lớp bằng chứng khớp nhau: TreadFi tự thiết kế chương trình điểm theo volume và viết blog bán bot cho mùa điểm; số liệu 805 user tạo 1.47 tỷ USD mỗi tháng; và hơn 60% tweet của user nói về point, airdrop hoặc farm. Phần dùng tổ chức (QRT, Kalshi, OEMS, self-hosted) có thật nhưng không hiện trên X retail, và TreadFi tách nó thành dòng Enterprise riêng.

## 2. Bảng bot

### 2.1 Danh mục

| Bot | Làm gì | Tham số user cấu hình | Sàn | Phí | Mức dùng trong cộng đồng farm |
|---|---|---|---|---|---|
| Market Maker Bot (MM) | Đặt đồng thời lệnh limit mua và bán quanh giá tham chiếu, chia notional 50/50, chạy như TWAP, làm mới lệnh định kỳ. Mặc định chỉ lệnh maker, "never crosses the spread". Tự tạm dừng khi exposure lệch một phía, tự tiếp tục khi cân lại. | Tài khoản, cặp (spot hoặc perp), margin hoặc notional, đòn bẩy (perp), POV preset (Aggressive 0.1 x volume 24h, Normal 0.05 x, Passive 0.01 x), thời lượng (tối thiểu theo Aggressive, tối đa 10 x Passive, Passive chặn 24 giờ), chế độ giá tham chiếu (Mid, Grid, RGrid, DGrid, Blend, Signal), spread -50 đến +50 bps, directional bias -1 đến +1, stop loss % margin (mặc định 10%), take profit %, grid reset (5, 12.5, 25, 50, 100 bps), lịch chạy, campaign slot. | Perp DEX và CEX, spot và perp. Không hỗ trợ futures, options. | Builder fee 2.5 bps gốc, giảm theo bậc volume 14 ngày tới 1 bps, giảm thêm theo TREAD hold tới 100%. Pacifica cố định 2 bps. Ondo làm tròn bps nguyên. Cộng phí maker của sàn. | Cao nhất. 55 tweet. Là "cỗ máy volume". |
| Delta Neutral Bot (DN) | Mở long một chân và short một chân cùng lúc, hai chân khớp bằng TWAP. Có thể khác sàn, khác cặp. Ăn chênh funding. Cảnh báo "Unbalanced" khi net exposure trên 1%. Nút Wind Down đóng cả hai chân. | Tài khoản chân long, tài khoản chân short, cặp mỗi chân, đòn bẩy, notional tổng (chia 50/50), thời lượng (tối đa 1 ngày), stop loss 50%, 75% (mặc định), 90% hoặc Uncapped, execution mode. | Mọi perp DEX và CEX đã kết nối. Docs nêu Extended, Hyperliquid, Pacifica. | Builder fee 2 bps mỗi chân trên sàn có builder fee. Phí sàn mỗi chân. | Thứ hai. 32 tweet. Dùng để farm hai sàn một lúc (long SILVER trên XYZ, short SILVER trên Cash) hoặc ăn funding giữa Extended, Pacifica, Nado. |
| Execution algos (Algo Orders) | Khớp lệnh một chiều giảm impact. Strategy: Impact Minimization, Time Constant, Target Participation Rate, Market Maker, Aggressive Maker, Aggressive Taker, Target Time, Market, Limit, IOC, Iceberg. Trajectory: TWAP, VWAP, Target Time, IS. | Cặp, side, size, urgency, duration, limit price, passive only, reduce only, max clip size, discretion, alpha tilt, OOL pause, dynamic limit price, order conditions, exit conditions, scale orders. | Mọi sàn kết nối, cả DEX swap. | Builder fee như trên với perp DEX. CEX không thu phí theo volume. | Thấp trong nhóm farm. 5 tweet. Dùng để mở đóng vị thế lớn. |
| Multi / Spread / Basis / Portfolio | Đặt một lệnh lên nhiều sàn cùng lúc, spread hai chân, basis spot với perp, danh mục. Chained orders chạy chuỗi lệnh. | Dynamic limit spread, exposure tolerance, directional bias. | Nhiều sàn. | Như trên. | Thấp. Nhắc ở giai đoạn đầu (11/2025) như "một click mở long trên Pacifica, HL, Paradex cùng lúc". |
| Campaigns | Nhiều user gộp MM bot làm DMM tập thể cho một token. Chọn slot thời gian, chọn preset khoá tham số, bot tự chạy khi slot bắt đầu. Thưởng theo uptime quote, boost slot, volume. | Slot, preset (mode, spread, bias), boost. | Ví dụ docs: ZAMA/USDC trên Hyperliquid, pool 2,500 USD mỗi tuần, ký quỹ tối thiểu 5,000 USD. | Có thể có phí sàn và builder. | Chưa thấy tweet. Chưa xác minh đã mở rộng. |
| Agents (beta): Desk Trader, Market Maker | Chat tiếng Anh, AI chuyển thành lệnh. Desk Trader chọn thuật toán khớp. Market Maker agent tự chọn cặp, mode, đòn bẩy, size rồi chạy MM bot. | Prompt: ticker, side, size, tài khoản whitelist, thời gian, ràng buộc. Lệnh có rủi ro hiện thẻ "Propose Start" để duyệt. Tối đa 24 giờ mỗi task, 25 task MM đồng thời. | Chỉ tài khoản đã whitelist. Không rút tiền được. | Không ghi phí riêng. | Tăng nhanh từ 08/2026. 38 tweet. |
| Custom agents và marketplace | User viết prompt "brief như nhân viên mới", đăng lên marketplace, người khác "hire" chạy bằng vốn của họ. Hỗ trợ loop điều kiện (chỉ chạy MM khi ADX, RSI báo sideway, dừng khi tin đỏ), "clone a trader" từ ví hoặc handle X. | Prompt, điều kiện, tài khoản whitelist. | Như Agents. | TIP-3 hứa chia phí cho creator và sponsor, chưa live. | Khoảng 230 agent (chưa xác minh). Agent phổ biến nhất vẫn là bọc MM bot. |
| Telegram bot (beta) | /order (chọn tài khoản, cặp, size, urgency), /balance, /portfolio, /status, /cancel, /price, cảnh báo lệnh. | Như trên. | Mọi sàn. | Như trên. | Thấp. |
| Prediction markets, DEX swap, Options (beta) | Polymarket, Kalshi, HL deployer markets; swap on-chain; options Deribit. | Theo từng mục. | Theo từng mục. | Theo từng mục. | Thấp trong nhóm farm. |

### 2.2 Sáu chế độ giá tham chiếu của MM bot

| Chế độ | Neo giá | Khi nào dùng | Ghi chú rủi ro |
|---|---|---|---|
| Mid (mặc định) | Mid của order book hiện tại, lệnh chạy theo thị trường, không nhìn giá vào. | Farm volume, farm điểm, bắt rebate. Docs nói thẳng "point farming, where fills and order completion matter more than per-trade PnL". | Thị trường rơi nhanh thì mua cao bán thấp. Spread 0 đến 2 bps fill nhiều, lãi không. |
| Grid | Giá khớp cuối. "Không mua cao hơn giá đã bán, không bán thấp hơn giá đã mua." Bảo đảm PnL trên spread từ 0 trở lên, phí vẫn mất. | Thị trường đi ngang. Khoá lợi nhuận mỗi vòng. | Trend mạnh thì kẹt một chân. Có Grid Reset: giá lệch X% thì chân chậm chuyển sang giá mid để cân lại. |
| RGrid (Reverse Grid) | Giá trung bình exposure (VWAP phần fill gần nhất, tham số discretion). Mua trên trung bình khi giá lên, bán dưới khi giá xuống. Có dùng lệnh taker. | Thị trường trend. | Đi ngang thì kẹt. Passive Only bị tắt. |
| DGrid (Dynamic Grid) | Mỗi 30 giây đo variance ratio của tài sản proxy (BTC, XAUT, S&P 500, CL) và realized move của cặp, tự chuyển Grid hoặc RGrid (ngưỡng 1.15 và 1.25), tự chọn spread tối đa 50 bps, soft reset khoảng 4 x spread. | Cặp vừa ngang vừa trend trong ngày. Cộng đồng gọi là "endgame for MM bots" (04/2026). | Stop loss đo realized PnL ở pha Grid, unrealized ở pha RGrid. Proxy không tương quan thì chọn sai. |
| Blend | Trung bình có trọng số giữa mid sàn đang quote và mid sàn lớn hơn (blend weight 0 đến 100%, mặc định 50%). | Sàn thanh khoản mỏng, giá lệch sàn lớn. | Docs có ví dụ số không khớp công thức. |
| Signal | Mid sàn dịch theo RSI của cặp tín hiệu (Binance, Bybit, OKX, Bitget, Gate, Hyperliquid). RSI 70 dịch xuống 20 bps, RSI 30 dịch lên 20 bps, tối đa 50 bps. Spread 3 đến 20 bps. | Muốn nghiêng ngược vùng quá mua quá bán. | Phụ thuộc feed, thiếu tín hiệu thì không đặt lệnh. |

Preset trong docs: "Grid -1 bps" cho đa số; majors lúc giá chạy nhanh dùng Grid -1 bps, Aggressive, tránh mở cửa Mỹ, đóng cửa, FOMC, CPI; tài sản đi ngang OI thấp dùng Grid +5 bps; farm điểm maker (Ventuals) dùng Mid, spread trên 10 bps, thời lượng dài.

### 2.3 Chế độ nào cộng đồng dùng để farm

- Mid, spread 0, bias Neutral: setting "volume machine" phổ biến nhất (@devilonnn 10/2026, @OG_Branxi 12/2025 gọi Mid là "the volume machine" và Grid là "locking in profit").
- DGrid: được khen nhiều nhất từ 04/2026 (@OG_Branxi, @HyperliquidEco, @0xasrequired). Dùng khi muốn PnL quanh 0 mà không phải canh thị trường.
- Grid với reset 0.05% hoặc 0.125%, spread 1 bps, stop 5%: setting của @aaalex cho US500 20x (01/2026).
- Quy tắc tay: không dùng quá 50% margin cho bot; nếu đang xanh thì dừng, chốt, chạy lại; đòn bẩy trên 20x có thể bị sàn thanh lý trước khi stop loss kịp chạy (docs cảnh báo).

### 2.4 Cách user thiết lập

1. Đăng ký bằng email hoặc ví. Thời kỳ 11 đến 12/2025 cần "alpha code" mới mở được bot; code phát qua Discord và KOL. Hiện mở rộng, chưa xác minh thời điểm bỏ code.
2. Vào Key Management (icon ví góc phải). CEX: tạo API key trên sàn, whitelist IP của TreadFi, dán key vào. Hyperliquid, Extended, Pacifica và các perp DEX ví: bấm Connect, kết nối đúng ví đã dùng trên sàn, ký xác nhận. Hyperliquid cần tối thiểu 5 USDC và ví EVM. Có thể chọn sub-account hoặc vault.
3. Nạp tiền trực tiếp trên sàn. TreadFi không giữ tiền.
4. Menu Bots. Chọn Market Maker Bot hoặc Delta Neutral Bot. Điền form, xem Pre-Trade Analytics (margin có đủ, phí ước tính, max loss, POV, thời lượng, với DN có sided funding rate). Bấm Start.
5. Hoặc vào Agents, whitelist tài khoản, gõ yêu cầu, duyệt thẻ Propose Start.

Mô hình custody: tiền ở sàn, TreadFi chỉ có quyền giao dịch. Với CEX là API key trade-only (docs không nói rõ quyền). Với Hyperliquid là ký ví để uỷ quyền agent wallet và builder fee, khả năng cao theo cơ chế API wallet của HL; docs không nói key agent nằm ở đâu. Trang chủ ghi key "protected by encryption and secure authentication" và FAQ "Can tread.fi see my trades?" (không đọc được câu trả lời). Tổ chức có bản self-hosted AIO (AWS CDK, stable và nightly build) để key không rời hạ tầng của họ. Kết luận: với retail, key giao dịch nằm trên server TreadFi; với enterprise, tự host. Điểm cụ thể về mã hoá key chưa xác minh.

### 2.5 Theo dõi và quản lý

- Trang order: trạng thái bot (Running, Paused là trạng thái an toàn không phải lỗi, Insufficient Margin), uPnL và rPnL tách riêng, spread chart xem giá khớp hai chân, mức giá kích hoạt grid reset (xanh và đỏ).
- DN bot: Total Exposure, Net Exposure, funding ròng, trạng thái Opening, Open, Closing, Complete, Unbalanced. Cancel huỷ lệnh đang mở, Wind Down đóng cả hai chân, Delete chỉ xoá dòng không đóng vị thế.
- Huỷ MM bot sẽ tự đặt lệnh đóng phần dư để về net 0.
- Agents: mission log, pulse Thinking, Acting, Idle, Paused, Review mode khi gặp biến động bất thường. Pause không huỷ lệnh đang bay. Flatten dùng lệnh market.
- Telegram alerts cho lệnh. Trang Points theo dõi volume, phí đã tiết kiệm (TIP-2).
- Công cụ cộng đồng: treadtools (bảng xếp hạng), dashboard HypeStats của @yieldyumi (volume, PnL, cost mỗi 1 triệu, tỉ lệ stop loss theo từng biến).

## 3. Tool layer hay đặt lệnh thẳng

### 3.1 Vì sao TreadFi đóng gói bot như vậy

Nhìn vào cấu trúc MM bot và DN bot, mỗi lớp đóng gói giải quyết một vấn đề cụ thể:

- An toàn vốn: stop loss tính theo % margin trên PnL ròng realized cộng unrealized, kiểm tra liên tục; exposure pause khi lệch một phía; grid reset làm "cầu dao mềm" trước stop loss; DN bot đóng cả hai chân khi một chân chạm ngưỡng, dùng reduce-only, cho 5 đến 10 phút để thoát trật tự; huỷ bot thì tự đóng phần dư. Agent không thể bỏ sót các bước này vì chúng nằm trong engine.
- Kiểm soát maker hay taker: Passive Only là mặc định của MM bot. Trên Hyperliquid taker tốn khoảng 450 USD mỗi 1 triệu USD (tweet @mystcapital), maker gần 0 hoặc được rebate. Toàn bộ kinh tế farm điểm phụ thuộc vào việc không bao giờ cross spread. Một agent đặt lệnh thô bằng LLM rất khó bảo đảm điều này 100% số lần.
- Rate limit và làm mới lệnh: MM bot huỷ và đặt lại lệnh theo chu kỳ để bám top of book, DGrid đánh giá mỗi 30 giây, DN bot kiểm tra stop mỗi 10 giây. Vòng lặp này cần chạy trong engine có hàng đợi và giới hạn request theo sàn, không phải trong vòng lặp gọi LLM.
- Giải thích được: Pre-Trade Analytics hiện phí ước tính, max loss, POV, thời lượng, funding ròng trước khi chạy. User farm quan tâm đúng một chỉ số là chi phí mỗi 1 triệu USD volume (CPM). Bot đóng gói cho phép ước tính CPM trước khi chạy và so sánh sau khi chạy.
- Thử nghiệm và so sánh: vì tham số bot hữu hạn, user và agent có thể chạy A rồi B rồi so CPM (@sportytechworld 06/10/2026 cho agent loop Option A và B cả tuần rồi báo cấu hình tốt nhất). Lệnh thô không có không gian tham số để so.
- Chống lạm dụng: bot chỉ limit hai chiều, TreadFi có thể nói với sàn đây là cung cấp thanh khoản, không phải wash trade (@Zen_Trades1 11/2025). Điều này quan trọng khi sàn chấm điểm theo chất lượng maker (Ventuals, RiseX, Ondo quote trong 5 bps).

Bằng chứng mạnh nhất là chính TreadFi. Họ có engine đặt lệnh thô đầy đủ, nhưng khi làm Agents họ không cho AI gọi lệnh thô. Market Maker agent "selects pairs, mode, leverage and position size" rồi chạy MM bot. Desk Trader agent chọn thuật toán TWAP, VWAP, Iceberg rồi giao cho engine. Custom agents là prompt bọc ngoài cùng bộ tool. Họ mô tả kiến trúc là "supervisor agent" điều phối "specialized agents" chạy loop (tweet 08/07/2026). Tức là AI ở tầng lập kế hoạch và tham số hoá, không ở tầng khớp lệnh.

### 3.2 Phía ủng hộ đặt lệnh thẳng

- Một số reward không nằm trong tham số bot có sẵn. Ví dụ Ondo thưởng quote trong 5 bps trên 10 cặp cụ thể 24/7, phải tránh giờ mở cửa và tin. User làm được bằng custom agent chỉ vì agent được đặt điều kiện bật tắt bot; nếu tiêu chí phức tạp hơn thì bot có sẵn không theo kịp.
- Bot bị thiết kế cho một kiểu thị trường. Grid kẹt khi trend, RGrid kẹt khi ngang. DGrid là cách TreadFi vá vấn đề này bằng cách thêm một lớp quyết định nữa, chứ không phải bằng lệnh thô. Điều này cho thấy khi bot thiếu, câu trả lời vẫn là thêm bot hoặc thêm tham số.
- Vào ra vị thế một chiều (nạp, rút, cân lại hai sàn sau khi DN bot lệch) cần lệnh thường. Nhưng TWAP hoặc limit có passive only và reduce only cũng là một "bot" rất mỏng, không phải lệnh thô.
- Sàn mới chưa có bot: agent có thể cần gọi API sàn trực tiếp. Đây là lý do thực tế nhất để giữ một tool đặt lệnh cấp thấp, nhưng nên khoá sau bước duyệt.

### 3.3 Đánh giá

Tool layer thắng cho sản phẩm farm điểm. Lý do gọn: mục tiêu là volume maker rẻ nhất với rủi ro vốn bị chặn, không phải alpha hướng. Mọi bất biến (chỉ maker, stop theo margin, cân exposure, đóng sạch) là thứ phải đúng mọi lần, nên phải nằm trong code, không trong prompt. LLM giỏi ở việc còn lại: chọn sàn đang có reward, chọn cặp đi ngang, chọn mode, chọn đòn bẩy, chọn giờ chạy, so CPM, và giải thích cho user.

Đặt lệnh thẳng chỉ nên có dưới dạng một tool TWAP mỏng với passive only và reduce only, dùng cho mở đóng vị thế và sửa lệch, có thẻ duyệt như "Propose Start" của TreadFi.

## 4. Đề xuất bộ tool v0

Năm tool, theo thứ tự ưu tiên:

| # | Tool | Tham số agent điền | Lý do |
|---|---|---|---|
| 1 | `market_maker` | sàn, cặp, margin, đòn bẩy, mode (mid, grid, dgrid), spread bps, POV (passive, normal, aggressive), thời lượng, stop loss %, take profit % | Cỗ máy volume. 55 tweet, bot dùng nhiều nhất. Mid cho volume thuần, Grid và DGrid khi cần PnL quanh 0. Bỏ RGrid, Blend, Signal ở v0 vì ít người dùng và thêm bề mặt lỗi. |
| 2 | `delta_neutral` | sàn A và cặp A (long), sàn B và cặp B (short), notional, đòn bẩy, thời lượng, stop loss (50, 75, 90, uncapped) | Farm hai sàn một lúc và ăn funding. 32 tweet. Là cách duy nhất farm volume có hướng mà không lộ delta. Cần sẵn `wind_down`. |
| 3 | `twap_execute` | sàn, cặp, side, size, thời lượng, passive_only, reduce_only, limit price | Lệnh một chiều cho vào ra vị thế, nạp vốn vào vị thế hedge, sửa lệch sau DN. Thay cho lệnh thô, vẫn giữ maker. |
| 4 | `flatten` | sàn hoặc tất cả, cặp hoặc tất cả | Nút dừng khẩn: huỷ mọi lệnh, đóng reduce-only. TreadFi có ở cả MM bot (huỷ là đóng dư), DN bot (Wind Down) và Agents (Cancel With Positions). Không có cái này thì không cho agent chạy không người. |
| 5 | `estimate_cost` (read-only) | sàn, cặp, notional, đòn bẩy, mode, thời lượng | Pre-Trade Analytics: phí builder, phí maker, max loss, CPM ước tính, volume 24h của cặp, funding hai chân. Agent dùng để chọn và để giải thích cho user trước khi gọi tool 1 hoặc 2. |

Bổ sung read-only cần có nhưng không phải bot: `scan_rewards` (chương trình điểm, boost, liquidity reward, rebate đang chạy theo sàn và cặp) và `market_regime` (volume 24h, realized move, variance ratio như DGrid dùng) để agent chọn cặp đi ngang. Hai tool này là nơi LLM tạo giá trị thật; TreadFi mới chỉ làm nửa chừng qua custom agent.

Giới hạn nên sao chép từ TreadFi: tối đa 24 giờ mỗi task, trần số task đồng thời, không quá 50% margin tài khoản cho bot (quy tắc cộng đồng), đòn bẩy trần 20x (docs cảnh báo thanh lý trước stop loss), mọi lệnh có rủi ro phải qua thẻ duyệt trừ re-quote và điều chỉnh trong task đã duyệt.

## Mirana (Minara)

Không tìm thấy sản phẩm tên "Mirana". Gần nhất là Minara (minara.ai), tự gọi là "AI-native financial OS", trade crypto và cổ phiếu, có nhắc Hyperliquid nhiều lần (backtest "90d · Hyperliquid", autopilot đọc "Hyperliquid Positions", đường dẫn /app/trade/perps/BTC). Luồng từ ý định đến khớp lệnh trên trang chủ: prompt tự nhiên kích hoạt tool call phân tích (screen token, giá, sentiment, smart money); user yêu cầu chiến lược một câu ("Vibe Trading") hoặc dùng Strategy Generation rồi backtest AI (return, Sharpe, max drawdown); một thẻ lệnh hiện size, đòn bẩy, stop loss với nút Confirm; sau đó có "Agentic Trading" và "24/7 Autopilot". Minara có cả hai lớp: chiến lược đóng gói của creator trên leaderboard (Momentum Basket, Funding Harvest, Weekly Strongest 3) và đặt lệnh trực tiếp qua thẻ Confirm. Không công bố custody. Số trên trang là demo. Điểm giống TreadFi: có bước duyệt trước lệnh và có marketplace chiến lược. Điểm khác: Minara nhắm alpha hướng và backtest, TreadFi nhắm khớp lệnh và volume. Chưa xác minh gì ngoài nội dung trang chủ.

## Nguồn

Chính thức:

- https://tread.fi
- https://docs.tread.fi/llms.txt
- https://docs.tread.fi/bots/market-maker-bot.md
- https://docs.tread.fi/bots/market-maker-bot/participation-rate-and-duration.md
- https://docs.tread.fi/bots/market-maker-bot/reference-price-modes-and-risk-controls.md (và các trang con mid-mode, grid-mode, reverse-grid-mode-rgrid, dynamic-grid-mode-dgrid, blend-mode, signal-mode)
- https://docs.tread.fi/bots/market-maker-bot/strategies-and-configurations.md
- https://docs.tread.fi/bots/market-maker-bot/campaigns.md
- https://docs.tread.fi/bots/delta-neutral-bot.md
- https://docs.tread.fi/agents/overview-and-getting-started.md
- https://docs.tread.fi/agents/agent-roster.md
- https://docs.tread.fi/agents/starting-and-managing-agents.md
- https://docs.tread.fi/account-creation-and-api-key-connection/connecting-to-exchanges.md (và trang con hyperliquid, extended)
- https://docs.tread.fi/account-creation-and-api-key-connection/fees-schedule.md
- https://docs.tread.fi/competitions-points-and-affiliates-program/season-1-trade-earn-vote/points-program.md
- https://docs.tread.fi/creating-and-submitting-orders.md
- https://docs.tread.fi/other-features/telegram-bot.md
- https://docs.tread.fi/enterprise.md
- https://tread.fi/blog/a-year-of-tread
- https://tread.fi/blog/hyperliquid-mm-volume-bot-airdrop-farming

Bên thứ ba:

- https://defillama.com/protocol/tread.fi-perps (trả 403 khi fetch, số lấy qua kết quả search)
- https://docs.lighter.xyz/integrations
- https://minara.ai

Tweet (x.com/i/status/ID):

- @tread_fi: 2096432985075827087 (Terminal 88.6%, Agents 10.5%, MCP 0.9%), 2108525732570051013 (100B+ volume), 2095788968520683928 (custom agents là gì), 2074695830993527090 (supervisor agent), 2078035138219524262 (giới thiệu custom agents), 2098989061742665856 (TIP-1, TIP-2), 2100761562017599733 (TIP-3, TIP-4), 2101286944181510201 (bậc phí), 2100624087873618048 (đốt 3,023,113.03 TREAD), 2106016108004401341 (QRT), 2097347706419343495 (Kalshi), 2047325291224445168 (DGrid), 2014223246800068785 (RGrid), 1998556783242260797 (thread tính năng MM 12/2025), 2000829496245862735 (MM bot v2.0), 2110749197398147565 (Arcus)
- @davidyjeong: 2045909699032465806 (intent driven, không nhập lệnh tay), 2002050734842687958 (Grid mode)
- @OG_Branxi: 2056350592726094274 (S1: 24.5B, builder rank), 2053145187246034968 (1 triệu đầu trên RiseX, DGrid), 2106401921011720595 (4M Entropy), 2112585173359632602 (210 USD mỗi 1M), 2110392982839124301 (1,000 USD phí tiết kiệm), 2001263394394112275 (Mid vs Grid), 2014186170230640782 (Grid vs RGrid), 2030311113406210155 (DN bot), 2068260462723252283 (các agent), 2083210776802972157 (test agent vs TrueNorth), 2036685211208155388 (volume mỗi điểm), 2099019806984822911
- @sportytechworld: 2114252358905688549 (60M volume agent), 2116418596905640299 (Ondo 5 bps), 2117198256887505340, 2107341603371467037 (loop A B so CPM), 2095479989345128531 (230 agent), 2098378763948233011, 2094360455531897144 (agent theo điều kiện), 2063900553244270916 (DN bot 90% maker), 2100159959791243385 (máy tính điểm)
- @muarmemuar: 2049136732914799030 (DefiLlama 3.3M doanh thu, 805 user), 2106692428392440198, 2104155156610711561
- @HyperliquidEco: 2100242804517564613 (250M volume, 30k phí), 2115872141463953765 (HIP4), 2064616409590608332 (Ondo), 2047646189424677029 (DGrid)
- @Zen_Trades1: 1991743744534540354 (1.09M volume, 544.68 USD phí)
- @devilonnn: 2106763693962781116 (setting Entropy)
- @edwardmorra_btc: 2107125731989839924, 2107163715334222307 (DN bot funding)
- @laivietnam91: 2105506666456420365 (TIP-2, chạy bot qua Agent), 2042172353384488974
- @mystcapital: 1998705251315011859 (taker 450 USD mỗi 1M), 1995136512690925917 (cần Lighter)
- @onchainmonk: 1991326142683578554 (Lighter soon), 1990980883525087307, 2002287490951155888
- @Tom_Degen68: 2100798505736663227 (tiền ở sàn, 100B routed, 30k trader)
- @philarmonia0x: 2094205339247726878 (DefiLlama 20B, 4M phí)
- @aixbt_agent: 2108483914201493747 (ARR 13.4M, chưa xác minh)
- @Lady_Farming: 2101383593616392319 (ý kiến ngược về phí)
- @louisdives: 2117194001971593610 (2.5 bps), 2117153879939068344
- @aaalex: 2014098827939090700, 2010720190607278561 (setting Grid US500)
- @irlbogush: 2025668297807855645, 2024054164662227177
- @0xBellinili: 2064219952727400952, 2032807709201084721, 2011733995579384107
- @AlphaWolfPRMR: 2107498777632182662 (farm 3 lớp)
- @0xbeiu: 2116153154790388089 (MM HIP-4)
- @katexbt: 2100233392902283709 (TGE), 2110995381781749846 (Ondo rebate), 2076679961709334899
- @Pepotothemooon: 2024856803339878862 (DN long XYZ short Cash)
- @BeNJ4cker_: 2088267571288223770 (preset Pure Market Maker)
- @Perp338: 2101215224137162972
- @yieldyumi: 2029052899557269525 (dashboard HypeStats)
- @0xhustler18: 2005400030254088399 (bot cần code)
- @ninejobs: 2081080873391174140
- @Dhruv_Sniper: 2094094955933552976 (custom agent CCI)
