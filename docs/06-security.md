# Block 5: Security

---

## Slide 0 — Title Slide

**Security**

> 🗣 *Цей блок — про дві речі, які вас здивують. Перша: token bloat — проблема, яка вдарить по бюджету раніше, ніж по безпеці. Друга: MCP має системні вразливості, і вендор офіційно каже "це не баг". Почнемо з токенів.*

> `[image prompt]` 🖼 *Dark background with a cracked shield icon and a token/coin overflow visual beside it. Two distinct threat symbols — cost and security. Minimal, bold.*

---

## Slide 1 — The OX Security Incident (April 2026)

**Architectural RCE in official Anthropic MCP SDKs.**

- Affected: Python, TypeScript, Java, Rust SDKs
- Scale: **~200,000 vulnerable instances**, 150M+ downloads
- All major clients vulnerable: Cursor, VS Code, Windsurf, Claude Code, Gemini CLI
- **CVE-2026-30615 (Windsurf)** — zero-click, no user interaction required
- 9 out of 11 MCP marketplaces allowed malicious PoC servers through

**Anthropic's response:** *"This is expected behavior — STDIO was designed for this."*

> The protocol won't fix this. That's an official position.

> 🗣 *У середині квітня 2026 дослідники OX Security опублікували архітектурну RCE-вразливість в офіційних MCP SDK від Anthropic — у всіх чотирьох мовах. 200 тисяч вразливих інстансів. Cursor, VS Code, Windsurf, Claude Code, Gemini CLI — всі вразливі. CVE-2026-30615 у Windsurf — zero-click, без жодної взаємодії з боку користувача.*
>
> 🗣 *Але найважливіше тут не сама вразливість — а відповідь Anthropic: "це очікувана поведінка, STDIO саме для цього і створений". Вони офіційно відмовились фіксити це на рівні протоколу. Це не технічна позиція — це політична. І її потрібно вміти пояснити клієнту.*

> `[image prompt]` 🖼 *A news-style alert card: "CRITICAL — April 2026" with logos of Cursor, VS Code, Windsurf, Claude Code struck through in red. "200,000 instances" in large text. Dark background, urgent red accent.*

---

## Slide 2 — Attack Classes

| Attack | How it works |
|--------|-------------|
| **Prompt injection** | AI reads malicious content (issue, PDF, README) and executes hidden instructions |
| **Tool poisoning** | Attacker changes tool description metadata — user never sees it, LLM acts on it |
| **Rug pull** | Server behaves safely on install, changes behavior days later |
| **Cross-tool hijacking** | One server intercepts calls addressed to another |
| **Supply chain** | STDIO server inherits your user permissions — reads `.env`, SSH keys, shell history |

> 🗣 *П'ять класів атак, які варто знати. Prompt injection — AI читає заражений контент: issue в GitHub, веб-сторінку, PDF-файл — і виконує приховані інструкції. Tool poisoning — атакувальник змінює опис інструмента в метаданих. Користувач це не бачить, а LLM читає і діє відповідно. Rug pull — сервер безпечний у день установки, а через тиждень вже ексфільтрує API ключі. Cross-tool hijacking — один сервер перехоплює виклики, адресовані іншому. І supply chain — STDIO-сервер успадковує ваші user permissions і може читати .env, SSH-ключі, histoію шелу.*

> `[image prompt]` 🖼 *Five labeled threat cards in a grid, each with a simple icon: injection syringe, poisoned tool wrench, rug being pulled, intercepted arrow, chain with broken link. Dark background, red/amber accent.*

---

## Slide 3 — The Lethal Trifecta

**Simon Willison's model for when attacks become near-inevitable:**

> When an agent has all three simultaneously:
> 1. Access to **private data**
> 2. **Untrusted input** (web pages, user files, external APIs)
> 3. An **exfiltration channel** (outbound HTTP, email, any write tool)

> → Attack is almost guaranteed to succeed if attempted.

**Recent CVEs in the wild:**

- **CVE-2025-49596** (MCP Inspector, CVSS 9.4) — RCE just by visiting a malicious site with Inspector running
- **CVE-2026-25536** (TypeScript SDK 1.10.0–1.25.3) — cross-client data leak via Streamable HTTP
- **CVE-2025-68143/68144/68145** — prompt injection via official `mcp-server-git` (AI reads a malicious README)

> 🗣 *Simon Willison описав "смертельне тріо": коли агент одночасно має доступ до приватних даних, отримує недовірений ввід ззовні і має канал для ексфільтрації — атака майже гарантовано спрацює, якщо її спробують. Це не теоретично — CVE-2025-68143 показує: достатньо щоб агент прочитав шкідливий README у Git репозиторії. Жодної додаткової взаємодії.*

> `[image prompt]` 🖼 *A triangle diagram with three labeled corners: "Private Data", "Untrusted Input", "Exfiltration Channel". Center of triangle glows red with a skull/danger icon. "Lethal Trifecta" label. Dark background.*

---

## Slide 4 — Team Checklist

**Practical mitigations — apply before connecting any server to production.**

- ✅ Install servers only from the **official MCP Registry** or verified vendors (Microsoft, Figma, Sentry, etc.)
- ✅ Untrusted servers — **Docker / sandbox only**, no host filesystem access
- ✅ GitHub MCP: use `--toolsets` scope, never `--all`
- ✅ **Human-in-the-loop** on all destructive actions — always, no exceptions
- ✅ Short-lived, **scoped tokens** — no over-permissioned service accounts
- ✅ **Monitor tool invocations** — know what the agent actually called
- ✅ **Audit trail at gateway level** — required for any enterprise client delivery
- ✅ Treat external config as **untrusted input** — if a user can influence MCP config, that's an RCE vector
- ✅ Use **OWASP MCP Top 10** as your internal security review checklist

> 🗣 *Практичний чекліст для команди. Перше правило: ставити тільки з офіційного MCP Registry або від перевірених вендорів. Друге: недовірені сервери — тільки в Docker, без доступу до хост-системи. Третє: GitHub MCP підтримує `--toolsets` — скоупте, а не "all". Четверте: human-in-the-loop на будь-яку деструктивну дію, завжди, без виключень. І п'яте: якщо ви плануєте deliver MCP-агентів клієнту в production — audit trail на рівні gateway є обов'язковим, не опціональним.*

> `[image prompt]` 🖼 *A clean checklist card with 5-6 items, each with a green checkmark. Security shield icon at top. Dark background, professional security audit aesthetic.*

---

## Slide 5 — OWASP MCP Top 10

**The living security standard for MCP — beta pilot as of 2026.**

| # | Category |
|---|----------|
| 1 | Prompt Injection |
| 2 | Input Validation |
| 3 | Auth Failures |
| 4 | Session Management |
| 5 | Integrity Issues |
| 6 | Trust Model |
| 7 | Credentials |
| 8 | Network Security |
| 9 | Command Injection |
| 10 | Supply Chain (signatures, dependency monitoring) |

> Use this as your internal security review checklist before delivering any MCP-enabled product to a client.

> 🗣 *OWASP випустили MCP Top 10 — живий документ у стані beta pilot станом на 2026. Це те саме, що OWASP Top 10 для веб, але для MCP. Якщо ви робите security review перед передачею продукту клієнту — це ваш чекліст. Зверніть увагу на №10: supply chain. Підписи пакетів і моніторинг залежностей — саме це дозволило б зловити деякі з квітневих CVE раніше.*

> `[image prompt]` 🖼 *OWASP-style numbered list card, dark background, each item on its own line with a numbered badge. "OWASP MCP Top 10" header with OWASP logo style. Professional security documentation aesthetic.*

---

## Slide 6 — Key Takeaway

**The protocol cannot fix this — and that's official.**

> MCP's security model relies on host implementations and operational policy — not the protocol itself.

Your responsibilities as a team delivering MCP-based products:
- Understand the threat model before connecting any server
- Never give an agent more permissions than the task requires
- Design for audit, not just for function

> 🗣 *Мета-інсайт, яким варто завершити цей блок: сам протокол ці проблеми не вирішує — і це офіційна позиція. Це не баги реалізації, це design trade-offs. Anthropic свідомо залишили це на відповідальності host implementations і операційних політик. Ми не чекаємо патча — ми самі закриваємо це на рівні того, як налаштовуємо і деплоїмо. Для аутсорс-компанії це означає одне: клієнти будуть питати про безпеку MCP, і ви маєте мати відповідь — не "це проблема Anthropic".*

> `[image prompt]` 🖼 *A cracked shield with "Protocol" written on it, below it three solid pillars labeled "Host Implementation", "Operational Policy", "Audit Trail". Dark background, message of human responsibility over tool reliance.*
