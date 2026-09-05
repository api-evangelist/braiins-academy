---
name: schedule-and-unwind-a-hashrate-contract
description: Quote, schedule, monitor and — when needed — cancel or terminate a fixed-duration Braiins Hashpower contract, using the API's own advisory operations to cost the decision before committing funds.
api: braiins-academy:braiins-hashpower-api
base_url: https://hashpower.braiins.com/v1
generated: '2026-09-04'
method: generated
source: openapi/braiins-academy-braiins-hashpower-openapi.yml
operations:
  - getContractSettings
  - getCurrentContractPricing
  - getCurrentContractCancelFees
  - checkContractSpeedAvailability
  - quoteContractCreation
  - scheduleContract
  - getActiveContracts
  - getContracts
  - getContractDetail
  - getContractReservations
  - getContractSettlements
  - getContractSpeedHistory
  - getContractDeliveryHistory
  - getContractActivity
  - cancelContract
  - terminateContract
---

# Schedule and unwind a Braiins Hashpower contract

A contract buys a fixed amount of hashrate for a fixed period with guaranteed delivery. Unlike a
spot bid it reserves capital up front, and unwinding it costs money — so this API gives you an
unusual amount of rope to rehearse first. Use it.

## Before you start

Owner token in the `apikey` header for everything except `getCurrentContractCancelFees`, which
accepts an optional key (without one it returns generic fee layers only, not yours).

## Step 1 — read the rules and the price

- `getContractSettings` (`GET /contract/settings`) — the constraints a contract must satisfy.
- `getCurrentContractPricing` (`GET /contract/pricing`) — live pricing.
- `getCurrentContractCancelFees` (`GET /contract/cancel-fee`) — the active standard and
  time-limited cancellation-fee layers, plus your own individual layer when you send a key.

**Read the cancellation fee before you schedule, not after.** It is published in advance
specifically so you can price the exit alongside the entry.

## Step 2 — rehearse

Both of these are explicitly advisory and reserve nothing:

- `checkContractSpeedAvailability` (`POST /contract/availability`) — is the capacity there.
- `quoteContractCreation` (`POST /contract/quote`) — returns the hashrate cost, the premium, the
  **cancellation-weighted premium on the Contractual Funding Tail**, available capacity and your
  balance for a proposed contract.

Braiins states plainly that a quote does not reserve capacity or funds and that scheduling
recomputes every check. Treat a quote as an estimate that can move, not a hold.

## Step 3 — schedule

`scheduleContract` (`POST /contract`). Pricing, funds, policy and capacity are recalculated
atomically at this point and the required funds are reserved.

There is no idempotency key. On a timeout, call `getActiveContracts` or `getContracts` and look
for the contract before retrying.

## Step 4 — monitor

- `getActiveContracts` (`GET /contract/active`), `getContracts` (`GET /contract`)
- `getContractDetail` (`GET /contract/{contract_id}/detail`)
- `getContractReservations` (`GET /contract/{contract_id}/reservation`) — what is held
- `getContractSettlements` (`GET /contract/{contract_id}/settlement`) — what has been paid
- `getContractSpeedHistory` and `getContractDeliveryHistory` — promised versus delivered
- `getContractActivity` (`GET /contract/activity`) — the event log

## Step 5 — choose the right exit

These are two different operations with two different windows. Picking the wrong one wastes a
call and, if you guess, money.

| Situation | Operation | Window |
|---|---|---|
| Contract scheduled, **delivery has not started** | `cancelContract` (`POST /contract/{contract_id}:cancel`) | Only before delivery begins. Cancellation rules and fees apply; Flux evaluates the request and the response reports whether it was **accepted** — a cancellation request is not a guaranteed cancellation. |
| Contract is **already delivering** | `terminateContract` (`POST /contract/{contract_id}:terminate`) | Before scheduled expiry. Permanent. Can trigger final accounting. |

Check the contract's state with `getContractDetail` before choosing. Then price the exit with
`getCurrentContractCancelFees` before sending it.

## Errors and limits

- Read the `grpc-message` response header, URL-decoded, for the reason behind a 400 or 403.
- 100 requests/minute per API credential on every operation here except the cancel-fee lookup,
  which is 100/minute per client IP. No rate-limit headers are returned.
