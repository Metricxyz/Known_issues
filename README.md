# Metric — Audits and Known Issues

Security audit reports and known-issue documents for the Metric OMM smart contracts. Issues listed or acknowledged in these documents are considered known and are not eligible for bug bounty rewards.

## Sherlock
| Date | Report |
|---|---|
| 2026-07-06 | [Collaborative Audit Report](sherlock/2026.07.06%20-%20Final%20-%20Metric%20Collaborative%20Audit%20Report%201783346590.pdf) |
| 2026-09-15 | [Public Bug Bounty Audit Contest Report](sherlock/2026.09.15%20-%20Final%20-%20Metric%20Public%20Bug%20Bounty%20Audit%20Contest%20Report%201789468517.pdf) |
| 2026-10-10 | [Collaborative Audit Report](sherlock/2026.10.10%20-%20Final%20-%20Metric%20Collaborative%20Audit%20Report%201791595461.pdf) |

## Zellic
| Date | Report |
|---|---|
| 2025-06-29 | [Metric OMM Smart Contract Security Assessment](zellic/Metric%20OMM%20-%20Zellic%20Audit%20Report.pdf) |
| 2026-10-06 | [OMM Reaudit Smart Contract Security Assessment](zellic/OMM%20Reaudit%20-%20Zellic%20Audit%20Report.pdf) |

## Other
| Document | Description |
|---|---|
| [contest-known-issues.pdf](other/contest-known-issues.pdf) | Known economic behaviour of the bin design (audit contest) |

## Reported issues

Issues reported to security@metric.xyz after the audits above. They are known and are not eligible for bug bounty rewards, including reports that share a root cause with them.

### Fix pending

Details will be added once the fixes are deployed.

| Reported | Component |
|---|---|
| 2026-10 | Liquidity adder (`MetricOmmPoolLiquidityAdder`) |
| 2026-10 | Core swap math (`SwapMath`) |
| 2026-10 | Multiplier oracle (`ScheduledMultiplier`) |
| 2026-10 | Pricing of multiplier tokens on BNB Chain |
| 2026-10 | Frontend client bundle configuration |

### Smart contracts

| Reported | Issue | Status |
|---|---|---|
| 2026-10 | The stop-loss extension's `afterSwap` check reads every bin the swap crossed, so its gas grows with the number of bins crossed in one swap. | Known. No funds at risk |
| 2026-10 | Pool creation reverts above 80 bins per side on some chains. | Known. The app limits pool creation accordingly |
| 2026-10 | `MetricOmmSwapQuoter` single-hop exact-output quotes return a partial fill instead of reverting when the requested output exceeds pool depth. The router rejects the resulting swap. | Known. No funds at risk |
| 2026-10 | `MultiplierOracle` feed creation is permissionless and does not bind a token to its underlying asset. | By design. Each pool's curator chooses its price provider |
| 2026-10 | Pools may use custom price providers that are not published by Metric. | By design. Each pool's curator chooses its price provider |
| 2026-11 | Stop-loss extension (`OracleValueStopLossExtension`): watermarks are not seeded when liquidity is added. They initialise on the first swap that touches a live bin, so that swap is not drawdown-protected. | By design. Documented in the contract header |
| 2026-11 | Stop-loss extension: watermarks are not cleared when liquidity is removed. An emptied bin keeps its old watermark until a later swap crosses it, so liquidity re-added before that crossing can block swaps. After the crossing resets the watermark, the next swap on that bin is again unprotected. | By design. Documented in the contract header |
| 2026-11 | Stop-loss extension: only bins crossed by a swap are checked, a breach blocks only the affected direction, watermarks decay lazily per bin, and a drawdown setting of 0 disables the check. | By design. Documented in the contract header |
| 2026-11 | Oracle `register()` clears the blacklist flag of the pool being registered once the registration fee is paid. Overpayment is not refunded. | By design. Documented in the contract |
| 2026-11 | Updater grants on a price provider remain in place after the provider's ownership is transferred. | By design. The new owner can revoke them |
| 2026-11 | A multi-hop quote path may visit the same pool more than once. | By design |
| 2026-11 | The factory's cap on `bidAskRefAsymmetryX8` applies at pool creation only, and a value of `type(uint16).max` disables the check. | By design. Documented in the factory |

### Web, app and API

| Reported | Issue | Status |
|---|---|---|
| 2026-10 | On Ethereum, on-chain price provider reads can be stale or revert between trades, because pools receive their price update in the same block as each trade. | By design |
| 2026-10 | Chains not listed in `/public/v1/chains` have no tier or routing data in the public API. | By design. Those chains are not publicly supported yet |
| 2026-10 | The app does not set a gas limit; the wallet's estimate can occasionally run out of gas. | Known |
| 2026-10 | Email spoofing (SPF/DMARC configuration). | Out of scope |
