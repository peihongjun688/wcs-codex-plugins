# Claude 插件版 · v1.2.1（Claude Code / Claude 市场用）

> 本目录 `claude-plugin/` 是 **web-console-starter-full v1.2.1 的 Claude 插件形态**，与 SkillHub 市场包（`*-skillhub.zip`）**不是同一套格式**，专供 Claude Code / Claude 生态上传安装。

## 为什么有这份

你在 Claude 里安装 skillhub 格式的 zip 时报错：

```
Invalid plugin: missing `.claude-plugin/plugin.json`
```

因为 Anthropic（Claude）插件有自己的 manifest 规范，**必填顶层 `.claude-plugin/plugin.json`**，与 SkillHub 的「顶层 SKILL.md + frontmatter」是两套体系。这份就是按 Claude 官方规范打的。

## 目录结构（Claude 官方规范）

```
claude-plugin/                      ← 把这个目录打成 zip 上传
├── .claude-plugin/
│   └── plugin.json                  ← manifest（name/version/description/skills 声明）
├── SKILL.md                         ← 技能本体（根级单技能写法，frontmatter 的 name 即加载名）
├── skills 声明无须再建子目录        ← 见 plugin.json 的 "skills": ["./"]
├── README.md / AI-GUIDE.md / CHANGELOG.md / CLAUDE.md / AGENTS.md
├── assets/screenshots/              ← 11 张 H5 真机截图（README 预览表引用，安装后可看）
├── docs/  scripts/  frontend/  backend/  app.config.js  package.json
```

要点（都是 Claude 规范的坑）：
- **只有 plugin.json 放 `.claude-plugin/`**，其余组件目录必须放插件根目录，不能塞进 `.claude-plugin/` 里。
- `plugin.json` 的 `"skills": ["./"]` 声明根级 SKILL.md 即技能入口（官方 `claude plugin init` 生成的形态，实测可用）。
- 插件安装是整体拷贝到缓存，**不能引用目录外路径**；本包自包含，无外部引用。
- `backend/public`、`node_modules`、种子库等运行时产物已剔除（`start.bat` 首次运行自动 npm install + build）。

## 校验方式（官方命令，建议上传前跑一遍）

```bash
cd claude-plugin
claude plugin validate .
```

通过会显示 `Nothing to validate` 之类的成功提示，失败会列出具体问题。

## 上传到哪里

- **Claude Code 本地安装**：`/plugin` → Add marketplace（指向 GitHub 仓库或本地目录），或 `claude plugin install`。
- **Claude Code 市场**：把插件放进一个 git 仓库，仓库根加 `.claude-plugin/marketplace.json`（catalog），`/plugin marketplace add owner/repo` 分发（参考官方 Create and distribute a plugin marketplace）。
- **Claude Desktop / claude.ai 组织设置**：上传 zip 需经组织管理员配置（Claude Code 生态外可能不支持第三方插件直接上传，以官方渠道为准）。

## 版本

- v1.2.1：与本地真源 / 本地分发 zip 同版本同内容（剔除运行时产物）。
- 更新历史见 `CHANGELOG.md`。

## 相关包（同一 v1.2.1）

| 包 | 用途 |
|---|---|
| `web-console-starter-full-v1.2.1.zip` | 本地完整版（含真实皮肤源、开发全量） |
| `web-console-starter-full-v1.2.1-claude.zip` | **Claude 插件版（本目录打包）** |
| `web-console-starter-full-v1.2.0-skillhub.zip` | 市场合规版（在线已发布） |

> 规则备忘：**SkillHub 市场包禁图（含 png 必被拒）**，图片只进本地包 / Claude 插件包。Claude 插件**允许带图**（assets/screenshots 正常打包）。