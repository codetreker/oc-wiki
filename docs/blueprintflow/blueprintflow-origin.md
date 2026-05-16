# Blueprintflow 的来历：先有工地，后有方法论

## 第一条提交已经把答案写出来了

Blueprintflow 不是先被设计成一套方法论的。

它的第一条提交叫：`init: blueprintflow skills from borgee + project structure`。

这句话很重要。它没有说“创建一个通用工作流框架”，也没有说“设计一个多 agent 协作方法论”。它说的是：从 Borgee 项目里，把已经跑起来的一组 skills 拆出来。

顺序是反的。

不是先有方法论，再找项目验证。是先有一个真实项目，把多 agent 协作里的混乱、等待、漂移、返工都暴露出来，然后才把能工作的部分抽出来，变成 Blueprintflow。

所以 Blueprintflow 的起点不是书房，是工地。

## 最早的问题不是优雅，是活下来

早期 Blueprintflow 的 skill 名字很能说明问题。

`workflow` 先判断什么时候该用这套流程。`team-roles` 把 Architect、PM、Dev、QA、Teamlead 分出来。`milestone-fourpiece` 要求 milestone 开工前必须有 spec、stance、acceptance、content-lock。`pr-review-flow` 管 PR 怎么过审。`fast-cron` 和 `slow-cron` 一个防止 Teamlead 闲住，一个检查流程有没有漂移。`phase-plan`、`phase-exit-gate`、`blueprint-write`、`brainstorm` 则把模糊想法推进到阶段、验收和蓝图。

这些名字不像产品发布时会精心包装出来的功能名。它们更像现场贴在墙上的操作规程。

哪里容易乱，就补一条。哪里容易停，就补一个提醒。哪里容易靠记忆，就写成 artifact。哪里容易自说自话，就加 review gate。

这就是 Blueprintflow 一开始的性格：它不是为了描述理想协作，而是为了让协作真的能继续往前走。

## 第一次泛化：把 Borgee 拿掉

Blueprintflow 独立出来后的第一个大动作，是 PR #1：`remove Borgee-specific bindings`。

表面上看，这是一次普通清理：去掉项目名、路径绑定和只属于 Borgee 的例子。

但它回答了一个更大的问题：这套东西到底只是 Borgee 的内部习惯，还是可以离开 Borgee，成为别的项目也能使用的协作协议？

去绑定以后，Blueprintflow 才第一次有了产品形态。它不再只是某个项目里的 `.claude/skills`，而是一套可以被安装、解释、迁移和复用的 workflow skills。

不过这时它还不是今天我们讨论的“通用工作流模型”。它只是完成了第一层抽象：从一个项目的经验，变成一套产品工程协作技能。

这一步很关键。没有这一步，后面所有关于 pack、runtime、entrypoint、marketplace 的讨论都不会出现。

## Dogfood 是它真正的生长方式

Blueprintflow 很早就定下了一条规则：skills 跑出新经验，就开 PR 改 SKILL.md，并走角色 review。

也就是说，它从一开始就要求自己用自己的流程更新自己。

这不是一句漂亮话。看 git history，会发现它几乎一直是这样长大的。

发现 PR review 质量不够，就补 `pr-review-flow` 和 no-LGTM-with-open-issues。发现 milestone artifact 分散，就把默认结构收进 `docs/tasks/<m>/`。发现 UI 变更验收太弱，就补 e2e verification。发现不同 agent runtime 能力不一样，就补 runtime adapter。发现 Teamlead 被当成工兵，就补 coordinator / worker boundary。发现恢复现场靠上下文记忆，就补 notebook、resume 和 state ledger。

这解释了为什么 Blueprintflow 看起来会比普通流程重。

它不是为了重而重。每一层重量，背后都是一次实际失败：有人绕过 gate，有人丢了状态，有人把 review 当礼貌，有人把立场做偏了，有人把可以并行的工作串行化，有人把 Teamlead 的上下文耗在实现细节里。

Blueprintflow 的规则不是一次性设计出来的，是失败驱动长出来的。

## 城市工程隐喻让角色变成边界

到了 PR #6，README 被重写，Blueprintflow 第一次有了稳定的自我描述：一套多 agent 协作做产品的工作流方法论。

那一版引入了城市工程隐喻。

Architect 像总工程师，负责蓝图和结构。PM 像甲方代表，负责立场、用户价值和反约束。Dev 像施工队，负责实现。QA 像质检，负责验收证据。Teamlead 像总包，负责调度、推进和处理阻塞，但不亲自砌墙。

这个隐喻解决了早期一个很麻烦的问题：多 agent 协作时，角色很容易退化成“几个人头”。

Blueprintflow 反过来定义：角色不是人，角色是职责边界。

一个 agent 可以兼任多个角色，但某些职责必须独立。写代码的人不能是唯一验收者。Teamlead 不是最强工兵，而是流程驱动者。PM 不是写需求的人，而是守住产品立场的人。

从这里开始，Blueprintflow 不再只是一堆 skill。它开始有自己的组织哲学。

## 立场驱动，让它区别于普通项目管理

Blueprintflow 很早就拒绝把自己做成 PRD 流程。

它真正强调的不是“需求”，而是“立场”。

需求经常会漂。今天说要一个设置页，明天说要更灵活，后天开始堆选项。每一步看起来都合理，最后产品形状却变了。

所以 Blueprintflow 要求先写清楚：这是什么，不是什么；关键场景是什么，v0/v1 边界在哪里；哪些事情明确不做。

这就是反约束的价值。

写不出“不是什么”，通常说明想法还没有成形。没有反约束，agent 很容易把“多做一点”误当成“做得更好”。

后来那些看起来像文档要求的东西，spec anchor、acceptance anchor、stance blacklist、content-lock、PR cross-check，本质上都在防同一件事：产品判断在执行过程中蒸发。

Blueprintflow 真正害怕的不是代码不能跑，而是代码能跑、PR 也过了，但已经不是最初要做的东西。

## 执行原子从 milestone 变成 task

早期 Blueprintflow 的执行原子是 milestone。

那时强调“一 milestone 一 PR”：四件套和实现进入同一个 worktree、同一个 PR、一次 squash merge。这个设计解决了当时最直接的问题：不要把 spec PR、stance PR、implementation PR 拆成一串等待，导致上下文碎掉、历史碎掉、决策也碎掉。

后来这个模型被 v4.0.0 改掉了。

PR 原子从 milestone 变成 task。

这不是命名调整，而是执行模型变了。Milestone 适合表达用户可见结果，但经常太大，不适合作为一个 branch、一个 worktree、一个 PR 的单位。真正适合实现、验收和合并的，是 task。

于是层级变成：

```text
Phase -> Milestone -> Task -> PR
```

Milestone 仍然重要，但它变成 task 组。Task 才是执行和合并的原子。

这一步让 Blueprintflow 从“按成果交付”进一步细化成“按可验收边界交付”。

## current / next / tasks 解决了蓝图到底是什么

Blueprintflow 早期说过“蓝图先 freeze 再开工”。这句话有用，但不够精确。

后来它被拆成三层：

- `current`：已经实现、验收通过、可以作为当前事实的蓝图
- `next`：锁定、讨论中或实现中的未来产品判断
- `tasks`：从 next 走向 current 的施工路径

这个分层解决了一个长期问题：蓝图到底是计划，还是事实？

答案是：看它在哪一层。

`current` 是事实。`next` 是待实现或实现中的判断。`tasks` 是把判断变成事实的路径。

这也是后面 source trace、promotion、resume、backlog intake 能站住的原因。没有这个状态模型，Blueprintflow 很容易只剩下一堆“文档应该更新”的提醒；有了这个模型，它才知道每份文档处于什么状态，什么时候能被提升为当前事实。

## Teamlead 从提醒器变成驱动者

早期有 `fast-cron` 和 `slow-cron`。

`fast-cron` 处理 idle 派活，`slow-cron` 做漂移 audit。这个设计说明早期的理解是：流程需要定时提醒，否则会停。

后来这个理解变了。

v4.0.1 明确把 Teamlead 定义成主动驱动者，cron 和 reminder 只是 backstop。

这也是一次失败驱动的升级。

如果 Teamlead 只等提醒，流程会停。如果 Teamlead 亲自下场实现，协调上下文会被细节消耗，Dev 的边界也会被打穿。正确边界是：Teamlead 不砌墙，但必须保证工地在动。

所以后来的 Teamlead 标准变成两件事：流程有没有推进到下一个状态，blocker 有没有被移除；team 有没有在 runtime capacity 内被充分使用。

这让 Blueprintflow 从“多 agent 分工表”变成“多 agent 调度协议”。

## 运行时适配让它离开单一工具

Blueprintflow 起初强依赖 Claude Code 的 skill 形态。后来它必须面对不同 runtime：Claude Code、OpenClaw、Codex，以及不同的 agent 通讯、文件共享、cron、subagent 和权限能力。

runtime-adapter 的出现很关键。

它承认一件事：Blueprintflow 的核心规则应该独立于运行时，但执行方式必须适配运行时。

同一条规则，“通知 QA 验收”，在不同环境里可能是派 subagent、发消息、写 PR comment，或者由 parent thread 串行 fallback。同一条规则，“并行推进”，在不同 runtime 里也取决于是否真的有 subagent capacity。

这一步把 Blueprintflow 从“Claude Code skills”推向了“跨 agent runtime 的方法论”。

它还埋下了后来的问题：如果核心规则应该独立于运行时，那它到底应该以 skill 文本存在，还是应该有更明确的 flow contract 和 runtime adapter？

## 插件化暴露了 public surface 问题

中期另一条线，是 marketplace 和 plugin 化。

Blueprintflow 从散落的 skill 目录，变成 Claude plugin、Codex plugin、marketplace manifest、metadata、validation scripts 和版本规则。

这不是纯包装。发行形态会反过来逼问产品边界：用户安装后看到什么？哪些 skill 是入口？哪些只是内部阶段？display name 怎么写？version bump 如何和 changelog 绑定？破坏性改名要怎么处理？

也正是这条线，最后暴露出我们今天讨论的新问题：public skill 太多，入口太散。

如果每个子 skill 都要提醒自己“我是 Blueprintflow 的一部分”，那说明它们不该被当成同级 public entrypoint。用户要推进的是一个 work object，不是手动选择某个内部阶段。

所以来路和未来在这里接上了。

Blueprintflow 先从 Borgee 工地变成可复用 skills。可复用 skills 进一步变成 marketplace plugin。plugin 化暴露出 public surface 问题。public surface 问题又倒逼我们重新思考：Blueprintflow 到底是一堆 skills，还是一个能够判断、路由和验收的 flow runner？

## 它真正继承下来的东西

回头看，Blueprintflow 的来历可以浓缩成四句话。

第一，它来自真实项目，不来自抽象设计。

第二，它靠 dogfood 长大，每条重要规则背后都有一次协作失败或漂移风险。

第三，它的早期核心是产品工程：立场、蓝图、角色、Phase、Milestone、Task、PR、验收和 promotion。

第四，它一路在把隐性协作经验变成显性协议：路径、状态、角色、gate、ledger、runtime adapter。

这就是为什么今天我们能继续往前走。

如果没有 Borgee 里的实际工地，Blueprintflow 只会是一套漂亮流程图。如果没有后来的 marketplace、runtime adapter、current/next/tasks、task PR atom 和 Teamlead drive，它也不会积累出足够多的结构，去支撑“BF 可能是一种通用工作流模型”这个新判断。

下一篇讲的就是这个新判断：Blueprintflow 如何从产品工程 pack，扩展成通用的 evidence-gated work loop。

