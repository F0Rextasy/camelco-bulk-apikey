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

**Otomatik oluşturulan API anahtarlarıyla toplu CaMeL AI hesabı açın — OpenAI uyumlu çıkarım + 9Router bağlayıcısı.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Hızlı başlangıç](#-quick-start) · [🎁 Ücretsiz katman](#-free-tier--how-much-you-get) · [🤖 Modeller](#-models--all-575-of-them) · [🧪 API'yi kullanma](#-how-to-use-the-api) · [✅ Doğrulandı](#-verified-working--every-key-is-tested) · [🧭 Nasıl çalışır](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Seçenekler](#️-cli-options) · [🛰️ Proxy'ler](#️-behind-a-proxy) · [🐞 Sorunlar](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Bu neden var

CaMeL AI (camelco.de), her yeni hesaba **500.000 kota birimi (qpu=500.000 ile ≈ $1)** ve **günlük girişte +100.791 … +989.392 birim** (lane'in 67 hesabı üzerinden ölçüldü) veren bir New API çatalıdır. Kayıt e-posta + 6 karakterli koddur, captcha yoktur (`turnstile_check:false`); e-postalar kodu etiketinden *önce* koyar (`…邮箱验证。 c28676 验证码 10 分钟内有效…`) ve canlı anahtar satırı `gemini-2.5-flash-lite`'tır. Lane **67 hesap → 67 anahtar** üretti, her anahtar yalnızca canlı bir chat-200'den sonra tutuldu. Dikkat: panel **toplu üretilen hesapları dalgalar halinde yasaklar** — üretimden saatler sonra yapılan yeniden kontrol, 67 hesabın 54'ünün yasaklandığını buldu (`403 auth.user_banned`, giriş `账号已被封禁`); çıktı bu ayrımı açıkça sunar: **`keys.txt` = 13 canlı anahtar, `keys-dead.txt` = yasaklanan 54**, ve bir anahtara güvenmeden önce aracın kendi `--verify-only` komutu hakemdir.

## 🎁 Ücretsiz katman — ne kadar alırsınız

| Ne | Değer |
| --- | --- |
| Kayıt kredisi | **500.000 birim ≈ $1** (qpu=500.000) |
| Günlük giriş | **+100.791 … +989.392 birim/gün** (67 lane hesabının 63'ünde ödendi) |
| Kayıt | e-posta + 6 karakterli kod — **captcha yok** (`turnstile_check:false`); kod adımı yoğunluk altında kısıtlar (`429` / `sendCode never accepted`) — CLI ~20 sn geri çekilir ve taze bir posta kutusuna geçer (hesap başına en fazla 5) |
| Davetler | canlı panelde `quota_for_inviter` 1.500.000 / `quota_for_invitee` 1.000.000 birim |
| Katalog | canlı fiyatlandırma sayfasında 28 kullanılabilir grup genelinde **575 model girdisi** |
| Ölçeklenen birim | **bir hesap = bir anahtar** |

> 💡 Ücretsiz katman CaMeL AI tarafından belirlenir ve değişebilir. Canlı sayıları [camelco.de](https://camelco.de) adresinden kontrol edin — ve kararı aracın kendi kapısına bırakın:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modeller — 575'inin tamamı

Canlı fiyatlandırma sayfası **575 model girdisi** taşır (28 kullanılabilir grup). Doğrulanan anahtar satırı:

| Model Kimliği | Grup | Doğrulandı |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (yedek: inherit) | üretimde **67 anahtarın 64'ü** chat-200 — 58'i `default` üzerinden, 6'sı inherit üzerinden |
| `qwen-flash` | `default` | 1 anahtar chat-200 (gemini çağrısı aksadığında kapı yedeği) |
| `deepseek-v4.1-flash` | `default` | 2 anahtar chat-200 (akıl yürütme modeli — içerik `reasoning_content` içinde olabilir) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API nasıl kullanılır

Temel URL **`https://camelco.de/v1`** — OpenAI uyumlu.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Hızlı başlangıç

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Bir anahtar `keys.txt` dosyasına **yalnızca** `/v1/chat/completions` gerçek içerikle 200 yanıt verdikten sonra yazılır; `accounts.jsonl` kullanıcı adı / e-posta / quotaStart → quotaAfterCheckin / grup bilgisini tutar. Posta kutusu penceresi varsayılan olarak 300 sn'dir çünkü CaMeL AI postası yavaştır (kararsız bir SMTP gününde >150 sn ölçüldü) — `--mail-window` bunu kısaltır veya genişletir.

### 🖱️ Başlatıcı

Başlatıcı proxy listesini (isteğe bağlı — olmadan CLI doğrudan çalışır) ve sayıyı sorar, bunları hatırlar ve CLI'yi çalıştırır — üret, doğrula, çıkışları tara, ayarlar, yardım, 9Router'a Bağlan.

## ✅ Çalıştığı doğrulandı — her anahtar test edilir

| Kontrol | Sonuç |
| --- | --- |
| `GET /api/status` | **200** — e-posta doğrulama açık, giriş açık, captcha kapalı (`turnstile_check:false`); bu çatal `register_enabled` yayınlamaz — ret gövdesi sinyaldir |
| `GET /api/verification?email=…` | 67 üretilen hesabın tamamında **200** `{"message":"","success":true}` |
| `POST /api/user/register` | **200** — 67/67 hesap; yoğunluk altında panel `429`/"Verification code is incorrect or has expired" yanıtı verir — CLI kodları yeniden dener ve posta kutularını değiştirir |
| `POST /api/user/checkin` | **200** — 67 hesabın 63'ünde +100.791 … +989.392 birim |
| `POST /api/token/` (`default`, yedek inherit) | **200** — 48 karakterli anahtarlar bir kez döndürülür |
| Yazılmadan önce her anahtar için `POST /v1/chat/completions` | **200** — 67/67 anahtar canlı üretildi (`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| gönderilen CLI ile bağımsız yeniden kontrol (`--verify-only`, proxy çıkışı, 2026-10-21) | **13/67 canlı** — panel diğer 54'ünü yasakladı (`403 auth.user_banned`); dalga kronolojiktir (en yeni 13 hesap hâlâ **200** yanıtladı) ve çıktı ayrımı korur: **`keys.txt` = 13 canlı anahtar, `keys-dead.txt` = 54 yasaklı** |

## 🧭 Nasıl çalışır

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| Adım | Çağrı | Notlar |
| --- | --- | --- |
| durum | `GET /api/status` | posta + giriş kapıları; captcha kapalı |
| posta kutusu | Emailnator `POST /api/generate-email` | hesap başına bir taze @gmail.com (noktasız biçim) |
| kod gönder | `GET /api/verification?email=…` | 6 karakterli kod; bekleyen kod oturum çerezine bağlıdır — saklanır |
| kayıt | `POST /api/user/register` | kod etiketten önce gelen postalar; en fazla 3 aday denenir; 429 → ~25 sn geri çekilme |
| giriş | `POST /api/user/login` | oturum (+ çatal bir tane sunuyorsa `access_token`) |
| günlük giriş | `POST /api/user/checkin` | günlük bonus |
| anahtar | `POST /api/token/` grup `default` (yedek inherit) | bir kez yazılır |
| kanıt | `POST /v1/chat/completions` | içerikle 200 = anahtar canlı |

## ⚙️ CLI seçenekleri

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

| Ortam | Varsayılan | Anlam |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | çıkış havuzu, satır başına bir proxy — isteğe bağlı: havuz olmadan CLI doğrudan bağlanır |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | canlı doğrulama için model |

## 🛰️ Proxy arkasında

**İsteğe bağlı — ölçekte önerilir (IP başına limitler, yasaklar).** Araç proxy olmadan da çalışır.

- **Havuzla:** `--pool <file>` geçin (veya `CAMELCO_PROXIES` ayarlayın) ve her istek o listeden bir çıkıştan geçer — ölü bir hat o denemeyi başarısız kılar ve CLI bir sonraki çıkışa geçer. Posta hattı (Emailnator) aynı çıkış üzerinden erişilebilir.
- **Havuz olmadan:** CLI `running WITHOUT a proxy (direct)` yazdırır ve doğrudan bağlanır. Kayıt, günlük giriş, anahtar üretimi veya doğrulama konusunda hiçbir şey değişmez — yalnızca paketlerin nereden çıktığı değişir.
- Gerçek bir çalıştırmadan önce her iki moda karşı `--screen` çalıştırın: sahip olduğunuz çıkışları (veya doğrudan rotayı) CaMeL AI'ye karşı test eder.

## 📄 Dosyalar

| Yol | İçerik |
| --- | --- |
| `keys.txt` | satır başına bir canlı doğrulanmış anahtar (48 karakter) — git tarafından yok sayılır |
| `accounts.jsonl` | kullanıcı adı / e-posta / quotaStart / quotaAfterCheckin / grup / kapı |
| `proxies.txt` | çıkışlarınız — git tarafından yok sayılır |
| `outputs/` | çalıştırma günlükleri — git tarafından yok sayılır |

## 🔌 9Router'a Bağlan

[9Router](https://9router.com) kendi sunucunuzda barındırılan, OpenAI uyumlu bir yönlendiricidir:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Varsayılanlar: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Modeller `camelco/<model id>` olarak sunulur.

## 🔌 OmniRoute'a Bağlan

[OmniRoute](https://omniroute.online) kendi sunucunuzda barındırılan, OpenAI uyumlu bir yönlendiricidir:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Varsayılanlar: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Modeller `camelco/<model id>` olarak sunulur.
## 🔑 Bir anahtar size ne kazandırır

| | |
| --- | --- |
| 🏢 Sağlayıcı | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 Sağlayıcının verdikleri | kayıtta $1 (500.000 birim) + günlük giriş (+100k…+989k) |
| 🔑 Bir anahtar size ne kazandırır | kendi hesabında bir anahtar, `gemini-2.5-flash-lite` üzerinde sohbet doğrulamalı |
| 🌐 API biçimi | OpenAI uyumlu: `POST https://camelco.de/v1/chat/completions` |
| 🔒 Verileriniz | yalnızca `camelco.de` ve posta kutusu arka ucunuzla iletişim kurulur |

## 🐞 Sorunlar

CaMeL AI kayıt kapılarını, posta biçimini veya günlük giriş ödülünü değiştirirse, araç orada durur ve bunu bildirir (ret gövdesi kelimesi kelimesine raporlanır). Komut ve tam çıktıyla birlikte **[bir sorun açın](../../issues)**.

## 📄 Lisans

MIT © 2026 — bkz. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey katkıda bulunanlar

</div>