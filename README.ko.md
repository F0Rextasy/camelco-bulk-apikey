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

**자동 생성된 API 키로 CaMeL AI 계정을 대량 생성 — OpenAI 호환 추론 + 9Router 커넥터.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 빠른 시작](#-quick-start) · [🎁 무료 티어](#-free-tier--how-much-you-get) · [🤖 모델](#-models--all-575-of-them) · [🧪 API 사용법](#-how-to-use-the-api) · [✅ 검증됨](#-verified-working--every-key-is-tested) · [🧭 작동 방식](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ 옵션](#️-cli-options) · [🛰️ 프록시](#️-behind-a-proxy) · [🐞 이슈](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 이 도구가 존재하는 이유

CaMeL AI(camelco.de)는 New API 포크로, **모든 신규 계정에 500,000 할당량 단위(qpu=500,000 기준 약 $1)** 를 지급하고, 추가로 **일일 출석 체크 시 +100,791 … +989,392 단위**를 지급합니다(해당 레인의 67개 계정에서 측정). 가입은 이메일 + 6자리 코드 방식이며 캡차가 없습니다(`turnstile_check:false`). 메일은 코드를 라벨 *앞에* 배치하고(`…邮箱验证。 c28676 验证码 10 分钟内有效…`), 실제 키 라인은 `gemini-2.5-flash-lite`입니다. 이 레인은 **67개 계정 → 67개 키**를 생성했으며, 모든 키는 실제 채팅 200 응답을 받은 후에만 유지되었습니다. 주의: 패널은 **대량 생성된 계정을 주기적으로 차단**합니다 — 생성 몇 시간 후 재확인 결과 67개 중 54개가 차단되었습니다(`403 auth.user_banned`, 로그인 시 `账号已被封禁`). 체크아웃은 이 분할을 명시적으로 제공합니다: **`keys.txt` = 살아있는 13개 키, `keys-dead.txt` = 차단된 54개**, 그리고 도구 자체의 `--verify-only`가 키를 신뢰하기 전의 판정자입니다.

## 🎁 무료 티어 — 얼마나 받을 수 있나

| 항목 | 값 |
| --- | --- |
| 가입 크레딧 | **500,000 단위 ≈ $1** (qpu=500,000) |
| 일일 출석 체크 | **+100,791 … +989,392 단위/일** (레인의 67개 계정 중 63개에 지급됨) |
| 가입 | 이메일 + 6자리 코드 — **캡차 없음** (`turnstile_check:false`); 코드 단계는 폭주 시 제한됩니다(`429` / `sendCode never accepted`) — CLI는 약 20초 대기 후 새 메일박스로 전환합니다(계정당 최대 5회) |
| 추천 | 실제 패널에서 `quota_for_inviter` 1,500,000 / `quota_for_invitee` 1,000,000 단위 |
| 카탈로그 | 실제 가격표에 **575개 모델 항목**, 28개 사용 가능 그룹 |
| 확장 단위 | **계정 하나 = 키 하나** |

> 💡 무료 티어는 CaMeL AI가 설정하며 변경될 수 있습니다. 실제 수치는 [camelco.de](https://camelco.de)에서 확인하세요 — 그리고 도구 자체의 게이트가 판단하게 하세요:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 모델 — 575개 전부

실제 가격표에는 **575개 모델 항목**(28개 사용 가능 그룹)이 있습니다. 검증된 키 라인:

| 모델 ID | 그룹 | 검증됨 |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (폴백: inherit) | 생성 시 **67개 키 중 64개** 채팅 200 — 58개는 `default` 경유, 6개는 inherit 경유 |
| `qwen-flash` | `default` | 1개 키 채팅 200 (gemini 호출이 실패했을 때의 게이트 폴백) |
| `deepseek-v4.1-flash` | `default` | 2개 키 채팅 200 (추론 모델 — 콘텐츠가 `reasoning_content`에 있을 수 있음) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API 사용법

Base URL **`https://camelco.de/v1`** — OpenAI 호환.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 빠른 시작

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

키는 `/v1/chat/completions`가 실제 콘텐츠와 함께 200을 응답한 **후에만** `keys.txt`에 기록됩니다. `accounts.jsonl`은 username / e-mail / quotaStart → quotaAfterCheckin / group을 보관합니다. 메일박스 대기 시간은 기본 300초입니다. CaMeL AI 메일이 느리기 때문입니다(불안정한 SMTP 날에는 150초 초과 측정됨) — `--mail-window`로 줄이거나 늘릴 수 있습니다.

### 🖱️ 런처

런처는 프록시 목록(선택 사항 — 없으면 CLI는 직접 연결로 실행됨)과 개수를 묻고, 이를 기억한 뒤 CLI를 실행합니다 — 생성, 검증, 종료 풀 자체 테스트, 설정, 도움말, 9Router 연결.

## ✅ 검증됨 — 모든 키가 테스트됨

| 검사 | 결과 |
| --- | --- |
| `GET /api/status` | **200** — 이메일 인증 켜짐, 출석 체크 켜짐, 캡차 꺼짐(`turnstile_check:false`); 이 포크는 `register_enabled`를 공개하지 않음 — 거부 응답 본문이 신호임 |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` — 생성된 67개 계정 전부 |
| `POST /api/user/register` | **200** — 67/67 계정; 폭주 시 패널이 `429`/"Verification code is incorrect or has expired"를 응답 — CLI는 코드를 재시도하고 메일박스를 전환함 |
| `POST /api/user/checkin` | **200** — 67개 계정 중 63개에 +100,791 … +989,392 단위 |
| `POST /api/token/` (`default`, 폴백 inherit) | **200** — 48자 키가 한 번 반환됨 |
| 기록 전 키별 `POST /v1/chat/completions` | **200** — 67/67 키가 실제로 생성됨(`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| 제공된 CLI를 통한 독립 재확인 (`--verify-only`, proxy 종료, 2026-10-21) | **13/67 살아있음** — 패널이 나머지 54개를 차단함(`403 auth.user_banned`); 차단은 시간순으로 발생했으며(가장 최근 13개 계정은 여전히 **200** 응답), 체크아웃은 이 분할을 유지함: **`keys.txt` = 살아있는 13개 키, `keys-dead.txt` = 차단된 54개** |

## 🧭 작동 방식

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| 단계 | 호출 | 비고 |
| --- | --- | --- |
| 상태 | `GET /api/status` | 메일 + 출석 체크 게이트; 캡차 꺼짐 |
| 메일박스 | Emailnator `POST /api/generate-email` | 계정당 새 @gmail.com 하나(점 없는 형식) |
| 코드 전송 | `GET /api/verification?email=…` | 6자리 코드; 대기 중인 코드는 세션 쿠키에 묶여 있음 — 유지됨 |
| 가입 | `POST /api/user/register` | 라벨 앞에 코드가 오는 메일; 최대 3개 후보 시도; 429 → 약 25초 백오프 |
| 로그인 | `POST /api/user/login` | 세션 (+ 포크가 제공하는 경우 `access_token`) |
| 출석 체크 | `POST /api/user/checkin` | 일일 보너스 |
| 키 | `POST /api/token/` 그룹 `default` (폴백 inherit) | 한 번 기록됨 |
| 증명 | `POST /v1/chat/completions` | 콘텐츠와 함께 200 = 키가 살아있음 |

## ⚙️ CLI 옵션

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

| 환경 변수 | 기본값 | 의미 |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | 종료 풀, 프록시당 한 줄 — 선택 사항: 풀이 없으면 CLI는 직접 연결함 |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | 실제 검증에 사용할 모델 |

## 🛰️ 프록시 뒤에서

**선택 사항 — 대규모에서는 권장됨(IP별 제한, 차단).** 이 도구는 프록시 없이도 작동합니다.

- **풀 사용 시:** `--pool <file>`을 전달하거나 `CAMELCO_PROXIES`를 설정하면 모든 요청이 해당 목록의 종료 지점을 통해 나갑니다 — 죽은 회선은 해당 시도를 실패시키고 CLI는 다음 종료 지점으로 이동합니다. 메일 레일(Emailnator)도 같은 종료 지점을 통해 접근 가능합니다.
- **풀 없이:** CLI는 `running WITHOUT a proxy (direct)`를 출력하고 직접 연결합니다. 가입, 출석 체크, 키 생성, 검증에는 아무 변화가 없습니다 — 패킷이 나가는 위치만 다를 뿐입니다.
- 어느 모드에서든 `--screen`을 실행하세요: 실제 실행 전에 보유한 종료 지점(또는 직접 경로)을 CaMeL AI에 대해 테스트합니다.

## 📄 파일

| 경로 | 내용 |
| --- | --- |
| `keys.txt` | 실제 검증된 키 한 줄에 하나(48자) — git 무시됨 |
| `accounts.jsonl` | username / e-mail / quotaStart / quotaAfterCheckin / group / gate |
| `proxies.txt` | 사용자의 종료 지점 — git 무시됨 |
| `outputs/` | 실행 로그 — git 무시됨 |

## 🔌 9Router 연결

[9Router](https://9router.com)는 자체 호스팅, OpenAI 호환 라우터입니다:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

기본값: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. 모델은 `camelco/<model id>`로 제공됩니다.

## 🔌 OmniRoute 연결

[OmniRoute](https://omniroute.online)는 자체 호스팅, OpenAI 호환 라우터입니다:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

기본값: `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. 모델은 `camelco/<model id>`로 제공됩니다.
## 🔑 키 하나로 얻는 것

| | |
| --- | --- |
| 🏢 제공자 | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 제공자가 주는 것 | 가입 시 $1(500,000 단위) + 일일 출석 체크(+100k…+989k) |
| 🔑 키 하나로 얻는 것 | 자체 계정의 키 하나, `gemini-2.5-flash-lite`에서 채팅 검증됨 |
| 🌐 API 형태 | OpenAI 호환: `POST https://camelco.de/v1/chat/completions` |
| 🔒 데이터 | `camelco.de`와 사용자의 메일박스 백엔드만 접촉됨 |

## 🐞 이슈

CaMeL AI가 가입 게이트, 메일 형식 또는 출석 체크 보상을 변경하면, 도구는 그 지점에서 멈추고 그 사실을 알립니다(거부 응답 본문이 그대로 보고됨). 명령과 정확한 출력을 포함하여 **[이슈를 열어주세요](../../issues)**.

## 📄 라이선스

MIT © 2026 — [LICENSE](LICENSE) 참조.

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>