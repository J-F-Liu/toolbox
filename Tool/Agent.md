# Coding Agent

- [How AI coding agents work—and what to remember if you use them](https://arstechnica.com/information-technology/2025/12/how-do-ai-coding-agents-work-we-look-under-the-hood/)
- [Components of A Coding Agent](https://magazine.sebastianraschka.com/p/components-of-a-coding-agent)


coding agent 的本质区别，不在于 “会不会思考”，而在于它能不能把代码任务变成一个：
带有检索、编辑、执行、验证、修复的搜索过程。
Coding Agent = 精准检索（Context） + 最小编辑（Edit） + 硬验证（Verify） + 可回退规划（Plan）


上下文太少：激活不足，上下文太多：注意力稀释。
上下文窗口不是越大越好，而是存在一个动态最佳区间：
信息不足
    ↓
信息充分（最佳）
    ↓
信息冗余

很多 Agent 框架发现：80% 的问题只需要 5%~20% 的上下文窗口。
模型最需要的是高相关信息，而不是大量信息。即：Relevant Context >> Large Context
工程上的核心问题已经从 “如何扩大上下文窗口”，逐渐转向 “如何筛选、压缩和组织上下文”。
真正重要的能力不是 Long Context（长上下文），而是 Context Management（上下文管理）。很多 Agent 系统的效果差异，最终就体现在这一点上。

Liu et al. 2023 年的研究发现，模型对上下文头部和尾部的信息利用率显著高于中间部分。将关键信息埋在长上下文中间，性能会明显下降。

Google Antigravity: https://antigravity.google/

阿里Qoder: https://qoder.com/

字节Trae: https://www.trae.cn/

腾讯CodeBuddy: https://www.codebuddy.cn/home/

百度Comate: https://comate.baidu.com/zh

OpenCode：https://opencode.ai/

开源ClawCode: https://github.com/ultraworkers/claw-code

OpenRouter: https://openrouter.ai/
Cline: https://app.cline.bot/
LMStudio: https://lmstudio.ai/

pomfret: https://github.com/mikewei/pomfret
cargo binstall pomfret
pomfret --backend-timeout-secs 3000
http://localhost:8080/v1

Pi: https://pi.dev/
```
npm install -g --ignore-scripts @earendil-works/pi-coding-agent
bun add -g --ignore-scripts @earendil-works/pi-coding-agent
pi install npm:pi-web-access
pi update
```
VS Code Extension: pi Agent for VS Code，ACP Pro

cargo binstall symposium
cargo agents --version

cargo binstall lean-ctx
lean-ctx init --agent pi

[zerostack](https://github.com/gi-dellav/zerostack)
cargo install zerostack --features all
zerostack --print-config
zerostack --acp

[oh-my-pi](https://omp.sh/)
bun install -g @oh-my-pi/pi-coding-agent
Create ~/.omp/agent/.env with OMP_SKIP_SETUP=1
Create ~/.omp/agent/models.yml
omp --help
omp --list-models
omp --model deepseek/deepseek-v4-pro --tools off --no-skills --no-title
omp --provider claude-tap
omp acp
omp update

Number of built-in tools:
- Pi: file 4, search 2, shell 1, total 7
- zerostack: 
- Reasonix: file 13, search 4, shell 3, job 3, memory 3, task 4, skill & mcp 5, code 2, sub agent 4, total 41
- CodeWhale: file 7, search 4, shell 6, task 14, sub agent 3, rlm 5, schedule 8, git 7, github 5, 81

reasonix的突出特性是append_only，即每轮对话都只在上一轮对话的基础上追加，所以缓存命中率会很高，下来差不多能到95%~99%。


Pi 遵循的一条名为 "渐进式信息披露" 的核心原则。它不仅是 Pi 高效运作的秘诀，也能直接指导我们更好地与 AI 交互。

大语言模型（LLM）一个普遍的弱点 ——“迷失在中间”。

🧠 首因效应：让模型优先理解 “目标”，而非 “能力”
在提示词的开头，Pi 会先定义身份（如 “你是一名专家级编码助手”），然后呈现当前的任务目标、错误日志或用户指令等关键信息。

这种做法利用了 “首因效应”，即信息序列的开头部分更容易被记住和理解，让模型在一开始就建立起清晰的 “任务认知”，明确要解决的问题。

如果将大量工具定义放在开头，无异于在开始解题前要求模型背下整本工具书，这会严重挤占上下文窗口，也增加了模型在推理时分心的风险。

🎯 近因效应：确保 “行动” 在注意力高峰
当 LLM 理解任务、分析完代码（占据了上下文窗口的中部后），它需要决定如何行动。此时，Pi 将简洁的工具定义（read、write、edit、bash）恰好呈现在对话的最后。

这完美契合了 “近因效应”，即序列末尾的信息同样具有很高的注意力权重。这样一来，当模型准备调用工具时，这些定义正好处于其 “注意力焦点” 上，能确保行动指令精准无误。


System Prompt 不要静态的 “工具清单”，而要改成动态的 “路由规则”。
用自然语言告诉模型 “怎么用” 和 “什么时候用”。这部分逻辑是tools参数里的description写不下的。


现在 LLM 只有：token world model

但没有：executable software world model

它不知道：
- 类型系统真正含义
- runtime state
- concurrency consequence
- performance implication
- hidden invariants

它只是 “统计拟合”。

记忆不是存储，而是压缩后的可推理结构。
人类不会记住所有 token，而会形成：
- abstraction
- causal graph
- mental model
- architectural invariant

节约token = 提高智力
减少请求次数 = 节约token
高性能工具 = 更快完成任务

让 AI 一次性生成的结果，质量远不如让它反复打磨多轮。人类写东西不也是改出来的吗？

Goal 模式：执行完一轮之后，会回头审视自己刚才做的东西。哪里好，哪里不好，薄弱点在哪。然后针对薄弱点改进，再审视，再改进。循环往复，直到目标达成。让AI在审视成果时，不是"再看一遍"，而是多视角审视。切换为使用者、审阅者、攻击者这三种角色。

Goal 模式不是帮您"做完一件事"，而是帮您"把一件事做好"。

Hive 蜂巢模式：一个 Goal Master 坐镇中央，角色是项目经理——它负责把一个大目标拆成若干子任务，然后把这些任务贴到公告板上。多个 Worker 各自从公告板上认领任务，领了就干，彼此独立，互不干扰。

Goal Master 本身是运行在 Goal 模式下的——这意味着它有时间预算，会做探索/利用的切换，会从多个视角审视当前状态。它不只是开局分一次任务就完事了。Worker 交付之后，它会验收，验收完了继续观察：还有没有能改进的地方？有的话，再派发新任务。

子 agent 的核心价值是 context 隔离。它从零开始构建自己的对话历史，不继承主对话的消息，这意味着它搜索过程中产生的大量 grep 结果、代码片段都留在自己的 context 里，主对话只收到一段总结性的文本结论。

## SDD，spec-driven development
spec 驱动开发，是把意图前置，并且把它写成一种可以被引用、被对照的东西。
这里要把 spec 和传统需求文档分开。需求文档是给人看的，写完就躺在 wiki 里没人动。spec 不是，它是 agent 行动的依据，是 agent 在动手之前就被约定好的边界，也是事后判断 agent 有没有跑偏的基准。spec 是活的，它进到验证的回路里。

Contract 管的是"代码声称的契约"
Boundary 管的是"这些契约的有效边界在哪里"
Verification 管的是"怎么证明契约在边界内成立"
Modulation 管的是运行起来之后怎么动态调节行为

attestation schema：
claim       这段代码声称满足的具体主张（来自 spec）
method      用什么方法去检验这个主张
source      这个检验依据的是什么
verdict     检验的结论（通过 / 不通过 / 无法判定）
confidence  这个结论本身有多可信

把 spec 和验证当成一等公民，而不只是把审查流水线修得更快。

review 的质量是隐性的，藏在审查者脑子里，没法被复核，也没法被传递。
我们知道的，比我们说得出来的多 (we know more than we can tell)。

verification 是"这段代码的某个具体主张，能不能被一个明确的方法、依据一个明确的来源、给出一个明确的结论"。

把尽可能多的"审"从 review 转成 verification。

写代码的人在变少，判断代码的人会变多。
而判断这件事，迟早要从凭经验、凭直觉、凭说不清道不明的手感去 review，长成一条条带着 claim、method、source、verdict、confidence、谁都能拿去重跑的 verification。

Agent 写代码的能力已经超过绝大多数人，前提是有人掌舵，方向要自己定，设计要自己做。
只有那些踩过坑、做过决策、知道路通向哪里的老工程师，才知道该往哪个方向指。
因为它什么都能干、什么都能写，知道的东西又非常多，所以人的判断反而变得更重要。

甩手掌柜式的开发，做不了特别复杂的项目。
尤其是特别长、特别大的复杂模块，就会考验人的技术选型品位、经验和判断水平。
AI 的能力早就溢出了, 跟不上的是人。

另一类 Input 是软件软件工程层面的代码风格和技术选型：模块不能嵌套太深，文件超过 2000 行就得拆，这些约束来自经验，来自对“哪条路后面会省很多事”的判断。
重要的决策、架构的选择都由人来做; 机器负责帮助调研、补背景、补盲区、扩视野, 最后所有判断由人拍板, 机器去执行和编码。
并行五六个任务, 一天差不多四五十个高质量决策。从早上七八点到下午三四点, 人的精力基本就耗尽了。

AI 写出来的代码, 至少要换一个不同的 AI 去 review。
让它冷启动一个全新的、独立的 AI Agent 去审计这个 PR。

就像  0.99 的 100 次方和 1.01 的 100 次方之间, 差距巨大。
一天几十个决策这么复利下去, 最后做出来的软件质量, 会差到天上地下。

## 写得更快，不等于交付得更快

把 AI 工程产能概括为一个乘法公式：模型能力 × 上下文质量 × 任务分解 × 反馈速度 × 验证强度 × 人的判断。
它之所以是乘法，是因为任何一项接近零，整个系统都可能失效。


在 2024 年前缀缓存普及之后，所有的决策都基于三个现在已经成为行业共识的事实：
- 如果一个消息的开头 N 个 token 和上一轮完全一样，这 N 个 token 完全免费，也不需要重新推理。
- System Prompt 的中间和末尾是整个对话中注意力最低的区域。长内容放在 System 里，模型会逐渐忘记。
- 任何在会话过程中可能变化的内容，永远不应该放在 System Prompt 里。

很多人以为 System Prompt 的内容有更高的优先级。这只对 System 最开头前几百个 token 是对的。任何放在 System 后半部分的长内容，优先级远低于放在 User 开头的同样内容。


由于现在所有主流 LLM 提供商都支持前缀 KV 缓存，这个问题的答案已经从 2023 年的 "放 System" 变成了现在的：
永远不要把文件树 / 工作区状态放在 System Prompt。放在 User Prompt 的最顶部，并且每一轮对话都重新放一次。

文件树不是静态的。Agent 会创建、删除、重命名、移动文件。一旦你把文件树放在 System Prompt，你在整个会话期间永远不能更新它。Agent 会永远基于会话开始时的旧结构工作，这是 Coding Agent 最常见的失败模式，没有之一。

你可以根据当前任务调整文件树的细节：
- 小仓库：<100 文件，放完整的缩进文件树
- 中仓库：100~1000文件，放折叠的文件树，最多显示 3 层深度，标注隐藏的文件数量
- 大仓库：>1000 文件，只放根目录结构，加上最近修改的 20 个文件，明确提示 "使用 glob 工具搜索更多文件"

Agent 给 LLM 提供工具数量：大约 30-50 个工具就开始出现明显退化，超过 100-200 个工具时退化非常严重。
- 1 - 10 个工具（安全区）：在这个范围内，主流模型（如 GPT-4o, Claude 3.5 Sonnet 等）表现出色，工具选择和参数提取的准确率通常能保持在 80%~95% 以上。
- 10 - 20 个工具（性能转折点）：这是生产环境中的实践极限（Safe Zone 的边缘）。超过 15 个工具后，如果没有特别的优化，即使是最顶尖的模型，其推理能力和选择准确率也会开始出现可测量的下降。
- 30 - 50 个工具（严重退化区）：在这个区间，“选择困难症” 全面爆发。行业测试表明，将工具从 4 个增加到 50 个时，很多复杂任务（如跨领域调度）的准确率会从 40% 以上暴跌至 10% 甚至 2%。
- 100 个以上工具（灾难性失效）：当同时抛给模型 100 甚至数百个工具时，准确率几乎降至个位数（0%~20%），系统处于不可用状态。

## Skills Over MCP
Think about any product you’ve ever used… software or not. Most come with some sort of instruction manual that helps you figure out how to use it.

For developer products, that’s usually in the form of docs, tutorials, examples, and maybe troubleshooting guides. Companies that do this well are often praised for their attention to Developer Experience.

But now we’re asking our agents to become users of products on our behalf. We give them an MCP server that essentially provides a list of tool names and descriptions and expect them to figure it out without flailing or messing up. This is horrible Agent Experience.

Skills already solve the problem of teaching an agent how to do something in a way that avoids context bloat. If we ship MCP servers with accompanying skills, that should give the agent the tools it needs to execute but also the operational knowledge it needs to do it efficiently.

Essentially, ship the manual with the product.

Coding 是最自然的第一块楔子，因为代码世界已经为 Agent 准备好了半套操作系统：
- repository 和 filesystem 提供结构化外部记忆；
- shell、Git、compiler、test runner、rg 等提供可组合动作；
- Diff、类型检查和测试提供相对客观的 judge；
- branch、worktree 和 commit 让大部分动作可隔离、可回滚；
- 开发者愿意容忍 CLI、日志和显式权限等早期产品摩擦。

Coding 提供了最便宜的行动与验收环境，却只是 Agent 扩张的第一站。Harness 一旦学会读取文件、调用工具、保留状态、并行分解与验证结果，同样的骨架就可以向研究、运营、财务、文档、演示、设计和跨应用流程迁移；改变的主要是 Skills、连接器、artifact renderer、权限策略与 judge。

正式版 DeepSeek-V4-Flash使用 DeepSeek Harness 极简模式，并使用max档位，topp=0.95,temperature=1.0

Qwen3.8-Max，总参数2.4万亿，单次激活约95B，100万token上下文
Kimi K3，2.8万亿参数，
一个供应商最可怕的地方，不是它贵，是你离不开它。而一旦你有了替代方案，哪怕那个替代方案只有八十分，议价权就回来了。