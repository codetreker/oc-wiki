# Blueprintflow：我们是怎么发现它不只是产品工程流程的

## 那句重复出现的 guard

最早让人不舒服的，不是某个大理论。

是 Blueprintflow 的每个子 skill 里都有类似一句话：如果入口没有激活，就先回到入口。

这句话本来是保护机制。它防止 agent 直接跳进某个中间阶段，绕过 Teamlead、runtime、role boundary 和状态检查。但当二十多个 skill 都需要写同一类 guard 时，问题就不只是“文档有点啰嗦”了。

重复的 guard 在提醒我们：这些子 skill 不该是 public 入口。

用户并不关心自己该调用 `bf-phase-plan`、`bf-task-execute`，还是 `bf-milestone-progress`。用户关心的是：这个 issue 该往哪走？这个 idea 怎么变成可执行对象？这个 task 离 accepted 还差什么？

也就是说，用户要推进的是一个对象，不是一个 skill。

这是第一个转变：

> Blueprintflow 的入口不应该是一排 skill，而应该是一个能判断下一步的 flow runner。

这个判断还很保守。它只是在说：把 public surface 收干净，把内部阶段藏起来。但它已经把我们带离了原来的问题。原来的问题是“怎么让每个 skill 更清楚”；新的问题是“谁来判断当前对象该进入哪个流程”。

## OPC 把问题照亮了

讨论 OPC 时，真正有价值的不是“OPC 也有很多 agent”。

OPC 有一个更硬的东西：它把流程变成了可执行结构。它有 flow graph、node、gate、loop、state、review independence 和 mechanical verdict。

这让 Blueprintflow 的弱点变得清楚。

Blueprintflow 现在很多规则靠文字和 Teamlead 纪律维持：读哪些文档、派哪些角色、什么时候验收、什么时候推进。这个模型可以跑，但它对上下文和自律要求很高。一旦状态不清、证据松动、角色边界混掉，流程就会漂移。

OPC 让我们换了一个问法。

```text
不是：这个阶段的说明够不够详细？
而是：这个阶段的输入、产出、gate 和状态迁移是什么？
```

这一步很关键。它把 Blueprintflow 从“说明书”推向“状态机”。

流程不是一串建议。流程应该回答：当前对象在哪里，下一步要产出什么，谁来验证，gate 如何判定，通过后状态写到哪里。

没有这些东西，流程说明再长，也只是在要求 agent 自觉。

## 我们随后发现，两套东西有同一个骨架

OPC 的典型循环很简单：

```text
task
-> select flow
-> build / review / test
-> gate
-> iterate or finish
```

Blueprintflow 的产品工程循环看起来更复杂：

```text
idea / issue
-> blueprint / phase / milestone / task
-> implementation / verification / PR review
-> acceptance
-> current promotion
```

但把名字拿掉，骨架几乎一样。

都是先处理输入，把它变成可执行对象；再产出 artifacts；再让独立角色验证；最后由 gate 决定状态是否推进。失败或不充分就回去迭代，通过就进入下一个状态。

这时，Blueprintflow 的产品工程词汇开始退到第二层。

Phase、Milestone、Task、PR 仍然重要，但它们不再是 BF 的本体。它们只是产品工程这个领域里的对象类型。

BF 真正的本体更像这样：

```text
work object
-> artifacts
-> independent verification
-> gate
-> ledger transition
```

这个转变比“把多个 skill 合成一个入口”大得多。它意味着 BF 不是某套产品开发步骤，而是一种处理不确定工作的方式。

## 真正的转折：每个 flow 都是一个小 BF

接下来发生了最重要的一步。

我们发现 Blueprintflow 不是一条从 idea 到 current 的长流程。它由很多形状相同的小流程组成。

Issue triage 是一个小 BF。输入是 issue，产出是分类、路由和 trace。

Brainstorm 是一个小 BF。输入是模糊 idea，产出是边界、立场和结构化对象。

Milestone breakdown 是一个小 BF。输入是 selected milestone，产出是 task boundary 和 reviewed `task.md`。

Task execution 是一个小 BF。输入是 reviewed `task.md`，产出是 four-piece、design、code、tests、PR 和 acceptance evidence。

Phase exit 也是一个小 BF。输入是 accepted milestones，产出是 closure evidence、signoff 和 promotion readiness。

这些流程做的事不同，但形状相同：

```text
input
-> clarify or decompose
-> produce artifacts
-> verify independently
-> gate
-> update ledger
-> route the next object
```

这改变了我们对 Blueprintflow 的理解。

它不是一条主线，而是一种递归模式。一个大对象可以拆成小对象；一个流程的输出可以成为另一个流程的输入。BF 可以向内拆，也可以向外扩。

## 产品工程只是第一个 pack

一旦看到这个递归模式，产品工程就不再是边界。

研究也可以这样跑：问题进入，形成假设，设计实验，收集证据，得出结论。

事故处理也可以这样跑：alert 进入，诊断影响，制定止血动作，验证恢复，写 postmortem。

内容生产也可以这样跑：idea 进入，形成 brief，写 draft，review，publish。

决策也可以这样跑：problem 进入，列 options，做 tradeoff review，写 decision record。

这不是说“万物皆流程”。BF 有边界。只有能被表示成对象、状态、产出物、验收标准、gate 和 ledger 的工作，才适合进入 BF。

但这个边界明显不止产品工程。

所以更准确的说法是：

```text
BF Core = 通用的 evidence-gated work loop
Product Engineering = BF 的第一个 pack
```

原来的 Blueprintflow 没有被推翻。它被降级了：从“整个方法论本身”，变成“这个通用模型在产品工程里的一个实例”。

## Raw Input 不能直接执行

这个新理解带出一个基础区分：BF 的输入有两类。

第一类是 Raw Input。它是模糊意图，还不能执行。

```text
我们要优化 onboarding
研究一下 agent workflow
这个产品方向感觉不对
用户反馈支付流程很乱
```

这类输入不能直接进入执行循环。它必须先经过 shaping。

在产品工程里，这个 shaping 叫 brainstorm、triage 或 blueprint write。在研究里，它可能是定义研究问题和假设。在事故处理中，它可能是确认影响范围和止血目标。

名字可以不同，职责一样：把 Raw Input 塑造成 Work Object。

第二类是 Work Object。它已经有边界，可以被推进。

一个 Work Object 至少要回答：

- 要推进什么？
- 为什么现在要做？
- 做什么，不做什么？
- 要推进到哪个状态？
- 怎么判断成功？
- 需要留下什么证据？
- 哪些限制不能破？
- 通过后进入哪个流程？

所以 BF 的入口不是“收到需求就执行”。BF 的入口应该先判断：这是一段 Raw Input，还是一个已经成形的 Work Object？

```text
Raw Input
-> domain shaping
-> Work Object
-> execution / verification / transition flow
```

这是整个模型里最容易被低估的一点。很多失败的执行，不是实现能力不够，而是 Raw Input 被当成 Work Object 直接执行了。

## 新定义

经过这轮讨论，Blueprintflow 可以重新定义为：

> BF 用产出物、独立验证、闸门和状态账本，把不确定的工作对象推进为可验收的状态迁移。

更短一点：

> BF 是通用的 evidence-gated work loop。

在这个定义下，BF Core 关心的是通用 contract：

```text
Work Object
Artifact
Acceptance Criteria
Gate
Ledger
Route
```

Product Engineering Pack 才关心具体领域词汇：

```text
issue
idea
blueprint
phase
milestone
task
PR
current promotion
```

未来可以有 Research Pack、Incident Pack、Content Pack、Decision Pack 和 Operations Pack。它们不需要共享同一套领域词汇，但必须共享同一个 BF Core contract。

## 这次真正改变了什么

这次讨论真正改变的不是目录结构，也不是工具选择。

它改变的是我们问问题的方式。

以前我们会问：还缺哪个 skill？哪个阶段的说明不够？入口怎么命名？

现在应该问：

```text
这个 Work Object 是什么？
它现在处于什么状态？
需要产出什么 artifact？
谁来独立验证？
gate 怎么判断？
状态写到哪里？
通过后路由到哪个对象或流程？
```

这组问题才是 BF 的核心。

产品工程只是第一个场景。真正被发现的东西，是一套把不确定工作推进为可验收事实的通用模式。

