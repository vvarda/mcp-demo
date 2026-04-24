# Block 2: Anatomy of MCP

---

## Slide 0 — Title Slide

**Anatomy of MCP**

> 🗣 *Тепер заглибимось у те, як MCP влаштований зсередини. Три рівні, три примітиви, два транспорти — і жодної магії.*

> `[image prompt]` 🖼 *An exploded-view technical diagram of interconnected layers labeled "Host", "Client", "Server". Clean blueprint style, dark background, neon blue accent lines.*

---

## Slide 1 — The Architecture

**Host → Client → Server**

| Layer | Role | Examples |
|-------|------|---------|
| **Host** | User-facing environment | Cursor, Claude Desktop, Claude Code |
| **Client** | Communication layer inside the host | Discovers tools, passes definitions to model |
| **Server** | Lightweight app exposing capabilities | Filesystem, Postgres, GitHub, Figma, Jira |

> 🗣 *Три рівні. Хост — це те, з чим працює користувач: IDE, термінал, чат. Клієнт — прошарок усередині хоста, який підключається до серверів і збирає список того, що вони вміють. Сервер — легковажний застосунок, який відкриває доступ до чогось конкретного: файлової системи, бази даних, зовнішнього API. Сервер нічого не знає про модель — він просто описує свої можливості.*

> `[image prompt]` 🖼 *Three-tier vertical diagram: "Host" at top (IDE icon), "Client" in middle (connector layer), "Server" at bottom (multiple service icons: DB, GitHub, filesystem). Arrows flow top to bottom. Minimal, dark mode, blueprint aesthetic.*

---

## Slide 2 — Three Primitives

| Primitive | What it is | Controlled by |
|-----------|-----------|---------------|
| **Tools** | Executable actions | Model |
| **Resources** | Read-only data via URI | App |
| **Prompts** | Reusable templates | User |

> Most public servers expose **tools only** — resources and prompts are underutilized.

> 🗣 *Три примітиви — це все, що сервер може надати. Tools — це дії: щось виконати, щось змінити, щось дістати. Модель сама вирішує, коли їх викликати. Resources — це read-only дані, доступні за URI, як файли або записи з бази. Prompts — готові шаблони, які користувач може запустити вручну. На практиці 90% серверів реалізують тільки tools. Resources і prompts — недовикористані, хоча у них є потенціал: наприклад, онбординг-документація як resources — це вже реальна кілер-фіча для аутсорс-команд.*

> `[image prompt]` 🖼 *Three labeled boxes side by side: "Tools" (wrench icon, blue), "Resources" (document/URI icon, teal), "Prompts" (template/chat icon, purple). Simple icons, dark background, clean spacing.*

---

## Slide 3 — MCP vs Function Calling

**Not the same thing.**

- **Function calling** — how the LLM invokes a function
- **MCP** — how functions are registered, described, and discovered

> MCP sits *above* function calling. It's the layer that makes tools discoverable.

> 🗣 *Часто плутають MCP із function calling — це різні речі на різних рівнях. Function calling — це механізм усередині моделі: як LLM формує виклик функції у відповідь. MCP — це шар вище: як інструменти взагалі потрапляють у контекст моделі. Сервер описує свої tools у JSON Schema, клієнт при підключенні їх дискаверить, модель отримує перелік як частину контекстного вікна і сама вирішує, що і коли викликати. MCP — це реєстрація і discovery, function calling — це виклик.*

> `[image prompt]` 🖼 *Two-layer stack diagram: top layer "MCP — Discovery & Registration", bottom layer "Function Calling — Invocation". Clean separation line between them. Dark background, minimal typography.*

---

## Slide 4 — Three Transports

| Transport | Use case | Latency |
|-----------|----------|---------|
| **STDIO** | Local process, same machine | ~1ms |
| **Streamable HTTP** | Remote / cloud server | 10–100ms |
| **Custom** | gRPC, WebSocket — IoT, Edge | varies |

> **Streamable HTTP** replaced HTTP+SSE in 2026. Sessions tracked via `Mcp-Session-Id` header. Origin validation protects against DNS Rebinding attacks.

> 🗣 *Три транспорти. STDIO — найпростіший: клієнт запускає сервер як дочірній процес і спілкується через stdin/stdout. Ніяких портів, ніякої автентифікації. Але саме через спрощені дефолти STDIO став найчастішою точкою атак у 2026 — про це більше в блоці про безпеку.*
>
> 🗣 *Streamable HTTP у 2026-му повністю витіснив старий HTTP+SSE. Запити йдуть як HTTP POST, асинхронні відповіді — через Server-Sent Events. Сесії відстежуються через заголовок `Mcp-Session-Id`, Origin validation захищає від DNS Rebinding атак. Це production-транспорт для хмарних серверів.*
>
> 🗣 *І є Custom transports — протокол свідомо агностичний до транспорту. Великі компанії можуть поверх gRPC або WebSocket реалізувати власне для IoT або Edge-сценаріїв. Це edge case, але важливо знати, що така гнучкість є.*

> `[image prompt]` 🖼 *Three-column comparison: "STDIO" (local pipe, two boxes on same machine), "Streamable HTTP" (cloud server with HTTP POST + SSE stream arrows), "Custom" (gRPC/WebSocket icon with edge device). Dark mode, minimal technical diagram.*

---

## Slide 5 — How Discovery Works

1. Server describes its tools in **JSON Schema**
2. Client connects → fetches the list
3. LLM receives tool definitions as part of its **context window**
4. Model decides what to call — and when

> **Example:** Sentry MCP server exposes `search_issues`, `get_issue_details`, `search_events`. User types "root cause this issue" → model picks `get_issue_details`.

> 🗣 *Як це працює на практиці. Сервер описує кожен tool у JSON Schema: назва, що робить, які параметри приймає. При підключенні клієнт запитує список — це один стандартний RPC-виклик. Всі описи потрапляють у контекстне вікно моделі разом з повідомленням користувача. Далі модель сама — без додаткового коду — обирає, який tool викликати і з якими аргументами. Для прикладу: Sentry MCP сервер виставляє кілька інструментів, і коли користувач пише "знайди root cause цього issue", модель сама обирає потрібний.*

> `[image prompt]` 🖼 *Sequential flow diagram: "Server (JSON Schema)" → "Client (connect + fetch)" → "LLM Context Window (tool list)" → "Model Decision". Horizontal arrow flow, numbered steps, dark background, accent on the context window box.*

---

## Slide 6 — Key Takeaway

**Remember the three primitives:**

> **Tools** = do something &nbsp;·&nbsp; **Resources** = read something &nbsp;·&nbsp; **Prompts** = template something

- MCP is LSP for AI agents — same pattern, same transport (JSON-RPC 2.0)
- Most of what you need already exists as a ready-made server

> 🗣 *Якщо запам'ятати одне з цього блоку — то три примітиви: tools це дія, resources це дані, prompts це шаблон. І ще одна аналогія, яка добре клікає для тих, хто налаштовував IDE: MCP — це LSP, але для AI-агентів. Той самий патерн: є протокол, є сервери під кожну мову або інструмент, є клієнт у редакторі. Якщо ти колись конфігурив language server у VS Code — ти вже розумієш архітектуру MCP.*

> `[image prompt]` 🖼 *Clean summary card with three icons in a row (wrench = Tools, document = Resources, template = Prompts) with one-word labels. Below, a subtle "MCP ≈ LSP for AI" equation. Minimal, dark background, bold typography.*
