<!--
  Kubera — investor one-pager.
  Keep it to ~one page per language. [[ … ]] = fill with verified data before sending.
  Do NOT include unverifiable claims (audit, TVL, raise terms, backers) until confirmed.
-->

# Kubera — One-Pager

> **An open, provable Hyperliquid.** Open infrastructure for on-chain perpetuals — self-hostable, API-compatible, and verifiable.
> **一个开放、可证明的 Hyperliquid。** 链上永续合约的开放基础设施 —— 可自托管、API 兼容、可验证。

---

## Problem / 问题
**EN —** Perpetual futures are the largest market in crypto, and on-chain perps are rapidly taking share from centralized exchanges. But the category leader is a **closed, single-operator** stack: users and institutions must trust one black box for matching, risk, and solvency. There is no credibly-neutral, open, self-hostable equivalent.

**中文 —** 永续合约是加密最大的市场,链上永续正快速从中心化交易所抢占份额。但品类龙头是**封闭、单一运营方**的技术栈:用户与机构必须把撮合、风控、偿付能力都托付给一个黑箱。市场上缺少可信中立、开放、可自托管的同类产品。

## Solution / 解决方案
**EN —** Kubera is an open-source perpetuals chain that is **drop-in compatible with the Hyperliquid API and SDK**, **deterministic and state-provable**, and **self-hostable as a single binary**. Anyone — a fund, an exchange, a region — can run it, verify it, and extend it without trusting a closed operator.

**中文 —** Kubera 是一条开源永续合约链:**与 Hyperliquid API/SDK 无缝兼容**、**确定性且状态可证明**、**单二进制即可自托管**。任何基金、交易所或地区都能自行运行、验证并扩展,无需信任封闭运营方。

## Why now / 为什么是现在
- **EN —** Perps dominate crypto volume; on-chain share is growing fast. The proven demand exists; the open, neutral supply does not — yet.
- **EN —** Institutions increasingly require *provable* execution & solvency over black boxes.
- **EN —** API/SDK compatibility means the entire existing bot & market-maker ecosystem is addressable with ~zero switching cost.
- **中文 —** 永续主导加密交易量,链上份额快速增长;需求已被验证,但开放中立的供给还不存在。
- **中文 —** 机构越来越要求可证明的执行与偿付能力,而非黑箱。
- **中文 —** API/SDK 兼容意味着现有机器人与做市商生态可零迁移成本接入。

<!-- TODO: add 1–2 sourced market figures (on-chain perp daily volume / category TAM). -->

## Product & moat / 产品与壁垒
| | **EN** | **中文** |
|---|---|---|
| Compatibility | Same API + Rust SDK as the incumbent; verified by SDK-conformance test. | 与龙头相同 API+Rust SDK;一致性测试验证。 |
| Verifiability | Byte-reproducible state roots + Merkle state proofs (JMT); no floats in consensus. | 字节级可复现状态根 + Merkle 证明(JMT);共识无浮点。 |
| Solvency | Full liquidation waterfall: margin → insurance fund → ADL; per-block conservation invariant. | 完整清算瀑布:保证金→保险基金→ADL;逐块守恒不变量。 |
| Reliability | Crash-transparent recovery, loom-verified concurrency, fuzzing, hard CI gate. | 崩溃透明恢复、loom 验证并发、fuzz、硬 CI。 |
| Openness | Apache-2.0, self-hostable — credibly neutral by construction. | Apache-2.0、可自托管 —— 设计上即可信中立。 |

## Status / 现状
**EN —** **P1 (single-node chain) is built and self-verifying.** End-to-end trading, persistence, proofs, crash recovery, and Hyperliquid-SDK conformance all pass under a hard CI gate. Next: multi-node BFT consensus (designed), then markets/liquidity, then mainnet.

**中文 —** **P1(单节点链)已完成且自校验。** 端到端交易、持久化、证明、崩溃恢复、Hyperliquid SDK 一致性均在硬 CI 门禁下通过。下一步:多节点 BFT 共识(已设计)→市场/流动性→主网。

<!-- TODO: replace with verified traction (testnet live, design partners, pipeline). -->

## Business model / 商业模式
<!-- TODO: choose & state. Common options for open perps infra: -->
<!-- EN: protocol/sequencer fees · managed-node / enterprise hosting · liquidity-vault economics · token (if applicable). -->
<!-- 中文: 协议/排序器费用 · 托管节点/企业部署 · 流动性金库经济 · 代币(若适用)。 -->
[[ describe revenue model ]]

## Team / 团队
[[ founders & key hires — names, prior track record ]]
<!-- Investors weight team heavily; lead with relevant exchange / trading / infra experience. -->

## The ask / 融资
**EN —** Raising **[[ stage + amount ]]** to **[[ use of funds: e.g. ship multi-node consensus + testnet, grow core eng, security audit ]]**.
**中文 —** 本轮融资 **[[ 轮次 + 金额 ]]**,用于 **[[ 资金用途:如交付多节点共识+测试网、扩充核心工程、安全审计 ]]**。

## Contact / 联系
- 📧 [[ contact@kubera.xyz ]] · 🐦 [[ @kubera_xyz ]] · 🌐 [[ kubera.xyz ]] · 💻 https://github.com/kubera-io

<sub>This document contains forward-looking statements and placeholders pending verification. / 本文含前瞻性陈述及待核实占位内容。</sub>
