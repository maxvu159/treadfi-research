# 24. R3: luồng agent TreadFi vs Quantpets

> Đầu ra của [[r3-harness-research]], 9/11/2026. Bản chữ của hai artifact: [Harness 11 tầng](https://claude.ai/artifact/TJVJhr27bhHPsY9pC4CmEc) và [Ba luồng user](https://claude.ai/artifact/3i5YmsNxkosdV8XpXrq196). Sơ đồ dưới là mermaid, Obsidian tự vẽ (kiểu vẽ tay chỉ hiện trên artifact). Ô đỏ nét đứt = Quantpets thiếu, ô xanh nét đứt = Quantpets hơn, ô xanh dương = tool thật của TreadFi.

## 0. Năm từ cần biết

- **agent**: một "nhân viên" AI có bộ tool riêng. TreadFi có 10 agent gốc, cộng agent do user tạo trên marketplace.
- **tool**: một việc agent được gọi, có form cố định (tên, trường bắt buộc, trần). Tên tool trong trang là tên thật trong catalog TreadFi.
- **mandate**: tờ giao việc cho agent: tài khoản, cặp, vốn tối đa, mục tiêu. Agent chỉ được làm trong tờ này.
- **CPM**: cost per million, phí bỏ ra cho mỗi $1M volume. Số quan trọng nhất khi farm.
- **D23**: quyết định của Duy (30/10): agent Quantpets không bao giờ bật bot hay đặt lệnh, chỉ đề xuất.

TreadFi chưa có Lighter nên ví dụ bên TreadFi chạy trên Hyperliquid, Quantpets trên Lighter. Cùng một bài: người mới có $500, muốn farm volume.

## 1. Dùng chiến lược có sẵn

Ví dụ: "Tôi có $500, farm volume BTC perp giúp tôi."

### TreadFi

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  U(["User: farm volume BTC, $500"]):::user --> O["Orchestrator<br/><small>(LLM cửa chính: hiểu ý, chọn agent)</small>"]
  O --> Q{"Thiếu gì?"}
  Q -->|thiếu tài khoản| QC["Question card<br/><small>(hỏi đúng 1 câu, có nút chọn)</small>"]:::user
  QC --> O
  Q -->|đủ| A["Chọn agent Tunable Spread Quoter<br/><small>(agent quote liên tục, 1 trong 10 agent gốc)</small>"]
  A --> T1[/"list_accounts<br/><small>(tài khoản nào được phép đặt lệnh)</small>"/]:::tool
  T1 --> T2[/"list_pairs<br/><small>(tên cặp chuẩn BTC:PERP-USDC)</small>"/]:::tool
  T2 --> T3[/"get_pretrade_analytics<br/><small>(bạn sẽ chiếm bao nhiêu % volume, biến động, cảnh báo khớp không hết)</small>"/]:::tool
  T3 --> C["Confirm card<br/><small>(tài khoản, cặp, vốn $500, đòn bẩy, thời lượng, stop loss 10%, nút Start)</small>"]
  C --> S(["User bấm Start"]):::user
  S --> T4[/"place_mm_order<br/><small>(24 trường: cặp, vốn, đòn bẩy, thời lượng, spread, stop loss, số lần lặp...)</small>"/]:::tool
  T4 --> E["Hệ thống lệnh của Tread đặt quote 2 chiều<br/><small>(stop loss ở mức bot)</small>"]
  E --> NB["Không có backtest<br/><small>(chỉ có số của các lần chạy trước)</small>"]:::minus
  NB --> M[/"list_mm_orders, get_order_pnl<br/><small>(PnL, phí, lý do dừng)</small>"/]:::tool
  M --> K(["User: dừng"]):::user --> T5[/"cancel_order<br/><small>(huỷ quote, kiểm lại ở sàn)</small>"/]:::tool
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef tool fill:#e6ebff,stroke:#1a3dff,color:#1b1d1f
  classDef minus fill:#f1f1ee,stroke:#9a9a94,stroke-dasharray:5 4,color:#1b1d1f
```

- **1 lớp LLM**: Orchestrator đứng trên, user nói một câu. Agent con và tool nằm dưới, user không phải biết.
- **ước tính trước**: Pretrade là một tool đọc riêng, guide bắt gọi trước khi đặt.
- **duyệt 1 lần**: Một thẻ, đủ số, một nút. Sau Start agent theo dõi và báo.
- **lưu ý nguồn**: Luồng này là chat trong app (đã thấy khi Max long BTC). Tên tool lấy từ catalog MCP; qua MCP thì không có Orchestrator, LLM ngoài tự gọi tool.

### Quantpets (hôm nay)

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  U(["User: farm volume, $500"]):::user --> G0["Không có ai hỏi ý<br/><small>(user tự đoán template nào)</small>"]:::gap
  G0 --> ST(["User vào Studio, chọn template Volume<br/><small>(1 trong 3 template gốc)</small>"]):::user
  ST --> W["Workspace: chỉnh tay 4 núm<br/><small>(spread, nhịp đặt lại, đòn bẩy, điều kiện dừng)</small>"]
  W --> CH["Chat kiểu cũ: tune<br/><small>(chỉ chỉnh núm, không chọn hộ)</small>"]
  CH --> BT["Backtest<br/><small>(nến 1 phút, ra cost per $1K và volume)</small>"]:::plus
  BT --> G1["Không có ước tính từ thị trường thật trước khi bật<br/><small>(chiếm bao nhiêu % volume, có khớp hết không)</small>"]:::gap
  G1 --> GL(["User bấm Go live"]):::user
  GL --> CK["5 check rời<br/><small>(có backtest, tiền trên sàn, đòn bẩy, quyền trade, phí)</small>"]
  CK --> R["Runner đặt lệnh 24/7<br/><small>(trần position, đòn bẩy, drawdown kiểm mỗi vòng)</small>"]:::plus
  R --> P["Portfolio: card bot"]
  P --> K(["User: Stop / Stop & close"]):::user --> RC["Dừng chỉ xong khi sàn xác nhận hết lệnh"]
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef gap fill:#fbe3e3,stroke:#c23b3b,stroke-dasharray:5 4,color:#1b1d1f
  classDef plus fill:#e3f4ea,stroke:#1f8f5a,stroke-dasharray:5 4,color:#1b1d1f
```

- **thiếu lớp trên**: User phải tự chọn template, tự chỉnh núm. Người mới không biết Volume khác Low risk ở đâu.
- **thiếu ước tính**: Trước khi bật chỉ có số backtest từ nến. Hai template MM dính stop 5% ngay ngày đầu, nên số này chưa tin được.
- **hơn**: Có backtest. Trần rủi ro mức tài khoản nằm trong runner, TreadFi qua MCP không có.
- **6 màn**: Studio → núm → chat → backtest → Go live → 5 check. TreadFi: nói 1 câu, trả lời 1 card, bấm 1 nút.

### Nên làm như TreadFi

1. **Một câu hỏi đầu vào thay cho picker.** User gõ "farm volume $500", agent chọn template Volume, điền núm, hiện 1 card. (giải thích: harness v1 đã có tool `propose_template` làm đúng việc này, nhưng chat agent đang ẩn sau `?chat=agent`. Chỉ cần bật mặc định.)
2. **Một card duyệt thay cho 5 check rời.** Card ghi: template, vốn, cost per $1K từ backtest, cảnh báo, nút Start. (giải thích: 5 check giữ nguyên, gom vào một thẻ. Để agent được mở thẻ này thì D23 phải đổi.)
3. **Tool ước tính từ thị trường thật.** Trả: chiếm bao nhiêu % volume, có khớp hết không, phí dự kiến. (giải thích: TreadFi có `get_pretrade_analytics`. Quantpets đọc được sổ lệnh sàn qua adapter sẵn có, thiếu tool gói lại.)

## 2. Tự tạo chiến lược riêng

Ví dụ, cùng đề bài hai bên: "Bot farm volume, tự giảm spread khi phí vượt ngân sách, dừng khi vượt $50 phí."

### TreadFi: tạo agent riêng

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  U(["User vào Marketplace → Build agent<br/><small>(một form, không phải chat)</small>"]):::user --> B1["Mục tiêu 1 câu<br/><small>(farm volume, giữ phí dưới ngân sách)</small>"]
  B1 --> B2["Loại agent<br/><small>(chỉ đọc / đặt lệnh một lần / chạy liên tục)</small>"]
  B2 --> B3["Tick tool được dùng<br/><small>(đọc: get_balances, list_mm_orders; ghi: place_mm_order, cancel_order)</small>"]
  B3 --> B4["Trường bắt buộc khi giao việc<br/><small>(tài khoản, cặp, vốn tối đa, ngân sách phí)</small>"]
  B4 --> B5["Giới hạn cứng<br/><small>(vốn tối đa, hệ thống chặn nếu vượt)</small>"]
  B5 --> B6["Luật dừng, luật chỉnh<br/><small>(phí > $50 thì dừng; phí tăng thì nới spread. Logic trong agent, không phải hệ thống chặn)</small>"]
  B6 --> B7["Trường báo cáo<br/><small>(volume, phí, PnL, CPM, đã chỉnh gì)</small>"]
  B7 --> B8(["Test với ngân sách nhỏ"]):::user
  B8 --> B9["Publish lên Marketplace"]
  B9 --> O["Dùng như luồng 1: nói với Orchestrator, nó chọn agent này"]
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
```

- **agent = tool + tờ giao việc**: Không viết code. Tick tool có sẵn, khai trường bắt buộc, đặt trần.
- **ghép được**: Một agent dùng nhiều tool: đặt quote, đọc phí, huỷ. Không bị dính vào một loại bot.
- **chưa thấy UI**: Orchestrator tự nhận "chưa có tài liệu builder chi tiết". Các bước trên là khung nó mô tả, cần vào Marketplace bấm thử.

### Quantpets: tạo chiến lược trong AI Studio

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  U(["User vào Studio, phải chọn 1 trong 3 template trước"]):::user --> G0["Chiến lược dính với 1 template<br/><small>(không ghép quote + đo phí + tự dừng; không có chỗ đặt luật 'phí > $50 thì dừng')</small>"]:::gap
  G0 --> AG(["Mở chat agent"]):::user
  AG --> T1[/"propose_template<br/><small>(đề xuất template + núm, code kiểm trước khi hiện)</small>"/]:::tool
  T1 --> T2[/"propose_edit, propose_challenger<br/><small>(sửa núm; đưa bản thách đấu để so)</small>"/]:::tool
  T2 --> T3[/"run_backtest, offer_study<br/><small>(backtest trên dữ liệu chưa thấy, có chống overfit)</small>"/]:::tool
  T3 --> T4[/"get_backtest, diagnose<br/><small>(30 kiểu cảnh báo tự động)</small>"/]:::tool
  T4 --> P1["Phần này TreadFi không có"]:::plus
  P1 --> G1["Không có tờ giao việc, không có tool ghi<br/><small>(agent không đề xuất được 'chạy với $500, dừng khi phí > $50')</small>"]:::gap
  G1 --> T5[/"open_go_live<br/><small>(chỉ mở màn, user tự bấm)</small>"/]:::tool
  T5 --> GL(["User Go live như luồng 1"]):::user
  GL --> G2["Không publish được thành agent cho người khác thuê<br/><small>(chỉ publish bộ tham số để copy)</small>"]:::gap
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef tool fill:#e6ebff,stroke:#1a3dff,color:#1b1d1f
  classDef gap fill:#fbe3e3,stroke:#c23b3b,stroke-dasharray:5 4,color:#1b1d1f
  classDef plus fill:#e3f4ea,stroke:#1f8f5a,stroke-dasharray:5 4,color:#1b1d1f
```

- **hơn**: Harness v1 có backtest, bản thách đấu, chống overfit, cảnh báo bằng code. TreadFi không có backtest.
- **dính template**: Studio bắt chọn template trước, chat chỉ chỉnh núm của template đó. Đề bài "tự giảm spread khi phí vượt ngân sách" không có chỗ đặt.
- **agent không có việc**: Hết cảnh báo là hết. Không có tờ giao việc (vốn, trần, mục tiêu) nên không có gì để agent chạy tiếp.

### Nên làm như TreadFi

1. **Vào bằng câu hỏi, không bằng picker.** Studio mở chat trước, agent gọi `propose_template`, rồi mới hiện pet tương ứng. (giải thích: đảo thứ tự 2 màn, không cần code mới.)
2. **Tờ giao việc là một form, không phải câu chat.** Tài khoản sàn, template, vốn tối đa, drawdown tối đa, ngân sách phí. (giải thích: TreadFi chỉ chặn cứng vốn tối đa. Quantpets đã có trần trong runner, chỉ cần nối form này vào đó.)
3. **Sau v0: cho một chiến lược có luật chỉnh và luật dừng theo phí.** (giải thích: đây là cái làm "farm, as required." khác một bot MM thường. Quantpets có 3 template rời nhau. Việc lớn, để track chiến lược T5 quyết.)

## 3. Dùng chiến lược của người khác (marketplace)

Ví dụ: creator X đã đăng "bot farm volume BTC". Bên TreadFi đó là một agent để thuê, bên Quantpets đó là một listing để copy. Khác nhau này là bản chất, không phải do chọn ví dụ.

### TreadFi: thuê agent

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  U(["User mở Marketplace"]):::user --> L["Hồ sơ agent<br/><small>(mô tả, tool được dùng, trường bắt buộc, số người đang thuê)</small>"]
  L --> H(["Bấm Hire"]):::user
  H --> R["Agent vào danh sách của user<br/><small>(thuê ≠ đang chạy, ≠ được phép đặt lệnh)</small>"]
  R --> ASK(["User: dùng agent X, $500, BTC"]):::user
  ASK --> O["Orchestrator lập tờ giao việc<br/><small>(tài khoản, cặp, vốn tối đa $500, ngân sách phí)</small>"]
  O --> C["Confirm card"] --> S(["User Start"]):::user
  S --> A1[/"place_mm_order, lặp nhiều phiên"/]:::tool
  A1 --> A2[/"list_mm_orders<br/><small>(đo volume, phí, PnL mỗi phiên)</small>"/]:::tool
  A2 --> A3["Agent tính CPM, chỉnh spread<br/><small>(logic của creator X)</small>"]
  A3 -->|phí trong ngân sách| A1
  A3 -->|phí vượt| A4[/"cancel_schedule<br/><small>(dừng lịch, đóng phiên đang chạy)</small>"/]:::tool
  A4 --> RP["Báo cáo: volume, phí, CPM, đã chỉnh gì"]
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef tool fill:#e6ebff,stroke:#1a3dff,color:#1b1d1f
```

- **thuê = thêm năng lực**: Agent thuê về chạy trên tài khoản và trần của bạn, vẫn hỏi bạn trước khi đặt.
- **có vòng lặp**: Agent của người khác mang theo logic: đo, so, chỉnh, dừng. Không chỉ là một bộ tham số.

### Quantpets: copy listing

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  U(["User mở Discover<br/><small>(10 listing đã publish)</small>"]):::user --> L["Trang listing<br/><small>(số backtest chuẩn 30 ngày trên $100k)</small>"]
  L --> G0["Không có nhãn 'chưa chạy thật'<br/><small>(gate đang ở chế độ uat: cái gì cũng publish được)</small>"]:::gap
  G0 --> CP(["Bấm Copy"]):::user
  CP --> CL["Clone thành chiến lược riêng<br/><small>(không xem được núm, không sửa, không publish lại)</small>"]
  CL --> GL(["Go live như luồng 1: 5 check"]):::user
  GL --> R["Runner chạy với vốn và auto-stop của user"]
  R --> G1["Copy là bộ tham số đóng băng<br/><small>(không có logic đo phí, chỉnh, dừng theo ngân sách)</small>"]:::gap
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef gap fill:#fbe3e3,stroke:#c23b3b,stroke-dasharray:5 4,color:#1b1d1f
```

- **đúng chuẩn**: Copy chạy trên sàn và vốn của người copy, không xem được núm của creator.
- **copy ≠ thuê**: Copy là chụp tham số. Thuê là mang về một agent có vòng lặp. Người mới copy xong vẫn phải tự canh.
- **không biết listing đã chạy thật chưa**: Production để gate `uat`, publish từ backtest, không nhãn.

### Nên làm như TreadFi

1. **Nhãn trên listing: "đã chạy thật N ngày" hoặc "chỉ backtest".** (giải thích: gate có sẵn 3 chế độ, chỉ thiếu nhãn trên card. Quyết định bỏ nhãn (PD-21, 31/10) Duy ghi "Bean và Frank đã duyệt", Max cần xác nhận với Frank.)
2. **Copy xong mở thẳng card duyệt, không đi lại Studio.** (giải thích: TreadFi thuê xong là nói với Orchestrator được ngay.)
3. **Sau v0: listing kèm luật chạy (ngân sách phí, khi nào dừng).** (giải thích: bước biến copy thành thuê. Cần tool ghi và cổng kiểm ở trang Harness trước.)

## Tóm lại


## Nguồn
- `sources/research/2026-11-09-treadfi-mcp-tools.md`, `2026-11-09-treadfi-mcp-guide.md` (catalog MCP thật, 29 tool)
- `sources/research/2026-11-08-treadfi-agent-task-chat.md`, `-2.md` (2 phiên chat của Max với Orchestrator)
- `docs/19-product-map.md`, `docs/15-tool-layer-scorecard.md`, `docs/14b-q2-agent-execution-sheets.md`
- Ảnh: `docs/assets/harness/`
---

# Phần B. Bản đồ catalog TreadFi (9/11)

> Bản chữ của artifact [Bản đồ catalog](https://claude.ai/artifact/4F1DsqhhDziTDU7qpusX5U). Trả lời 3 câu: user muốn gì thì đi qua tool nào; agent phức tạp đi qua nhiều tool thì sao; thuê agent thì catalog đổi gì.

## B1. Catalog: 29 tool, 6 nhóm

**A. Tài khoản và cặp** (6 tool)
- `list_accounts`tài khoản nào, sàn nào, được đặt lệnh hay chỉ xem
- `list_pairs`tìm tên cặp chuẩn trên sàn, gõ sai vẫn ra
- `get_balances`margin trống, margin đang dùng, equity
- `get_leverage`đòn bẩy đang đặt cho cặp
- `get_pair_max_leverage`đòn bẩy tối đa sàn cho phép
- `set_leverage` WRITEđổi đòn bẩy

**B. Ước tính trước** (2 tool)
- `get_order_book`sổ lệnh đầu bảng, spread hiện tại
- `get_pretrade_analytics`lệnh này sẽ chiếm bao nhiêu % volume, biến động, cảnh báo khớp không hết. Guide bắt gọi trước khi đặt

**C. Đặt lệnh** (5 tool, toàn WRITE)
- `place_order`1 lệnh, chọn cách rải lệnh theo thời gian (8 kiểu, ví dụ TWAP = chia đều theo phút), có TP/SL
- `place_mm_order`bot quote 2 chiều, lặp được, lịch được
- `place_dn_order`bot 2 chân long/short trên 2 tài khoản, có max hold
- `place_multi_order`nhiều lệnh cùng lúc trên 1 tài khoản
- `place_chained_order`chuỗi lệnh chạy lần lượt theo thứ tự

**D. Theo dõi** (10 tool)
- `list_orders`lệnh đang sống (30 ngày gần nhất)
- `get_order_history`lệnh đã xong, huỷ, hết hạn
- `get_order_stats`1 lệnh đi tới đâu: khớp bao nhiêu, phí
- `get_order_pnl`PnL một lần chạy bot MM hoặc DN
- `list_mm_orders`cả lịch sử bot MM một lần gọi: PnL, phí, lý do dừng
- `list_schedules`lịch nào sắp nổ, đã chạy mấy lần
- `get_working_orders`các lệnh con đang treo của một bot
- `get_recent_fills`từng lần khớp: giá, số lượng, phí
- `get_sibling_orders`lệnh đi kèm: TP, SL, lệnh đóng DN
- `get_order_details`dán link lệnh vào là ra chi tiết

**E. Điều khiển** (4 tool, toàn WRITE)
- `cancel_order`huỷ 1 lệnh hoặc 1 bot. Không dừng lịch lặp
- `cancel_schedule`dừng lịch MM và đóng phiên đang chạy. Chỉ cho lịch MM, lịch DN bị từ chối
- `pause_order` / `resume_order`tạm dừng, chạy tiếp

**F. Thi đấu** (2 tool)
- `list_competitions`giải nào đang mở, đã đăng ký chưa
- `get_competition_board`bảng xếp hạng một giải

**Không có trong catalog: dữ liệu thị trường.** Không có tool đọc nến, funding, chỉ báo, order flow, ví người khác, Polymarket. Cũng không có tool chuyển tiền giữa Tread Wallet và sàn, dù chat có làm được. Nghĩa là **catalog MCP chỉ là phần thực thi và tài khoản**. Các agent phân tích (General Research, Execution Quant, Portfolio Analyzer, Prediction Analyst, HyperTracker) dùng một bộ tool khác trong app mà ta không nhìn thấy qua MCP. Khi vẽ các luồng dưới, phần đó ghi là "tool nội bộ, không thấy".

## B2. Luồng tổng: user muốn gì thì đi qua nhóm nào

Một sơ đồ cho mọi case. Orchestrator (con tiếp tân nhận câu hỏi, chọn agent) rẽ theo ý định. Question card = thẻ hỏi có nút chọn; confirm card = thẻ xác nhận trước khi đặt lệnh. Mỗi nhánh ghi agent nào làm và đi qua nhóm tool nào (chữ A đến F theo mục 1).

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  U(["User nói một câu"]):::user --> O["Orchestrator<br/><small>(hiểu ý định, hỏi thiếu gì bằng question card, chọn agent)</small>"]
  O --> I{"Ý định?"}

  I -->|"hỏi thị trường, nên long không"| R["General Research Agent<br/><small>(chỉ đọc)</small>"]
  R --> RX["Tool nội bộ: nến, funding, order flow<br/><small>(không có trong catalog MCP)</small>"]:::hidden
  RX --> RR["Báo cáo: xu hướng, vùng giá, điều kiện sai<br/><small>(không đặt lệnh)</small>"]

  I -->|"tôi đang lời lỗ sao, khớp tốt không"| PA["Portfolio Analyzer / Execution Quant<br/><small>(chỉ đọc)</small>"]
  PA --> PD["D: get_order_history, get_order_pnl, get_recent_fills<br/><small>(+ tool nội bộ tính giá khớp trung bình, trượt giá)</small>"]:::D
  PD --> PR["Báo cáo tài khoản"]

  I -->|"long BTC $5, 10x"| DT["Desk Trader"]
  DT --> DA["A: list_accounts, list_pairs, get_pair_max_leverage<br/><small>(10x có vượt trần sàn không)</small>"]:::A
  DA --> DB["B: get_pretrade_analytics"]:::B
  DB --> DC1["Confirm card"] --> S1(["User Start"]):::user
  S1 --> DL["A: set_leverage 10"]:::A
  DL --> DC["C: place_order<br/><small>(mua $50 = $5 margin × 10, TWAP 15 phút, stop_loss_pct)</small>"]:::C
  DC --> DD["D: get_order_stats, get_sibling_orders<br/><small>(tiến độ, chân TP/SL)</small>"]:::D

  I -->|"farm volume, $500"| MM["Tunable Spread Quoter / Market Maker"]
  MM --> MA["A: list_accounts, list_pairs, get_balances<br/><small>(còn bao nhiêu margin trống)</small><br/>B: get_pretrade_analytics"]:::A
  MA --> MC1["Confirm card"] --> S2(["User Start"]):::user
  S2 --> MC["C: place_mm_order<br/><small>(spread, SL 10%, repeat_count)</small>"]:::C
  MC --> MD["D: list_mm_orders, list_schedules"]:::D

  I -->|"ăn funding chênh 2 sàn"| DN["Delta Neutral"]
  DN --> NA["A: 2 tài khoản, 2 cặp cùng coin"]:::A
  NA --> NC1["Confirm card"] --> S3(["User Start"]):::user
  S3 --> NC["C: place_dn_order<br/><small>(notional chia 50/50, max hold, SL)</small>"]:::C
  NC --> ND["D: list_orders MULTI_ORDER, get_order_pnl, get_sibling_orders<br/><small>(chân đóng DN)</small>"]:::D

  I -->|"dừng, tạm dừng, đóng"| ST["Agent đang quản lý lệnh đó<br/><small>(hoặc Desk Trader nếu không còn agent)</small>"]
  ST --> SE["E: cancel_order / cancel_schedule / pause / resume<br/><small>(chat web: chạy thẳng theo lệnh user, không có card. Đóng vị thế = place_order reduce_only, tức chỉ được đóng, không mở thêm)</small>"]:::E


  DD --> O2["Orchestrator báo lại user<br/><small>(mọi nhánh quay về đây)</small>"]
  MD --> O2
  ND --> O2
  SE --> O2
  PR --> O2
  RR --> O2
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef A fill:#e6ebff,stroke:#1a3dff,color:#1b1d1f
  classDef B fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef C fill:#fbf0d9,stroke:#b7791f,color:#1b1d1f
  classDef D fill:#ece6fb,stroke:#6b4fbb,color:#1b1d1f
  classDef E fill:#fbe3e3,stroke:#c23b3b,color:#1b1d1f
  classDef F fill:#eeeeea,stroke:#9a9a94,color:#1b1d1f
  classDef hidden fill:#fff,stroke:#9a9a94,stroke-dasharray:5 4,color:#1b1d1f
```

- **cùng một khung**: Mọi nhánh đặt lệnh đi A → B → card → C → D. Khác nhau chỉ ở tool C nào và trường nào. Đây là cái "có standard" mà bạn tìm.
- **chỉ đọc = không qua C**: Research, Portfolio, Execution Quant không bao giờ chạm nhóm C. Không có cách nào từ báo cáo nhảy thẳng sang lệnh.
- **nhánh dừng là riêng**: Orchestrator tìm agent đang giữ lệnh đó rồi chuyển việc, để hai agent không cùng đụng một vị thế.
- **ngoài sơ đồ**: Nhóm F (giải thi đấu) Orchestrator trả lời thẳng, không agent nào trong 10 con. Polymarket và thể thao đi qua Prediction Analyst, Sports Consultant bằng tool nội bộ.
- **catalog không có B cho DN**: Pretrade chỉ nhận 1 cặp. DN 2 chân không có ước tính gộp; agent phải gọi 2 lần hoặc bỏ qua.

## B3. Bảng case: muốn gì thì gọi tool nào

| User muốn | Agent | Thứ tự tool | Trường quan trọng |
|---|---|---|---|
| Nên long BTC không? | General Research | tool nội bộ (nến, funding, order flow) → báo cáo | không có lệnh |
| Long BTC $5, 10x | Desk Trader | `list_accounts` → `list_pairs` → `get_pair_max_leverage` → `get_pretrade_analytics` → card → `set_leverage` → `place_order` → `get_order_stats` → `get_sibling_orders` | set_leverage 10 trước; quote_asset_qty 50 ($5 margin × 10), strategy TWAP, duration_minutes 15, stop_loss_pct |
| Farm volume $500 | Tunable Spread Quoter (liên tục) hoặc Market Maker (một phiên) | A → `get_pretrade_analytics` → card → `place_mm_order` → `list_mm_orders` → `cancel_schedule` | margin_usd 500, leverage, duration_minutes, default_spread_bps, stop_loss_pct 10, repeat_count, pov_preset |
| Ăn funding chênh HL và Ondo | Delta Neutral | `list_accounts` (1 lần, ra cả 2 tài khoản) → `list_pairs` (1 lần, 2 sàn) → card → `place_dn_order` → `list_orders` → `get_order_pnl` → `get_sibling_orders` | long_pair, short_pair, 2 account, notional_usd, duration_minutes, position_duration_minutes, dn_stoploss_percent |
| Mua $1,000 chia 3 đợt, đợt sau chỉ chạy khi đợt trước xong | Desk Trader | A → B → card → `place_chained_order` → `get_order_stats` (có current_leg) | legs theo thứ tự ưu tiên |
| Mỗi sáng 6h chạy bot MM 2 giờ, 7 ngày | Market Maker | card → `place_mm_order` với start_datetime + repeat_trigger daily → `list_schedules` | repeat_count 7, repeat_trigger daily |
| Tôi lời lỗ sao tuần này? | Portfolio Analyzer | `get_order_history` → `get_order_pnl` từng run → `get_recent_fills` | after/before, statuses |
| Lệnh này khớp có tốt không? | Execution Quant | `get_order_details` (dán link) → `get_recent_fills` → tool nội bộ tính VWAP, slippage | ref = link lệnh |
| Đóng hết, dừng hết | agent đang giữ lệnh | `list_orders` → `cancel_schedule` (lịch MM) → `cancel_order` (từng bot, DN) → `place_order` reduce_only (đóng vị thế) | cancel một run không dừng lịch; cancel_schedule không nhận lịch DN; đóng vị thế là một lệnh ngược, không phải cancel |
| Mua ETH và SOL cùng lúc, rải 1 giờ | Desk Trader | A → B ×2 → card → `place_multi_order` → `get_working_orders` (lệnh con nào đang treo) | legs, strategy TWAP, duration_minutes 60 |
| Sắp ra tin, tạm dừng bot rồi chạy lại | agent đang giữ bot | `list_orders` → `pause_order` → `resume_order` | target_kind, target_id |
| Tôi đứng hạng mấy trong giải? | Orchestrator trả lời thẳng | `list_competitions` → `get_competition_board` | slug |
| Đổi đòn bẩy cặp X lên 5 | Desk Trader | `get_pair_max_leverage` → card → `set_leverage` | account_id, pair, leverage |
| Ví nào trên HL đang thắng? | HyperTracker | tool nội bộ (ví công khai) |  |

Hai dòng "tool nội bộ" là việc agent làm bằng bộ tool không có trong catalog MCP. Còn lại 29 tool đều xuất hiện ít nhất một lần.

## B4. Agent phức tạp: một agent đi qua nhiều nhóm

Ví dụ: "Farm funding + volume": tìm cặp có funding chênh, mở DN, chạy MM trên chân long để lấy thêm volume, đo phí mỗi giờ, phí vượt ngân sách thì thu hẹp, hết funding thì đóng cả hai.

```mermaid
%%{init: {"look":"handDrawn","theme":"neutral","flowchart":{"curve":"basis","htmlLabels":true}}}%%
flowchart TD
  M0["Tờ giao việc: 2 tài khoản, coin"] --> M["Trần: vốn tối đa $1,000 (hệ thống chặn)<br/>phí $50/ngày, giữ tối đa 3 ngày (agent tự canh)"]
  M --> F1["Tìm funding chênh<br/><small>(tool nội bộ của Research, không trong catalog)</small>"]:::hidden
  F1 --> F2["A: list_accounts, list_pairs ×2"]:::A
  F2 --> F3["B: get_pretrade_analytics ×2<br/><small>(mỗi chân một lần)</small>"]:::B
  F3 --> CC["Confirm card<br/><small>(mỗi lệnh ghi một thẻ; gộp 2 lệnh vào một thẻ chưa thấy)</small>"] --> S(["User Start"]):::user
  S --> F4["C: place_dn_order<br/><small>(position_duration_minutes = 3 ngày)</small>"]:::C
  F4 --> F5["C: place_mm_order trên chân long<br/><small>(margin nhỏ, spread rộng, repeat on_complete)</small>"]:::C
  F5 --> L{"Mỗi giờ"}
  L --> F6["D: get_order_pnl DN, list_mm_orders<br/><small>(phí, PnL, volume)</small>"]:::D
  F6 --> F7["Tính CPM (phí trên mỗi $1M volume), so ngân sách<br/><small>(logic trong agent, hệ thống không chặn)</small>"]
  F7 -->|"phí ổn"| L
  F7 -->|"phí vượt"| F8["E: cancel_schedule MM<br/><small>(giữ DN)</small>"]:::E
  F7 -->|"funding hết chênh"| F9["E: cancel_order DN<br/><small>(chân đóng DN tự nổ, kiểm bằng get_sibling_orders)</small>"]:::E
  F8 --> RP["Báo cáo: volume, phí, PnL, CPM, đã đổi gì"]
  F9 --> RP
  classDef user fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef A fill:#e6ebff,stroke:#1a3dff,color:#1b1d1f
  classDef B fill:#e3f4ea,stroke:#1f8f5a,color:#1b1d1f
  classDef C fill:#fbf0d9,stroke:#b7791f,color:#1b1d1f
  classDef D fill:#ece6fb,stroke:#6b4fbb,color:#1b1d1f
  classDef E fill:#fbe3e3,stroke:#c23b3b,color:#1b1d1f
  classDef hidden fill:#fff,stroke:#9a9a94,stroke-dasharray:5 4,color:#1b1d1f
```

- **ghép = gọi nhiều tool C**: Không có tool "DN + MM". Agent gọi `place_dn_order` rồi `place_mm_order`, hai run riêng, agent tự giữ quan hệ giữa chúng.
- **vòng lặp là của agent**: Đo, so, chỉnh, dừng là prompt và logic của người tạo agent. Hệ thống chỉ chặn cứng vốn tối đa và tài khoản.
- **chỗ yếu**: Hai run không biết nhau. DN dừng mà MM còn chạy thì thành vị thế một chiều. Agent phải tự huỷ MM trước, hệ thống không làm hộ.
- **thiếu dữ liệu**: Catalog không có funding, nên bước tìm cặp phải nhờ tool nội bộ hoặc nguồn ngoài.
- **gọi lại không đặt trùng**: Mỗi lệnh ghi có một mã riêng. Vòng lặp mỗi giờ gọi lại cùng mã thì không tạo lệnh thứ hai; hệ thống trả "retryable" thì chờ rồi gọi lại cùng mã.

## B5. Thuê agent trên marketplace: catalog có đổi không

Không. Thuê chỉ đổi **ai viết luật** và **ai điền mandate**. Tool vẫn là 29 tool, chạy trên tài khoản và trần của người thuê.

|  | Agent gốc (10 con) | Agent tự tạo | Agent thuê trên marketplace |
|---|---|---|---|
| Tool dùng | 29 tool + tool nội bộ | chọn trong 29 tool (không thêm được) | bộ tool người tạo đã tick, đọc được trong hồ sơ |
| Luật chạy | Tread viết | bạn viết | người tạo viết, bạn không sửa được |
| Mandate (tờ giao việc) | Orchestrator hỏi bạn | bạn khai trường bắt buộc (ví dụ ngân sách phí, giữ tối đa là trường đề xuất; TreadFi chỉ chắc chắn chặn `total_margin_cap_usd`), Orchestrator hỏi bạn | trường bắt buộc do người tạo khai, Orchestrator hỏi bạn |
| Tiền và key | luôn là tài khoản của bạn. Người tạo agent không chạm được. Chỉ vốn tối đa là hệ thống chặn cứng | (như trái) | (như trái) |
| Xác nhận | chat web: tool C (đặt lệnh) qua confirm card; tool E (huỷ, dừng) chạy thẳng khi user ra lệnh. Thuê không bỏ được thẻ này. Qua MCP không có thẻ nào | (như trái) | (như trái) |
| Ví dụ | Desk Trader, Delta Neutral, Tunable Spread Quoter | agent "funding + volume" ở mục 4 | "farm, as required." (8 người thuê, bản v3, theo ảnh tweet 6/9 trong docs/14b) |

## B6. Ghi chú cho Quantpets

1. **Nhóm A, D, E Quantpets đã có trong phần chạy lệnh (runner), nhưng agent chưa gọi được.** (giải thích: chưa có tool ghi và chưa có cổng kiểm trước tool ghi. Xem trang Harness.)
2. **Nhóm B thiếu hẳn: không có ước tính từ sổ lệnh thật trước khi bật.** (giải thích: chỉ có backtest từ nến.)
3. **Luồng tổng của Quantpets sẽ ngắn hơn:** user nói một câu → agent chọn 1 trong 3 template (Volume ≈ place_mm_order, Low risk ≈ place_dn_order, High ≈ place_order có rule) → ước tính → 1 thẻ → Start → runner → pet. Một Orchestrator nhỏ rẽ 4 nhánh (tạo, sửa, xem, dừng) là đủ cho v0; không cần 10 agent.
