# Block 4: Delivery / PM / Observability

---

## Slide 0 — Title Slide

**Delivery / PM / Observability**

> 🗣 *Цей блок — для PM'ів, delivery leads і тих, хто хоче показати цінність MCP не технічній аудиторії. Менше перемикань між вкладками. Менше ручного синтезу. Більше часу на реальні рішення.*

> `[image prompt]` 🖼 *Connected nodes representing Jira, Sentry, GitHub, Figma all feeding into a central AI agent. Abstract delivery pipeline aesthetic, dark background, project management vibes.*

---

## Slide 1 — The Core Idea

**MCP works best as context federation — not autonomous execution.**

> The real value: AI that sees **ticket + design + code + CI + incidents** simultaneously — not just the file you have open.

- Reduces role friction: less tab switching between Jira, Sentry, GitHub, Figma
- Strongest effect in **synthesis work** — summarizing, correlating, triaging
- Not a replacement for PM or QA — a reduction in "management minutes" per day

> 🗣 *Головна ідея, яку важливо донести: MCP найкраще працює не як автономний агент, а як федерація контексту поверх вашого існуючого workflow. Цінність не в тому, що AI щось робить замість людини — а в тому, що він бачить усе одночасно: тікет, дизайн, код, CI-логи, помилки з production. Людина теж може це все зібрати — але витратить 40 хвилин на перемикання між вкладками.*

> `[image prompt]` 🖼 *A human silhouette in the center surrounded by 6 tool icons (Jira, GitHub, Figma, Sentry, docs, CI). Arrows all converge into one AI context window. "Context federation" label. Dark mode.*

---

## Slide 2 — Three Scenarios That Actually Work

**Bug triage:**
> `root cause this Sentry issue: <link>` → stack trace + breadcrumbs + release info → hypothesis + fix → GitHub issue created, linked to Sentry

**Sprint planning:**
> `read Figma file <link> and break it into Jira epics and tasks by pages and organisms` → structured backlog in minutes
> ⚠️ Generated structure needs review — AI doesn't know your estimates or domain dependencies

**Release investigation:**
> `look at recent production errors this week, correlate with the last 3 releases, find hotfix candidates` → Sentry MCP + GitHub MCP → suspect PRs surfaced

> 🗣 *Три сценарії, де це вже реально працює. Перший: bug triage — від посилання на Sentry до GitHub issue з повним контекстом за одну LLM-сесію. Другий: sprint planning — Figma-файл перетворюється на структурований Jira backlog. Є навіть готовий skill "Figma-to-Jira". Але застереження: AI не знає ваші estimates і домен — це відправна точка, не фінальна версія. Третій: release investigation — агент сам корелює помилки з релізами і знаходить підозрювані PR'и. Sentry і GitHub разом.*

> `[image prompt]` 🖼 *Three vertical scenario cards side by side: "Bug Triage" (Sentry→GitHub arrow), "Sprint Planning" (Figma→Jira arrow), "Release Investigation" (Sentry+GitHub→PR list). Minimal card design, dark background.*

---

## Slide 3 — High-ROI Use Cases for Outsource Teams

| Use Case | What AI does | Measurable KPI |
|----------|-------------|----------------|
| **Engineering copilot** | Ticket + design + code + CI in one workflow | Onboarding time ↓ |
| **QA triage copilot** | Duplicate clustering, regression suspects, probable owner | Triage time ↓ |
| **Delivery / PM copilot** | Weekly risk digest, sprint summaries from commits + tickets | Summary prep time ↓ |
| **Presales / estimation** | Compare new RFP against 3 similar past delivery cases | Proposal prep time ↓ |

> 🗣 *Для аутсорс-команди є чотири use case'и з реальним ROI. Engineering copilot — особливо корисний для онбордингу: новий розробник отримує контекст проекту за годину, а не за тиждень. QA triage — не "замінює QA", а зменшує toil: дублі, регресії, підозрювані власники. Delivery copilot — найсильніший ефект у PM'ів, які щотижня вручну збирають статус з Jira, GitHub і Slack. Presales — порівняння нового RFP зі схожими проектами з архіву.*

> `[image prompt]` 🖼 *2x2 grid of use case cards, each with an icon and one-line description. Bottom right corner of each card shows a KPI arrow going down (time saved). Dark background, professional tone.*

---

## Slide 4 — Observability Layer

**Sentry AI Monitoring — track your agents in production.**

- Monitors AI agent workflows: tool calls, LLM interactions, token usage, latency
- Full trace of what the agent did and why
- If you're putting agents in production — this is the **first thing you deploy**

> Sellable story to clients: *"your bug → triage → assigned → PR — in one LLM session, with full audit trail"*

> 🗣 *Окремий шар, про який часто забувають: observability для самих агентів. Sentry AI Monitoring у 2026-му сам відстежує agent workflows — які tools викликались, скільки токенів витрачено, де затримки, де помилки. Якщо ви ставите агентів у production для клієнта, це перше, що треба налаштувати. Без цього ви сліпі.*
>
> 🗣 *І це ж — продаваний наратив для клієнта: "ваш баг — тріаж — assigned — PR за одну LLM-сесію, з повним audit trail". Це не фантастика, це вже реальний workflow.*

> `[image prompt]` 🖼 *A dashboard-style visualization showing AI agent trace: tool call chain (Sentry→GitHub→Jira) with timing, token count, and status indicators. Dark mode, monitoring/observability aesthetic.*

---

## Slide 5 — Anti-patterns

**Don't say:**
- ~~"This will replace senior developers"~~
- ~~"We can fully automate QA with MCP agents"~~
- ~~"PM gets a single source of truth automatically"~~

**Don't do:**
- Allow branch mutations or deploy actions without human approval
- Trust AI-generated severity or root cause without validation
- Ask AI to generate delivery status without access to actual primary artifacts

> 🗣 *Кілька речей, яких варто уникати — і в тому, що казати клієнту, і в тому, що будувати. "Замінить senior dev" — ні, і такий наратив підриває довіру. "Автоматизуємо QA повністю" — ні, зменшуємо toil. "PM отримає single source of truth" — тільки якщо джерела вже узгоджені між собою. Якщо Jira та реальність розходяться — AI узгоджено відтворить цей хаос.*

> `[image prompt]` 🖼 *Three crossed-out statement cards in red on the left ("Replace senior devs", "Automate QA fully", "Auto single source of truth"), contrasted with corrected green versions on the right. Dark background, clear visual contrast.*

---

## Slide 6 — Key Takeaway

**Frame pilots as workflow augmentation — not automation moonshot.**

1. **Start with read-heavy flows** — search, summarize, correlate
2. **Write actions only through approval** — never autonomous writes on day one
3. **Pick a use case with a measurable baseline:**

| KPI | Example |
|-----|---------|
| Triage time | From 2h to 20min per bug |
| Onboarding time | From 5 days to 1 day |
| Summary prep time | From 60min to 5min per week |
| Proposal prep time | From 3h to 45min per RFP |

> 🗣 *Як правильно підходити до пілоту з MCP. По-перше: починати з read-heavy flows — читати, шукати, агрегувати, резюмувати. Ніяких write-дій без підтвердження людини на першому етапі. По-друге: вибрати use case, де є вимірюваний baseline. Не "буде краще", а конкретно: triage займав дві години — стало 20 хвилин. Онбординг займав 5 днів — став один день. Це те, що можна показати клієнту і зафіксувати як цінність.*

> `[image prompt]` 🖼 *A before/after timeline comparison showing key metrics (triage time, onboarding, summary prep) decreasing from left to right. Clean data visualization style, dark background, green accent for "after" values.*
