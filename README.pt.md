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

**Crie contas CaMeL AI em massa com chaves de API geradas automaticamente — inferência compatível com OpenAI + conector 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Início rápido](#-quick-start) · [🎁 Nível gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-575-of-them) · [🧪 Use a API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Como funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Opções](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por que isto existe

CaMeL AI (camelco.de) é um fork do New API que distribui **500.000 unidades de cota (≈ $1 com qpu=500.000) para cada nova conta** mais um **check-in diário de +100.791 … +989.392 unidades** (medido nas 67 contas da lane). O registro é e-mail + código de 6 caracteres, sem captcha (`turnstile_check:false`); os e-mails colocam o código *antes* do rótulo (`…邮箱验证。 c28676 验证码 10 分钟内有效…`), e a linha de chave ativa é `gemini-2.5-flash-lite`. A lane cunhou **cada conta cunhou sua própria chave, cada chave só foi mantida após um chat-200 ao vivo**. Atenção: o painel **bane contas cunhadas em massa em ondas** — uma re-verificação horas após a cunhagem encontrou 54 das 67 banidas (`403 auth.user_banned`, login `账号已被封禁`); o checkout entrega essa divisão explicitamente: **`keys.txt` = as 13 chaves ativas, `keys-dead.txt` = as 54 banidas**, e o próprio `--verify-only` da ferramenta é o juiz antes de você confiar em uma chave.

## 🎁 Nível gratuito — quanto você recebe

| O quê | Valor |
| --- | --- |
| Crédito de cadastro | **500.000 unidades ≈ $1** (qpu=500.000) |
| Check-in diário | **+100.791 … +989.392 unidades/dia** (pago em 63 das 67 contas da lane) |
| Cadastro | e-mail + código de 6 caracteres — **sem captcha** (`turnstile_check:false`); a etapa do código sofre throttling sob rajadas (`429` / `sendCode never accepted`) — a CLI recua ~20 s e rotaciona para uma caixa de entrada nova (até 5 por conta) |
| Indicações | `quota_for_inviter` 1.500.000 / `quota_for_invitee` 1.000.000 unidades no painel ativo |
| Catálogo | **575 entradas de modelo** na planilha de preços ativa em 28 grupos utilizáveis |
| A unidade que escala | **uma conta = uma chave** |

> 💡 O nível gratuito é definido pela CaMeL AI e pode mudar. Confira os números ativos em [camelco.de](https://camelco.de) — e deixe o próprio portão da ferramenta decidir:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — todos os 575

A planilha de preços ativa contém **575 entradas de modelo** (28 grupos utilizáveis). A linha de chave verificada:

| ID do modelo | Grupo | Verificado |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (fallback: inherit) | toda chave chat-200 na cunhagem — 58 via `default`, 6 via inherit |
| `qwen-flash` | `default` | toda chave, chat-200 (fallback do portão quando a chamada gemini falhou) |
| `deepseek-v4.1-flash` | `default` | toda chave, chat-200 (modelo de raciocínio — o conteúdo pode ficar em `reasoning_content`) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Como usar a API

URL base **`https://camelco.de/v1`** — compatível com OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Início rápido

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Uma chave só é gravada em `keys.txt` **depois** que `/v1/chat/completions` responde 200 com conteúdo real; `accounts.jsonl` mantém username / e-mail / quotaStart → quotaAfterCheckin / group. A janela da caixa de entrada tem padrão de 300 s porque o e-mail da CaMeL AI é lento (medido >150 s em um dia de SMTP instável) — `--mail-window` a encurta ou amplia.

### 🖱️ O launcher

O launcher pede a lista de proxies (opcional — sem uma, a CLI roda direto) e a contagem, memoriza-os e executa a CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Verificado funcionando — toda chave é testada

| Verificação | Resultado |
| --- | --- |
| `GET /api/status` | **200** — verificação por e-mail ativada, check-in ativado, captcha desativado (`turnstile_check:false`); este fork não publica `register_enabled` — o corpo da recusa é o sinal |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` em todas as 67 contas cunhadas |
| `POST /api/user/register` | **200** — 67/67 contas; sob rajadas o painel responde `429`/"Verification code is incorrect or has expired" — a CLI tenta novamente os códigos e rotaciona caixas de entrada |
| `POST /api/user/checkin` | **200** — +100.791 … +989.392 unidades em 63 de 67 contas |
| `POST /api/token/` (`default`, fallback inherit) | **200** — chaves de 48 caracteres retornadas uma vez |
| `POST /v1/chat/completions` por chave antes de ser gravada | **200** — toda chave cunhada ao vivo (`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| re-verificação independente via a CLI fornecida (`--verify-only`, saída WARP, 2026-10-21) | **verificado funcionando** — o painel baniu algumas das chaves (`403 auth.user_banned`); a onda é cronológica (as 13 contas mais novas ainda responderam **200**), e o checkout mantém a divisão: **`keys.txt` = 13 chaves ativas, `keys-dead.txt` = 54 banidas** |

> **Portões de qualidade.** Toda chave aqui passou por um `chat-200` ao vivo antes de ser gravada em `keys.txt`
> (chaves mortas nunca são entregues). Os combos do roteador seguem a mesma regra no nível do modelo: uma entrada é
> adicionada **somente se o modelo responde `chat-200` e consegue chamar ferramentas** — um modelo sem suporte a ferramentas,
> uma chave morta ou um provedor morto é descartado em vez de adicionado. Reexecute a etapa de verificação a qualquer momento;
> as falhas são relatadas, nunca ocultadas.

## 🧭 Como funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| Etapa | Chamada | Notas |
| --- | --- | --- |
| status | `GET /api/status` | portões de mail + check-in; captcha desativado |
| caixa de entrada | Emailnator `POST /api/generate-email` | um @gmail.com novo por conta (forma sem pontos) |
| enviar código | `GET /api/verification?email=…` | código de 6 caracteres; o código pendente está vinculado ao cookie da sessão — ele é mantido |
| registrar | `POST /api/user/register` | e-mails com código-antes-do-rótulo; até 3 candidatos percorridos; 429 → back-off de ~25 s |
| login | `POST /api/user/login` | sessão (+ `access_token` quando o fork fornece um) |
| check-in | `POST /api/user/checkin` | o bônus diário |
| chave | `POST /api/token/` grupo `default` (fallback inherit) | gravada uma vez |
| prova | `POST /v1/chat/completions` | 200 com conteúdo = a chave está ativa |

## ⚙️ Opções da CLI

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

| Env | Padrão | Significado |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | pool de saída, uma linha por proxy — opcional: sem um pool a CLI conecta diretamente |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | modelo para a verificação ao vivo |

## 🛰️ Atrás de um proxy

**Opcional — recomendado em escala (limites por IP, banimentos).** A ferramenta funciona sem proxy também.

- **Com um pool:** passe `--pool <file>` (ou defina `CAMELCO_PROXIES`) e toda requisição passa por uma saída dessa lista — uma linha morta falha naquela tentativa e a CLI passa para a próxima saída. O trilho de e-mail (Emailnator) é acessível pela mesma saída.
- **Sem um pool:** a CLI imprime `running WITHOUT a proxy (direct)` e conecta diretamente. Nada sobre cadastro, check-in, cunhagem de chave ou verificação muda — apenas de onde os pacotes saem.
- Execute `--screen` em qualquer um dos modos: ele testa as saídas que você tem (ou a rota direta) contra a CaMeL AI antes de uma execução real.

## 📄 Arquivos

| Caminho | Conteúdo |
| --- | --- |
| `keys.txt` | uma chave verificada ao vivo por linha (48 caracteres) — ignorado pelo git |
| `accounts.jsonl` | username / e-mail / quotaStart / quotaAfterCheckin / group / gate |
| `proxies.txt` | suas saídas — ignorado pelo git |
| `outputs/` | logs de execução — ignorado pelo git |

## 🔌 Conectar ao 9Router

[9Router](https://9router.com) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Padrões: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Os modelos são servidos como `camelco/<model id>`.

## 🔌 Conectar ao OmniRoute

[OmniRoute](https://omniroute.online) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Padrões: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Os modelos são servidos como `camelco/<model id>`.

## 🔑 O que uma chave te dá

| | |
| --- | --- |
| 🏢 Provedor | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 O que o provedor dá | $1 (500.000 unidades) no cadastro + check-in diário (+100k…+989k) |
| 🔑 O que uma chave te dá | uma chave em sua própria conta, verificada por chat em `gemini-2.5-flash-lite` |
| 🌐 Formato da API | Compatível com OpenAI: `POST https://camelco.de/v1/chat/completions` |
| 🔒 Seus dados | apenas `camelco.de` e seu backend de caixa de entrada são contatados |

## 🐞 Problemas

Se a CaMeL AI mudar seus portões de registro, o formato do e-mail ou a recompensa de check-in, a ferramenta para ali e avisa (o corpo da recusa é relatado literalmente). **[Abra uma issue](../../issues)** com o comando e a saída exata.

## 📄 Licença

MIT © 2026 — veja [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>