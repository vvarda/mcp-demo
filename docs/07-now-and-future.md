# Block 7: Now & Future

---

## Slide 0 — Title Slide

**Now & Future**

> 🗣 *Фінальний блок — про те, що робити з усім цим прямо зараз. Що пілотити цього місяця, що зачекати, і куди весь цей стек рухається далі.*

> `[image prompt]` 🖼 *A timeline stretching from "Now" (bright, in focus) to "Future" (fading into the horizon). MCP logo prominent in the now zone, A2A and other protocols as distant silhouettes. Dark background, hopeful but grounded tone.*

---

## Slide 1 — Where We Are

**Past the peak of inflated expectations.**

> The tools that didn't work — complex multi-agent orchestrators without standardized interfaces — faded. What remained: reliable, simple MCP.

- April 2026: MCP is infrastructure, not experiment
- The protocol war is settled — the ecosystem is building on top
- Multi-agent autonomous workflows in production = still mostly demos and PoCs

> 🗣 *Де ми знаходимось у квітні 2026. Ми пройшли пік завищених очікувань. Складні мульти-агентні оркестратори без стандартизованих інтерфейсів відійшли на задній план. Залишилось те, що реально працює: надійний і простий MCP. Це хороша новина — означає, що зараз ми в зоні продуктивного впровадження, а не хайпу. Інвестиція в MCP сьогодні — це інвестиція в те, що вже стабільно, а не в те, що може змінитись завтра.*

> `[image prompt]` 🖼 *A Gartner-style hype cycle curve with a clear marker at "Slope of Enlightenment". "Multi-agent orchestrators" labeled at the trough. "MCP" labeled at the productive plateau. Dark background, clean data viz.*

---

## Slide 2 — MCP Is Not Alone: The Protocol Stack

**Four layers, four jobs.**

| Protocol | Layer | Answers |
|----------|-------|---------|
| **MCP** | Agent ↔ Tool / Data | How does an LLM call a tool? |
| **A2A** *(Google, Apr 2025)* | Agent ↔ Agent | How does one agent delegate to another? |
| **ACP** | Agent ↔ Editor | IDE-native agent communication (Zed, JetBrains, Neovim) |
| **WebMCP** | Agent ↔ Web | Early stage, under AAIF |

> All four are under or moving toward Linux Foundation. They complement, not compete.

> 🗣 *MCP — не єдиний протокол у стеку. Є чотири рівні, і кожен відповідає на своє питання. MCP — як LLM викликає інструмент. A2A від Google — як один агент делегує задачу іншому агенту. ACP — для комунікації агентів всередині редакторів: Zed, JetBrains, Neovim. WebMCP — як агент взаємодіє з вебом, найраніша стадія. Всі вони під Linux Foundation або рухаються туди. Це не конкуренція — це різні шари одного стеку.*

> `[image prompt]` 🖼 *A clean four-layer stack diagram: MCP (tools layer), A2A (agent-agent layer), ACP (editor layer), WebMCP (web layer). Each with a brief label and small icon. Dark background, layered architecture aesthetic.*

---

## Slide 3 — A2A: When It Becomes Relevant

**A2A v1.0 — Q1 2026. 150+ organizations in production.**

Already supported: Microsoft, AWS, Salesforce, SAP, ServiceNow, Google ADK, LangGraph, CrewAI, LlamaIndex, Semantic Kernel, AutoGen.

**How it works:**
- Each agent publishes an **Agent Card** — signed JSON at `/.well-known/agent-card.json`
- Capability negotiation + task lifecycle management between agents

> *"MCP first for sharing context; then A2A for dynamic interaction."* — ISG analyst

**When it's relevant for outsource teams:**
- Integrating multi-vendor enterprise systems (Salesforce ↔ ServiceNow ↔ SAP agents)
- **Not** when you're still building your first MCP setup

> 🗣 *A2A вже в production у 150+ організаціях. Але практична порада від аналітиків ISG звучить так: спочатку MCP для sharing context, потім A2A для dynamic interaction. Для аутсорс-команди у 2026 році — 95% use cases закриваються MCP. A2A стає цікавим тоді, коли ви інтегруєте multi-vendor enterprise: коли Salesforce-агент має делегувати задачу ServiceNow-агенту, а той — SAP-агенту. Якщо у вас ще немає базового MCP-досвіду — не починайте проєкт з A2A архітектури.*

> `[image prompt]` 🖼 *Agent Card concept diagram: three boxes (Salesforce Agent, ServiceNow Agent, SAP Agent) each with a /.well-known/agent-card.json label, connected by A2A delegation arrows. Dark background, enterprise architecture feel.*

---

## Slide 4 — Pilot This Month

**Start small. Measure one metric. Prove it.**

| Pilot | Setup | Measure |
|-------|-------|---------|
| **GitHub + Figma + Jira + Sentry** in one dev team | 1 tech lead + 2 devs | Time per ticket before vs after |
| **Playwright MCP** on one project without E2E coverage | 5 user flows | Coverage vs time budget |
| **Expo MCP** on one React Native project | 1 RN project from portfolio | Developer friction on device testing |

> Small scope. Measurable baseline. One team. One metric. That's enough to validate or kill the idea in 2–3 weeks.

> 🗣 *Що пілотувати вже цього місяця. Перший варіант — підключити GitHub, Figma, Jira і Sentry в IDE однієї невеликої команди: tech lead і двоє розробників. Вимірювати час на типовий тікет до і після. Другий — взяти один клієнтський проєкт без E2E-покриття і сформувати 5 user flows через Playwright MCP. Оцінити, скільки coverage можна отримати за розумний часовий бюджет. Третій — якщо є React Native в портфоліо, спробувати Expo MCP на одному проєкті. Ключовий принцип: маленький скоуп, вимірюваний baseline, одна команда, одна метрика. Цього достатньо, щоб за 2–3 тижні мати відповідь.*

> `[image prompt]` 🖼 *Three pilot cards arranged horizontally, each with a tool logo combo, team size icon, and a "measure" label with a simple metric. "Start this month" header in green. Dark background.*

---

## Slide 5 — Wait 2–4 Quarters

**Technically possible. Not yet production-safe.**

- ❌ **Self-hosted remote MCP servers for clients** — stateful HTTP doesn't scale yet; 2026 roadmap addresses this
- ❌ **Publishing client API as a public MCP server** — security surface too large, OAuth discipline not yet standardized
- ❌ **Multi-agent A2A workflows** — early production deployments, expensive to maintain
- ❌ **MCP as core architecture of a client-facing product** — fine for internal tools, not for consumer-facing without protocol SLA
- ❌ **Replacing QA teams with AI agents** — doesn't work; the economics don't add up

> 🗣 *А ось що не варто робити прямо зараз. Self-hosted remote MCP сервери для клієнтів — stateful HTTP поки погано масштабується, це в roadmap на 2026-й, але ще не готово. Публікація клієнтського API як публічного MCP-сервера — security surface завеликий, OAuth-дисципліна ще не стандартизована. Multi-agent A2A workflows в production — дорого підтримувати, поки це переважно демо. І, що важливо сказати прямо: повна заміна QA-команди AI-агентами не спрацьовує — економіка не сходиться.*

> `[image prompt]` 🖼 *Five items in a list, each with a red pause/clock icon instead of a checkmark. "Wait 2–4 quarters" header in amber. Dark background, calm warning aesthetic — not alarming, just honest.*

---

## Slide 6 — Key Takeaway

**MCP is the foundation of a new engineering discipline.**

> Companies that master building **secure, efficient, and scalable context networks** will lead in the era of autonomous AI.

For outsource teams specifically:
- **Standardize integration stacks** — use MCP servers instead of custom sync scripts
- **Invest in context security** — every `mcp.json` entry deserves the same review as a new PR
- **Use AI for QA and handoff** — Figma MCP + Playwright MCP can cut the most routine stages in half

> The shift: from **selling hours** → to **selling outcomes delivered by high-performance intelligent systems**.

> 🗣 *MCP у 2026-му — це фундамент нової інженерної дисципліни. Компанії, які першими опанують мистецтво будувати безпечні та ефективні контекстні мережі, будуть попереду. Для аутсорс-бізнесу це означає конкретний зсув: від продажу годин до продажу результату. Не "ми витратили 40 годин", а "ось результат, досягнутий за допомогою системи, яка дозволяє нам робити це ефективно". Це і є конкурентна перевага, яку дає MCP — не технічна іграшка, а новий спосіб deliverable.*

> `[image prompt]` 🖼 *A transformation visual: left side shows a clock with "hours" label (fading), right side shows a trophy/deliverable with "outcomes" label (glowing). Below, three pillars: "Standardize", "Secure", "Automate routines". Dark background, forward-looking aesthetic.*
