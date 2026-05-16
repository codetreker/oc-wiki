# Blueprintflow：从产品工程方法论到通用工作流模型

## 这次认知变化

一开始我们把 Blueprintflow 理解成一套产品工程治理方法论：从模糊 idea 到 blueprint，再到 Phase、Milestone、Task、PR、验收和 current promotion。

这个理解没有错，但它太窄了。今天讨论里更重要的发现是：Blueprintflow 背后的模式并不只属于产品工程。它本质上是一种通用的、递归的、证据驱动的工作推进模型。

可以把它概括成：

```text
输入一个模糊或未完成的工作对象
-> 明确边界
-> 拆成可执行步骤
-> 产出 artifacts
-> 独立验证
-> gate 决定通过、迭代或失败
-> 写回 ledger
-> 路由到下一个状态或下一个对象
```

这意味着 Blueprintflow 不应该只被定义为“软件产品开发流程”。它可以更抽象地定义为：

> BF 用产出物、独立验证、闸门和状态账本，把不确定的工作对象推进为可验收的状态迁移。

## 两类输入

BF 的入口有两种输入。

第一种是 **Raw Input**：模糊意图，还不能直接执行。

例如：

```text
我们要优化 onboarding
研究一下 agent workflow
这个产品方向感觉不对
用户反馈支付流程很乱
```

Raw Input 需要先经过 brainstorming。这里的 brainstorming 不是随便聊天，而是 shaping flow：它把模糊输入塑形成结构化的 Work Object。

第二种是 **Work Object**：已经结构化，可以被 BF flow 驱动。

一个 Work Object 至少应该说明：

- objective：要推进什么
- context：为什么现在要做
- boundary：做什么、不做什么
- desired state：希望推进到哪里
- acceptance criteria：怎么判断成功
- expected artifacts：应该留下什么产出物或证据
- constraints / risks：关键限制和红线
- next flow：接下来进入哪个流程

所以入口模型是：

```text
Raw Input
-> domain brainstorming
-> Work Object
-> execution / verification / transition flow
```

## 不同领域有不同的 brainstorming

Brainstorming 不是一个固定流程，而是一类 domain-specific shaping flow。

BF Core 只规定：Raw Input 必须被塑形成满足 Work Object contract 的结构化对象。但不同业务领域会有不同的问题、角色和产出物。

例如：

| 领域 | Brainstorming 关注点 | 可能产出 |
|---|---|---|
| Engineering | 用户价值、技术边界、模块影响、v0/v1、验收方式、PR 粒度 | blueprint anchor、milestone、task.md |
| Research | 研究问题、假设、变量、数据来源、评价方法、反证条件 | research work object、experiment plan |
| Incident | 影响范围、症状、时间线、假设、止血目标、验证信号 | incident work object、mitigation plan |
| Content | 受众、主张、渠道、语气、结构、成功指标 | content brief |
| Decision | 问题、选项、取舍、不可接受结果、决策证据 | decision record |

因此，Blueprintflow 的产品工程流程只是一个 pack，不是 BF 的全部。

## 每个 flow 都是一个小 BF

另一个关键发现是：BF 不是一条超长线性流程，而是很多同构 flow 的组合。

每个 flow 都遵循类似结构：

```text
input
-> decide / decompose
-> produce artifacts
-> review / verify
-> update ledger
-> route next flow
```

产品工程里的几个流程都可以看成小 BF：

| Flow | Input | Output / State |
|---|---|---|
| Issue triage | GitHub issue | 分类、路由、trace |
| Brainstorm | fuzzy idea / conflict | stance、boundary、Work Object |
| Iteration planning | selected issues / next anchors | Phase、Milestone |
| Milestone breakdown | selected milestone | task boundary、task.md |
| Task execution | reviewed task.md | PR、evidence、accepted task |
| Phase exit | accepted milestones | closure evidence、promotion readiness |
| Current promotion | accepted scope | current truth |

这说明 BF 的本体不是某条固定流程，而是一个可递归复用的 flow pattern。

## BF Core 与 Packs

更清晰的分层应该是：

```text
BF Core
  通用 work object / artifact / criteria / gate / ledger / route 模型

BF Packs
  某个领域对 BF Core 的实例化
```

当前 Blueprintflow 可以被视为第一个 pack：

```text
product-engineering pack
  issue / idea / blueprint / phase / milestone / task / PR / current promotion
```

未来还可以有：

```text
research pack
incident pack
content pack
decision pack
operations pack
```

这样，BF 既保留了 Blueprintflow 在产品工程里的经验，又不再被产品工程语境限制。

## 核心公理草案

这次讨论沉淀出的 BF 公理可以先记为：

1. BF 不直接执行 Raw Input；Raw Input 必须先被塑形成 Work Object。
2. Work Object 是 BF 推进的基本单位。
3. 每次状态推进必须有 artifact 或 evidence。
4. Producer 不能独自验收自己的产出。
5. Gate 决定 route：PASS、ITERATE、FAIL 或升级人工决策。
6. 状态必须写入 ledger，便于恢复、审计和交接。
7. 一个 flow 完成后，结果可以成为下一层 flow 的输入。

## 为什么重要

这个认知变化很重要，因为它把 Blueprintflow 从“产品工程流程集合”提升成了“通用工作状态迁移模型”。

这会影响后续所有设计：入口不只是选择某个 skill，而是运行某个 flow；产品工程不是唯一场景，而是一个 pack；brainstorming 也不是闲聊，而是 Raw Input 到 Work Object 的 shaping flow。

后续具体怎么实现、是否复用某个现有 runtime、目录怎么改，都应该建立在这个概念转变之上。

