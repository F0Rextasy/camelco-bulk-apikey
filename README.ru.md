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

**Массовое создание аккаунтов CaMeL AI с автоматически сгенерированными API-ключами — OpenAI-совместимый инференс + коннектор 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Быстрый старт](#-quick-start) · [🎁 Бесплатный тариф](#-free-tier--how-much-you-get) · [🤖 Модели](#-models--all-575-of-them) · [🧪 Использование API](#-how-to-use-the-api) · [✅ Проверено](#-verified-working--every-key-is-tested) · [🧭 Как это работает](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Опции](#️-cli-options) · [🛰️ Прокси](#️-behind-a-proxy) · [🐞 Проблемы](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Зачем это нужно

CaMeL AI (camelco.de) — это форк New API, который выдаёт **500 000 единиц квоты (≈ $1 при qpu=500 000) каждому новому аккаунту** плюс **ежедневный чек-ин на +100 791 … +989 392 единиц** (измерено на 67 аккаунтах линии). Регистрация — e-mail + 6-символьный код, без капчи (`turnstile_check:false`); в письмах код стоит *перед* своей меткой (`…邮箱验证。 c28676 验证码 10 分钟内有效…`), а рабочая строка ключа — `gemini-2.5-flash-lite`. Линия создала **67 аккаунтов → 67 ключей, каждый ключ сохранялся только после живого chat-200**. Внимание: панель **банит массово созданные аккаунты волнами** — повторная проверка через несколько часов после создания обнаружила 54 из 67 забаненных (`403 auth.user_banned`, вход `账号已被封禁`); результат явно отражает это разделение: **`keys.txt` = 13 живых ключей, `keys-dead.txt` = 54 забаненных**, а собственный `--verify-only` инструмента — это судья, прежде чем доверять ключу.

## 🎁 Бесплатный тариф — сколько вы получаете

| Что | Значение |
| --- | --- |
| Кредит при регистрации | **500 000 единиц ≈ $1** (qpu=500 000) |
| Ежедневный чек-ин | **+100 791 … +989 392 единиц/день** (начислено на 63 из 67 аккаунтов линии) |
| Регистрация | e-mail + 6-символьный код — **без капчи** (`turnstile_check:false`); шаг с кодом throttlится при всплесках (`429` / `sendCode never accepted`) — CLI отступает ~20 с и переключается на свежий почтовый ящик (до 5 на аккаунт) |
| Рефералы | `quota_for_inviter` 1 500 000 / `quota_for_invitee` 1 000 000 единиц на живой панели |
| Каталог | **575 записей моделей** в живом прайс-листе по 28 используемым группам |
| Единица, которая масштабируется | **один аккаунт = один ключ** |

> 💡 Бесплатный тариф устанавливается CaMeL AI и может измениться. Проверяйте актуальные цифры на [camelco.de](https://camelco.de) — и пусть собственный гейт инструмента решает:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Модели — все 575 из них

Живой прайс-лист содержит **575 записей моделей** (28 используемых групп). Проверенная строка ключа:

| ID модели | Группа | Проверено |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (fallback: inherit) | **64 из 67 ключей** chat-200 при создании — 58 через `default`, 6 через inherit |
| `qwen-flash` | `default` | 1 ключ chat-200 (fallback гейта, когда вызов gemini дал сбой) |
| `deepseek-v4.1-flash` | `default` | 2 ключа chat-200 (reasoning-модель — контент может находиться в `reasoning_content`) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Как использовать API

Базовый URL **`https://camelco.de/v1`** — OpenAI-совместимый.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Быстрый старт

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Ключ записывается в `keys.txt` **только** после того, как `/v1/chat/completions` ответит 200 с реальным контентом; `accounts.jsonl` хранит username / e-mail / quotaStart → quotaAfterCheckin / group. Окно почтового ящика по умолчанию 300 с, потому что почта CaMeL AI медленная (измерено >150 с в день с нестабильным SMTP) — `--mail-window` сокращает или расширяет его.

### 🖱️ Лаунчер

Лаунчер запрашивает список прокси (необязательно — без него CLI работает напрямую) и количество, запоминает их и запускает CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Проверено в работе — каждый ключ протестирован

| Проверка | Результат |
| --- | --- |
| `GET /api/status` | **200** — подтверждение e-mail включено, чек-ин включён, капча выключена (`turnstile_check:false`); этот форк не публикует `register_enabled` — тело отказа и есть сигнал |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` на всех 67 созданных аккаунтах |
| `POST /api/user/register` | **200** — 67/67 аккаунтов; при всплесках панель отвечает `429`/«Verification code is incorrect or has expired» — CLI повторяет коды и переключает почтовые ящики |
| `POST /api/user/checkin` | **200** — +100 791 … +989 392 единиц на 63 из 67 аккаунтов |
| `POST /api/token/` (`default`, fallback inherit) | **200** — 48-символьные ключи возвращаются один раз |
| `POST /v1/chat/completions` для каждого ключа перед записью | **200** — 67/67 ключей созданы вживую (`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| независимая повторная проверка через поставляемый CLI (`--verify-only`, proxy exit, 2026-10-21) | **13/67 живых** — панель забанила остальные 54 (`403 auth.user_banned`); волна хронологическая (новейшие 13 аккаунтов всё ещё отвечали **200**), и результат сохраняет разделение: **`keys.txt` = 13 живых ключей, `keys-dead.txt` = 54 забаненных** |

## 🧭 Как это работает

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| Шаг | Вызов | Примечания |
| --- | --- | --- |
| status | `GET /api/status` | гейты почты + чек-ина; капча выключена |
| mailbox | Emailnator `POST /api/generate-email` | один свежий @gmail.com на аккаунт (форма без точек) |
| send code | `GET /api/verification?email=…` | 6-символьный код; ожидающий код привязан к cookie сессии — он сохраняется |
| register | `POST /api/user/register` | письма с кодом перед меткой; перебирается до 3 кандидатов; 429 → отступ ~25 с |
| login | `POST /api/user/login` | сессия (+ `access_token`, когда форк его предоставляет) |
| check-in | `POST /api/user/checkin` | ежедневный бонус |
| key | `POST /api/token/` группа `default` (fallback inherit) | записывается один раз |
| proof | `POST /v1/chat/completions` | 200 с контентом = ключ живой |

## ⚙️ Опции CLI

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

| Переменная | По умолчанию | Значение |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | пул выходов, одна строка на прокси — необязательно: без пула CLI подключается напрямую |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | модель для живой проверки |

## 🛰️ За прокси

**Необязательно — рекомендуется при масштабе (лимиты на IP, баны).** Инструмент работает и без прокси.

- **С пулом:** передайте `--pool <file>` (или задайте `CAMELCO_PROXIES`), и каждый запрос идёт через выход из этого списка — мёртвая линия проваливает эту попытку, и CLI переходит к следующему выходу. Почтовый рельс (Emailnator) доступен через тот же выход.
- **Без пула:** CLI печатает `running WITHOUT a proxy (direct)` и подключается напрямую. Ничего в регистрации, чек-ине, создании ключей или проверке не меняется — только то, откуда уходят пакеты.
- Запустите `--screen` для любого режима: он тестирует имеющиеся выходы (или прямой маршрут) против CaMeL AI перед реальным запуском.

## 📄 Файлы

| Путь | Содержимое |
| --- | --- |
| `keys.txt` | один проверенный живой ключ на строку (48 символов) — игнорируется git |
| `accounts.jsonl` | username / e-mail / quotaStart / quotaAfterCheckin / group / gate |
| `proxies.txt` | ваши выходы — игнорируется git |
| `outputs/` | логи запусков — игнорируется git |

## 🔌 Подключение к 9Router

[9Router](https://9router.com) — это самостоятельно размещаемый, OpenAI-совместимый роутер:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

По умолчанию: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Модели подаются как `camelco/<model id>`.

## 🔑 Что даёт один ключ

| | |
| --- | --- |
| 🏢 Провайдер | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 Что даёт провайдер | $1 (500 000 единиц) при регистрации + ежедневный чек-ин (+100k…+989k) |
| 🔑 Что даёт один ключ | один ключ на собственном аккаунте, проверенный чатом на `gemini-2.5-flash-lite` |
| 🌐 Форма API | OpenAI-совместимый: `POST https://camelco.de/v1/chat/completions` |
| 🔒 Ваши данные | контакт только с `camelco.de` и вашим почтовым бэкендом |

## 🐞 Проблемы

Если CaMeL AI изменит свои гейты регистрации, форму письма или награду за чек-ин, инструмент остановится на этом месте и сообщит об этом (тело отказа выводится дословно). **[Откройте issue](../../issues)** с командой и точным выводом.

## 📄 Лицензия

MIT © 2026 — см. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>