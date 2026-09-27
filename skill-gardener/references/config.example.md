# skill-gardener 本地配置（模板）

> 复制本文件为同目录下的 `config.md`（已被 .gitignore 排除，不会提交），按需填写。
> `config.md` 是你的私有配置区：更新 skill（拉开源新版覆盖 SKILL.md / templates.md）时它保持不动，是唯一需要自己维护的文件。

## 用途

`config.md` 里可以放任何你不想随 skill 分发的私有约定，常见的三类：

1. **本机路径**（省去 Step 0 重复询问）
2. **多机器路径映射**（同一 hub 在不同机器上路径不同时，按 hostname 取行）
3. **产出登记区**（把「六、已产出的 grow-skill 登记」记在这，保持 SKILL.md 与发布版一致）

SKILL.md 的 Step 0 会先检查本文件：存在 `config.md` 时优先按它执行。

## 模板

```markdown
# skill-gardener 本地配置（私有，勿分发）

## 路径

- hub（skill 本体安装目录）：<实际绝对路径>
- runtime（当前 Agent 引擎的 skill 目录）：<实际路径>

## 多机器路径映射（可选）

> 使用前先跑 `hostname`，按行取当前机器的路径。每台机器各自维护自己的 config.md，可把其他机器的行抄进来备用。

| hostname | hub 绝对路径 | Agent 引擎 | runtime skill 路径 |
|---|---|---|---|
| <hostname-1> | /path/to/skill-hub | CodeBuddy | ~/.codebuddy/skills/ |
| <hostname-2> | /path/to/skill-hub | Claude Code | ~/.claude/skills/ |

## 产出登记区（可选）

| grow-skill | 形态 | 聚合/包壳对象 | 创建日期 |
|---|---|---|---|
| <领域>-controller | 聚合型/包壳型 | <收编了哪些 skill> | YYYY-MM-DD |
```
