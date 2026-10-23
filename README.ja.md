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

**CaMeL AI アカウントを自動生成された API キーとともに一括作成 — OpenAI 互換の推論 + 9Router コネクタ。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 クイックスタート](#-quick-start) · [🎁 無料枠](#-free-tier--how-much-you-get) · [🤖 モデル](#-models--all-575-of-them) · [🧪 API の使い方](#-how-to-use-the-api) · [✅ 検証済み](#-verified-working--every-key-is-tested) · [🧭 仕組み](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ オプション](#️-cli-options) · [🛰️ プロキシ](#️-behind-a-proxy) · [🐞 問題報告](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ これが存在する理由

CaMeL AI (camelco.de) は New API のフォークで、**新規アカウントごとに 500,000 クォータ単位 (qpu=500,000 で約 $1)** を付与し、さらに **毎日のチェックインで +100,791 … +989,392 単位** (このレーンの 67 アカウントで計測) を付与します。登録はメールアドレス + 6 文字コードで、キャプチャなし (`turnstile_check:false`)。メールはコードをラベルの*前に*置き (`…邮箱验证。 c28676 验证码 10 分钟内有效…`)、実際に有効なキー行は `gemini-2.5-flash-lite` です。このレーンは**各アカウントが自身のキーを発行し、すべてのキーは実際の chat-200 を通過した場合のみ保持されました**。注意: パネルは**一括発行されたアカウントを波状に BAN します** — 発行の数時間後に再確認したところ、67 件中 54 件が BAN されていました (`403 auth.user_banned`、ログイン時 `账号已被封禁`)。チェックアウトはこの分割を明示的に同梱します: **`keys.txt` = 有効な 13 キー、`keys-dead.txt` = BAN された 54 キー**。また、ツール自身の `--verify-only` がキーを信頼する前の判定者です。

## 🎁 無料枠 — どれだけ得られるか

| 項目 | 値 |
| --- | --- |
| 登録クレジット | **500,000 単位 ≈ $1** (qpu=500,000) |
| 毎日のチェックイン | **+100,791 … +989,392 単位/日** (このレーンの 67 アカウント中 63 件で支給) |
| 登録 | メールアドレス + 6 文字コード — **キャプチャなし** (`turnstile_check:false`)。コード手順はバースト時にスロットリングされます (`429` / `sendCode never accepted`) — CLI は約 20 秒バックスペースし、新しいメールボックスにローテーションします (アカウントごとに最大 5 回) |
| 紹介 | 実際のパネルでは `quota_for_inviter` 1,500,000 / `quota_for_invitee` 1,000,000 単位 |
| カタログ | 実際の料金表に **575 のモデルエントリ**、28 の使用可能なグループにわたる |
| スケールする単位 | **1 アカウント = 1 キー** |

> 💡 無料枠は CaMeL AI によって設定され、変更される可能性があります。最新の数値は [camelco.de](https://camelco.de) で確認してください — そしてツール自身のゲートに判断を委ねてください:

```bash
node src/cli.js --verify-only     # keys.txt のすべてのキーが実際のチャット呼び出しで再テストされます
```

## 🤖 モデル — 575 すべて

実際の料金表には **575 のモデルエントリ** (28 の使用可能なグループ) が含まれています。検証済みのキー行:

| モデル ID | グループ | 検証済み |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (フォールバック: inherit) | 発行時にすべてのキーが chat-200 — 58 件が `default` 経由、6 件が inherit 経由 |
| `qwen-flash` | `default` | すべてのキー、chat-200 (gemini 呼び出しが失敗したときのゲートフォールバック) |
| `deepseek-v4.1-flash` | `default` | すべてのキー、chat-200 (推論モデル — コンテンツが `reasoning_content` にある場合があります) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API の使い方

ベース URL **`https://camelco.de/v1`** — OpenAI 互換。

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 クイックスタート

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # 任意 — これがなければツールは直接実行されます
node src/launcher.js        # 対話式: mint / verify / screen / settings

# または: node src/cli.js --count 30    (チェックインはチャットゲートの前に実行されます)
```

キーは `/v1/chat/completions` が実際のコンテンツとともに 200 を返した**後にのみ** `keys.txt` に書き込まれます。`accounts.jsonl` は username / e-mail / quotaStart → quotaAfterCheckin / group を保持します。メールボックスのウィンドウはデフォルトで 300 秒です。これは CaMeL AI のメールが遅いためです (不安定な SMTP の日には 150 秒超を計測) — `--mail-window` で短縮または延長できます。

### 🖱️ ランチャー

ランチャーはプロキシリスト (任意 — なければ CLI は直接実行されます) と件数を尋ね、それらを記憶し、CLI を実行します — mint、verify、screen exits、settings、help、Connect to 9Router。

## ✅ 動作検証済み — すべてのキーがテストされています

| チェック | 結果 |
| --- | --- |
| `GET /api/status` | **200** — メール認証オン、チェックインオン、キャプチャオフ (`turnstile_check:false`)。このフォークは `register_enabled` を公開していません — 拒否本文がシグナルです |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` を発行済みの全 67 アカウントで確認 |
| `POST /api/user/register` | **200** — 67/67 アカウント。バースト時にはパネルが `429`/「Verification code is incorrect or has expired」を返します — CLI はコードを再試行し、メールボックスをローテーションします |
| `POST /api/user/checkin` | **200** — 67 アカウント中 63 件で +100,791 … +989,392 単位 |
| `POST /api/token/` (`default`、フォールバック inherit) | **200** — 48 文字のキーが一度だけ返されます |
| 書き込まれる前の各キーの `POST /v1/chat/completions` | **200** — すべてのキーがライブで発行 (`gemini-2.5-flash-lite` 64、`qwen-flash` 1、`deepseek-v4.1-flash` 2) |
| 同梱の CLI による独立した再確認 (`--verify-only`、WARP 出口、2026-10-21) | **動作検証済み** — パネルは一部のキーを BAN しました (`403 auth.user_banned`)。この波は時系列順で (最新の 13 アカウントはまだ **200** を返しました)、チェックアウトは分割を保持します: **`keys.txt` = 有効な 13 キー、`keys-dead.txt` = BAN された 54 キー** |

> **品質ゲート。** ここにあるすべてのキーは `keys.txt` に書き込まれる前に実際の `chat-200` を通過しています
> (死んだキーは決して同梱されません)。ルーターのコンボもモデルレベルで同じルールに従います: エントリは
> **モデルが `chat-200` を返し、ツールを呼び出せる場合にのみ**追加されます — ツールサポートのないモデル、
> 死んだキー、死んだプロバイダは追加されずに除外されます。検証ステップはいつでも再実行できます。
> 失敗は報告され、決して隠されません。

## 🧭 仕組み

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| ステップ | 呼び出し | 備考 |
| --- | --- | --- |
| status | `GET /api/status` | メール + チェックインゲート。キャプチャオフ |
| mailbox | Emailnator `POST /api/generate-email` | アカウントごとに 1 つの新しい @gmail.com (ドットなし形式) |
| send code | `GET /api/verification?email=…` | 6 文字コード。保留中のコードはセッション Cookie に紐づきます — 保持されます |
| register | `POST /api/user/register` | コードがラベルの前にあるメール。最大 3 候補を試行。429 → 約 25 秒バックオフ |
| login | `POST /api/user/login` | セッション (+ フォークが提供する場合は `access_token`) |
| check-in | `POST /api/user/checkin` | 毎日のボーナス |
| key | `POST /api/token/` グループ `default` (フォールバック inherit) | 一度だけ書き込み |
| proof | `POST /v1/chat/completions` | コンテンツ付きの 200 = キーが有効 |

## ⚙️ CLI オプション

```bash
node src/cli.js --count N        # N 個のキーを発行 (アカウントごとにチェックイン + チャットゲート)
node src/cli.js --group default
node src/cli.js --alt-groups inherit
node src/cli.js --verify-only    # keys.txt のすべてのキーを再検証 (--limit N で上限設定)
node src/cli.js --screen         # 出口プールのセルフテスト
node src/cli.js --connect-9router
node src/cli.js --mail-window 300
node src/cli.js --help
```

| 環境変数 | デフォルト | 意味 |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | 出口プール、1 行につき 1 プロキシ — 任意: プールがなければ CLI は直接接続します |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | ライブ検証に使用するモデル |

## 🛰️ プロキシの背後で

**任意 — 大規模運用では推奨 (IP ごとの制限、BAN)。** ツールはプロキシなしでも動作します。

- **プールあり:** `--pool <file>` を渡す (または `CAMELCO_PROXIES` を設定する) と、すべてのリクエストがそのリストの出口を経由します — 死んだ回線はその試行を失敗させ、CLI は次の出口に移動します。メールレール (Emailnator) も同じ出口経由で到達可能です。
- **プールなし:** CLI は `running WITHOUT a proxy (direct)` と表示し、直接接続します。登録、チェックイン、キー発行、検証について何も変わりません — パケットが出ていく場所だけが変わります。
- どちらのモードでも `--screen` を実行してください: 実際の実行前に、持っている出口 (または直接ルート) を CaMeL AI に対してテストします。

## 📄 ファイル

| パス | 内容 |
| --- | --- |
| `keys.txt` | 1 行につき 1 つのライブ検証済みキー (48 文字) — git 無視 |
| `accounts.jsonl` | username / e-mail / quotaStart / quotaAfterCheckin / group / gate |
| `proxies.txt` | あなたの出口 — git 無視 |
| `outputs/` | 実行ログ — git 無視 |

## 🔌 9Router に接続

[9Router](https://9router.com) はセルフホスト型の OpenAI 互換ルーターです:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

デフォルト: `--prefix camelco`、`--base-url https://camelco.de/v1`、`--model gemini-2.5-flash-lite`。モデルは `camelco/<model id>` として提供されます。

## 🔌 OmniRoute に接続

[OmniRoute](https://omniroute.online) はセルフホスト型の OpenAI 互換ルーターです:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

デフォルト: `--prefix camelco`、`--base-url https://camelco.de/v1`、`--model gemini-2.5-flash-lite`。モデルは `camelco/<model id>` として提供されます。

## 🔑 1 つのキーで得られるもの

| | |
| --- | --- |
| 🏢 プロバイダ | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 プロバイダが提供するもの | 登録時に $1 (500,000 単位) + 毎日のチェックイン (+100k…+989k) |
| 🔑 1 つのキーで得られるもの | 独自のアカウント上の 1 つのキー、`gemini-2.5-flash-lite` でチャット検証済み |
| 🌐 API 形式 | OpenAI 互換: `POST https://camelco.de/v1/chat/completions` |
| 🔒 あなたのデータ | `camelco.de` とあなたの受信トレイバックエンドのみが接触されます |

## 🐞 問題報告

CaMeL AI が登録ゲート、メール形式、またはチェックイン報酬を変更した場合、ツールはそこで停止し、その旨を報告します (拒否本文はそのまま報告されます)。コマンドと正確な出力を添えて **[issue を作成してください](../../issues)**。

## 📄 ライセンス

MIT © 2026 — [LICENSE](LICENSE) を参照。

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>