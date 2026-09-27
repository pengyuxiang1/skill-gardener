# skill-gardener

**一个给 skill 当园丁的元技能 —— 聚拢同类 skill 到一个 controller 后面，让经验随任务持续生长。**

**中文** | [English](README.md)

> skill 越装越多，工具越攒越乱，同一个坑被 AI 踩三遍——因为每个会话它都是全新记忆，而 skill 的说明书永远不会从你的淤青里学到任何东西。

**grow-skill = 某个领域里唯一的运行时入口：路由（什么场景用什么工具）+ 沉淀（踩过的每个坑都记下来，只增不覆盖）。**

姊妹仓库：**[skillforge](https://github.com/pengyuxiang1/skillforge)** —— 把一个工作流锻造成超级 skill（状态机 + 引导脚本 + 门禁 hook）。skillforge 负责把一个 skill 造好，skill-gardener 负责管好一群。

---

## 它解决什么问题

| 痛点 | 实际伤害 |
|------|---------|
| 同类 skill 太多 | AI 凭 description 撞运气选工具——而 description 从不写"什么时候别用我" |
| 经验蒸发 | 踩坑心得记在会话里，会话一关就没；外部 skill 一更新，本地笔记被整目录覆盖 |
| 安装散落 | 用户目录、项目目录、各家引擎目录——没人说得清自己到底有哪些 skill |

开源界的项目管的是**分发**（GUI 管理器、静态目录、MCP 注册中心），没有一个管**营业经验**——恰恰是真正值得留下的东西。

## 双层结构

```
┌ Agent 引擎（CodeBuddy / Claude Code / Codex / …）
│   └─ <领域>-controller/     ← 引擎内：路由表 + 经验日志（纯 Markdown skill）
└ 引擎之外
    └─ hub/<skill名>/        ← 你的安装目录：skill 本体，版本正本
```

- **hub** —— 你自己选的地方（Obsidian vault 内的目录、任意 git 仓库都行）。skill 本体以正本形式存放在这。引擎可换，hub 不动。
- **controller** —— 引擎里一个轻量 skill，只干两件事：**场景路由表**（每行从"用户会带着什么问题来"出发，不从"工具有什么功能"出发）+ **经验日志**（只增不覆盖）。

外部 skill 更新？覆盖的是 hub 里的本体——controller 的经验层毫发无损。**书再版，馆员的笔记不烧。**

## 安装

### 方式一：npx skills（推荐，支持 79 个 Agent，含 CodeBuddy）

```bash
npx skills add pengyuxiang1/skill-gardener                       # 自动检测已装的 agent
npx skills add pengyuxiang1/skill-gardener -a codebuddy -g -y    # 指定 agent + 全局 + 免确认
```

### 方式二：安装脚本

```bash
git clone https://github.com/pengyuxiang1/skill-gardener.git
cd skill-gardener
./install.sh                          # 自动检测已装的 agent
./install.sh --agent codebuddy        # 或：codebuddy / claude-code / codex / cursor
./install.sh --link                   # 软链代替复制（方便更新）
```

免 clone：

```bash
curl -fsSL https://raw.githubusercontent.com/pengyuxiang1/skill-gardener/main/install.sh | bash -s -- --agent codebuddy
```

### 方式三：让 agent 自己装

把下面这句丢给你的 AI 编程助手：

> Clone 本仓库，把 `skill-gardener/` 目录安装到你的 skills 目录（CodeBuddy：`~/.codebuddy/skills/`，Claude Code：`~/.claude/skills/`）。

## 快速开始

1. 对你的 agent 说："*我有好几个 `<某领域>` 的 skill，帮我建一个统一入口（创建 controller）*"
2. skill 会先跑 **Step 0**：问你 hub 放哪（任意 git 管理的目录都行，用 Obsidian 的可以放 vault 里），并识别当前引擎的 runtime 路径
3. 然后盘点领域资产 → 设计场景路由表 → 生成 controller →（聚合型）把 skill 本体收编进 hub
4. 从此该领域的每个任务收尾时：新坑追加到 controller 的 `references/experience.md`——带日期、带验证证据、只增不覆盖

## 设计原则

- **引擎无关** —— controller 是标准 SKILL.md，任何能加载 Agent Skills 的引擎都能用
- **不预设路径** —— 首次使用走初始化引导（两个问题），结果记入 `<hub>/CONFIG.md`
- **不掺私货** —— 本地约定（多机器路径映射、自己的登记表）放在 git ignore 的 `references/config.md` 里，更新 skill 永远不碰它
- **不为个例做设计** —— 能共享的方法，不能掺自己家的事

## 许可

[MIT](LICENSE)
