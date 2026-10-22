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

**Crea cuentas de CaMeL AI en masa con claves API autogeneradas — inferencia compatible con OpenAI + conector 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Inicio rápido](#-quick-start) · [🎁 Nivel gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-575-of-them) · [🧪 Usa la API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Cómo funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Opciones](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por qué existe esto

CaMeL AI (camelco.de) es un fork de New API que entrega **500.000 unidades de cuota (≈ $1 con qpu=500.000) a cada cuenta nueva** más un **check-in diario de +100.791 … +989.392 unidades** (medido en las 67 cuentas del carril). El registro es correo electrónico + código de 6 caracteres, sin captcha (`turnstile_check:false`); los correos colocan el código *antes* de su etiqueta (`…邮箱验证。 c28676 验证码 10 分钟内有效…`), y la línea de clave activa es `gemini-2.5-flash-lite`. El carril acuñó **67 cuentas → 67 claves, cada clave se conservó solo tras un chat-200 en vivo**. Aviso: el panel **banea cuentas acuñadas en masa por oleadas** — una reverificación horas después de la acuñación encontró 54 de las 67 baneadas (`403 auth.user_banned`, inicio de sesión `账号已被封禁`); el checkout entrega esa división explícitamente: **`keys.txt` = las 13 claves activas, `keys-dead.txt` = las 54 baneadas**, y el propio `--verify-only` de la herramienta es el juez antes de que confíes en una clave.

## 🎁 Nivel gratuito — cuánto obtienes

| Qué | Valor |
| --- | --- |
| Crédito de registro | **500.000 unidades ≈ $1** (qpu=500.000) |
| Check-in diario | **+100.791 … +989.392 unidades/día** (pagado en 63 de las 67 cuentas del carril) |
| Registro | correo electrónico + código de 6 caracteres — **sin captcha** (`turnstile_check:false`); el paso del código se limita bajo ráfagas (`429` / `sendCode never accepted`) — la CLI retrocede ~20 s y rota a un buzón nuevo (hasta 5 por cuenta) |
| Referidos | `quota_for_inviter` 1.500.000 / `quota_for_invitee` 1.000.000 unidades en el panel activo |
| Catálogo | **575 entradas de modelos** en la hoja de precios activa en 28 grupos utilizables |
| La unidad que escala | **una cuenta = una clave** |

> 💡 El nivel gratuito lo define CaMeL AI y puede cambiar. Consulta los números en vivo en [camelco.de](https://camelco.de) — y deja que la propia puerta de la herramienta decida:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — los 575

La hoja de precios activa contiene **575 entradas de modelos** (28 grupos utilizables). La línea de clave verificada:

| ID del modelo | Grupo | Verificado |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (reserva: inherit) | **64 de 67 claves** chat-200 en la acuñación — 58 vía `default`, 6 vía inherit |
| `qwen-flash` | `default` | 1 clave chat-200 (reserva de la puerta cuando la llamada a gemini falló) |
| `deepseek-v4.1-flash` | `default` | 2 claves chat-200 (modelo de razonamiento — el contenido puede estar en `reasoning_content`) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Cómo usar la API

URL base **`https://camelco.de/v1`** — compatible con OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Inicio rápido

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Una clave se escribe en `keys.txt` **solo** después de que `/v1/chat/completions` responda 200 con contenido real; `accounts.jsonl` conserva username / e-mail / quotaStart → quotaAfterCheckin / group. La ventana del buzón es de 300 s por defecto porque el correo de CaMeL AI es lento (medido >150 s en un día de SMTP inestable) — `--mail-window` la acorta o la amplía.

### 🖱️ El lanzador

El lanzador pide la lista de proxies (opcional — sin una, la CLI se ejecuta directa) y la cantidad, las recuerda y ejecuta la CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Verificado funcionando — cada clave se prueba

| Comprobación | Resultado |
| --- | --- |
| `GET /api/status` | **200** — verificación por correo activada, check-in activado, captcha desactivado (`turnstile_check:false`); este fork no publica `register_enabled` — el cuerpo de rechazo es la señal |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` en las 67 cuentas acuñadas |
| `POST /api/user/register` | **200** — 67/67 cuentas; bajo ráfagas el panel responde `429`/“Verification code is incorrect or has expired” — la CLI reintenta códigos y rota buzones |
| `POST /api/user/checkin` | **200** — +100.791 … +989.392 unidades en 63 de 67 cuentas |
| `POST /api/token/` (`default`, reserva inherit) | **200** — claves de 48 caracteres devueltas una vez |
| `POST /v1/chat/completions` por clave antes de escribirse | **200** — 67/67 claves acuñadas en vivo (`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| reverificación independiente vía la CLI incluida (`--verify-only`, salida proxy, 2026-10-21) | **13/67 activas** — el panel baneó las otras 54 (`403 auth.user_banned`); la oleada es cronológica (las 13 cuentas más nuevas aún respondían **200**), y el checkout conserva la división: **`keys.txt` = 13 claves activas, `keys-dead.txt` = 54 baneadas** |

## 🧭 Cómo funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| Paso | Llamada | Notas |
| --- | --- | --- |
| status | `GET /api/status` | puertas de correo + check-in; captcha desactivado |
| buzón | Emailnator `POST /api/generate-email` | un @gmail.com nuevo por cuenta (forma sin puntos) |
| enviar código | `GET /api/verification?email=…` | código de 6 caracteres; el código pendiente está vinculado a la cookie de sesión — se conserva |
| registrar | `POST /api/user/register` | correos con código-antes-de-etiqueta; se recorren hasta 3 candidatos; 429 → retroceso de ~25 s |
| iniciar sesión | `POST /api/user/login` | sesión (+ `access_token` cuando el fork lo incluye) |
| check-in | `POST /api/user/checkin` | el bono diario |
| clave | `POST /api/token/` grupo `default` (reserva inherit) | escrita una vez |
| prueba | `POST /v1/chat/completions` | 200 con contenido = la clave está activa |

## ⚙️ Opciones de la CLI

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

| Env | Por defecto | Significado |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | pool de salidas, una línea por proxy — opcional: sin un pool la CLI se conecta directamente |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | modelo para la verificación en vivo |

## 🛰️ Detrás de un proxy

**Opcional — recomendado a escala (límites por IP, baneos).** La herramienta también funciona sin proxy.

- **Con un pool:** pasa `--pool <file>` (o define `CAMELCO_PROXIES`) y cada solicitud pasa por una salida de esa lista — una línea muerta hace fallar ese intento y la CLI pasa a la siguiente salida. El carril de correo (Emailnator) es accesible a través de la misma salida.
- **Sin un pool:** la CLI imprime `running WITHOUT a proxy (direct)` y se conecta directamente. Nada del registro, check-in, acuñación de claves o verificación cambia — solo desde dónde salen los paquetes.
- Ejecuta `--screen` contra cualquiera de los dos modos: prueba las salidas que tengas (o la ruta directa) contra CaMeL AI antes de una ejecución real.

## 📄 Archivos

| Ruta | Contenido |
| --- | --- |
| `keys.txt` | una clave verificada en vivo por línea (48 caracteres) — ignorada por git |
| `accounts.jsonl` | username / e-mail / quotaStart / quotaAfterCheckin / group / gate |
| `proxies.txt` | tus salidas — ignorado por git |
| `outputs/` | registros de ejecución — ignorados por git |

## 🔌 Conectar con 9Router

[9Router](https://9router.com) es un router autoalojado compatible con OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Valores por defecto: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Los modelos se sirven como `camelco/<model id>`.

## 🔑 Qué te da una clave

| | |
| --- | --- |
| 🏢 Proveedor | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 Qué da el proveedor | $1 (500.000 unidades) al registrarte + check-in diario (+100k…+989k) |
| 🔑 Qué te da una clave | una clave en su propia cuenta, verificada por chat en `gemini-2.5-flash-lite` |
| 🌐 Forma de la API | Compatible con OpenAI: `POST https://camelco.de/v1/chat/completions` |
| 🔒 Tus datos | solo se contacta con `camelco.de` y tu backend de buzón |

## 🐞 Problemas

Si CaMeL AI cambia sus puertas de registro, la forma del correo o la recompensa del check-in, la herramienta se detiene ahí y lo indica (el cuerpo de rechazo se reporta textualmente). **[Abre un issue](../../issues)** con el comando y la salida exacta.

## 📄 Licencia

MIT © 2026 — consulta [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>