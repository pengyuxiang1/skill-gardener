---
name: create-grow-skill
description: 元技能：聚合同类型互补的多个 skill（或为单个外部 skill 包壳），创建"经验沉淀+skill路由"型的 grow-skill（如 browser-controller）。引擎无关、路径可配置（CodeBuddy/Claude Code/Codex 等均可使用），首次使用有前置初始化引导，适配任意 hub 安装路径。当用户想为某类工具（浏览器/数据库/部署/文档处理等）建立统一入口、想给外部 skill 沉淀使用经验、想创建 XX-controller 类 skill 时使用。触发词：创建controller、聚合同类skill、grow skill、经验沉淀层、统一工具入口、包壳外部skill、初始化grow-skill、skill管理仓库。
---

# create-grow-skill —— 创建成长型 controller skill 的元技能

## 一、什么是 grow-skill

grow-skill = 某个领域里**唯一的运行时入口**，只做两件事：

1. **路由**：场景 → 该用哪个工具/skill（不含具体脚本，指路即可）
2. **沉淀**：经验日志（追加式）+ 已验证案例（提速 agent 后续使用）

与普通 skill 的区别：普通 skill 解决"怎么做某件事"；grow-skill 解决"这件事该用哪个 skill 做，以及大家踩过什么坑"。

**跨引擎双层结构**（本方法引擎无关、不绑定任何安装路径，可整体共享给他人）：

```
┌ Agent 引擎（可换：CodeBuddy / Claude Code / Codex / …）
│   └─ <runtime>/<领域>-controller/    引擎内封装入口（纯 Markdown skill：路由表 + 经验）
└ 引擎之外
    └─ <hub>/<skill名>/               安装目录（外部/被收编 skill 本体，版本正本）
```

- **controller**：引擎内的一层封装，对接当前引擎加载 skill——引擎只看到这一个入口
- **hub**：引擎外的安装目录，用户自选（如 Obsidian vault 内目录、任意 git 仓库）——skill 本体与经验独立于引擎存在
- controller 用标准 SKILL.md 格式，任何支持 Agent Skills 的引擎都能加载

## 二、适用判断

| 情况 | 是否创建 grow-skill | 形态 |
|------|--------------------|------|
| 多个同类互补 skill 并存 | ✅ **聚合型** | 路由表为主，收编被聚合 skill |
| 单个外部 skill（官方更新会覆盖，经验无处沉淀） | ✅ **包壳型** | 路由极简（一层直指），experience.md 为主 |
| 单个内部 skill，自有沉淀机制 | ❌ 用 skill-creator 直接改进本体 | — |

**判断信号**：用户说"工具太多怕 AI 用错"、"XX 的经验没地方记"、"每次都要翻好几个 skill"。

## 三、创建 SOP（按序执行）

### Step 0：初始化（首次使用时执行）

> **先查本地配置区**：若本 skill 目录下存在 `references/config.md`（自行创建，git ignore，模板见 [references/config.example.md](references/config.example.md)），**先读它**——其中可含本机路径、多机器路径映射、产出登记位置等私有约定，优先级高于本文件默认行为。已含路径配置 → 跳过本步。

本方法依赖两个路径，**首次使用必须先引导用户配置**（不假设用户的笔记软件与 Agent 引擎）：

1. **确定 hub**：问用户「skill 本体集中安装到哪个目录？」——建议选有 git 管理的目录（如 Obsidian vault 内的 `resource/skill-hub/`，或任意独立 git 仓库），没有则引导新建
2. **确定 runtime**：识别当前 Agent 引擎的 skill 加载目录，识别不了就问用户「你的 skill 装在哪个目录？」

   | Agent 引擎 | 用户级 runtime | 项目级 runtime |
   |-----------|--------------|---------------|
   | CodeBuddy | `~/.codebuddy/skills/` | `<project>/.codebuddy/skills/` |
   | Claude Code | `~/.claude/skills/` | `<project>/.claude/skills/` |
   | Codex CLI | `~/.codex/skills/` | `<project>/.codex/skills/` |
   | 其他 | 一般 `~/.<engine>/skills/`，以官方文档为准 | 同左 |

3. **写入配置**：把两个路径记入 `<hub>/CONFIG.md`（骨架见 [references/templates.md](references/templates.md) 模板 E），作为路径查找的事实源；也可同时记入本地 `references/config.md`

> 已有 CONFIG.md → 直接读它拿两个路径，跳过本步。
> 生成 controller 时，把模板中的 `<hub>` / `<runtime>` 占位符替换为实际路径。

### Step 1：盘点领域资产
- 列出 runtime 目录（Step 0 配置）内同领域 skill、机器上的外部 CLI（`which` 验证）、未安装的备选工具
- 每个工具记录：版本、定位、独特能力、登录态/依赖关系

### Step 2：设计场景路由表
- **从用户任务场景出发**（不是从工具功能出发）：问"用户会带着什么问题来？"
- 每行 = 场景 | 首选工具 | skill 文档路径 | 关键提示（坑的一句话版）

### Step 3：生成 controller skill
- 位置：runtime 目录（Step 0 配置）下的 `<领域>-controller/`
- 结构标准（骨架模板见 [references/templates.md](references/templates.md)）：
  ```
  <领域>-controller/
  ├── SKILL.md                  # 路由表 + 命令速查 + 沉淀规范（精炼，<500行）
  └── references/
      ├── experience.md         # 经验日志（追加式，只增不覆盖）
      └── cases/                # 已验证的完整案例
  ```

### Step 4：收编被聚合的 skill（仅聚合型）
- 把被聚合的 skill 从 runtime 目录 **mv 到** `<hub>/<skill名>/`（安装目录，版本正本）
- 在 `<hub>/INDEX.md` 登记归属：`<skill名> → 归 <领域>-controller 管理`
- 包壳型跳过此步（外部 skill 本体不在我们目录里）
- controller 生成后如何同步/提交（git 或其他方式），由用户自行管理，本方法不做约束

### Step 5：经验冷启动
- 收拢既有散落经验（旧 _EXPERIENCE.md、对话中踩过的坑、已有案例文档）进 experience.md
- **每条必须带**：日期 / 场景 / 验证证据（复现命令或产物路径）
- 无经验则写"待验证"占位，禁止编造

### Step 6：验证
- 下个会话测试：AI 遇到该领域任务是否只匹配到 controller（不再匹配到被收编的 skill）
- 被聚合 skill 若被官方机制装回 runtime（如包管理器的 install-skill），删除之，正本在 hub

## 四、grow-skill 的维护规则（写进每个 controller 的 SKILL.md）

1. 经验只追加到 controller 自己的 `references/experience.md`，**不写入 hub**（hub 是纯安装目录）
2. 新坑格式：`日期 | 坑 | 绕法`，错误认知写"实测修正"保留轨迹
3. 完整案例（方法论+协议+产物）→ 新建 `references/cases/<站点或场景>-capture.md`
4. 官方 skill 升级 → 按 hub INDEX.md 的同步命令更新本体，经验层不动
5. 路由表变化（新工具/新场景）→ 双写：controller SKILL.md + hub INDEX.md

## 五、命名与登记

- 命名：`<领域>-controller`（browser-controller、db-controller、deploy-controller…）
- 新建完成后登记一行产出记录：登记在本文件下方登记表，或本地 `references/config.md` 的登记区（保持本文件与发布版一致时用后者）

## 六、已产出的 grow-skill 登记

| grow-skill | 形态 | 聚合/包壳对象 | 创建日期 |
|-----------|------|--------------|----------|
| （登记你的产出） | | | |

## 参考

- 结构骨架与模板文件：[references/templates.md](references/templates.md)（含 controller / experience / case / INDEX 条目 / CONFIG 五个骨架）
- 本地私有配置（多机器路径映射、登记区等）：[references/config.example.md](references/config.example.md)
