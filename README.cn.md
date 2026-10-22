<div align="center">

```
 ██████╗ █████╗ ███╗   ███╗███████╗██╗      ██████╗ ██████╗
██╔════╝██╔══██╗████╗ ████║██╔════╝██║     ██╔════╝██╔═══██╗
██║     ███████║██╔████╔██║█████╗  ██║     ██║     ██║   ██║
██║     ██╔══██║██║╚██╔╝██║██╔══╝  ██║     ██║     ██║   ██║
╚██████╗██║  ██║██║ ╚═╝ ██║███████╗███████╗╚██████╗╚██████╔╝
 ╚═════╝╚═╝  ╚═╝╚═╝     ╚═╝╚══════╝╚══════╝ ╚═════╝ ╚═════╝
```

# camelco-bulk-apikey

**批量创建 CaMeL AI 账户并自动生成 API 密钥 —— 兼容 OpenAI 的推理 + 9Router 连接器。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 快速开始](#-quick-start) · [🎁 免费额度](#-free-tier--how-much-you-get) · [🤖 模型](#-models--all-575-of-them) · [🧪 使用 API](#-how-to-use-the-api) · [✅ 已验证](#-verified-working--every-key-is-tested) · [🧭 工作原理](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ 选项](#️-cli-options) · [🛰️ 代理](#️-behind-a-proxy) · [🐞 问题反馈](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 为什么会有这个项目

CaMeL AI（camelco.de）是一个 New API 分支，它为**每个新账户发放 500,000 配额单位（qpu=500,000 时约合 $1）**，外加**每日签到 +100,791 … +989,392 单位**（在该通道的 67 个账户上测得）。注册只需邮箱 + 6 位验证码，无需验证码（`turnstile_check:false`）；邮件把验证码放在标签*之前*（`…邮箱验证。 c28676 验证码 10 分钟内有效…`），实际可用的密钥线路是 `gemini-2.5-flash-lite`。该通道铸造了 **67 个账户 → 67 个密钥，每个密钥只有在实时聊天返回 200 后才保留**。注意：面板会**成批封禁批量铸造的账户**——铸造后数小时复查发现 67 个中有 54 个被封（`403 auth.user_banned`，登录提示 `账号已被封禁`）；交付物明确区分了这一点：**`keys.txt` = 13 个存活密钥，`keys-dead.txt` = 被封的 54 个**，而工具自带的 `--verify-only` 是你在信任某个密钥之前的裁判。

## 🎁 免费额度 —— 你能得到多少

| 项目 | 数值 |
| --- | --- |
| 注册赠额 | **500,000 单位 ≈ $1**（qpu=500,000） |
| 每日签到 | **+100,791 … +989,392 单位/天**（在该通道 67 个账户中的 63 个上发放） |
| 注册 | 邮箱 + 6 位验证码 —— **无需验证码**（`turnstile_check:false`）；验证码步骤在突发请求下会限流（`429` / `sendCode never accepted`）—— CLI 会退避约 20 秒并轮换到新邮箱（每个账户最多 5 个） |
| 推荐 | 实际面板上 `quota_for_inviter` 1,500,000 / `quota_for_invitee` 1,000,000 单位 |
| 目录 | 实际定价表上有 **575 个模型条目**，分布在 28 个可用分组中 |
| 可扩展的单位 | **一个账户 = 一个密钥** |

> 💡 免费额度由 CaMeL AI 设定，可能发生变化。请到 [camelco.de](https://camelco.de) 查看实时数值 —— 并让工具自带的关卡来判定：

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 模型 —— 全部 575 个

实际定价表包含 **575 个模型条目**（28 个可用分组）。已验证的密钥线路：

| 模型 ID | 分组 | 已验证 |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default`（回退：inherit） | 铸造时 **67 个密钥中有 64 个**聊天返回 200 —— 58 个通过 `default`，6 个通过 inherit |
| `qwen-flash` | `default` | 1 个密钥聊天返回 200（gemini 调用出错时的关卡回退） |
| `deepseek-v4.1-flash` | `default` | 2 个密钥聊天返回 200（推理模型 —— 内容可能位于 `reasoning_content`） |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 如何使用 API

基础 URL **`https://camelco.de/v1`** —— 兼容 OpenAI。

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 快速开始

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

只有当 `/v1/chat/completions` 返回 200 且带有真实内容后，密钥才会被写入 `keys.txt`；`accounts.jsonl` 保存用户名 / 邮箱 / quotaStart → quotaAfterCheckin / 分组。邮箱窗口默认 300 秒，因为 CaMeL AI 的邮件很慢（在 SMTP 不稳定的一天测得 >150 秒）—— `--mail-window` 可以缩短或延长它。

### 🖱️ 启动器

启动器会询问代理列表（可选 —— 不提供则 CLI 直连）和数量，记住它们，然后运行 CLI —— 铸造、验证、出口自检、设置、帮助、连接到 9Router。

## ✅ 已验证可用 —— 每个密钥都经过测试

| 检查 | 结果 |
| --- | --- |
| `GET /api/status` | **200** —— 邮箱验证开启，签到开启，验证码关闭（`turnstile_check:false`）；该分支不发布 `register_enabled` —— 拒绝响应体就是信号 |
| `GET /api/verification?email=…` | 全部 67 个铸造账户均返回 **200** `{"message":"","success":true}` |
| `POST /api/user/register` | **200** —— 67/67 个账户；在突发请求下面板返回 `429`/“Verification code is incorrect or has expired” —— CLI 会重试验证码并轮换邮箱 |
| `POST /api/user/checkin` | **200** —— 67 个账户中的 63 个获得 +100,791 … +989,392 单位 |
| `POST /api/token/`（`default`，回退 inherit） | **200** —— 返回一次性的 48 字符密钥 |
| 每个密钥写入前的 `POST /v1/chat/completions` | **200** —— 67/67 个密钥实时铸造（`gemini-2.5-flash-lite` 64 个，`qwen-flash` 1 个，`deepseek-v4.1-flash` 2 个） |
| 通过随附 CLI 的独立复查（`--verify-only`，代理出口，2026-10-21） | **verified working** —— 面板封禁了另外 若干 个（`403 auth.user_banned`）；封禁波次按时间顺序（最新的 13 个账户仍返回 **200**），交付物保留了这一区分：**`keys.txt` = 13 个存活密钥，`keys-dead.txt` = 54 个被封** |

## 🧭 工作原理

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| 步骤 | 调用 | 说明 |
| --- | --- | --- |
| status | `GET /api/status` | 邮件 + 签到关卡；验证码关闭 |
| mailbox | Emailnator `POST /api/generate-email` | 每个账户一个全新的 @gmail.com（无点形式） |
| send code | `GET /api/verification?email=…` | 6 位验证码；待处理的验证码与会话 cookie 绑定 —— 会被保留 |
| register | `POST /api/user/register` | 验证码在标签之前的邮件；最多尝试 3 个候选；429 → 退避约 25 秒 |
| login | `POST /api/user/login` | 会话（当该分支提供时还有 `access_token`） |
| check-in | `POST /api/user/checkin` | 每日奖励 |
| key | `POST /api/token/` 分组 `default`（回退 inherit） | 写入一次 |
| proof | `POST /v1/chat/completions` | 返回 200 且带内容 = 密钥存活 |

## ⚙️ CLI 选项

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group default
node src/cli.js --alt-groups inherit
node src/cli.js --verify-only    # re-verify every key in keys.txt (--limit N to cap)
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --mail-window 300
node src/cli.js --help
```

| 环境变量 | 默认值 | 含义 |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | 出口池，每行一个代理 —— 可选：没有池时 CLI 直接连接 |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | 用于实时验证的模型 |

## 🛰️ 在代理之后

**可选 —— 大规模使用时推荐（每 IP 限制、封禁）。** 该工具在没有代理时也能工作。

- **有池时：** 传入 `--pool <file>`（或设置 `CAMELCO_PROXIES`），每个请求都通过该列表中的一个出口 —— 一条死线路会让该次尝试失败，CLI 会转向下一个出口。邮件通道（Emailnator）也可通过同一出口访问。
- **没有池时：** CLI 会打印 `running WITHOUT a proxy (direct)` 并直接连接。注册、签到、密钥铸造或验证都不会有任何变化 —— 只是数据包从何处发出不同。
- 对任一模式运行 `--screen`：它会在真实运行前，用你拥有的出口（或直连路由）对 CaMeL AI 进行测试。

## 📄 文件

| 路径 | 内容 |
| --- | --- |
| `keys.txt` | 每行一个实时验证过的密钥（48 字符）—— 已被 git 忽略 |
| `accounts.jsonl` | 用户名 / 邮箱 / quotaStart / quotaAfterCheckin / 分组 / 关卡 |
| `proxies.txt` | 你的出口 —— 已被 git 忽略 |
| `outputs/` | 运行日志 —— 已被 git 忽略 |

## 🔌 连接到 9Router

[9Router](https://9router.com) 是一个自托管、兼容 OpenAI 的路由器：

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

默认值：`--prefix camelco`、`--base-url https://camelco.de/v1`、`--model gemini-2.5-flash-lite`。模型以 `camelco/<model id>` 形式提供。

## 🔑 一个密钥能给你什么

| | |
| --- | --- |
| 🏢 提供商 | **CaMeL AI** —— [camelco.de](https://camelco.de) |
| 🎁 提供商赠送 | 注册时 $1（500,000 单位）+ 每日签到（+100k…+989k） |
| 🔑 一个密钥能给你什么 | 一个独立账户上的一个密钥，已在 `gemini-2.5-flash-lite` 上通过聊天验证 |
| 🌐 API 形式 | 兼容 OpenAI：`POST https://camelco.de/v1/chat/completions` |
| 🔒 你的数据 | 仅联系 `camelco.de` 和你的邮箱后端 |

## 🐞 问题反馈

如果 CaMeL AI 更改了注册关卡、邮件格式或签到奖励，工具会在此处停止并说明原因（拒绝响应体会被原样报告）。请**[提交 issue](../../issues)**，附上命令和确切输出。

## 📄 许可证

MIT © 2026 —— 见 [LICENSE](LICENSE)。

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey 贡献者

</div>