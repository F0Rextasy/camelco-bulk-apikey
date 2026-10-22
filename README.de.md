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

**CaMeL-AI-Konten in großen Mengen erstellen mit automatisch generierten API-Schlüsseln — OpenAI-kompatible Inferenz + 9Router-Connector.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Schnellstart](#-quick-start) · [🎁 Kostenlose Stufe](#-free-tier--how-much-you-get) · [🤖 Modelle](#-models--all-575-of-them) · [🧪 Die API verwenden](#-how-to-use-the-api) · [✅ Verifiziert](#-verified-working--every-key-is-tested) · [🧭 So funktioniert es](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Optionen](#️-cli-options) · [🛰️ Proxys](#️-behind-a-proxy) · [🐞 Probleme](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Warum es das gibt

CaMeL AI (camelco.de) ist ein New-API-Fork, der jedem neuen Konto **500.000 Quota-Einheiten (≈ 1 $ bei qpu=500.000)** gutschreibt, plus einen **täglichen Check-in von +100.791 … +989.392 Einheiten** (gemessen über die 67 Konten der Lane). Die Registrierung erfolgt per E-Mail + 6-stelligem Code, ohne Captcha (`turnstile_check:false`); die Mails setzen den Code *vor* sein Label (`…邮箱验证。 c28676 验证码 10 分钟内有效…`), und die aktive Schlüsselzeile ist `gemini-2.5-flash-lite`. Die Lane hat **67 Konten → 67 Schlüssel** erzeugt, wobei jeder Schlüssel nur behalten wurde, nachdem ein Live-Chat-200 bestand. Achtung: Das Panel **sperrt in großen Mengen erzeugte Konten in Wellen** — eine erneute Prüfung Stunden nach der Erstellung fand 54 der 67 gesperrt (`403 auth.user_banned`, Login `账号已被封禁`); der Checkout liefert diese Aufteilung explizit mit: **`keys.txt` = die 13 aktiven Schlüssel, `keys-dead.txt` = die 54 gesperrten**, und das eigene `--verify-only` des Tools ist der Richter, bevor du einem Schlüssel vertraust.

## 🎁 Kostenlose Stufe — wie viel du bekommst

| Was | Wert |
| --- | --- |
| Anmeldeguthaben | **500.000 Einheiten ≈ 1 $** (qpu=500.000) |
| Täglicher Check-in | **+100.791 … +989.392 Einheiten/Tag** (ausgezahlt auf 63 der 67 Konten der Lane) |
| Anmeldung | E-Mail + 6-stelliger Code — **kein Captcha** (`turnstile_check:false`); der Code-Schritt drosselt bei Lastspitzen (`429` / `sendCode never accepted`) — die CLI wartet rückwärts ~20 s und wechselt zu einem frischen Postfach (bis zu 5 pro Konto) |
| Empfehlungen | `quota_for_inviter` 1.500.000 / `quota_for_invitee` 1.000.000 Einheiten auf dem Live-Panel |
| Katalog | **575 Modelleinträge** auf dem Live-Preisblatt über 28 nutzbare Gruppen |
| Die Einheit, die skaliert | **ein Konto = ein Schlüssel** |

> 💡 Die kostenlose Stufe wird von CaMeL AI festgelegt und kann sich ändern. Prüfe die Live-Zahlen auf [camelco.de](https://camelco.de) — und lass das eigene Gate des Tools entscheiden:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelle — alle 575 davon

Das Live-Preisblatt enthält **575 Modelleinträge** (28 nutzbare Gruppen). Die verifizierte Schlüsselzeile:

| Modell-ID | Gruppe | Verifiziert |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (Fallback: inherit) | **64 von 67 Schlüsseln** chat-200 bei Erstellung — 58 über `default`, 6 über inherit |
| `qwen-flash` | `default` | 1 Schlüssel chat-200 (Gate-Fallback, wenn der Gemini-Aufruf stockte) |
| `deepseek-v4.1-flash` | `default` | 2 Schlüssel chat-200 (Reasoning-Modell — Inhalt kann in `reasoning_content` liegen) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 So verwendest du die API

Basis-URL **`https://camelco.de/v1`** — OpenAI-kompatibel.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Schnellstart

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Ein Schlüssel wird **nur** dann in `keys.txt` geschrieben, wenn `/v1/chat/completions` mit 200 und echtem Inhalt antwortet; `accounts.jsonl` speichert Benutzername / E-Mail / quotaStart → quotaAfterCheckin / Gruppe. Das Postfach-Fenster beträgt standardmäßig 300 s, weil die CaMeL-AI-Mail langsam ist (gemessen >150 s an einem instabilen SMTP-Tag) — `--mail-window` verkürzt oder erweitert es.

### 🖱️ Der Launcher

Der Launcher fragt nach der Proxy-Liste (optional — ohne eine läuft die CLI direkt) und der Anzahl, merkt sie sich und führt die CLI aus — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Verifiziert funktionierend — jeder Schlüssel wird getestet

| Prüfung | Ergebnis |
| --- | --- |
| `GET /api/status` | **200** — E-Mail-Verifizierung an, Check-in an, Captcha aus (`turnstile_check:false`); dieser Fork veröffentlicht kein `register_enabled` — der Ablehnungs-Body ist das Signal |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` bei allen 67 erstellten Konten |
| `POST /api/user/register` | **200** — 67/67 Konten; bei Lastspitzen antwortet das Panel mit `429`/„Verification code is incorrect or has expired“ — die CLI wiederholt Codes und wechselt Postfächer |
| `POST /api/user/checkin` | **200** — +100.791 … +989.392 Einheiten bei 63 von 67 Konten |
| `POST /api/token/` (`default`, Fallback inherit) | **200** — 48-stellige Schlüssel einmalig zurückgegeben |
| `POST /v1/chat/completions` pro Schlüssel, bevor er geschrieben wird | **200** — 67/67 Schlüssel live erstellt (`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| unabhängige erneute Prüfung über die mitgelieferte CLI (`--verify-only`, proxy-Exit, 2026-10-21) | **13/67 aktiv** — das Panel hat die anderen 54 gesperrt (`403 auth.user_banned`); die Welle ist chronologisch (die neuesten 13 Konten antworteten noch mit **200**), und der Checkout behält die Aufteilung bei: **`keys.txt` = 13 aktive Schlüssel, `keys-dead.txt` = 54 gesperrte** |

## 🧭 So funktioniert es

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| Schritt | Aufruf | Hinweise |
| --- | --- | --- |
| status | `GET /api/status` | Mail- + Check-in-Gates; Captcha aus |
| Postfach | Emailnator `POST /api/generate-email` | eine frische @gmail.com pro Konto (punktfreie Form) |
| Code senden | `GET /api/verification?email=…` | 6-stelliger Code; der ausstehende Code ist an das Session-Cookie gebunden — es wird behalten |
| registrieren | `POST /api/user/register` | Mails mit Code vor Label; bis zu 3 Kandidaten werden durchlaufen; 429 → ~25 s Back-off |
| anmelden | `POST /api/user/login` | Session (+ `access_token`, wenn der Fork einen mitliefert) |
| Check-in | `POST /api/user/checkin` | der tägliche Bonus |
| Schlüssel | `POST /api/token/` Gruppe `default` (Fallback inherit) | einmalig geschrieben |
| Nachweis | `POST /v1/chat/completions` | 200 mit Inhalt = der Schlüssel ist aktiv |

## ⚙️ CLI-Optionen

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

| Env | Standard | Bedeutung |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | Exit-Pool, eine Zeile pro Proxy — optional: ohne einen Pool verbindet sich die CLI direkt |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | Modell für die Live-Verifizierung |

## 🛰️ Hinter einem Proxy

**Optional — bei Skalierung empfohlen (Limits pro IP, Sperren).** Das Tool funktioniert auch ohne Proxy.

- **Mit einem Pool:** übergib `--pool <file>` (oder setze `CAMELCO_PROXIES`), und jede Anfrage geht über einen Exit aus dieser Liste — eine tote Leitung lässt diesen Versuch scheitern, und die CLI wechselt zum nächsten Exit. Die Mail-Schiene (Emailnator) ist über denselben Exit erreichbar.
- **Ohne einen Pool:** die CLI gibt `running WITHOUT a proxy (direct)` aus und verbindet sich direkt. An Anmeldung, Check-in, Schlüsselerstellung oder Verifizierung ändert sich nichts — nur daran, wo die Pakete rausgehen.
- Führe `--screen` gegen beide Modi aus: Es testet die Exits, die du hast (oder die direkte Route) gegen CaMeL AI, bevor ein echter Lauf startet.

## 📄 Dateien

| Pfad | Inhalt |
| --- | --- |
| `keys.txt` | ein live-verifizierter Schlüssel pro Zeile (48 Zeichen) — von Git ignoriert |
| `accounts.jsonl` | Benutzername / E-Mail / quotaStart / quotaAfterCheckin / Gruppe / Gate |
| `proxies.txt` | deine Exits — von Git ignoriert |
| `outputs/` | Lauf-Logs — von Git ignoriert |

## 🔌 Mit 9Router verbinden

[9Router](https://9router.com) ist ein selbst gehosteter, OpenAI-kompatibler Router:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Standardwerte: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Modelle werden als `camelco/<model id>` bereitgestellt.

## 🔌 Mit OmniRoute verbinden

[OmniRoute](https://omniroute.online) ist ein selbst gehosteter, OpenAI-kompatibler Router:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Standardwerte: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Modelle werden als `camelco/<model id>` bereitgestellt.
## 🔑 Was ein Schlüssel dir bringt

| | |
| --- | --- |
| 🏢 Anbieter | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 Was der Anbieter gibt | 1 $ (500.000 Einheiten) bei Anmeldung + täglicher Check-in (+100k…+989k) |
| 🔑 Was ein Schlüssel dir bringt | ein Schlüssel auf seinem eigenen Konto, chat-verifiziert auf `gemini-2.5-flash-lite` |
| 🌐 API-Form | OpenAI-kompatibel: `POST https://camelco.de/v1/chat/completions` |
| 🔒 Deine Daten | nur `camelco.de` und dein Postfach-Backend werden kontaktiert |

## 🐞 Probleme

Wenn CaMeL AI seine Registrierungs-Gates, die Mail-Form oder die Check-in-Belohnung ändert, stoppt das Tool dort und sagt es (der Ablehnungs-Body wird wörtlich gemeldet). **[Öffne ein Issue](../../issues)** mit dem Befehl und der exakten Ausgabe.

## 📄 Lizenz

MIT © 2026 — siehe [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>