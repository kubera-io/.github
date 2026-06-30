<!--
  Kubera —— 组织介绍页(中文版)。
  与英文版 profile/README.md 对应,放在同一目录,随 profile/ 一起推送到 kubera-io/.github。
  [[ … ]] 为待填占位;未经核实的内容(审计、TVL、融资条款)请勿对外发布。
-->

<p align="center">
  <img src="./assets/kubera-logo.svg" alt="Kubera" width="140"/>
</p>

<h1 align="center">Kubera</h1>

<p align="center">
  <b>开放、可验证的永续合约交易所技术栈。</b><br/>
  <i>开源、可自托管的永续 DEX —— 兼容 Hyperliquid、确定性执行、状态可证明。</i>
</p>

<p align="center">
  <a href="./README.md">🌐 English / 英文版</a>
</p>

<p align="center">
  <img alt="Built with Rust" src="https://img.shields.io/badge/built%20with-Rust-000000?logo=rust"/>
  <img alt="Status" src="https://img.shields.io/badge/status-active%20development-brightgreen"/>
  <img alt="API" src="https://img.shields.io/badge/API-Hyperliquid--compatible-5b8def"/>
  <img alt="Contracts" src="https://img.shields.io/badge/contracts-Foundry%20%2B%20Solidity-363636"/>
  <img alt="License" src="https://img.shields.io/badge/license-Apache--2.0-blue"/>
</p>

---

## 🪙 Kubera 是什么

Kubera 致力于打造链上永续合约的开放基础设施 —— 永续合约是加密领域交易量最大的品类。Hyperliquid 证明了"专为永续打造的高性能交易所"能够赢得巨大交易量;Kubera 把这套架构**开放化**:一条**可自托管、可审计、API 兼容**的永续合约链,让团队和机构能够自行运行、验证并扩展 —— 无需信任单一封闭运营方。

> **一句话:** *一个开放、可证明的 Hyperliquid —— 你能自己运行、自己审计的永续基础设施。*

---

## 📈 为什么是现在

- **永续是加密最大的市场。** 永续合约占据加密交易量的主导地位,链上永续正以加速度从中心化场所抢占份额。
- **品类龙头是封闭的。** 当前领先的链上永续场所是封闭、单一运营方的技术栈。市场上缺少可信中立、开放、可自托管的同类产品 —— 这正是 Kubera 要填补的空白。
- **机构需要可验证性。** 基金、做市商与合规场所越来越要求**可证明**的执行与偿付能力,而非黑箱。Kubera 从设计上即是确定性、状态可证明的。
- **兼容性降低迁移壁垒。** Kubera 使用与现有龙头相同的 API 与 SDK,现有量化机器人、做市商与工具**开箱即用** —— 迁移成本近乎为零。

<!-- TODO: 补充带来源的市场数字(链上永续日交易量、品类 TAM),不要用无出处数字。 -->

---

## ⚡ 差异化

| | |
|---|---|
| 🔌 **兼容 Hyperliquid** | 与 Hyperliquid API 和 Rust SDK 直接兼容 —— 由一致性测试验证:**真实**上游 SDK 签出的下单/转账,节点照单全收。现有工具无缝接入。 |
| 🔍 **可验证 · 确定性** | 字节级可复现的状态根、Merkle 状态证明(JMT)、定点数运算且**共识层无浮点**。任何人都能重放历史、验证每个区块。 |
| 🛡️ **机构级风控引擎** | 完整清算瀑布 —— 维持保证金 → 保险基金 → 自动减仓(ADL)。没有任何正经永续场所会无偿赦免坏账,Kubera 也不会。偿付能力被强制执行且可证明。 |
| 🔒 **资金不会泄漏** | 逐块守恒不变量证明 USDC 不被凭空创造或销毁(手续费→保险基金、坏账正确社会化)。**正是这套检查**发现并修复了两个真实记账 bug。 |
| 🧱 **为"不出故障"而设计** | 崩溃透明恢复(恢复后的节点与从未崩溃的节点逐字节一致)、用 **loom** 证明并发无死锁、属性化 fuzz、黄金确定性测试,以及硬性 CI 门禁(`fmt` + `clippy -D warnings` + 全量测试 + loom)。 |
| 🌉 **桥接路径已成型** | EVM 侧桥合约与 Rust watcher / signer / relayer 同步实现,并用跨语言 EIP-712 与 calldata 向量锁定 Solidity ↔ Rust 的字节级兼容。 |
| 🪶 **可自托管** | 单个 Rust 二进制:撮合、永续、风控、存储、HTTP/WS API 一体。可本地、私有云或托管运行。 |

---

## 🏗️ 架构

Kubera 是一台运行在版本化 Merkle 存储之上的确定性状态机。所有写入经由单一 commit 路径,产出可复现的状态根;撮合引擎、永续/风控引擎、预言机都是这台状态机之上的模块。

```mermaid
flowchart TB
    EVM["EVM 链<br/>(Bridge2 合约)"] -->|"deposits · lifecycle certs"| PROD
    SDK["交易者 / 机器人 / 做市商<br/>(兼容 Hyperliquid 的 SDK)"] -->|"POST /exchange · /info · WS"| RPC["RPC / WebSocket API"]
    RPC --> POOL["交易池<br/>(EIP-712 验签)"]
    BFT["HotStuff BFT 栈<br/>(quorum-store · libp2p · MDBX)"] -.-> PROD
    POOL --> PROD["出块器<br/>(确定性 · 不产空块)"]
    PROD --> SM["状态机"]
    subgraph SM_MODULES["执行模块"]
      MATCH["撮合引擎<br/>(per-market 锁)"]
      PERPS["永续 + 风控引擎<br/>(保证金 · 资金费 · 清算瀑布 · ADL)"]
      ORACLE["预言机定价"]
      BRIDGE["桥接会计<br/>(存款 · 提现 · 生命周期)"]
    end
    SM --> SM_MODULES
    SM -->|"StateTxn 写集"| STORE["存储 (MDBX)"]
    STORE --> JMT["JMT 状态树<br/>(版本化 · 证明 · 在线剪枝)"]
    JMT -->|"字节级可复现"| ROOT["state_root + block_hash"]
    SM -->|"withdrawal records"| RELAY["提现签名器 / 中继器"] -->|"EIP-712 quorum txs"| EVM
    PROD -.指标.-> OBS["Prometheus /metrics"]
```

<sub>chain 模块 —— codec · primitives · crypto · jmt · storage · execution · transaction-pool · rpc · node · consensus · sync。兄弟工程提供可复用 perp engine、HotStuff 栈与 EVM 合约。</sub>

---

## 🗺️ 路线图

| 阶段 | 范围 | 状态 |
|---|---|---|
| **P1 —— 单节点链** | 可自托管的永续 DEX:HL 兼容 API、确定性执行、持久化 JMT 状态、清算瀑布+保险基金+ADL、崩溃恢复、可观测性、硬 CI。 | ✅ **已完成** |
| **P2 —— 多节点共识** | 可复用 HotStuff 系共识栈、quorum-store、libp2p 传输、MDBX 持久化、chain validator mode、state sync、重启/重入路径。 | 🛠️ **分层已实现 · hardening 中** |
| **P3 —— 市场与流动性** | 现货市场、逐仓/全仓保证金、TWAP+触发订单、子账户(vault 地址路由)、成交量费率档+推荐返佣 —— **已交付**;HLP 式流动性金库为下一步。 | 🛠️ **市场已交付 · 流动性下一步** |
| **P4 —— 桥与主网** | EVM 侧桥合约、deposit/lifecycle watcher、提现签名+中继、验证者集合同步、预言机委员会、测试网→主网、治理。 | 🛠️ **桥已实现 · 生产化待加固** |

<!-- TODO: 确定后补上时间节点/季度;投资人关注时间线。 -->

---

## ✅ 现状

P1 已完成且自校验:单节点链端到端跑通(提交→交易池→出块→执行→持久化→查询),并有硬性 CI 门禁(rustfmt、clippy `-D warnings`、全量测试、loom 并发模型)。测试覆盖单元、集成、黄金确定性、属性化 fuzz、崩溃恢复,以及 Hyperliquid SDK 一致性。

在其之上,市场层现已建成:现货交易、逐仓与全仓保证金、TWAP 与触发订单、带 vault 地址路由的子账户,以及按成交量分档的手续费与推荐返佣 —— 每项都在逐块守恒不变量与 CI 门禁不破的前提下落地。配套发布参考前端(`interface`):其浏览器内 EIP-712 签名与节点逐字节一致(覆盖全部动作类型),并有一套面向运行中链的实节点端到端测试驱动每个新模块。

可复用引擎边界现在已经清晰。`perp-engine` 是纯 Rust 业务库,拆分为确定性类型、定点数学、撮合、风控、结算、核心执行与仿真 crate;它不依赖 chain、数据库、网络、系统时间、随机数或浮点数,`chain` 通过 adapter 边界消费它。

共识与桥不再只是纸面设计。`hotstuff` 是独立 Rust workspace,包含共识核心、quorum-store/数据可用性、transport-agnostic 网络层、libp2p 适配器、节点 runtime、MDBX 持久化和可运行的 4 节点参考应用。`chain` 已有 validator-mode 接线、state sync、bridge deposit/lifecycle system inputs、withdrawal records、签名/中继路径与验证者集合同步;但多验证者路径仍需要真实多机 soak、startup catch-up/config-digest 加固和运维 runbook,在这些完成前不应称为生产 ready。

EVM 侧桥包(`contracts`)已用 Foundry 实现:`Bridge2.sol` 锁定 EIP-2612 资产,并在严格超过 2/3 的 L1 验证者授权下释放资产,支持两阶段提现、hot/cold 验证者集合、deposit-only pause、重放保护、finalizer 与 cold-quorum 恢复。Solidity 与 Rust 共享桥 EIP-712 domain、签名操作、calldata、validator-set hash 和 deposit event 的测试向量。

<!-- TODO: 替换为已核实的进展(测试网上线、合作方、设计伙伴、LOI、审计状态)。未审计前请勿声称"已审计"。 -->

---

## 💼 致投资人

Kubera 的定位,是当前领先封闭永续场所的"开放、可信中立"对位者 —— 承接同样的需求(加密最大的市场),却采用龙头无法采用的模式:**开放、可自托管、可验证**,并以**无缝兼容**让现有全部机器人与做市商生态从第一天起即可触达。

---

## 📦 代码仓库

- **`chain`** —— 单节点永续链(P1)+ 市场层(P3 现货/保证金/子账户/费率)。
- **`perp-engine`** —— 确定性永续业务库:types / math / matching / risk / settlement / core / simulation。
- **`hotstuff`** —— 可复用 HotStuff 系 BFT 栈:共识核心、quorum-store、libp2p、节点 runtime、MDBX 持久化、本地 4 节点 demo。
- **`contracts`** —— Foundry 桥合约包:`Bridge2`、EIP-712 签名、Multicall3、本地脚本、Solidity ↔ Rust 向量。
- **`interface`** —— 参考 Web 前端(Preact + viem):浏览器内签名逐字节一致 + 实节点端到端。

---

## 📬 联系我们

我们开放共建。商务合作、做市或投资请联系:

- 📧 **[[ contact@kubera.xyz ]]**
- 🐦 **[[ @kubera_xyz ]]**
- 🌐 **[[ kubera.xyz ]]**

<p align="center"><sub>© Kubera · 共建开放永续基础设施 · <a href="./README.md">English</a></sub></p>
