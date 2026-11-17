<p align="center">
  <img src="wb2api-preview.png" alt="WorkBuddy2API Panel" width="120">
</p>

<h1 align="center">WorkBuddy2API Panel</h1>

<p align="center">
  <b>把腾讯 CodeBuddy 账号变成 OpenAI 兼容 API 的多账号网关 · 附 Web 管理面板</b><br>
  OAuth 登录 · 账号池轮转 · 熔断冷却 · 定时签到 / 旅行 / 保活 · 成长任务一键完成
</p>

<p align="center">
  <img alt="Go" src="https://img.shields.io/badge/Go-1.22.5-00ADD8?logo=go&logoColor=white&style=flat-square">
  <img alt="API" src="https://img.shields.io/badge/API-OpenAI_Compatible-412991?style=flat-square">
  <img alt="Transport" src="https://img.shields.io/badge/Transport-SSE%20%2F%20Streaming-0DBD8B?style=flat-square">
</p>

---

> [!IMPORTANT]
> **本仓库是个人 fork**（`2430858348/workbuddy2api-panel-public`），在上游 [linguo2625469/workbuddy2api-panel](https://github.com/linguo2625469/workbuddy2api-panel) 基础上做了修复与增强。
>
> 上游已声明停止维护（2026-10-21）。本 fork 不承诺长期维护，仅记录自用中发现并修复的问题。**上游随时可能收紧反代导致不可用**；使用非官方客户端导致的账号风控 / 封禁由使用者自行承担。
>
> **仅限自用账号**（签到 / 保活 / 个人工具接入），禁止批量小号分发额度、二次打包或收费售卖。

### 本分支改动（相对上游）

| 改动 | 说明 |
|---|---|
| 解除 `enterprise_switch` 误判门控 | 实测新注册账号同样带该 JWT 声明，旅行领奖 / 成长任务 / 连登均正常入账；原门控导致这些功能被静默跳过。现仅对真正的企业版账号门控 |
| 启动补跑（`catchUpMissed`） | 开机时若当天任务时点已过，立即补跑一次，解决漏签 |
| `-web-session` 会话模式 | 桌面快捷方式启动的服务在面板页面关闭后自动停止；开机自启保持常驻 |
| 面板存储降级 | `localStorage` 被禁用时退回内存，避免 `SecurityError` 导致面板空白 |
| 自定义图标 | 面板 favicon 与桌面快捷方式统一为定制图标 |
| 开机自启 | 启动文件夹快捷方式，登录后自动拉起服务 |

> 完整技术文档（配置字段、API 端点、实现原理、FAQ 等）见 [上游仓库](https://github.com/linguo2625469/workbuddy2api-panel)。

---

## 快速开始（Windows 单文件）

项目目录：`D:\software\workbuddy\workbuddy2api-panel\`

**1. 启动**

双击桌面的 **WorkBuddy2API** 图标——会自动拉起服务并打开面板。

- 服务未运行时：最小化启动，等 3 秒后开网页
- 服务已在运行时：直接开网页，不会重复启动

**2. 添加账号**

面板打开 `http://127.0.0.1:7863/panel/`，输入 `config.json` 里的 `api_key`，点右上角「**添加账号**」，在浏览器完成授权即可（热加载，无需重启）。

**3. 配置客户端**

```
Base URL:  http://127.0.0.1:7863/v1
API Key:   <config.json 中的 api_key>
Model:     cn:hy4flash-free（免费模型，适合测试）
```

**4. 定时任务**（服务运行时自动执行）

| 时间 | 任务 |
|---|---|
| 9:00 / 21:00 | 签到、猫猫旅行 |
| 10:00 | 活跃上报 |
| 22:00 / 23:00 | token 保活 / 夜猫子 |
| 每天 1:00 | 成长任务队列 |
| 每 5 分钟 | 余额刷新 |

开机晚了会自动补跑当天已过的任务。

---

## 使用声明（必读）

本项目开发初衷只有一个：**方便个人管理自己的 CodeBuddy 账号**——自动签到、保活、给自己的本地工具提供一个 OpenAI 兼容入口。免费、开源、按「原样」提供。

- **一切商用 / 售卖行为与本项目及作者无关**。不授权、不支持、不参与任何面向公众的 API 售卖、账号池出租、卡密收费分发。
- **批量注册与转售接口配额违反目标平台服务条款**。请勿消耗社区的善意。
- **请勿购买任何「收费版」「卡密版」「公益中转版」**。此类分发无法审计，存在被植入后门、窃取 `auths/` 中明文凭证的风险。
- 本项目基于 **MIT 协议**开源，允许自由使用与修改源码；但不赋予以本项目名义售卖、捆绑分发或要求作者背书的权利。再分发时请保留原始出处 `https://github.com/Sliverkiss/workbuddy2api`。

## 免责声明

本项目仅供学习和研究使用。使用者需遵守 CodeBuddy 服务条款，自行承担使用风险（包括账号封禁、条款违约等）。作者不对任何因使用本项目产生的直接或间接损失负责。

## License

[MIT License](LICENSE)
