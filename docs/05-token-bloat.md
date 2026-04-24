# Block 5: Token Bloat

---

## Slide 0 — Title Slide

**Token Bloat**

> 🗣 *Є одна проблема, яка вдарить по вашому бюджету і швидкості швидше, ніж будь-яка security-вразливість. І більшість команд стикається з нею щойно підключає більше двох-трьох серверів.*

> `[image prompt]` 🖼 *A context window bar filling up rapidly with JSON schema text, crowding out the actual user prompt which is tiny at the bottom. Overflow warning icon. Dark background, visual metaphor for context saturation.*

---

## Slide 1 — The Scale of the Problem

**You haven't typed a word yet. A third of your context is already gone.**

| Setup | Token overhead |
|-------|---------------|
| GitHub MCP (94 tools) | ~17,600 tokens |
| Atlassian MCP (Jira + Confluence) | ~10,000 tokens |
| GitHub + Jira + AWS | **30,000+ tokens per request** |
| MySQL server (106 tools) | ~54,600 tokens |
| 8 production servers (224 tools) | ~66,000 tokens = **33% of Claude Sonnet's context** |

> Cost: **$0.01–0.10 per request** just for tool schema overhead, before any actual work.

> 🗣 *Подивіться на цифри. GitHub MCP сам по собі — 17 600 токенів щоразу. Якщо у вас підключені GitHub, Jira і AWS одночасно — ви платите 30 тисяч токенів до того, як написали перше слово. Один розробник задокументував свій сетап з 8 серверами: 224 інструменти, 66 тисяч токенів — це рівно третина контекстного вікна Claude Sonnet зайнята ще до старту. І це не безкоштовно: від одного до десяти центів за запит лише на описи інструментів. За день активного використання командою — це вже помітний рядок у бюджеті.*

> `[image prompt]` 🖼 *A bar chart comparing token overhead across setups: GitHub, Jira+Confluence, GitHub+Jira+AWS, MySQL 106 tools, 8 servers. The last bar is red and labeled "33% of context window". Dark background, cost label overlay.*

---

## Slide 2 — Two Types of Bloat

**Most teams only know about schema bloat. Response bloat is often worse.**

**Schema bloat** — token cost of loading tool *definitions* into context
> Happens upfront, every request. Predictable and measurable.

**Response bloat** — token cost of tool *outputs* flowing back
> A single HRIS or CRM API call can return hundreds of thousands of characters of raw JSON.
> Gets less attention — but often consumes more context than schemas.

> 🗣 *Є два типи token bloat, і більшість людей знає тільки про перший. Schema bloat — це вартість завантаження описів інструментів у контекст на початку кожного запиту. Але є ще response bloat: коли інструмент повертає відповідь, вона теж лягає в контекст. Один виклик до HRIS або CRM може повернути сотні тисяч символів сирого JSON. Це часто більша проблема, ніж schema bloat — але про неї не говорять, бо вона менш очевидна.*

> `[image prompt]` 🖼 *Two-panel diagram: left panel "Schema Bloat" showing tool definitions flooding into context at start; right panel "Response Bloat" showing massive JSON payload returning from a tool call. Both overwhelming the context bar. Dark background.*

---

## Slide 3 — CLI Solved This 50 Years Ago

> *"You connected 8 MCP servers. You haven't typed a word. 33% of your context is gone. CLI tools solved the exact same constraint decades ago."* — Miguel MS

**How CLI handles it:** hierarchical discovery on demand
```
git --help          → list of command groups
git commit --help   → details for that one command
```

**How MCP currently works:** dump the entire manual on startup.

> The fix is simple in principle: lazy discovery mode — expose one entry point, let the agent navigate to what it needs, pay only for what it actually uses.

> 🗣 *Є чудова аналогія. CLI-інструменти вирішили точно таку ж проблему 50 років тому: обмежений, дорогий інтерфейсний простір. Рішення — ієрархічний discovery. Ти вводиш `git --help`, бачиш список команд. Потім `git commit --help` — бачиш деталі конкретної команди. Ти платиш тільки за те, що реально досліджуєш. MCP поки що робить навпаки: викидає весь каталог інструментів у контекст при старті. Це як якби CLI при запуску друкував повну документацію на 600 сторінок перед кожною командою.*

> `[image prompt]` 🖼 *Split panel: left "CLI (50 years ago)" with clean hierarchical drill-down arrows (git → commit → --help). Right "MCP today" with all tool schemas dumped at once. "Same constraint, different solutions" label. Dark background.*

---

## Slide 4 — Four Approaches Compared

| Approach | Solves | Token savings | Effort |
|----------|--------|--------------|--------|
| **Schema compression** | Schema bloat | 50–97% | Low |
| **Search-first discovery** | Schema bloat | 85–97% | Medium |
| **Response filtering** | Response bloat | Varies | Medium |
| **Code-based execution** | Both | Max | High |

**Ready-to-use proxies today:**

- **mcp-compressor** *(Atlassian Labs)* — 70–97%, drop-in wrapper, no server changes
- **mcp-lazy-proxy** — 6.5–6.7× reduction, measures savings to `~/.mcp-proxy-metrics.jsonl`
- **Claude Code MCP Tool Search** *(v2.1.7+)* — auto-activates when tools exceed 10% of context, 85% reduction

> 🗣 *Є чотири підходи до вирішення. Schema compression — стискаємо описи інструментів перед тим, як вони потрапляють у контекст. Search-first discovery — модель бачить тільки два мета-інструменти і запитує схему конкретного інструменту за потребою. Response filtering — фільтруємо, що повертає інструмент, а не все підряд. Code-based execution — найрадикальніший: агент пише код фільтрації, який виконується на стороні сервера, в контекст потрапляє тільки результат. Перші два вирішують schema bloat, третій — response bloat, четвертий — обидва.*

> `[image prompt]` 🖼 *Four labeled approach cards in a 2x2 grid, each showing savings percentage and a small visual metaphor (scissors for compression, search icon for discovery, filter for response filtering, code brackets for code mode). Dark background.*

---

## Slide 5 — mcp-compressor in Depth

**Atlassian Labs, open source. Drop-in wrapper for any MCP server.**

Instead of sending all schemas upfront, exposes **two meta-tools:**
- `get_tool_schema(tool_name)` — fetch full schema on demand
- `invoke_tool(tool_name, input)` — execute it

**Compression levels:**

| Level | What model sees | Use case |
|-------|----------------|---------|
| `low` | Full descriptions | Unusual tools the model won't understand without context |
| `medium` *(default)* | First sentence only | Most servers |
| `high` | Tool name + param names | Large toolsets |
| `max` | `list_tools()` only | Rarely-used servers |

> Works with stdio, HTTP, SSE. Available in Python and TypeScript. Zero functionality loss.

> 🗣 *mcp-compressor від Atlassian Labs — найзрілніший готовий інструмент. Він стає проксі між вашим клієнтом і існуючим MCP-сервером. Модель бачить тільки два інструменти: отримати схему і викликати. Детальна схема підтягується тільки у момент, коли модель реально збирається щось викликати. Чотири рівні компресії — від "мінімальний текст" до "тільки список назв". Важлива деталь: функціональність не змінюється зовсім — агент все одно може отримати повну схему, просто не тримає її в контексті весь час.*
>
> 🗣 *Цікавий побічний ефект: mcp-compressor також робить user-configured third-party сервери безпечнішими за замовчуванням — бо модель не отримує повні описи потенційно шкідливих тулзів в контекст одразу. Це пов'язує token bloat і security в одне рішення.*

> `[image prompt]` 🖼 *Diagram: MCP Client → mcp-compressor proxy (labeled compression levels low/medium/high/max) → MCP Server. Arrow from proxy to model context shows "2 tools" instead of "90+ tools". On-demand fetch arrow from model back to proxy. Dark background.*

---

## Slide 6 — Key Takeaway

**Token bloat is a cost and quality problem — not just an engineering inconvenience.**

- Schema bloat is predictable — measure it before connecting servers to production
- Response bloat is often bigger — don't ignore tool outputs
- **Start with mcp-compressor on any server with 20+ tools**
- Claude Code handles this automatically — other clients don't

> Rule of thumb: if your tool schemas exceed 10% of context window → switch to search-first or compression.

> 🗣 *Практичний висновок. Token bloat — це дві окремі проблеми: вхідна і вихідна. Виміряйте обидві перед тим, як підключати сервери в production. mcp-compressor — це ваш перший інструмент для будь-якого сервера з 20+ тулзами, drop-in, без змін у серверному коді. І є важлива відмінність між клієнтами: Claude Code з версії 2.1.7 вирішує schema bloat автоматично. Cursor, Claude Desktop та інші — ні. Якщо ви будуєте для клієнта продукт на MCP — уточніть, який клієнт буде використовуватись, бо поведінка різна.*

> `[image prompt]` 🖼 *Clean summary card: two counters "Schema Bloat ✓" and "Response Bloat ✓" with tool icons. Below: "mcp-compressor" as primary recommendation badge. Dark background, minimal data-viz style.*
