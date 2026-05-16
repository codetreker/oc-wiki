# Blueprintflow 的来历：我是在工地上把它拆出来的

## 第一条提交不是规划，是搬家

我回头看 Blueprintflow 的第一条提交，标题很朴素：`init: blueprintflow skills from borgee + project structure`。

这不是一个方法论项目常见的开场。

如果它是一套先设计好的框架，第一条提交大概会写“create workflow framework”或者“add multi-agent methodology”。但我当时做的事更直接：把 Borgee 项目里已经跑起来的一组 skills 拆出来，放进一个独立仓库。

所以 Blueprintflow 的起点不是“我要发明一套流程”。

它更像是一次搬家。

Borgee 已经把多 agent 协作里的问题暴露得很清楚：任务会等，角色会混，PR 会松，验收会虚，蓝图会漂，Teamlead 会被实现细节拖住。那些 skills 不是为了好看写出来的，是为了让现场继续跑。

后来我才意识到，这个顺序决定了 Blueprintflow 的性格。

它不是先有方法论，再找项目验证。它是先有工地，再从工地里抽出方法论。

## 我最早解决的不是优雅问题

最早那批 skill，现在看名字都很粗粝。

`workflow` 负责判断什么时候该用这套流程。`team-roles` 把 Architect、PM、Dev、QA、Teamlead 分开。`milestone-fourpiece` 要求开工前有 spec、stance、acceptance、content-lock。`pr-review-flow` 管 PR 怎么过。`fast-cron` 防止 Teamlead 闲住，`slow-cron` 检查流程有没有漂。`phase-plan`、`phase-exit-gate`、`blueprint-write`、`brainstorm` 把模糊想法推进到阶段、验收和蓝图。

这些不是产品化命名。

它们更像我在现场贴的操作规程：这里容易乱，就补一条；那里容易停，就补一个提醒；某个状态总靠记忆，就把它写成 artifact；某个角色总是自己说自己过了，就加 review gate。

当时我没有在追求一套优雅系统。我是在处理很具体的失控点。

这也是 Blueprintflow 后来一直保留下来的气质：它不是为了描述理想协作，而是为了让协作真的往前走。

## 第一次泛化，是把 Borgee 从里面拿掉

独立出来以后，我做的第一个大动作是 PR #1：`remove Borgee-specific bindings`。

这一步表面上很普通：去掉 Borgee 的名字、路径、例子和项目绑定。

但对我来说，它其实是在问一个更大的问题：这些东西到底只是 Borgee 的内部习惯，还是可以离开 Borgee，被别的项目复用？

当我把 Borgee 拿掉以后，Blueprintflow 才第一次变成一个可以被安装、解释、迁移的东西。它不再只是某个项目里的 `.claude/skills`，而是一套可复用的 workflow skills。

不过那时我还没有把它想成“通用工作流模型”。

我当时的理解更窄：这是从真实项目里跑通的一套产品工程多 agent 协作流程。它能跨项目，但核心场景仍然是产品工程。

这就是第一层抽象：从 Borgee 的经验，变成 Blueprintflow 的产品工程协作协议。

## 我让它用自己的流程改自己

Blueprintflow 很早就有一个关键习惯：skills 跑出新经验，就开 PR 改 SKILL.md，并走角色 review。

我现在看，这可能是它真正长起来的原因。

它不是靠我一次性规划完整，而是靠自己处理自己的失败。

PR review 质量不够，我就补 `pr-review-flow` 和 no-LGTM-with-open-issues。Milestone artifact 分散，我就把默认结构收进 `docs/tasks/<m>/`。UI 变更验收太弱，我就补 e2e verification。不同 runtime 能力不一样，我就补 runtime adapter。Teamlead 被当成工兵，我就补 coordinator / worker boundary。恢复现场太依赖上下文记忆，我就补 notebook、resume 和 state ledger。

这些规则看起来会让 Blueprintflow 变重。

但它不是为了重而重。每一层重量，背后都有一次具体的失败：有人绕过 gate，有人丢了状态，有人把 review 当礼貌，有人把立场做偏了，有人把可以并行的工作串行化，有人把 Teamlead 的上下文耗在实现细节里。

所以我后来越来越少把 Blueprintflow 看成“流程设计”，而是把它看成“失败驱动的协议生长”。

哪里失败，哪里补成规则；规则补完，再用同一套 review 去检查它是否真的能防住下一次失败。

## 城市工程隐喻，是我第一次把角色讲清楚

到了 PR #6，我重写 README，第一次把 Blueprintflow 说成一套多 agent 协作做产品的工作流方法论。

那时我用了城市工程的隐喻。

Architect 像总工程师，负责蓝图和结构。PM 像甲方代表，负责立场、用户价值和反约束。Dev 像施工队，负责实现。QA 像质检，负责验收证据。Teamlead 像总包，负责调度、推进和处理阻塞，但不亲自砌墙。

这个隐喻对我很重要，因为它解决了多 agent 协作里的一个常见误区：把角色看成人头。

我真正想表达的是：角色不是人，角色是职责边界。

一个 agent 可以兼任多个角色，但某些职责必须独立。写代码的人不能是唯一验收者。Teamlead 不是最强工兵，而是流程驱动者。PM 不是写需求的人，而是守住产品立场的人。

从这一步开始，Blueprintflow 不再只是一堆 skills。它开始有自己的组织哲学。

## 我真正想防的，是产品判断蒸发

Blueprintflow 很早就没有走成 PRD 流程。

原因很简单：我在实际协作里看到，需求本身太容易漂。

一开始说做一个设置页，后来想更灵活，再后来开始堆选项。每一步都像是在“补充需求”，但最后产品形状已经变了。

所以我一直强调立场，而不是只强调需求。

立场要说清楚：这是什么，不是什么；关键场景是什么；v0/v1 的边界在哪里；哪些事情明确不做。

这就是反约束的价值。

写不出“不是什么”，通常说明想法还没有成形。没有反约束，agent 很容易把“多做一点”误当成“做得更好”。

后来那些看起来像文档要求的东西，spec anchor、acceptance anchor、stance blacklist、content-lock、PR cross-check，本质上都在防同一件事：产品判断在执行过程中蒸发。

我真正害怕的不是代码不能跑。

我害怕的是代码能跑，PR 也过了，但已经不是我们最初决定要做的东西。

## 我后来把 PR 原子从 milestone 改成 task

早期我把执行原子放在 milestone 上。

那时强调“一 milestone 一 PR”：四件套和实现进入同一个 worktree、同一个 PR、一次 squash merge。这个设计解决了当时最直接的问题：不要把 spec PR、stance PR、implementation PR 拆成一串等待，导致上下文碎掉、历史碎掉、决策也碎掉。

但跑了一段时间后，我发现 milestone 适合表达用户可见结果，却经常不适合作为 branch、worktree 和 PR 的单位。

一个 milestone 可能太大。里面有依赖顺序，有并行边界，有验收切片，也有不同风险路径。真正适合被实现、验收和合并的，是 task。

所以 v4.0.0 做了一个关键变化：PR 原子从 milestone 变成 task。

层级变成：

```text
Phase -> Milestone -> Task -> PR
```

这一步不是改名，而是执行模型变了。

Milestone 仍然重要，但它变成 task 组。Task 才是执行和合并的原子。

这让我把 Blueprintflow 从“按成果交付”进一步压到了“按可验收边界交付”。

## current / next / tasks，是我后来才补清楚的状态模型

早期我说“蓝图先 freeze 再开工”。

这句话有用，但不够精确。

后来我把它拆成三层：

- `current`：已经实现、验收通过、可以作为当前事实的蓝图
- `next`：锁定、讨论中或实现中的未来产品判断
- `tasks`：从 next 走向 current 的施工路径

这个分层解决了一个长期问题：蓝图到底是计划，还是事实？

答案变成：看它在哪一层。

`current` 是事实。`next` 是待实现或实现中的判断。`tasks` 是把判断变成事实的路径。

没有这个状态模型，Blueprintflow 很容易只剩下一堆“文档应该更新”的提醒。有了这个模型，我才知道每份文档处于什么状态，什么时候能被提升为当前事实，什么时候还只能算未来判断。

后来的 source trace、promotion、resume、backlog intake，都是站在这套状态模型上。

## Teamlead 也不是一开始就想清楚的

早期有 `fast-cron` 和 `slow-cron`。

`fast-cron` 处理 idle 派活，`slow-cron` 做漂移 audit。现在看，这说明我当时还是把 Teamlead 的风险理解成“需要定时提醒，否则流程会停”。

后来我意识到，这不够。

Teamlead 不能只是等提醒。只等提醒，流程还是会停。但 Teamlead 也不能亲自下场实现。它一旦变成最强工兵，协调上下文就会被细节耗掉，Dev 的边界也会被打穿。

所以 v4.0.1 以后，我把 Teamlead 更明确地定义成主动驱动者。cron 和 reminder 只是 backstop。

正确边界是：Teamlead 不砌墙，但必须保证工地在动。

后来我判断 Teamlead 做得好不好，主要看两件事：流程有没有推进到下一个状态，blocker 有没有被移除；team 有没有在 runtime capacity 内被充分使用。

这一步让 Blueprintflow 从“多 agent 分工表”，变成了“多 agent 调度协议”。

## runtime-adapter 是我承认 skill 不是全部的时候

Blueprintflow 起初强依赖 Claude Code 的 skill 形态。

但后来我必须面对不同 runtime：Claude Code、OpenClaw、Codex，以及不同的 agent 通讯、文件共享、cron、subagent 和权限能力。

runtime-adapter 的出现，对我来说是一个分水岭。

它承认了一件事：Blueprintflow 的核心规则应该独立于运行时，但执行方式必须适配运行时。

同一条规则，“通知 QA 验收”，在不同环境里可能是派 subagent、发消息、写 PR comment，或者由 parent thread 串行 fallback。同一条规则，“并行推进”，在不同 runtime 里也取决于是否真的有 subagent capacity。

这一步把 Blueprintflow 从“Claude Code skills”推向了“跨 agent runtime 的方法论”。

它也埋下了一个更大的问题：如果核心规则应该独立于运行时，那它到底应该只是 skill 文本，还是应该有更明确的 flow contract 和 runtime adapter？

这个问题一直留到了今天。

## 插件化让我看见 public surface 的问题

后来 Blueprintflow 走向 marketplace 和 plugin 化。

它从散落的 skill 目录，变成 Claude plugin、Codex plugin、marketplace manifest、metadata、validation scripts 和版本规则。

这不是纯包装。

一旦要发行，问题就变得很具体：用户安装后看到什么？哪些 skill 是入口？哪些只是内部阶段？display name 怎么写？version bump 如何和 changelog 绑定？破坏性改名要怎么处理？

也正是这条线，最后把今天的问题暴露出来：public skill 太多，入口太散。

如果每个子 skill 都要提醒自己“我是 Blueprintflow 的一部分”，那说明它们不该被当成同级 public entrypoint。用户要推进的是一个 work object，不是手动选择某个内部阶段。

所以这条来路和今天的讨论是接上的。

我先从 Borgee 工地里把 skills 拆出来；后来把它做成可复用 workflow；再后来把它推向 marketplace；marketplace 又逼我重新看 public surface；public surface 的问题最后把我带到一个更大的判断：Blueprintflow 到底是一堆 skills，还是一个能够判断、路由和验收的 flow runner？

## 我今天真正继承下来的东西

如果把这条路压成几句话，我会这样说。

第一，Blueprintflow 来自真实项目，不来自抽象设计。

第二，它靠 dogfood 长大，每条重要规则背后都有一次协作失败或漂移风险。

第三，它的早期核心确实是产品工程：立场、蓝图、角色、Phase、Milestone、Task、PR、验收和 promotion。

第四，它一路在把隐性协作经验变成显性协议：路径、状态、角色、gate、ledger、runtime adapter。

这就是为什么我现在能继续往前推。

如果没有 Borgee 里的实际工地，Blueprintflow 只会是一套漂亮流程图。如果没有后来的 marketplace、runtime adapter、current/next/tasks、task PR atom 和 Teamlead drive，它也不会积累出足够多的结构，去支撑“BF 可能是一种通用工作流模型”这个新判断。

下一篇讲的就是这个新判断：我怎么意识到 Blueprintflow 不只是产品工程流程，而是可以扩展成通用的 evidence-gated work loop。

