# Skill 是什么：给新手看的真实用例讲解

一句话：`Skill` 是把一类任务的经验、流程、工具、模板和验收标准固化下来，让 Agent 下次遇到同类任务时能稳定复用。

如果说大模型是“脑”，MCP 是“连接外部系统的插线板”，Agent 是“会执行任务的人”，那么 Skill 就是“这个人做某类任务时必须遵守的作业指导书”。

## 1. 为什么需要 Skill

新手刚用 AI 时，通常会这样做：

```text
帮我写一篇商品文案。
帮我做一张图。
帮我分析一下报错。
```

这种用法能解决一次问题，但每次都要重新解释背景、规则、格式、禁忌和验收标准。结果就是：

- 同一个任务每次输出不稳定。
- AI 容易忘记业务规则。
- 复杂任务靠临场发挥，难复盘。
- 新人、运营、技术、客服之间没有统一标准。

Skill 要解决的就是这个问题：把“这类任务应该怎么做”沉淀成可复用资产。

## 2. Skill 不是普通提示词

普通提示词通常是一段话。Skill 是一个可被 Codex 发现和加载的任务包。

一个完整 Skill 通常包含：

| 组成 | 作用 | 新手理解 |
| --- | --- | --- |
| `SKILL.md` | 核心说明文件 | 告诉 Agent 什么时候用、怎么做 |
| `name` | Skill 名称 | 例如 `imagegen` |
| `description` | 触发描述 | Agent 判断是否该用这个 Skill 的关键 |
| 工作流 | 多步骤流程 | 先查什么、再做什么、最后怎么验证 |
| `scripts/` | 可执行脚本 | 把容易出错的重复动作变成程序 |
| `references/` | 参考资料 | 业务规则、API 文档、模板说明 |
| `assets/` | 输出素材 | 品牌模板、字体、图片、PPT 模板 |
| 验收标准 | 检查结果 | 判断这次任务是否完成 |

所以 Skill 不是“高级提示词”，而是“提示词 + SOP + 工具 + 资料 + 验收标准”的组合。

## 3. Skill、Agent、MCP 的关系

```text
Skill = 怎么做
MCP = 连到哪里、能调用什么工具
Agent = 按 Skill 的方法，通过 MCP 或本地工具完成任务
```

举例：自动发布一篇电商新品文章。

- `Skill` 规定文章结构、品牌语气、SEO 规则、禁用词、人工审核流程。
- `MCP` 连接商品库、CMS、图片库、社媒排程工具。
- `Agent` 读取商品信息，生成文章，创建草稿，上传封面，等待审批，发布后回收数据。

没有 Skill，Agent 可能每次都换一种写法。没有 MCP，Agent 可能只能写文案，不能真正发布。没有 Agent，Skill 和 MCP 都只是静态说明和接口。

## 4. 真实用例一：`openai-docs`

本机真实 Skill：`C:\Users\xjf\.codex\skills\.system\openai-docs\SKILL.md`

它解决的问题：当用户问 OpenAI 产品、Codex、API、模型选择等问题时，不能靠记忆乱答，要优先查官方文档。

这个 Skill 的关键规则：

- 先判断是不是 OpenAI / Codex 相关问题。
- 对 Codex 问题优先走官方手册路径。
- 对 API、模型、参数问题优先查 OpenAI 官方文档。
- 如果查不到，才进入受控的不确定回答。
- 不允许编造价格、模型能力、API 参数、产品发布状态。

它的价值：

- 把“必须查官方来源”的规则固化了。
- 减少 AI 对快速变化产品的幻觉。
- 让回答可追溯，有来源。

对应电商启发：

如果你做跨境电商，也可以做一个 `platform-policy-docs` Skill。遇到 Amazon、TikTok Shop、Shopify、Meta Ads 的规则问题时，Agent 必须优先查官方政策，而不是凭经验回答。

## 5. 真实用例二：`imagegen`

本机真实 Skill：`C:\Users\xjf\.codex\skills\.system\imagegen\SKILL.md`

它解决的问题：用户要生成图片、改图、做透明背景、批量生成素材时，Agent 不能随便写几句提示词就结束，而要按图片生产流程执行。

这个 Skill 规定了：

- 什么时候应该生成位图，什么时候应该直接改 SVG、HTML、CSS。
- 默认用内置图片生成能力，只有特定情况才走 CLI。
- 透明背景先走色键去背流程，复杂情况才询问是否使用真透明模型路径。
- 项目要用的图片必须保存进工作目录，不能只留在临时路径。
- 输出后要检查主体、风格、构图、文字准确性、约束条件。

它的价值：

- 把图片生成从“玩一下”变成“可交付资产流程”。
- 防止图片生成后找不到文件。
- 防止透明图、项目素材、批量图出现不可控路径和不可复用问题。

电商真实落地：

可以做一个 `ecommerce-product-image` Skill：

- 输入：商品白底图、品牌色、平台尺寸、活动主题。
- 流程：生成场景图、主图、广告图、朋友圈海报。
- 规则：商品不能变形，价格不能错，Logo 不能丢，文字必须人工审。
- 输出：多尺寸 PNG/JPG，保存到商品素材库。

## 6. 真实用例三：`d2-dd373-market-diagnosis`

本机真实 Skill：`C:\Users\xjf\.codex\skills\d2-dd373-market-diagnosis\SKILL.md`

它解决的问题：当市场价格更新失败时，不能猜原因，必须先看真实运行状态。

这个 Skill 的关键规则：

- 不允许只根据旧 CSV、历史文件、配置快照推断问题。
- 必须先检查 live API、Docker 服务、平台健康状态。
- 必须打开真实配置里的 DD373 URL，不允许先猜 URL。
- 不允许一上来重启 Docker Desktop 或 WSL。
- 修完后必须跑针对性验证。

它的价值：

- 把“不要猜，先看真实运行状态”写成硬规则。
- 避免误删、误重启、误修配置。
- 让排障过程可复现、可验证。

电商真实落地：

可以做一个 `ecommerce-price-sync-diagnosis` Skill，用来处理价格、库存、ERP、店铺后台同步失败：

- 先查店铺 API 返回。
- 再查 ERP 当前库存。
- 再查同步任务日志。
- 再查平台后台是否限流或授权过期。
- 禁止直接批量改价。
- 修复后抽查 SKU、订单、库存和前台展示。

## 7. 真实用例四：`diablo4-gem-package-poster`

本机真实 Skill：`C:\Users\xjf\.codex\skills\diablo4-gem-package-poster\SKILL.md`

它解决的问题：生成面向玩家的宝石套餐海报时，不能每次重新设计版式，也不能把内部成本、利润、后台分析暴露给玩家。

这个 Skill 规定了：

- 必须确认职业、套餐名、宝石数量、价格、BD 图、英雄图。
- 必须使用固定脚本生成海报，而不是手工重画。
- 海报只展示玩家该看的信息。
- 不展示成本、利润、内部来源说明。
- 不同职业使用不同视觉风格。

它的价值：

- 把营销物料变成稳定生产线。
- 把“对外展示”和“内部分析”隔离。
- 避免运营物料泄露敏感商业信息。

电商真实落地：

可以做一个 `campaign-poster` Skill：

- 输入：活动名称、商品、价格、库存、优惠、品牌素材。
- 输出：朋友圈海报、小红书封面、直播预告图、详情页 Banner。
- 规则：不暴露成本，不显示内部毛利，不错写价格，不夸大功效。
- 脚本：按固定模板生成多尺寸图片。

## 8. 真实用例五：`pocket-codex-huawei-install`

本机真实 Skill：`C:\Users\xjf\.codex\skills\pocket-codex-huawei-install\SKILL.md`

它解决的问题：华为 / HarmonyOS 手机上安装测试 APK 时，可能出现设备特定的拦截或提示。这个 Skill 固化了一个受控测试场景下的处理流程。

这个 Skill 的关键规则：

- 先确认是不是 Pocket-Codex / RustDesk Android APK。
- 再确认设备是不是华为或 HarmonyOS。
- 手动安装前先关闭 Wi-Fi 和移动数据。
- 安装完成后再恢复联网。
- 如果失败，记录准确提示、APK 路径、包名、版本号和安装方式。
- 不把它包装成通用安全绕过建议。

它的价值：

- 把一次踩坑经验变成可复用排障流程。
- 把适用边界写清楚，避免误用。
- 把失败信息采集标准化。

电商真实落地：

可以做一个 `shop-app-install-support` Skill，用来处理导购 App、门店 App、扫码收银 App 的安装问题：

- 区分安卓、iOS、品牌机型、企业签名、版本降级。
- 不把所有问题都归因到“手机拦截”。
- 先采集系统版本、安装包、错误提示、版本号。
- 再给出设备特定处理步骤。

## 9. 电商应该优先沉淀哪些 Skill

电商团队不要一开始就做几十个 Skill。先从高频、重复、容易出错、有明确验收标准的任务做起。

优先级建议：

| Skill 名称 | 解决的问题 | 输入 | 输出 |
| --- | --- | --- | --- |
| `product-copywriting` | 商品标题、详情页、卖点 | SKU、卖点、人群、平台 | 标题、五点描述、详情页文案 |
| `seo-article-publishing` | 自动生成并发布文章 | 商品、关键词、活动 | 文章草稿、SEO 描述、社媒摘要 |
| `product-image-generation` | 商品图、广告图、海报 | 商品图、品牌规范、尺寸 | 多平台图片素材 |
| `lead-capture` | 评论、私信、表单获客 | 渠道、用户动作、活动 | 标签、线索评分、跟进任务 |
| `private-domain-onboarding` | 公域转私域后的欢迎和培育 | 来源、权益、用户标签 | 欢迎语、三天 SOP、七天 SOP |
| `customer-service-policy` | 客服和售后回复 | 订单、物流、政策、问题 | 客服回复、升级人工摘要 |
| `campaign-review` | 活动复盘 | GMV、流量、转化、ROI | 复盘报告、问题、行动项 |

## 10. 一个电商 Skill 应该怎么写

下面是一个简化版示例。

```markdown
---
name: ecommerce-product-copywriting
description: Generate ecommerce product titles, bullet points, detail-page copy, SEO descriptions, and platform-specific selling points from SKU data, audience, brand voice, and compliance constraints. Use when Codex needs to create or revise product copy for Shopify, Amazon, TikTok Shop, 小红书, 抖音, or private-domain sales materials.
---

# Ecommerce Product Copywriting

## Workflow

1. Read product facts first: SKU, material, size, price, inventory, target audience.
2. Identify platform: Shopify, Amazon, TikTok Shop, 小红书, 抖音, 私域.
3. Generate copy using the platform structure.
4. Check banned words, false claims, price consistency, and missing facts.
5. Output title, bullet points, detail-page sections, SEO description, and CTA.

## Guardrails

- Do not invent certifications, medical effects, guarantees, sales volume, or reviews.
- Do not change price, size, material, or inventory.
- If product facts are missing, mark them as missing instead of guessing.
- Keep internal margin, supplier, and cost data out of customer-facing output.

## Validation

- Product facts match source data.
- Claims are evidence-backed.
- Output matches platform format.
- CTA and links are present.
```

这个 Skill 的重点不是“帮我写文案”，而是把文案生产变成标准流程。

## 11. 判断一个任务是否值得做成 Skill

满足越多，越值得做成 Skill：

- 每周重复出现。
- 每次都要解释同样背景。
- 出错成本高。
- 有固定格式。
- 有明确验收标准。
- 需要调用固定工具或脚本。
- 需要遵守平台规则、品牌规范或安全边界。
- 需要新人也能按同一标准完成。

不适合做 Skill 的情况：

- 一次性闲聊。
- 创意探索，没有固定标准。
- 需求还没稳定。
- 流程还没跑通过。
- 规则每天都变，但没人维护。

## 12. 新手最容易误解的地方

### 误解一：Skill 越长越好

不是。Skill 应该只放 Agent 真正需要的内容。太长会浪费上下文，也会让 Agent 抓不住重点。

### 误解二：Skill 可以替代业务系统

不能。Skill 是方法，不能替代商品库、订单系统、CRM、客服系统。真正执行动作还需要 MCP、API、脚本或人工。

### 误解三：Skill 写完就永远正确

不能。Skill 是工作流资产，需要根据真实任务持续迭代。每次使用后，都应该把失败点、成功模板、检查项沉淀回 Skill。

### 误解四：Skill 是给人看的说明文

不是。Skill 是给 Agent 执行任务看的操作说明。要少讲道理，多写步骤、边界、输入、输出和验证。

## 13. 最终理解

```text
Prompt 解决一次对话。
Skill 解决一类任务。
MCP 解决连接外部系统。
Agent 负责按 Skill 调 MCP 和工具完成任务。
工作流资产化，才是 AI 真正进入业务的开始。
```

对电商来说，Skill 的价值不是“让 AI 写得更像人”，而是让内容生产、图片生产、获客承接、客服导购、私域转化和经营复盘形成稳定、可审计、可迭代的生产线。

## 来源与本机用例

核对时间：2026-06-15。

- OpenAI Codex Skills：<https://developers.openai.com/codex/skills>
- 本机 Skill：`C:\Users\xjf\.codex\skills\.system\openai-docs\SKILL.md`
- 本机 Skill：`C:\Users\xjf\.codex\skills\.system\imagegen\SKILL.md`
- 本机 Skill：`C:\Users\xjf\.codex\skills\.system\skill-creator\SKILL.md`
- 本机 Skill：`C:\Users\xjf\.codex\skills\d2-dd373-market-diagnosis\SKILL.md`
- 本机 Skill：`C:\Users\xjf\.codex\skills\diablo4-gem-package-poster\SKILL.md`
- 本机 Skill：`C:\Users\xjf\.codex\skills\pocket-codex-huawei-install\SKILL.md`
