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

**Créez en masse des comptes CaMeL AI avec des clés API générées automatiquement — inférence compatible OpenAI + connecteur 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-575%20served-412991?style=for-the-badge)](https://camelco.de/pricing)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Démarrage rapide](#-quick-start) · [🎁 Offre gratuite](#-free-tier--how-much-you-get) · [🤖 Modèles](#-models--all-575-of-them) · [🧪 Utiliser l'API](#-how-to-use-the-api) · [✅ Vérifié](#-verified-working--every-key-is-tested) · [🧭 Comment ça marche](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Options](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problèmes](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Pourquoi cela existe

CaMeL AI (camelco.de) est un fork de New API qui attribue **500 000 unités de quota (≈ 1 $ à qpu=500 000) à chaque nouveau compte**, plus un **check-in quotidien de +100 791 … +989 392 unités** (mesuré sur les 67 comptes de la voie). L'inscription se fait par e-mail + code à 6 caractères, sans captcha (`turnstile_check:false`) ; les e-mails placent le code *avant* son libellé (`…邮箱验证。 c28676 验证码 10 分钟内有效…`), et la ligne de clé active est `gemini-2.5-flash-lite`. La voie a produit **67 comptes → 67 clés, chaque clé conservée uniquement après un chat-200 en direct**. Attention : le panneau **bannit les comptes créés en masse par vagues** — une re-vérification quelques heures après la création a trouvé 54 des 67 bannis (`403 auth.user_banned`, connexion `账号已被封禁`) ; le dépôt livre cette répartition explicitement : **`keys.txt` = les 13 clés actives, `keys-dead.txt` = les 54 bannies**, et le `--verify-only` de l'outil lui-même est le juge avant de faire confiance à une clé.

## 🎁 Offre gratuite — ce que vous obtenez

| Quoi | Valeur |
| --- | --- |
| Crédit d'inscription | **500 000 unités ≈ 1 $** (qpu=500 000) |
| Check-in quotidien | **+100 791 … +989 392 unités/jour** (versé sur 63 des 67 comptes de la voie) |
| Inscription | e-mail + code à 6 caractères — **pas de captcha** (`turnstile_check:false`) ; l'étape du code est limitée en cas de rafales (`429` / `sendCode never accepted`) — le CLI recule d'environ 20 s et bascule vers une nouvelle boîte mail (jusqu'à 5 par compte) |
| Parrainages | `quota_for_inviter` 1 500 000 / `quota_for_invitee` 1 000 000 unités sur le panneau actif |
| Catalogue | **575 entrées de modèles** sur la grille tarifaire active, réparties sur 28 groupes utilisables |
| L'unité qui monte en échelle | **un compte = une clé** |

> 💡 L'offre gratuite est définie par CaMeL AI et peut changer. Vérifiez les chiffres en direct sur [camelco.de](https://camelco.de) — et laissez la barrière de l'outil décider :

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modèles — les 575

La grille tarifaire active contient **575 entrées de modèles** (28 groupes utilisables). La ligne de clé vérifiée :

| ID du modèle | Groupe | Vérifié |
| --- | --- | --- |
| `gemini-2.5-flash-lite` | `default` (repli : inherit) | **64 des 67 clés** chat-200 à la création — 58 via `default`, 6 via inherit |
| `qwen-flash` | `default` | 1 clé chat-200 (repli de la barrière quand l'appel gemini a hoqueté) |
| `deepseek-v4.1-flash` | `default` | 2 clés chat-200 (modèle de raisonnement — le contenu peut se trouver dans `reasoning_content`) |

```bash
curl https://camelco.de/v1/chat/completions \
  -H "Authorization: Bearer GrJ4F7Q5…" -H "Content-Type: application/json" \
  -d '{"model":"gemini-2.5-flash-lite","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Comment utiliser l'API

URL de base **`https://camelco.de/v1`** — compatible OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://camelco.de/v1", api_key="GrJ4F7Q5…")
print(c.chat.completions.create(model="gemini-2.5-flash-lite",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Démarrage rapide

```bash
cd camelco-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings
# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Une clé n'est écrite dans `keys.txt` **qu'après** que `/v1/chat/completions` a répondu 200 avec du contenu réel ; `accounts.jsonl` conserve username / e-mail / quotaStart → quotaAfterCheckin / group. La fenêtre de la boîte mail est par défaut de 300 s car le courrier de CaMeL AI est lent (mesuré >150 s un jour où le SMTP était instable) — `--mail-window` la raccourcit ou l'élargit.

### 🖱️ Le lanceur

Le lanceur demande la liste de proxies (facultative — sans elle, le CLI s'exécute en direct) et le nombre, les mémorise, et exécute le CLI — création, vérification, test des sorties, paramètres, aide, Connect to 9Router.

## ✅ Vérifié fonctionnel — chaque clé est testée

| Vérification | Résultat |
| --- | --- |
| `GET /api/status` | **200** — vérification e-mail activée, check-in activé, captcha désactivé (`turnstile_check:false`) ; ce fork ne publie pas `register_enabled` — le corps du refus est le signal |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` sur les 67 comptes créés |
| `POST /api/user/register` | **200** — 67/67 comptes ; en cas de rafales, le panneau répond `429`/« Verification code is incorrect or has expired » — le CLI réessaie les codes et fait tourner les boîtes mail |
| `POST /api/user/checkin` | **200** — +100 791 … +989 392 unités sur 63 des 67 comptes |
| `POST /api/token/` (`default`, repli inherit) | **200** — clés de 48 caractères renvoyées une seule fois |
| `POST /v1/chat/completions` par clé avant son écriture | **200** — 67/67 clés créées en direct (`gemini-2.5-flash-lite` 64, `qwen-flash` 1, `deepseek-v4.1-flash` 2) |
| re-vérification indépendante via le CLI fourni (`--verify-only`, sortie WARP, 2026-10-21) | **13/67 actives** — le panneau a banni les 54 autres (`403 auth.user_banned`) ; la vague est chronologique (les 13 comptes les plus récents répondaient encore **200**), et le dépôt conserve la répartition : **`keys.txt` = 13 clés actives, `keys-dead.txt` = 54 bannies** |

## 🧭 Comment ça marche

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: CaMeL AI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://camelco.de/v1/chat/completions → real content = the key is live
```

| Étape | Appel | Remarques |
| --- | --- | --- |
| status | `GET /api/status` | barrières mail + check-in ; captcha désactivé |
| boîte mail | Emailnator `POST /api/generate-email` | une nouvelle adresse @gmail.com par compte (forme sans point) |
| envoi du code | `GET /api/verification?email=…` | code à 6 caractères ; le code en attente est lié au cookie de session — il est conservé |
| inscription | `POST /api/user/register` | e-mails avec code avant libellé ; jusqu'à 3 candidats essayés ; 429 → recul d'environ 25 s |
| connexion | `POST /api/user/login` | session (+ `access_token` quand le fork en fournit un) |
| check-in | `POST /api/user/checkin` | le bonus quotidien |
| clé | `POST /api/token/` groupe `default` (repli inherit) | écrite une seule fois |
| preuve | `POST /v1/chat/completions` | 200 avec contenu = la clé est active |

## ⚙️ Options du CLI

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

| Env | Défaut | Signification |
| --- | --- | --- |
| `CAMELCO_PROXIES` | `proxies.txt` | pool de sorties, une ligne par proxy — facultatif : sans pool, le CLI se connecte directement |
| `CAMELCO_MODEL` | `gemini-2.5-flash-lite` | modèle pour la vérification en direct |

## 🛰️ Derrière un proxy

**Facultatif — recommandé à grande échelle (limites par IP, bannissements).** L'outil fonctionne aussi sans proxy.

- **Avec un pool :** passez `--pool <file>` (ou définissez `CAMELCO_PROXIES`) et chaque requête passe par une sortie de cette liste — une ligne morte fait échouer cette tentative et le CLI passe à la sortie suivante. Le rail mail (Emailnator) est joignable via la même sortie.
- **Sans pool :** le CLI affiche `running WITHOUT a proxy (direct)` et se connecte directement. Rien ne change pour l'inscription, le check-in, la création de clés ou la vérification — seul le point de départ des paquets change.
- Lancez `--screen` dans l'un ou l'autre mode : il teste les sorties dont vous disposez (ou la route directe) contre CaMeL AI avant une exécution réelle.

## 📄 Fichiers

| Chemin | Contenu |
| --- | --- |
| `keys.txt` | une clé vérifiée active par ligne (48 caractères) — ignoré par git |
| `accounts.jsonl` | username / e-mail / quotaStart / quotaAfterCheckin / group / gate |
| `proxies.txt` | vos sorties — ignoré par git |
| `outputs/` | journaux d'exécution — ignorés par git |

## 🔌 Se connecter à 9Router

[9Router](https://9router.com) est un routeur auto-hébergé compatible OpenAI :

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Défauts : `--prefix camelco`, `--base-url https://camelco.de/v1`, `--model gemini-2.5-flash-lite`. Les modèles sont servis sous `camelco/<model id>`.

## 🔑 Ce qu'une clé vous apporte

| | |
| --- | --- |
| 🏢 Fournisseur | **CaMeL AI** — [camelco.de](https://camelco.de) |
| 🎁 Ce que le fournisseur donne | 1 $ (500 000 unités) à l'inscription + check-in quotidien (+100k…+989k) |
| 🔑 Ce qu'une clé vous apporte | une clé sur son propre compte, vérifiée par chat sur `gemini-2.5-flash-lite` |
| 🌐 Forme de l'API | compatible OpenAI : `POST https://camelco.de/v1/chat/completions` |
| 🔒 Vos données | seuls `camelco.de` et votre backend de boîte mail sont contactés |

## 🐞 Problèmes

Si CaMeL AI modifie ses barrières d'inscription, la forme des e-mails ou la récompense de check-in, l'outil s'arrête là et le signale (le corps du refus est rapporté textuellement). **[Ouvrez un ticket](../../issues)** avec la commande et la sortie exacte.

## 📄 Licence

MIT © 2026 — voir [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 camelco-bulk-apikey contributors

</div>