---
name: buy-hashrate-on-the-spot-market
description: Place, monitor, adjust and cancel a spot bid for Bitcoin hashrate on the Braiins Hashpower marketplace, without over-committing funds or getting stuck in a grace period.
api: braiins-academy:braiins-hashpower-api
base_url: https://hashpower.braiins.com/v1
generated: '2026-09-04'
method: generated
source: openapi/braiins-academy-braiins-hashpower-openapi.yml
operations:
  - spotGetMarketSettings
  - getFeeStructure
  - spotGetMarketStats
  - spotGetOrderbookSnapshot
  - getAccountBalances
  - spotPlaceBid
  - spotGetCurrentBids
  - spotGetBidDetail
  - spotGetBidSpeedHistory
  - spotGetBidDeliveryHistory
  - spotEditBid
  - spotCancelBid
---

# Buy hashrate on the Braiins Hashpower spot market

Braiins Hashpower is a spot market for Bitcoin mining hashrate. You place a bid with a satoshi
budget; matched hashrate is delivered to a mining pool you nominate and settled hourly.

## Before you start

- Authenticate with the `apikey` header. Placing or editing a bid requires an **owner** token; a
  read-only token can call every step below except `spotPlaceBid`, `spotEditBid` and
  `spotCancelBid`.
- Market data (`spotGetOrderbookSnapshot`, `spotGetMarketTrades`, `spotGetMarketBars`,
  `spotGetMarketStats`) needs no credential at all. Use it to build and test before you hold a token.
- **There is no idempotency key on this API.** If `spotPlaceBid` times out, do not blind-retry —
  call `spotGetCurrentBids` first and check whether the bid landed.

## Step 1 — read the market rules before you compute anything

Call `spotGetMarketSettings` (`GET /spot/settings`). It returns the live price tick, the hashrate
unit, minimum and maximum bid amounts, minimum durations, the **bid grace period**, and the edit
timing rules. The server validates every order against these values, so a bid computed from
yesterday's numbers is a 400.

Two fields decide what you can undo later:

- the grace period, during which `spotCancelBid` is **rejected**
- the interval that must elapse before a hashrate limit can be decreased again

Read `hr_unit` here too — spot price fields (`price_sat`) are satoshi per that unit.

## Step 2 — price the trade

- `getFeeStructure` (`GET /spot/fee`) — the current fee schedule. As of the beta the Spot Bid Fee
  is 0%, charged continuously from the settled amount during execution.
- `spotGetMarketStats` and `spotGetOrderbookSnapshot` — where the market actually is.
- `getAccountBalances` (`GET /account/balance`) — available, reserved and total. A bid reserves funds.

## Step 3 — place the bid

`spotPlaceBid` (`POST /spot/bid`). Set `cl_order_id` to a value you generate: it is the only
caller-controlled handle on the record, and both `spotEditBid` and `spotCancelBid` accept it in
place of the server-assigned `order_id`. Do **not** treat it as an idempotency key — Braiins does
not document duplicate-submission behaviour.

The response carries the server-assigned public `order_id`. Store both.

## Step 4 — watch delivery

- `spotGetCurrentBids` (`GET /spot/bid/current`) — active bids only.
- `spotGetBidDetail` (`GET /spot/bid/detail/{order_id}`).
- `spotGetBidSpeedHistory` (`GET /spot/bid/speed/{order_id}`) — the hashrate time series.
- `spotGetBidDeliveryHistory` (`GET /spot/bid/delivery/{order_id}`) — what was actually delivered
  and settled.

Settlement is hourly, so poll on that order of magnitude, not per second.

## Step 5 — adjust, and know what you cannot take back

`spotEditBid` (`PUT /spot/bid`) updates only the fields you send; omitted fields are unchanged.
It is **not symmetric**: market timing and range rules can prevent price or hashrate-limit
*decreases*, and a hashrate limit can only be decreased again after the interval named in
`spotGetMarketSettings`. Raising a bid is easy to do and hard to undo — decide the ceiling first.

## Step 6 — cancel

`spotCancelBid` (`DELETE /spot/bid`) with a JSON body carrying **exactly one** of `order_id` or
`cl_order_id`. Sending both is a 400.

Cancellation is refused while the grace period from step 1 is still running. If you get a
rejection, do not retry in a tight loop — you are burning the 100 requests/minute budget against
a condition that is time-based. Wait out the grace period and cancel once.

## Errors and limits

- 400 invalid or rule-violating, 401 missing/invalid key, 403 wrong ACL role or someone else's
  bid, 404 unknown resource, 429 over the limit, `default` upstream failure.
- **The reason is in the `grpc-message` response header, URL-encoded — not in the body.** Always
  read and percent-decode it; the status code alone will not tell you which rule failed.
- 100 requests/minute per API credential on every authenticated operation; 500/minute per client
  IP on the anonymous market-data operations. **No rate-limit or Retry-After headers are
  returned** — track your own budget.
