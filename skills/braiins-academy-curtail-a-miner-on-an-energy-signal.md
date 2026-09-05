---
name: curtail-a-miner-on-an-energy-signal
description: Reduce, pause and restore a Braiins OS miner's power draw in response to an energy price or grid signal, over the device-local Braiins OS Public API, without leaving the machine in a state you cannot back out of.
api: braiins-academy:braiins-os-api
base_url: http://<miner>/api/v1
generated: '2026-09-04'
method: generated
source: openapi/braiins-academy-braiins-os-public-rest-api-openapi.json
operations:
  - login
  - getApiVersion
  - getConstraints
  - getMinerDetails
  - getMinerDetailedStatus
  - getPerformanceMode
  - getTunerState
  - setPowerTarget
  - decrementPowerTarget
  - relativePowerTarget
  - setDps
  - pauseMining
  - resumeMining
  - setDefaultPowerTarget
  - getCoolingState
---

# Curtail a Braiins OS miner on an energy signal

The Braiins OS Public API runs **on the miner**, not on a Braiins server: the OpenAPI declares
`servers: http://miner/`, and the host is that ASIC's address on your network. Ports 80 (REST)
and 50051 (gRPC) must be reachable. Everything below is per-device; for a farm, drive the same
verbs across a range with `braiins-toolbox` (see `cli/braiins-academy-cli.yml`).

## Step 1 — authenticate to the device

`POST /api/v1/auth/login` (`login`) with the miner's username and password returns a token. Send
it in the `Authorization` header on every subsequent request.

The published document declares no `securitySchemes`, so a generated client will not add the
header for you — add it yourself.

## Step 2 — establish what this device can do

- `getApiVersion` (`GET /api/v1/version/`) — the API version actually running. The contract has
  shipped 15 releases; do not assume 1.14.0 features exist on an older build.
- `getConstraints` (`GET /api/v1/configuration/constraints`) — the device's own limits. A power
  target outside them is a **412 Precondition Failed**, so read this before computing a target,
  not after.
- `getMinerDetails`, `getPerformanceMode`, `getTunerState` — where the machine is now.

## Step 3 — choose the curtailment mechanism

Two different tools; they are not interchangeable.

**Dynamic Performance Scaling** — `setDps` (`PUT /api/v1/performance/dps`). Braiins OS scales the
power target down on its own toward a floor and back up when conditions allow. Use it for
sustained thermal or grid pressure where you want the firmware to manage the ramp. Note that a
miner in DPS cooldown reports `DpsCooldown` as its detailed status with an expected resume time —
that is normal, not a fault.

**Direct power targeting** — `setPowerTarget` (`PUT /api/v1/performance/power-target`) sets an
absolute watt target; `decrementPowerTarget` and `relativePowerTarget` (`PATCH`) move it by a
step. Use these when an external signal (a price feed, a demand-response instruction) already
decides the number.

**Full stop** — `pauseMining` (`PUT /api/v1/actions/pause`), reversed by `resumeMining`
(`PUT /api/v1/actions/resume`). Use for a hard curtailment event. The miner reports `UserPause`
so a later reader can tell your pause from a thermal one.

## Step 4 — verify the machine did what you asked

`getMinerDetailedStatus` (`GET /api/v1/miner/status/detailed`, added in Public API 1.14.0) is the
operation to poll. It returns the state **and the reason**: `Normal`, `UserPause`, `ThermalPause`
(with a `ThermalPauseReason`), `DpsCooldown` with an expected resume time, `DeadPools`,
`MissingLicense`, `TunerError`, `HardwareError`, `CoolingDown`, `Preheating`, and more.

Prefer it to `getMinerStatus`, which the gRPC contract deprecated in 1.14.0 in favour of it.

Pair it with `getCoolingState` (`GET /api/v1/cooling/state`) — fan states and the hottest sensor
— when the reason is thermal.

## Step 5 — back out

Every change above has a documented reversal, and none of them has a deadline:

| Did | Undo |
|---|---|
| `setPowerTarget` / `decrementPowerTarget` / `relativePowerTarget` | `setDefaultPowerTarget` (`PUT /api/v1/performance/power-target/default`) |
| `setHashrateTarget` | `setDefaultHashrateTarget` |
| `setDps on` | `setDps off` |
| `pauseMining` | `resumeMining` |
| `stop` | `start` |
| `setAdvancedSettings` | `resetAllAdvancedSettings` (`DELETE /api/v1/advanced-settings/`) |

**Do not reach for `factory_reset`** (`PUT /api/v1/actions/factory-reset`) as a way out of a bad
configuration. It clears the miner's configuration and optionally its network settings, it is
irreversible, and it can leave a device you can no longer reach. Use the specific default-restore
above instead.

## Retry discipline

There is **no idempotency key** on this API. A timed-out `setPowerTarget` may or may not have
applied. Re-read `getPerformanceMode` or `getTunerState` and compare before sending it again —
especially for the increment/decrement operations, where a blind retry compounds.

Expect 409 (the device rejects the transition in its current state), 412 (a constraint blocks the
value) and 501 (unsupported on this hardware model or firmware build). None of them returns a
structured error body; the OpenAPI declares descriptions only.
