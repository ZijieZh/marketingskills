# Marketing Skills 中文向导

这是一个面向 AI Agent 的营销技能库，适合让 Claude Code、OpenAI Codex、Cursor、Windsurf 等支持 Agent Skills 的工具参与增长、转化、SEO、广告、邮件、销售赋能和营销运营工作。

> 当前仓库包含 45 个营销 skills、112 个参考文件、43 个 eval 文件、93 个工具集成说明和 64 个零依赖 CLI 工具。

## 这个项目是什么

`marketingskills` 不是一个普通的提示词合集，而是一套按营销任务拆分的 Agent Skills：

- 每个 skill 放在 `skills/<skill-name>/SKILL.md`，通过 frontmatter 描述触发场景。
- 复杂知识放在 `references/`，让 agent 按需读取，避免一次性塞入过多上下文。
- 部分 skill 带有 `evals/evals.json`，用于验证输出质量。
- `tools/` 目录提供营销工具注册表、API 集成说明和可直接运行的 Node.js CLI。

它的核心设计是：先建立产品、用户、定位和竞品上下文，再让不同营销 skill 基于同一份事实工作。

## 适合谁

适合：

- 创始人或增长负责人，想用 AI 快速搭建营销执行系统。
- 技术营销、增长工程、RevOps、SEO、CRO、内容和广告团队。
- 想把营销方法论沉淀成可复用工作流的 AI Agent 用户。
- 需要把营销任务交给 Codex、Claude Code、Cursor 等 coding agent 协作的人。

不适合：

- 只想要一次性闲聊建议，不打算沉淀产品上下文。
- 不愿意提供真实产品、用户、渠道、转化数据的人。
- 期待 AI 自动绕过平台规则、发送垃圾邮件或生成误导性营销内容的人。

## 最重要的使用原则

先运行 `product-marketing`，再运行其他 skill。

`product-marketing` 会创建 `.agents/product-marketing.md`，记录产品定位、目标用户、痛点、竞品、差异化、品牌语气和客户原话。其他 skill 会优先读取这份上下文，避免每次都重新解释你的业务。

推荐顺序：

1. 建立产品营销上下文：`product-marketing`
2. 选择当前任务：例如 `cro`、`copywriting`、`seo-audit`、`ads`
3. 给 agent 提供真实输入：页面链接、文案、数据、目标、限制条件
4. 要求 agent 输出可执行产物：实验方案、文案、追踪计划、邮件序列、页面结构等
5. 把结果落到你的项目文件或营销文档里，形成可复用资产

## 安装方式

### 方式一：使用 npx skills

安装全部 skills：

```bash
npx skills add ZijieZh/marketingskills
```

只安装常用 skills：

```bash
npx skills add ZijieZh/marketingskills --skill product-marketing cro copywriting seo-audit analytics
```

查看可安装列表：

```bash
npx skills add ZijieZh/marketingskills --list
```

### 方式二：Claude Code 插件方式

在 Claude Code 中执行：

```text
/plugin marketplace add ZijieZh/marketingskills
/plugin install marketing-skills
```

### 方式三：手动复制

macOS / Linux / WSL：

```bash
git clone https://github.com/ZijieZh/marketingskills.git
mkdir -p .agents/skills
cp -r marketingskills/skills/* .agents/skills/
```

Windows PowerShell：

```powershell
git clone https://github.com/ZijieZh/marketingskills.git
New-Item -ItemType Directory -Force .agents\skills | Out-Null
Copy-Item -Recurse -Force marketingskills\skills\* .agents\skills\
```

如果你的 agent 不读取项目内 `.agents/skills/`，把 `skills` 目录复制到该工具的全局 skills 目录。Codex Desktop 环境常见路径包括：

```powershell
C:\Users\<你的用户名>\.agents\skills
C:\Users\<你的用户名>\.codex\skills
```

## 第一次怎么用

### 1. 建立产品上下文

对 agent 说：

```text
使用 product-marketing skill，读取当前项目，帮我创建 .agents/product-marketing.md。
先自动根据 README、官网文案、package.json 和现有文档起草一版，然后问我缺什么。
```

如果你没有代码项目，可以直接提供业务信息：

```text
使用 product-marketing skill，从零开始帮我梳理产品营销上下文。
我的产品是：...
目标用户是：...
目前最想解决的问题是：...
```

### 2. 选择一个具体营销任务

不要只说“帮我做营销”。更好的方式是给出任务、目标和素材：

```text
使用 cro skill，分析这个落地页为什么转化低。
目标是提升试用注册率。
页面路径是 app/landing/page.tsx。
请输出：问题清单、优先级、A/B 测试假设和改动建议。
```

```text
使用 copywriting skill，基于 .agents/product-marketing.md 重写首页 hero 区。
要求：面向 B2B SaaS 创始人，语气直接，不要夸张词。
输出 5 个版本，并说明每个版本的定位差异。
```

```text
使用 analytics skill，帮我设计注册漏斗事件。
技术栈是 Next.js + GA4 + GTM。
输出事件命名、参数、触发位置和验证方法。
```

### 3. 让 agent 产出可落地文件

建议要求 agent 把结果写入项目文件，例如：

```text
把最终方案保存到 docs/marketing/cro-audit.md。
如果涉及代码修改，先列出计划，再改动。
```

## 按场景选择 skill

### 转化优化

- `cro`：网页和表单转化率优化
- `signup`：注册、试用、开户流程优化
- `onboarding`：新用户激活和首次价值达成
- `popups`：弹窗、浮层、横幅转化
- `paywalls`：付费墙、升级页、功能门槛

### 内容与文案

- `copywriting`：从零写营销页、落地页、首页文案
- `copy-editing`：改写、压缩、润色已有文案
- `content-strategy`：内容策略、主题集群、编辑日历
- `emails`：生命周期邮件、欢迎邮件、自动化邮件流
- `cold-email`：B2B 冷启动外呼邮件和跟进序列
- `social`：社媒内容、复用、监听和互动
- `image` / `video`：AI 图片与视频营销素材

### SEO 与发现

- `seo-audit`：技术 SEO、站内 SEO、流量下跌诊断
- `ai-seo`：AI 搜索、LLM 引用、AEO / GEO / LLMO
- `programmatic-seo`：批量模板页、目录页、城市页
- `site-architecture`：网站结构、导航、URL 和内链
- `schema`：结构化数据、JSON-LD、富摘要
- `aso`：App Store / Google Play 优化

### 广告、测试与数据

- `ads`：Google、Meta、LinkedIn 等广告投放策略
- `ad-creative`：批量广告文案和创意变体
- `ab-testing`：实验设计、样本量、假设和测试计划
- `analytics`：GA4、GTM、事件追踪、归因和测量

### 增长、留存与变现

- `marketing-plan`：完整营销计划和 AARRR 路线图
- `marketing-ideas`：增长创意库
- `offers`：报价、赠品、保证、稀缺性和价值包装
- `pricing`：定价、套餐、包装和货币化
- `churn-prevention`：取消流程、挽留、催付和流失恢复
- `referrals`：推荐、联盟、口碑增长
- `free-tools`：免费工具、计算器、SEO 型获客工具
- `lead-magnets`：白皮书、模板、清单等线索磁铁

### 销售、RevOps 与获客

- `revops`：线索生命周期、评分、路由、CRM 自动化
- `sales-enablement`：销售材料、话术、异议处理、演示脚本
- `prospecting`：找潜客、建名单、线索评分
- `competitor-profiling`：竞品档案和竞争情报
- `competitors`：竞品对比页、alternative 页、battle card
- `public-relations`：媒体关系、新闻劫持、记者 pitch
- `directory-submissions`：Product Hunt、G2、AI 目录提交
- `co-marketing`：联合营销和合作伙伴活动
- `community-marketing`：社区增长、品牌倡导者、用户社区

## 工具集成怎么用

`tools/REGISTRY.md` 是工具索引。它告诉 agent 哪些营销工具有 API、MCP、CLI 或 SDK。

常见组合：

- 数据分析：`ga4`、`mixpanel`、`amplitude`、`posthog`
- SEO：`google-search-console`、`semrush`、`ahrefs`、`dataforseo`
- 广告：`google-ads`、`meta-ads`、`linkedin-ads`、`tiktok-ads`
- 邮件：`mailchimp`、`customer-io`、`sendgrid`、`resend`
- 销售和线索：`apollo`、`clay`、`clearbit`、`zoominfo`
- 自动化：`zapier`、`composio`

使用时可以这样说：

```text
使用 analytics skill，并参考 tools/REGISTRY.md。
我现在用 GA4 和 GTM，帮我设计并验证注册漏斗追踪方案。
```

不要把 API key、密码或客户隐私数据直接写进 skill 文件。凭据应放在环境变量、本地密钥管理器或平台自己的安全配置中。

## Windows 使用注意事项

Windows 用户建议优先使用 PowerShell 或 WSL 二选一，不要在同一个命令里混用路径风格。

PowerShell 路径示例：

```powershell
C:\Users\<你的用户名>\.agents\skills
node tools\clis\ga4.js
```

WSL / Git Bash 路径示例：

```bash
/mnt/c/Users/<你的用户名>/.agents/skills
node tools/clis/ga4.js
```

运行 Node.js CLI 前先确认 Node 版本：

```powershell
node --version
```

建议使用 Node.js 18 或更高版本。

## 维护和更新

更新仓库：

```bash
git pull
```

重新安装全部 skills：

```bash
npx skills add ZijieZh/marketingskills
```

如果从 v1.x 升级到 v2.x，注意旧 skill 名称会残留。英文 README 的 `Upgrading from v1.x to v2.0` 部分列出了完整旧名到新名的迁移表。

## 项目分析

这个项目的强项：

- 覆盖面完整：从定位、文案、SEO、广告、数据、留存到销售赋能，基本覆盖 SaaS 和软件产品的主流营销工作。
- 上下文设计清晰：`product-marketing` 是中心上下文，其他 skill 围绕它协同，避免碎片化执行。
- 知识分层合理：`SKILL.md` 保持工作流简洁，细节放到 `references/`，适合 agent 按需加载。
- 工程化程度较高：有 eval、版本记录、插件 manifest、工具 registry 和 CLI，超出普通提示词库。

需要注意的地方：

- 默认语境偏海外 SaaS / 创业公司营销，中国本土平台、电商和品牌投放场景需要二次本地化。
- 很多工具集成依赖外部账号、API key 或 MCP 配置，安装 skill 不等于已经拥有平台权限。
- skill 数量多，第一次使用时不要全都试；先从 `product-marketing`、`cro`、`copywriting`、`seo-audit`、`analytics` 五个开始。
- 部分策略类 skill 会给出强假设，落地前仍需要业务负责人用真实数据复核。

推荐的中文使用路径：

1. 先为自己的项目建立 `.agents/product-marketing.md`。
2. 选择一个最痛的业务问题，例如“首页不转化”“SEO 没流量”“注册后不激活”。
3. 调用一个主 skill，不要一次混用太多。
4. 要求 agent 输出可验证的计划、实验或文件改动。
5. 把有效产物沉淀回项目文档，形成自己的营销操作系统。

## 继续阅读

- 英文 README：[README.md](README.md)
- 贡献指南：[CONTRIBUTING.md](CONTRIBUTING.md)
- 版本记录：[VERSIONS.md](VERSIONS.md)
- 工具注册表：[tools/REGISTRY.md](tools/REGISTRY.md)
- Agent 维护规则：[AGENTS.md](AGENTS.md)
