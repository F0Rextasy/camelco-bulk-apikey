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

**Bulk-create CaMeL AI accounts with auto-generated API keys — OpenAI-compatible inference + 9Router connector.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Quick start](#-quick-start) · [🎁 Free tier](#-free-tier--how-much-you-get) · [🤖 Models](#-models--all-575-of-them) · [🧪 Use the API](#-how-to-use-the-api) · [✅ Verified](#-verified-working--every-key-is-tested) · [🧭 How it works](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Options](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Issues](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Why this exists

CaMeL AI (camelco.de) is a New API fork that hands out **500,000 quota units (≈ $1 at qpu=500,000) to every new account** plus a **daily check-in of +100,791 … +989,392 units** (measured across the lane's 67 accounts). Registration is e-mail + 6-char code, captcha-free (`turnstile_check:false`); the mails put the code *before* its label (`…邮箱验证。 c28676 验证码 10 分钟内有效…`), and the live key line is `gemini-2.5-flash-lite`. The lane minted **67 accounts → 67 keys, every key kept only after a live chat-200**. Heads-up: the panel **bans bulk-minted accounts in waves** — a re-check hours after the mint found 54 of the 67 banned (`403 auth.user_banned`, login `账号已被封禁`); the checkout ships that split explicitly: **`keys.txt` = the 13 live keys, `keys-dead.txt` = the banned 54**, and the tool's own `--verify-only` is the judge before you trust a key.

## 🎁 Free tier — how much you get

| What | Value |
| --- | --- |
| Signup credit | **500,000 units ≈ $1** (qpu=500,000) |
| Daily check-in | **+100,791 … +989,392 units/day** (paid on 63 of the 67 lane accounts) |
| Signup | e-mail + 6-char code — **no captcha** (`turnstile_check:false`); the code step throttles under bursts (`429` / `sendCode never accepted`) — the CLI back-spaces ~20 s and rotates to a fresh mailbox (up to 5 per account) |
| Referrals | `quota_for_inviter` 1,500,000 / `quota_for_invitee` 1,000,000 units on the live panel |
| Catalogue | **575 model entries** on the live pricing sheet across 28 usable groups |
| The unit that scales | **one account = one key** |

> 💡 The free tier is set by CaMeL AI and can change. Check the live numbers at [camelco.de](https://camelco.de) — and let the tool's own gate decide:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Models — all 575 of them

The live pricing sheet carries **575 model entries** (28 usable groups). The verified key line:

| Model ID | Group | Verified |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (fallback: inherit) | **64 of 67 keys** chat-200 at mint — 58 via `default`, 6 via inherit |
| `qwen-flash` | `default` | 1 key chat-200 (gate fallback when the gemini call hiccuped) |
| `deepseek-v4.1-flash` | `default` | 2 keys chat-200 (reasoning model — content may sit in `reasoning_content`) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 How to use the API

Base URL **`https://camelco.de/v1`** — OpenAI-compatible.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Quick start

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings
# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

A key is written to `keys.txt` **only** after `/v1/chat/completions` answers 200 with real content; `accounts.jsonl` keeps username / e-mail / quotaStart → quotaAfterCheckin / group. The mailbox window defaults to 300 s because CaMeL AI mail is slow (measured >150 s on a flaky SMTP day) — `--mail-window` shortens or widens it.

### 🖱️ The launcher

The launcher asks for the proxy list (optional — without one the CLI runs direct) and count, remembers them, and runs the CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Verified working — every key is tested

| Check | Result |
| --- | --- |
| `GET /api/status` | **200** — e-mail verification on, check-in on, captcha off (`turnstile_check:false`); this fork does not publish `register_enabled` — the refusal body is the signal |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` on all 67 minted accounts |
| `POST /api/user/register` | **200** — 67/67 accounts; under bursts the panel answers `429`/“Verification code is incorrect or has expired” — the CLI retries codes and rotates mailboxes |
| `POST /api/user/checkin` | **200** — +100,791 … +989,392 units on 63 of 67 accounts |
| `POST /api/token/` (`default`, fallback inherit) | **200** — 48-char keys returned once |
| `POST /v1/chat/completions` per key before it is written | **200** — 67/67 keys minted live (`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| independent re-check via the shipped CLI (`--verify-only`, WARP exit, 2026-10-21) | **13/67 live** — the panel banned the other 54 (`403 auth.user_banned`); the wave is chronological (the newest 13 accounts still answered **200**), and the checkout keeps the split: **`keys.txt` = 13 live keys, `keys-dead.txt` = 54 banned** |

## 🧭 How it works

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| Step | Call | Notes |
| --- | --- | --- |
| status | `GET /api/status` | mail + check-in gates; captcha off |
| mailbox | Emailnator `POST /api/generate-email` | one fresh @gmail.com per account (dot-free form) |
| send code | `GET /api/verification?email=…` | 6-char code; the pending code is tied to the session cookie — it is kept |
| register | `POST /api/user/register` | code-before-label mails; up to 3 candidates walked; 429 → ~25 s back-off |
| login | `POST /api/user/login` | session (+ `access_token` when the fork ships one) |
| check-in | `POST /api/user/checkin` | the daily bonus |
| key | `POST /api/token/` group `default` (fallback inherit) | written once |
| proof | `POST /v1/chat/completions` | 200 with content = the key is live |

## ⚙️ CLI options

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

| Env | Default | Meaning |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | exit pool, one line per proxy — optional: without a pool the CLI connects directly |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | model for the live verification |

## 🛰️ Behind a proxy

**Optional — recommended at scale (per-IP limits, bans).** The tool works without a proxy too.

- **With a pool:** pass `--pool <file>` (or set `CAMELCO_PROXIES`) and every request goes through an exit from that list — a dead line fails that attempt and the CLI moves to the next exit. The mail rail (Emailnator) is reachable through the same exit.
- **Without a pool:** the CLI prints `running WITHOUT a proxy (direct)` and connects directly. Nothing about sign-up, check-in, key minting or verification changes — only where the packets leave from.
- Run `--screen` against either mode: it tests the exits you have (or the direct route) against CaMeL AI before a real run.

## 📄 Files

| Path | Contents |
| --- | --- |
| `keys.txt` | one live-verified key per line (48-char) — git-ignored |
| `accounts.jsonl` | username / e-mail / quotaStart / quotaAfterCheckin / group / gate |
| `proxies.txt` | your exits — git-ignored |
| `outputs/` | run logs — git-ignored |

## 🔌 Connect to 9Router

[9Router](https://9router.com) is a self-hosted, OpenAI-compatible router:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Defaults: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Models are served as `camelco/<model id>`.

## 🔑 What one key gets you

| | |
| --- | --- |
| 🏢 Provider | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 What the provider gives | $1 (500,000 units) on signup + daily check-in (+100k…+989k) |
| 🔑 What one key gets you | one key on its own account, chat-verified on `gemini-2.5-flash-lite` |
| 🌐 API shape | OpenAI-compatible: `POST https://camelco.de/v1/chat/completions` |
| 🔒 Your data | only `camelco.de` and your inbox backend are contacted |

## 🐞 Issues

If CaMeL AI changes its register gates, the mail shape or the check-in reward, the tool stops there and says so (the refusal body is reported verbatim). **[Open an issue](../../issues)** with the command and the exact output.

## 📄 License

MIT © 2026 — see [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>
