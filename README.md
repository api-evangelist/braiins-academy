# Braiins

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Braiins is a Prague-based Bitcoin mining technology company that operates the world's longest-running Bitcoin mining pool (Slush Pool, now Braiins Pool, mining since 2010), builds Braiins OS autotuning ASIC firmware, and co-developed Stratum V2, the successor mining-pool protocol. Three of its products publish an API: Braiins OS ships a Public API over both gRPC and REST that runs on the miner itself (63 REST operations, 16 proto3 service files); Braiins Hashpower publishes an OpenAPI 3.1 contract for its spot hashrate marketplace and fixed-duration contracts (35 operations); and Braiins Pool exposes six read-only JSON endpoints for pool, worker and reward statistics. Braiins Manager gates its own public API behind a paid tier. Braiins completed a SOC 2 Type 2 audit in 2025 and publishes a vulnerability disclosure policy with a five-business-day response commitment.

**Areas:** Bitcoin Mining · Cryptocurrency · Mining Pool · Mining Firmware · Blockchain · Stratum V2 · Hashrate Marketplace · Mining Management · ASIC · Energy

## Braiins on APIs.io

Everything profiled for Braiins is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/braiins-academy/](https://apis.io/providers/braiins-academy/)** — or go straight to one of the APIs or properties below.

### APIs (19)

- [Braiins Pool API](https://apis.io/apis/braiins-academy/braiins-pool-api/) — Read-only JSON API for Braiins Pool, the oldest active Bitcoin mining pool (first block 2010, 1.29M+ BTC mined). Six documented endpoints: pool stats and…
- [Braiins Accounts API](https://apis.io/apis/braiins-academy/braiins-academy-accounts-api/) — Read caller-owned balances and accounting transactions.
- [Braiins Actions API](https://apis.io/apis/braiins-academy/braiins-academy-actions-api/) — The Actions API from Braiins — 8 operation(s) for actions.
- [Braiins Advanced Settings API](https://apis.io/apis/braiins-academy/braiins-academy-advanced-settings-api/) — The Advanced Settings API from Braiins — 2 operation(s) for advanced settings.
- [Braiins Authentication API](https://apis.io/apis/braiins-academy/braiins-academy-authentication-api/) — The Authentication API from Braiins — 2 operation(s) for authentication.
- [Braiins Bid orders API](https://apis.io/apis/braiins-academy/braiins-academy-bid-orders-api/) — Create, update, cancel, and inspect caller-owned spot-market bids.
- [Braiins Configuration API](https://apis.io/apis/braiins-academy/braiins-academy-configuration-api/) — The Configuration API from Braiins — 2 operation(s) for configuration.
- [Braiins Contracts API](https://apis.io/apis/braiins-academy/braiins-academy-contracts-api/) — Quote, schedule, manage, and inspect caller-owned fixed-duration contracts.
- [Braiins Cooling API](https://apis.io/apis/braiins-academy/braiins-academy-cooling-api/) — The Cooling API from Braiins — 2 operation(s) for cooling.
- [Braiins Documentation API](https://apis.io/apis/braiins-academy/braiins-academy-documentation-api/) — The Documentation API from Braiins — 1 operation(s) for documentation.
- [Braiins License API](https://apis.io/apis/braiins-academy/braiins-academy-license-api/) — The License API from Braiins — 2 operation(s) for license.
- [Braiins Market configuration API](https://apis.io/apis/braiins-academy/braiins-academy-market-configuration-api/) — Spot-market rules, units, and fees needed to construct valid orders.
- [Braiins Market data API](https://apis.io/apis/braiins-academy/braiins-academy-market-data-api/) — Public statistics, order-book snapshots, trades, and OHLCV bars.
- [Braiins Miner API](https://apis.io/apis/braiins-academy/braiins-academy-miner-api/) — The Miner API from Braiins — 8 operation(s) for miner.
- [Braiins Network API](https://apis.io/apis/braiins-academy/braiins-academy-network-api/) — The Network API from Braiins — 2 operation(s) for network.
- [Braiins Performance API](https://apis.io/apis/braiins-academy/braiins-academy-performance-api/) — The Performance API from Braiins — 18 operation(s) for performance.
- [Braiins Pools API](https://apis.io/apis/braiins-academy/braiins-academy-pools-api/) — The Pools API from Braiins — 3 operation(s) for pools.
- [Braiins Upgrade API](https://apis.io/apis/braiins-academy/braiins-academy-upgrade-api/) — The Upgrade API from Braiins — 3 operation(s) for upgrade.
- [Braiins Version API](https://apis.io/apis/braiins-academy/braiins-academy-version-api/) — The Version API from Braiins — 1 operation(s) for version.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [braiins-academy-mcp.yml](https://apis.io/servers/braiins-academy/braiins-academy-mcp-yml/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Braiins Academy Braiins Hashpower Overlay](https://apis.io/overlays/braiins-academy/braiins-academy-braiins-hashpower-overlay/)
- [Braiins Academy Braiins Os Public Rest Api Overlay](https://apis.io/overlays/braiins-academy/braiins-academy-braiins-os-public-rest-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Braiins Academy Authentication](https://apis.io/security/braiins-academy/braiins-academy-authentication/)
- [Braiins Academy Domain Security](https://apis.io/security/braiins-academy/braiins-academy-domain-security/)
- [Braiins Academy Trust Center](https://apis.io/security/braiins-academy/braiins-academy-trust-center/)
- [Braiins Academy Vulnerability Disclosure](https://apis.io/security/braiins-academy/braiins-academy-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Braiins Academy Plans Pricing](https://apis.io/plans/braiins-academy/braiins-academy-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Braiins Academy Rate Limits](https://apis.io/rate-limits/braiins-academy/braiins-academy-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Braiins Academy Finops](https://apis.io/finops/braiins-academy/braiins-academy-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://braiins.com)
- [Documentation](https://academy.braiins.com)
- [Getting started](https://academy.braiins.com/braiins-hashpower/quick-start.md)
- [API reference](https://developer.braiins-os.com/latest/openapi.html)
- [Pricing](https://braiins.com/manager/pricing)
- [Sign up](https://manager.braiins.com/sign-up)
- [Support](https://help.braiins.com/en/support/tickets/new)
- [Blog](https://braiins.com/blog)
- [Terms of service](https://braiins.com/legal/braiins-general-terms-and-conditions)
- [Privacy policy](https://braiins.com/legal/privacy-policy)
- [GitHub](https://github.com/braiins)

## API Evangelist rating

**Kin Score 61.5** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 49.4 |
| Contract governance | 18.2 |
| Operational transparency | 68.4 |
| Developer ergonomics | 66.1 |
| Access clarity | 78.9 |

**Agent readiness 27.0** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | verified |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 29.5** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/braiins-academy/](https://apis.io/providers/braiins-academy/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Braiins: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/braiins-academy/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
