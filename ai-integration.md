# AI-READWRITE: 跨 AI 接入与同步规范

本仓库是纯 Markdown + Git 的个人记忆源。GitHub 公共内容可由任何能访问 GitHub 的工具读取；写入能力必须单独授权。平台产品能力与连接器会变化，以下说明的是通用接法，不代表所有客户端都内置自动同步。

## 内容读取

仓库：`https://github.com/njx970101/personal-ai-archive`

- 浏览器：直接打开仓库文件或 README 中的路径。
- GitHub Contents API：`GET https://api.github.com/repos/njx970101/personal-ai-archive/contents/{path}`。公共仓库读取通常无需 token；API 返回 JSON，文件正文位于 `content` 字段（Base64 编码）。
- Raw 文件：`https://raw.githubusercontent.com/njx970101/personal-ai-archive/main/{path}`，适合支持 URL 导入的客户端。
- 本地/设备副本：`git clone https://github.com/njx970101/personal-ai-archive.git`，之后 `git pull` 获取更新。

建议每个新 AI 会话先读 `README.md`、`workflow/preferences.md`、`identity/profile.md`，再按任务需要读取 `memory/`、`skills/` 和 `archive/`，避免不必要地摄入全部档案。

对本档案用户的已知协作偏好：优先用简体中文，直接处理明确授权的工作；对配置和故障报告给出实际验证证据。不要新建用户未要求的 provider/API key，不要把未知个人背景写成事实。

## ChatGPT、Gemini、Claude 与其他助手

1. 首选平台提供的 GitHub 连接器/应用，授权只读访问本仓库（需要写回时才授予写权限）。
2. 若客户端支持 MCP、远程文件或 URL 导入，可使用 GitHub API/Raw URL。
3. 若没有连接器，手动导入 Markdown 或在本地 clone 后由可访问文件的客户端读取。
4. 某些客户端要求登录或显式授权；公开仓库只免除读取认证，不会自动赋予写权限。

GitHub API 适配器是统一的数据接口，不等于 ChatGPT、Gemini 或 Claude 官方客户端都原生支持任意 GitHub API 调用。

## Meta Muse、OpenAI Dots 与 Agent

仅当相应产品提供 GitHub、MCP、HTTP 工具或可配置 Agent 插件时，配置上述只读/读写端点。先核验产品名称、连接器和权限模型；无法确认的产品能力不要假设存在。Agent 侧统一提示词见下方“跨 Agent 指令”。

## 跨 Agent 指令

将以下规则作为自定义指令或 Agent system prompt：

> 开始任务时读取本仓库 `workflow/preferences.md` 和 `identity/profile.md`，并按任务读取相关记忆。完成任务后，只沉淀对未来有长期价值、已由用户明确表达或确认的信息。偏好写入 `memory/preferences.md`，决策写入 `memory/decisions.md`，经验写入 `memory/lessons.md`，技能变化写入 `skills/`，已完成项目写入 `archive/index.md`。保留 Markdown 结构，不记录完整对话，不编造事实，不写入凭据、敏感个人信息、机密或第三方隐私。每次修改给出清晰 Git commit；说明更新了哪些文件。对身份核心信息的推断只能先提出建议，不能当作事实写入。

## 自动写回规则

GitHub 不会因为 AI 完成一次任务就自动得知并保存对话。要实现自动写回，AI 客户端/Agent 必须被配置为调用有写权限的 GitHub Connector、MCP server 或 GitHub Contents API。

建议按此流程自动化：

1. 任务结束时提取候选经验；没有可长期复用内容时不写入。
2. 按上述目录映射生成最小 Markdown 更新，并检查是否含秘密、隐私、机密或未经确认的推断。
3. 通过 GitHub Contents API 创建或更新文件。每次写入都提供当前文件的 `sha`，并写入 `message`；GitHub 会为该变更创建提交。
4. 若平台支持分支/PR，优先写到短期分支并创建 PR；否则直接提交到 `main`，但遵守下面的保护规则。
5. 更新后读取文件核验内容，并向用户报告提交链接和变更摘要。

Contents API 写入需 `Contents: write` 权限的 Fine-grained PAT 或 GitHub App 安装令牌。不要将 token 放入此仓库、Markdown、提示词或对话；只放在平台的 Secret/凭据管理中。仅读访问时不要授予写权限。

## 版本保护与回退

- 每次 Git commit / GitHub Contents API 写入都会留下提交历史；在 GitHub 文件页选择 **History** 可检查单文件历史。
- 要撤销变更，使用 GitHub **Revert** 或 `git revert <commit>`，生成一条反向提交并保留审计轨迹。
- `main` 已配置分支规则，禁止删除分支、强制推送和非线性历史。日常小更新可正常形成提交；大批量或高风险改动建议走 PR。
- 可在 GitHub 仓库 **Settings → Rules → Rulesets** 查看规则状态。规则保护历史，不替代备份，也不阻止错误内容通过普通提交进入仓库；定期检查 History 和 PR。

## 公开仓库安全边界

这是公开仓库。只放入你愿意公开给互联网的信息。绝不提交 API key、密码、访问令牌、身份证明、住址、财务/健康细节、私人对话、客户数据或保密项目资料。若需要保存私密档案，应另建 private 仓库并重新检查所有平台授权。
