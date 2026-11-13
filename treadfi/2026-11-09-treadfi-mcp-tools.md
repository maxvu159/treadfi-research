# TreadFi MCP tool catalog (GET /api/mcp/tools), 2026-11-09

> Lấy bằng MCP token của Max, chỉ đọc catalog, không gọi dispatch. JSON gốc: `2026-11-09-treadfi-mcp-tools.json`. Guide: `2026-11-09-treadfi-mcp-guide.md`.
> 29 tool: 19 READ, 10 WRITE.

| Tool | Cờ | Bắt buộc | Mô tả (1 dòng) |
|---|---|---|---|
| `get_balances` | READ |  | Read free / used margin and equity for one or more of the caller's connected exchange accounts |
| `get_order_book` | READ |  | Snapshot top-of-book for ``symbol`` on the account's exchange |
| `get_pretrade_analytics` | READ |  | Project what a hypothetical order would look like BEFORE you propose it: expected POV (% of expected volume over the duration), projected pr |
| `get_leverage` | READ |  | Read the venue's stored leverage (and margin mode when present) for an account on a given pair |
| `get_pair_max_leverage` | READ |  | Read the venue's MAX allowable leverage for a ``(exchange, pair)`` -- the public cap, not the account's current setting |
| `list_pairs` | READ |  | List tradable pairs on a venue |
| `list_accounts` | READ |  | List the caller's exchange accounts as ``{id, name, exchange, tier}`` |
| `list_orders` | READ |  | List the caller's LIVE orders, multi-orders, and chained orders -- every non-terminal target |
| `get_order_history` | READ |  | The caller's RECENT orders, multi-orders, and chained orders, most-recent-first and BOUNDED -- the read for TERMINAL history (completed / ca |
| `get_order_stats` | READ |  | Cheap, fixed-size execution summary for ONE order: status, executed qty / notional, fill / fee totals, and last-fill timestamp -- the headli |
| `get_order_pnl` | READ |  | PnL for ONE MULTI_ORDER run (an MM / DN bot run): ``pnl`` (net), ``realized_pnl``, ``unrealized_pnl``, plus ``fees`` and ``executed_notional |
| `list_mm_orders` | READ |  | The caller's market-maker (MM) bot runs, newest first -- the SAME feed the Tread.fi MM history table renders, so ONE call reads a run end-to |
| `list_schedules` | READ |  | The caller's order SCHEDULES, newest first -- the rows behind the web UI's schedule tab |
| `get_working_orders` | READ |  | The working child OMS orders UNDER a given target -- the legs a MULTI_ORDER parent is currently working, the single order itself for an ORDE |
| `get_recent_fills` | READ |  | Recent fills under a given target, newest first -- per-fill detail (price, qty, fee, timestamp) for an order, multi-order, or chained order  |
| `get_sibling_orders` | READ |  | Sibling orders of a target -- siblings, not children |
| `get_order_details` | READ |  | Load ONE of the caller's orders behind a tread.fi link (or a bare id) and return its details |
| `place_order` | WRITE |  | Place a single live order on one of the caller's TRADE accounts |
| `place_mm_order` | WRITE |  | Place a market-maker (MM) bot order on ONE of the caller's TRADE accounts: a two-leg buy+sell spread that continuously quotes around a refer |
| `place_dn_order` | WRITE |  | Place a delta-neutral (DN) bot order: a two-leg pair-trade that BUYs ``long_pair`` on one of the caller's TRADE accounts and SELLs ``short_p |
| `place_multi_order` | WRITE |  | Place a GENERAL multi-order on the caller's TRADE accounts: ONE parent order wrapping N child orders that execute together, all sharing the  |
| `place_chained_order` | WRITE |  | Place an ORDERED chain of single orders |
| `cancel_order` | WRITE |  | Cancel one of the caller's live orders |
| `cancel_schedule` | WRITE |  | Stop one of the caller's market-maker SCHEDULES -- the MCP equivalent of the schedule tab's X |
| `pause_order` | WRITE |  | Pause one of the caller's live orders so it stops working |
| `resume_order` | WRITE |  | Resume one of the caller's paused orders |
| `set_leverage` | WRITE |  | Set the venue's stored leverage for one of the caller's accounts on a given pair |
| `list_competitions` | READ |  | List trading competitions visible to the caller |
| `get_competition_board` | READ |  | One competition's leaderboard: the top 25 of each race (points / live volume / live PnL) plus the caller's own row |

## Trường của từng tool WRITE

### `place_order`

Place a single live order on one of the caller's TRADE accounts. This is a WRITE that moves real money -- size it yourself and monitor it; nothing on the MCP side manages the position after it lands. Required: ``account_id`` (a TRADE account from list_accounts), ``pair`` (canonical ``BASE[:VARIANT]-QUOTE`` from list_pairs), ``side`` (buy/sell), and EXACTLY ONE quantity frame (``base_asset_qty`` / ``quote_asset_qty`` / ``target_order_qty``). Size with get_pretrade_analytics first. The order is scoped to the caller's own eligible accounts -- a foreign account is rejected. Every call requires a fresh idempotency key; a retry reusing it returns the first result instead of double-placing.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `place_mm_order`

Place a market-maker (MM) bot order on ONE of the caller's TRADE accounts: a two-leg buy+sell spread that continuously quotes around a reference price for ``duration_minutes``. Size it with ``notional_usd`` (total volume worked) OR ``margin_usd`` (collateral locked; notional = margin * leverage * 20). Quoting mode is ``reference_price``: ``mid`` (symmetric around mid), ``grid`` / ``reverse_grid`` (laddered), ``dynamic_grid`` (a self-driving grid that auto-tunes its spread from realized vol), ``blend`` (mid blended with a reference venue -- requires ``reference_exchange``), or ``signal`` (quote tracks an RSI signal -- requires ``signal_name``). Risk exits: ``stop_loss_pct`` defaults to 10, ``take_profit_pct`` is OFF unless passed -- set both to reproduce a web-UI config. This is a WRITE that moves real money. The order is scoped to the caller's own eligible accounts -- a foreign account is rejected. Optionally SCHEDULE or REPEAT the session (same semantics as the web MM form): ``start_datetime`` delays the (first) run to an ISO-8601 time; ``repeat_count`` (1-20) with ``repeat_trigger='on_complete'`` (the default) re-runs back-to-back -- each next run starts when the previous completes -- while ``repeat_trigger='daily'`` runs once per day at the ``start_datetime`` clock time (daily REQUIRES ``start_datetime``). A scheduled placement (``start_datetime`` set) returns a schedule_id, NOT an order: it won't appear in list_mm_orders until it fires -- inspect it with list_schedules an

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `place_dn_order`

Place a delta-neutral (DN) bot order: a two-leg pair-trade that BUYs ``long_pair`` on one of the caller's TRADE accounts and SELLs ``short_pair`` on another, worked in via TWAP over ``duration_minutes``. The two legs are usually the same asset family across two venues (the funding-divergence premise of DN). ``notional_usd`` is the total two-leg notional, split 50/50. Optionally auto-unwind: ``position_duration_minutes`` (max hold before the closing order fires) and/or ``dn_stoploss_percent``. This is a WRITE that moves real money. The order is scoped to the caller's own eligible accounts -- a foreign account is rejected. Every call requires a fresh idempotency key; a retry reusing it returns the first result instead of double-placing. Pairs are canonical ``BASE[:VARIANT]-QUOTE`` (resolve via list_pairs); accounts are TRADE accounts from list_accounts.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `place_multi_order`

Place a GENERAL multi-order on the caller's TRADE accounts: ONE parent order wrapping N child orders that execute together, all sharing the algo config you pass at the top level (strategy / duration / shaping knobs). Each ``legs`` entry becomes a child order with its own pair / side / quantity (and optional per-leg account override -- otherwise the top-level ``account_id`` applies). Use this to fan one execution intent across legs at once (e.g. TWAP-buy ETH and SOL over an hour) without placing each separately. A multi-order is the lower-level OMS construct; the market-maker (MM) and delta-neutral (DN) BOT PRODUCTS built on it have their own dedicated tools -- use place_mm_order / place_dn_order for those, NOT this. A WRITE that moves real money; scoped to the caller's own accounts -- a foreign account on any leg is rejected. Every call requires a fresh idempotency key; a retry reusing it returns the first result instead of re-placing.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `place_chained_order`

Place an ORDERED chain of single orders. The OMS runs the legs strictly in array order -- each leg auto-starts once the previous one completes (sequencing is automatic; never gate a leg on the prior leg's fill via ``order_condition``). Pass ``legs=[{account_id, pair, side, <qty>, strategy, ...}, ...]`` -- 2 to 10 heterogeneous legs. A typical use is an open leg followed by a conditional ``reduce_only`` exit leg. This is a WRITE that moves real money; every leg's account is scoped to the caller's own accounts -- a foreign / unknown account on any leg rejects the whole chain. Every call requires a fresh idempotency key. For a SINGLE order use place_order; for a simultaneous multi-pair fan-out use place_multi_order.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `cancel_order`

Cancel one of the caller's live orders. Keyed by ``target_kind`` (ORDER / MULTI_ORDER / CHAINED_ORDER) + ``target_id`` -- get both from list_orders. For a CHAINED_ORDER the whole chain is unwound; for a MULTI_ORDER the parent and its legs. A WRITE: owner-scoped (a foreign id resolves to a non-leaky 'not found'), and every call requires a fresh idempotency key.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `cancel_schedule`

Stop one of the caller's market-maker SCHEDULES -- the MCP equivalent of the schedule tab's X. The composite kill switch: cancels the schedule FIRST (so no next iteration fires mid-teardown), THEN cancels any live MultiOrder session(s) it spawned, reduce-only-flattening residual exposure. This is the right tool when cancel_order alone doesn't stick because the schedule re-places the bot: get the ``schedule_id`` from list_schedules (or the ``schedule_id`` echoed on list_mm_orders rows / returned by a scheduled place_mm_order) and cancel it here. A session that is ALREADY draining is left to finish flattening (cancelling a drain abandons it and strands the residual) and reported under ``skipped_cleaning_up`` -- that is a success, do NOT cancel_order it. Returns the structured stop report (``schedule_cancelled`` / ``orders_cancelled`` / ``skipped_cleaning_up`` / ``residual_flattening_engaged`` / ``residual_remaining``). Idempotent and safe on an already-terminal schedule -- it still sweeps any live session left quoting. Only ``market_maker`` schedules; a delta_neutral schedule is rejected (its positions manage their own lifecycle -- cancel those orders directly). A WRITE: owner-scoped (a foreign id resolves to a non-leaky 'not found'), and every call requires a fresh idempotency key.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `pause_order`

Pause one of the caller's live orders so it stops working. Keyed by ``target_kind`` (ORDER / MULTI_ORDER / CHAINED_ORDER) + ``target_id`` -- get both from list_orders. Resume later with resume_order. A WRITE: owner-scoped (a foreign id resolves to a non-leaky 'not found'), and every call requires a fresh idempotency key.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `resume_order`

Resume one of the caller's paused orders. Keyed by ``target_kind`` (ORDER / MULTI_ORDER / CHAINED_ORDER) + ``target_id`` -- get both from list_orders. The inverse of pause_order. A WRITE: owner-scoped (a foreign id resolves to a non-leaky 'not found'), and every call requires a fresh idempotency key.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|

### `set_leverage`

Set the venue's stored leverage for one of the caller's accounts on a given pair. Required: ``account_id`` (a TRADE account from list_accounts), ``pair`` (canonical ``BASE[:VARIANT]-QUOTE``), and ``leverage`` (the target multiplier). The account is owner-scoped -- a foreign account is rejected as 'account not found'. The venue may decline the change when a position is open; the result reports the effective leverage and whether it changed. A WRITE: every call requires a fresh idempotency key.

| Trường | Kiểu | Giới hạn | Ghi chú |
|---|---|---|---|
