---
name: roll-a-fleet-onto-braiins-os
description: Discover ASICs on a network, install or upgrade Braiins OS across them, point them at a Stratum V2 pool endpoint, and roll the change back to stock firmware if it goes wrong.
api: braiins-academy:braiins-os-api
generated: '2026-09-04'
method: generated
source: >-
  openapi/braiins-academy-braiins-os-public-rest-api-openapi.json,
  cli/braiins-academy-cli.yml,
  https://academy.braiins.com/braiins-pool/stratum-v2-manual.md
operations:
  - systemUpgrade
  - restoreStock
  - getAutoUpgradeStatus
  - updateAutoUpgradeConfig
  - applyContractKey
  - getLicenseState
  - getPools
  - createPoolGroup
  - setBatchPools
  - updatePoolGroup
  - deletePool
  - getMinerDetailedStatus
---

# Roll a fleet onto Braiins OS

This is a two-layer job. Discovery and fan-out are the **Braiins Toolbox CLI**; per-device
verification and pool configuration are the **Braiins OS Public API**. Use each for what it is
good at — the API has no concept of a fleet, and the CLI has no concept of a response body you
can assert on.

## Prerequisites

- Target devices reachable on TCP 22, 80 and 50051.
- Devices must be able to reach `e5a33065.bos.braiins.com` on port 3336 or the install will not
  proceed.
- Braiins OS 23.03+ for the Public API; some Toolbox commands also work against Bitmain and
  MicroBT stock firmware.

## Step 1 — discover (CLI)

```
braiins-toolbox scan --installable-only --format csv
```

`--installable-only` filters to devices Braiins OS can actually be put on. Keep the output as
your `--ip-file` for every later step.

## Step 2 — install or upgrade (CLI, then verify per device)

```
braiins-toolbox firmware install --ip-file fleet.txt --contract-code <code>
braiins-toolbox firmware upgrade --ip-file fleet.txt --target-version <version>
```

Per device, the API equivalents are `systemUpgrade` (`POST /api/v1/upgrade/system-upgrade`) and,
for licensing, `applyContractKey` (`PUT /api/v1/license/apply-contract-key`). Since Public API
1.14.0 `applyContractKey` accepts `keep_existing` to retain licences already on the miner — set
it unless you intend to replace them.

Verify with `getLicenseState` (`GET /api/v1/license/license`). A miner with no valid licence
reports `MissingLicense` as its detailed status and will not hash.

## Step 3 — point the fleet at a pool

Fleet-wide:

```
braiins-toolbox miner set-pool-urls --url-file pools.txt --ip-file fleet.txt
```

Per device, use the pool group operations: `getPools` (`GET /api/v1/pools/`), `createPoolGroup`
(`POST /api/v1/pools/`), `setBatchPools` (`PUT /api/v1/pools/batch`), `updatePoolGroup`
(`PUT /api/v1/pools/{uid}`), `deletePool` (`DELETE /api/v1/pools/{uid}`).

**The protocol is chosen by the URL scheme.** Stratum V2 on Braiins Pool looks like:

```
stratum2+tcp://stratum.braiins.com:3333/<pool public key>
```

The key in the path is what lets the miner verify the endpoint and refuse a man-in-the-middle
attempting to hijack hashrate — it is the whole security argument for V2, and omitting it drops
you back to V1. Keep a `stratum+tcp://` entry as the backup pool in the same group; the load
balance strategy on the group decides how work is split.

Nothing in the contract exposes "which protocol version is this pool" as a typed field. If you
need to assert V2, assert on the URL scheme in `PoolConfiguration.url`.

## Step 4 — confirm each device is actually hashing

Poll `getMinerDetailedStatus` (`GET /api/v1/miner/status/detailed`). The reasons that matter
after a roll-out are `DeadPools` (every configured pool is unreachable — usually a wrong URL or
a blocked port), `MissingLicense`, `UnsupportedHardware` and `TunerError`.

`Preheating`, `WaitingWhileCold`, `Defrosting`, `CoolingDown` and `DelayedStart` are normal
start-up states, not failures. Do not roll back on them.

## Step 5 — roll back

```
braiins-toolbox firmware restore --ip-file fleet.txt --concurrency 300
```

Per device this is `restoreStock` (`POST /api/v1/upgrade/restore-stock`), which puts the vendor
firmware back. Braiins documents the path but states no window or precondition for it — treat it
as available but unrehearsed, and prove it on one machine before you need it on a thousand.

## Ongoing

`getAutoUpgradeStatus` and `updateAutoUpgradeConfig` (`GET`/`PATCH /api/v1/upgrade/auto-upgrade`)
opt the fleet in to staying current. Braiins ships Braiins OS on a calendar-versioned cadence
(26.08.1 as of 2026-08-25) and the Public API contract separately on semver — watch
https://academy.braiins.com/braiins-os/papi-changelog.md for contract changes, because there is
no Sunset or Deprecation header to catch them at runtime.
