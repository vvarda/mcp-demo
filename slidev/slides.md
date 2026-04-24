---
theme: default
title: "MCP for LLMs — Full Presentation"
info: All 8 blocks — MCP Presentation 2026
colorSchema: dark
highlighter: shiki
transition: slide-left
fonts:
  sans: Inter
  mono: Fira Code
---

# MCP & The State of AI Tooling in 2026

<div class="pt-12 text-gray-400 text-sm">Block 1 of 8</div>

<img src="/slide-00-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
🎤 PRESENTER SHORTCUTS:
  S — open Presenter View (notes on second screen)
  F — fullscreen
  D — toggle dark/light mode
  B — blackout screen (pause moment)
  ←/→ — prev / next slide
  Esc — overview / slide grid

Цей блок — про те, що таке MCP, чому воно виграло і де ми зараз. Швидкий огляд перед тим, як заглиблюватись у деталі.
-->

---
layout: two-cols
---

# What Is MCP?

**Model Context Protocol**

- Open standard for **agent ↔ tool / data source** communication
- Announced by Anthropic — **November 2024**
- Transferred to Linux Foundation (via AAIF) — **December 2025**
- Technical core: **JSON-RPC 2.0** over stdio / Streamable HTTP

::right::

<img src="/slide-01-what-is-mcp.png" class="rounded-xl mt-4 w-full" />

<!--
MCP — це відкритий протокол, який стандартизує, як AI-агенти підключаються до інструментів і джерел даних. Anthropic запустили його у листопаді 2024, а вже через рік передали під крило Linux Foundation — тобто це вже не проєкт однієї компанії.

Технічно — це просто JSON-RPC 2.0. Тобто звичайні JSON-повідомлення у форматі "запит → відповідь" або "сервер надсилає подію". Нічого магічного. Поверх цього є два транспорти: stdio — для локальних процесів, коли клієнт і сервер на одній машині спілкуються через стандартний ввід/вивід; і Streamable HTTP — для віддалених серверів через звичайний HTTP із підтримкою стримінгу. Важливо розуміти: MCP не диктує, що саме робить інструмент — він лише стандартизує, як його знайти і як викликати.
-->

---
layout: two-cols
zoom: 0.9
---

# MCP Servers in the Wild

<div class="space-y-3 text-sm">

<div>
  <div class="text-xs text-blue-400 uppercase tracking-widest font-semibold mb-1">🌐 Web / Frontend</div>
  <div class="grid grid-cols-2 gap-x-3 gap-y-0.5 text-xs">
    <div><strong>GitHub</strong> — PRs, CI, code</div>
    <div><strong>Figma</strong> — design → code</div>
    <div><strong>Context7</strong> — live docs</div>
    <div><strong>Playwright</strong> — E2E, A11y</div>
    <div><strong>Sentry</strong> — errors, traces</div>
    <div><strong>Jira / Linear</strong> — tickets</div>
  </div>
</div>

<div>
  <div class="text-xs text-teal-400 uppercase tracking-widest font-semibold mb-1">⚙️ Backend</div>
  <div class="grid grid-cols-2 gap-x-3 gap-y-0.5 text-xs">
    <div><strong>PostgreSQL MCP</strong> — queries, schema</div>
    <div><strong>Docker MCP</strong> — containers</div>
    <div><strong>Supabase MCP</strong> — DB + auth</div>
    <div><strong>AWS MCP</strong> — S3, Lambda, CF</div>
    <div><strong>Redis MCP</strong> — cache ops</div>
    <div><strong>Prisma MCP</strong> — ORM + migrations</div>
  </div>
</div>

<div>
  <div class="text-xs text-purple-400 uppercase tracking-widest font-semibold mb-1">📱 Mobile — iOS · Android · Flutter</div>
  <div class="grid grid-cols-2 gap-x-3 gap-y-0.5 text-xs">
    <div><strong>Mobile MCP</strong> — real devices + sim</div>
    <div><strong>Xcode MCP</strong> — builds, schemes</div>
    <div><strong>Appium MCP</strong> — cross-platform</div>
    <div><strong>ADB MCP</strong> — Android shell</div>
  </div>
  <div class="text-xs text-gray-500 mt-1">Flutter → Mobile MCP через semantic tree (FlutterDriver-сумісний)</div>
</div>

</div>

<div class="mt-2 text-teal-400 text-xs font-medium">One server = one capability. Mix as needed.</div>

::right::

<img src="/slide-01b-servers.png" class="rounded-xl w-full" />

<!--
Конкретні приклади — щоб "MCP" не залишалось абстракцією.

🌐 Веб/фронтенд — базовий стек: GitHub + Context7 + Playwright + Sentry. Це чотири сервери, які дають результат з першого дня. Figma — наступний крок.

⚙️ Бекенд — є офіційні сервери від провайдерів: PostgreSQL MCP від Supabase/community, Supabase MCP — офіційний, дуже активний. Docker MCP — офіційний від Docker Inc., AWS MCP — від AWS. Redis і Prisma — популярні community-сервери для тих, хто використовує ці інструменти. Принцип той самий: агент може читати схему БД, запускати міграції, перевіряти стан контейнерів.

📱 Мобільна розробка — Mobile MCP підтримує iOS (через WebDriverAgent) і Android (через ADB + UIAutomator). Він використовує нативне дерево доступності — не комп'ютерний зір. Xcode MCP дає доступ до builds, schemes, симуляторів. ADB MCP — прямий shell до Android-пристроїв.

Flutter: Mobile MCP працює з Flutter-додатками через Semantic Tree — Flutter підтримує accessibility tree, тому Mobile MCP може взаємодіяти з елементами без screenshot-аналізу. Для більш складних сценаріїв — Appium MCP зі Flutter plugin. Повноцінного офіційного Flutter MCP поки немає — Mobile MCP є практичним рішенням.

Важливо: кожен сервер — незалежна одиниця. Команда підключає те, що потрібно для свого стека. Немає локіну на одного вендора.
-->

---

# The N×M Problem

<img src="/slide-02-nxm-problem.png" class="w-full rounded-xl mt-2" style="max-height: 370px; object-fit: contain; object-position: top;" />

<div class="grid grid-cols-2 gap-8 mt-4 text-sm">
  <div class="text-red-400">N tools × M clients = <strong>N×M</strong> custom integrations</div>
  <div class="text-green-400">N tools + M clients = <strong>N+M</strong> standard connectors</div>
</div>

<!--
Every AI vendor had its own plugin format. Every service had its own API shape. MCP replaced that chaos with one contract.

До MCP кожна пара "інструмент + клієнт" потребувала окремої інтеграції. Якщо у вас 10 інструментів і 10 клієнтів — це 100 з'єднань. MCP зводить це до 20. Саме цю проблему він і вирішував з першого дня.

Хороша аналогія — USB. До появи USB кожен виробник мав свій роз'єм: принтер, мишка, клавіатура — всі різні. USB-C замінив це одним стандартом. MCP — те саме для AI-агентів. Тепер не важливо, який агент: Cursor, Claude Desktop, GitHub Copilot — всі говорять однією мовою з інструментами. Розробнику достатньо написати MCP-сервер один раз, і він одразу працює з усіма клієнтами.

Ще один кут зору для тих, хто думає про бізнес-вплив: до MCP кожна команда витрачала час на написання "glue code" — прослоїв між AI і своїми інструментами. GitHub Copilot не знав, як відкрити тікет у Jira. Claude не міг читати логи з Sentry. Кожне підключення — окремий проєкт. З MCP ці інтеграції стають одноразовими і переносними. Той, хто написав Sentry MCP — написав його для всіх. Це зрушує центр ваги з "як підключити" на "що робити після підключення".
-->

---
layout: two-cols
---

# Industry Adoption

**All major AI vendors under one standard**

| Vendor | Adopted |
|---|---|
| Anthropic | Nov 2024 *(creator)* |
| OpenAI | Mar 2025 |
| Google DeepMind | Apr 2025 |
| Microsoft, AWS, Cloudflare | 2025 |

<div class="mt-4 p-3 bg-blue-900/40 rounded-lg border border-blue-500/30 text-sm">

**AAIF:** 146 members — Google, Microsoft, AWS, Bloomberg, JPMorgan, Autodesk and more.

</div>

::right::

<img src="/slide-03-industry.png" class="rounded-xl mt-4 w-full" />

<!--
Те, що всі топові AI-вендори підписались під одним стандартом — це безпрецедентно. Ще рік тому OpenAI і Anthropic мали несумісні формати для tool calls. Зараз вони в одному фонді під Linux Foundation.

Важливо розуміти швидкість: Anthropic запустили MCP у листопаді 2024. OpenAI приєднались у березні 2025 — тобто через 4 місяці. Google — через 5. Це неймовірно швидко для індустрії, де стандарти зазвичай узгоджуються роками. Для порівняння: REST став де-факто стандартом через ~7 років після появи, GraphQL — через ~4 роки. MCP пройшов цей шлях менш ніж за рік.

Склад AAIF говорить про щось важливе: разом з tech-компаніями там Bloomberg, JPMorgan, American Express, Autodesk. Фінансові та enterprise-гравці зайшли дуже рано. Для них стандартизація критична — вони не можуть дозволити собі ставити на конкуруючі несумісні формати і переписувати інтеграції кожні 2 роки. Їх присутність в AAIF — це сигнал ринку, що MCP не зникне.

Що це означає для команди прямо зараз: якщо ви будуєте MCP-сервер або інтегруєте MCP у свій продукт — ви будуєте на стандарті, який підтримують і OpenAI, і Google, і Microsoft одночасно. Це перший раз в AI-індустрії, коли таке відбувається. Ризик "поставив на невірного коня" практично нульовий.
-->

---
layout: two-cols
---

# Ecosystem Growth

| Metric | Nov 2024 | Apr 2025 | Apr 2026 |
|---|---|---|---|
| Public servers | ~50 | ~700 | 1,000+ |
| SDK downloads / mo | < 1M | ~50M | 150M+ |
| Standardization | Proprietary | Community | Linux Foundation |
| Transport | STDIO | HTTP + SSE | Streamable HTTP |

<div class="mt-4 text-teal-400 font-bold">
  17 months · 50 → 1,000+ servers
</div>

::right::

<img src="/slide-04-growth.png" class="rounded-xl mt-4 w-full" />

<!--
За 17 місяців — від 50 серверів до тисячі офіційних і незліченної кількості приватних. Від менше мільйона завантажень до 150 мільйонів на місяць. Такої швидкості адопції в нашій індустрії майже не буває.
-->

---
layout: two-cols
---

# Market & Forecast

<div class="p-4 bg-gray-800/60 rounded-xl border border-gray-600/40 mb-4">
  <div class="text-4xl font-bold text-blue-400 mb-1">$10.3B</div>
  <div class="text-sm text-gray-300">MCP-compatible solutions market *(Apr 2026)*</div>
</div>

<div class="p-4 bg-gray-800/60 rounded-xl border border-gray-600/40 mb-4">
  <div class="text-4xl font-bold text-teal-400 mb-1">34.6%</div>
  <div class="text-sm text-gray-300">CAGR projected growth</div>
</div>

- **75%** of API gateway vendors — MCP by end of 2026 *(Gartner)*
- **50%** of iPaaS vendors — same timeline

::right::

<img src="/slide-05-market.png" class="rounded-xl mt-4 w-full" />

<!--
Ринок вже оцінюється в понад 10 мільярдів доларів із темпом зростання 35% на рік. За прогнозом Gartner, до кінця 2026-го більшість API-шлюзів і платформ інтеграцій матимуть підтримку MCP з коробки.
-->

---
layout: two-cols
---

# The Protocol War Is Over.

<div class="text-2xl font-bold text-yellow-400 mt-2 mb-6">MCP won the tool layer.</div>

<div class="space-y-3">
<div class="p-3 bg-gray-800/60 rounded-lg border border-gray-600/40">
  <div class="text-blue-400 font-bold mb-1">17 months</div>
  <div class="text-sm text-gray-300">Anthropic experiment → Linux Foundation standard</div>
</div>
<div class="p-3 bg-gray-800/60 rounded-lg border border-gray-600/40">
  <div class="text-teal-400 font-bold mb-1">A2A</div>
  <div class="text-sm text-gray-300">Google's agent↔agent layer — complementary, not competing</div>
</div>
<div class="p-3 bg-gray-800/60 rounded-lg border border-gray-600/40">
  <div class="text-green-400 font-bold mb-1">Safe bet</div>
  <div class="text-sm text-gray-300">Betting on MCP is safe as of Q2 2026</div>
</div>
</div>

::right::

<img src="/slide-06-takeaway.png" class="rounded-xl mt-4 w-full" />

<!--
Коротко: протокольна війна завершилась. MCP виграв на рівні інструментів. Зараз можна будувати на MCP без ризику поставити на переможця-одинака.

Окремо варто сказати про A2A — Agent-to-Agent протокол від Google, анонсований у квітні 2025. Якщо MCP вирішує задачу "агент ↔ інструмент або дані", то A2A — це про те, як один агент може делегувати задачу іншому агенту. Тобто це вищий рівень: не "дістань мені дані з бази", а "вирішуй цю підзадачу, агенте, і поверни результат". Обидва протоколи зараз під Linux Foundation, вони доповнюють, а не замінюють один одного. MCP — горизонтальний шар між агентом і світом інструментів, A2A — вертикальний шар між агентами в багатоагентних системах.
-->

---
layout: center
class: text-center
transition: fade
---

# Anatomy of MCP

<div class="text-gray-400 mt-4">Block 2 of 8</div>

<img src="/b2-s0-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
Тепер заглибимось у те, як MCP влаштований зсередини. Три рівні, три примітиви, два транспорти — і жодної магії.
-->

---
layout: two-cols
---

# The Architecture

**Host → Client → Server**

| Layer | Role | Examples |
|---|---|---|
| **Host** | User-facing environment | Cursor, Claude Desktop, Claude Code |
| **Client** | Communication layer | Discovers tools, passes to model |
| **Server** | Exposes capabilities | Filesystem, GitHub, Figma, Jira |

::right::

<img src="/b2-s1-architecture.png" class="rounded-xl mt-4 w-full" />

<!--
Три рівні. Хост — це те, з чим працює користувач: IDE, термінал, чат. Клієнт — прошарок усередині хоста. Сервер — легковажний застосунок, який відкриває доступ до чогось конкретного. Сервер нічого не знає про модель.
-->

---
layout: two-cols
---

# Three Primitives

| Primitive | What it is | Controlled by |
|---|---|---|
| **Tools** | Executable actions | Model |
| **Resources** | Read-only data via URI | App |
| **Prompts** | Reusable templates | User |

<div class="mt-4 p-3 bg-amber-900/30 rounded-lg border border-amber-500/30 text-sm">
Most public servers expose <strong>tools only</strong> — resources and prompts are underutilized.
</div>

::right::

<img src="/b2-s2-primitives.png" class="rounded-xl mt-4 w-full" />

<!--
Три примітиви — це все, що сервер може надати. Tools — це дії. Resources — read-only дані за URI. Prompts — готові шаблони. На практиці 90% серверів реалізують тільки tools.

🔧 Tools — це кнопки, які AI може натискати сам. Ти описуєш що вміє кожна кнопка звичайною мовою, і модель сама розбирається коли і яку натиснути. Наприклад: "відкрити PR", "запустити тест", "знайти помилку в Sentry". Важлива деталь: якщо опис розмитий — модель або викликає не те, або взагалі ігнорує. Тому чим точніше написано що робить tool — тим краще поведінка агента.

📄 Resources — це документи, які AI може читати, але не змінювати. Уяви як папка з read-only файлами: архітектурні рішення, специфікації API, онбординг-документи. Замість того щоб щоразу вручну вставляти їх у чат — вони просто завжди доступні агенту. На практиці цю можливість майже ніхто не використовує, хоча саме тут є великий потенціал.

💬 Prompts — це збережені рецепти. Замість того щоб щоразу писати той самий довгий запит — зберігаєш його один раз і запускаєш командою. Наприклад: `/review-pr` завжди перевіряє код за однаковим чеклістом, `/explain-error` — завжди пояснює помилку за одним шаблоном. Корисно для команд, де важливо щоб усі робили речі однаково.
-->

---
layout: two-cols
---

# MCP vs Function Calling

**Not the same thing.**

- **Function calling** — how the LLM invokes a function
- **MCP** — how functions are registered, described, and discovered

<div class="mt-6 p-4 bg-blue-900/30 rounded-lg border border-blue-500/30">
MCP sits <em>above</em> function calling. It's the layer that makes tools <strong>discoverable</strong>.
</div>

::right::

<img src="/b2-s3-vs-function-calling.png" class="rounded-xl mt-4 w-full" />

<!--
Function calling — це механізм усередині моделі. MCP — це шар вище: як інструменти взагалі потрапляють у контекст моделі. MCP — реєстрація і discovery, function calling — виклик.

Проста аналогія: уяви ресторан. Function calling — це момент, коли офіціант приходить до столика і каже "ваше замовлення". MCP — це меню. Без меню офіціант не знає що пропонувати, а ти не знаєш що замовляти. MCP — це те, як інструменти взагалі потрапляють у "меню" моделі.

Технічно: коли ти підключаєш MCP-сервер, клієнт робить один запит і отримує список всіх tools з описами. Ці описи вставляються прямо в системний промпт — модель їх "читає" разом із твоїм повідомленням і вже знає що може зробити. Далі function calling — це просто деталь реалізації того, як модель оформляє виклик. GPT, Claude, Gemini — у всіх різний синтаксис function calling, але MCP над ними — один і той самий.

Практичний висновок для розробника: якщо хтось питає "навіщо MCP якщо є function calling" — відповідь проста. Function calling вирішує як викликати функцію яку ти вже знаєш. MCP вирішує як дізнатись які функції взагалі існують, підключити їх стандартно і не переписувати це для кожної моделі окремо.
-->

---
layout: two-cols
---

# Three Transports

| Transport | Use case | Latency |
|---|---|---|
| **STDIO** | Local process, same machine | ~1ms |
| **Streamable HTTP** | Remote / cloud server | 10–100ms |
| **Custom** | gRPC, WebSocket — IoT, Edge | varies |

<div class="mt-4 text-sm text-gray-400">
Streamable HTTP replaced HTTP+SSE in 2026. Sessions tracked via <code>Mcp-Session-Id</code> header.
</div>

::right::

<img src="/b2-s4-transports.png" class="rounded-xl mt-4 w-full" />

<!--
STDIO — найпростіший: клієнт запускає сервер як дочірній процес через stdin/stdout. Streamable HTTP у 2026-му повністю витіснив старий HTTP+SSE. Custom transports — gRPC або WebSocket для IoT або Edge.

🖥 STDIO — дві програми "розмовляють" через термінал, ніяких портів і налаштувань. Саме тому це перше що пробують — один рядок і вже підключено. Мінус: тільки локально, не для команди.

🌐 Streamable HTTP — сервер живе в інтернеті, доступний для всієї команди. Замінив старий формат у 2026, тепер є стандартний заголовок для сесій. Це production-варіант для хмарних серверів.

⚙️ Custom — для enterprise: gRPC, WebSocket, IoT. 99% команд ніколи не торкаються, але важливо що протокол не прив'язаний до транспорту — можна зробити будь-що за потреби.
-->

---
layout: two-cols
---

# How Discovery Works

1. Server describes its tools in **JSON Schema**
2. Client connects → fetches the list
3. LLM receives tool definitions as part of its **context window**
4. Model decides what to call — and when

<div class="mt-4 p-3 bg-gray-800/60 rounded-lg text-sm">
<strong>Example:</strong> Sentry MCP exposes <code>search_issues</code>, <code>get_issue_details</code>.<br/>
User: "root cause this issue" → model picks <code>get_issue_details</code>.
</div>

::right::

<img src="/b2-s5-discovery.png" class="rounded-xl mt-4 w-full" />

<!--
Сервер описує кожен tool у JSON Schema. При підключенні клієнт запитує список. Всі описи потрапляють у контекстне вікно моделі. Далі модель сама обирає, який tool викликати і з якими аргументами.

Важлива деталь — це відбувається автоматично кожен раз при старті. Ти не переписуєш нічого вручну. Сервер оновився, додав новий tool — наступного разу при підключенні модель вже про нього знає.
-->

---
layout: center
class: text-center
---

# Remember the Three Primitives

<div class="grid grid-cols-3 gap-6 mt-8">
  <div class="p-6 bg-blue-900/40 rounded-xl border border-blue-500/30">
    <div class="text-3xl mb-2">🔧</div>
    <div class="text-xl font-bold text-blue-400">Tools</div>
    <div class="text-sm text-gray-300 mt-1">do something</div>
  </div>
  <div class="p-6 bg-teal-900/40 rounded-xl border border-teal-500/30">
    <div class="text-3xl mb-2">📄</div>
    <div class="text-xl font-bold text-teal-400">Resources</div>
    <div class="text-sm text-gray-300 mt-1">read something</div>
  </div>
  <div class="p-6 bg-purple-900/40 rounded-xl border border-purple-500/30">
    <div class="text-3xl mb-2">💬</div>
    <div class="text-xl font-bold text-purple-400">Prompts</div>
    <div class="text-sm text-gray-300 mt-1">template something</div>
  </div>
</div>

<div class="mt-8 text-xl text-gray-300">MCP ≈ LSP for AI agents — same pattern, same transport (JSON-RPC 2.0)</div>

<!--
Якщо запам'ятати одне з цього блоку — три примітиви. І аналогія: MCP — це LSP, але для AI-агентів. Той самий патерн. Якщо ти конфігурив language server у VS Code — ти вже розумієш архітектуру MCP.

LSP — Language Server Protocol — це стандарт, який Microsoft запустили у 2016 році. До нього кожен редактор (VS Code, Vim, Emacs, IntelliJ) мав писати підтримку кожної мови окремо: автодоповнення для Python, підсвітка помилок для TypeScript, go-to-definition для Go — все своє для кожної пари. N редакторів × M мов = N×M реалізацій. Звучить знайомо?

LSP вирішив це одним протоколом: мовний сервер запускається як окремий процес, редактор підключається і запитує "що ти вмієш?" — отримує список можливостей. Далі все через JSON-RPC: "де визначення цієї змінної?", "які є автодоповнення?", "що означає ця помилка?". Написав один Python LSP — він одразу працює у VS Code, Vim, Neovim і будь-якому іншому редакторі.

MCP — це та сама ідея, один рівень вище. Замість "редактор ↔ мовний сервер" маємо "AI-агент ↔ інструмент". Той самий JSON-RPC, та сама архітектура клієнт-сервер, те саме discovery при підключенні. Навіть проблема ідентична: до MCP кожен AI-клієнт писав інтеграції з кожним інструментом окремо.
-->

---
layout: center
class: text-center
transition: fade
---

# Web & Mobile Ecosystem

<div class="text-gray-400 mt-4">Block 3 of 8</div>

<img src="/b3-s0-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
Цей блок — про конкретні MCP-сервери, які вже зараз змінюють щоденну роботу web і mobile команд. Не теорія — інструменти, які можна поставити сьогодні.
-->

---
layout: two-cols
class: text-sm
---

# The Stack at a Glance

| Server | What it unlocks |
|---|---|
| **GitHub MCP** | PR review, CI logs, rerun workflows |
| **Context7** | Live, version-specific docs |
| **Figma Dev Mode MCP** | Real design structure → code |
| **Playwright MCP** | E2E tests, A11y audits |
| **Chrome DevTools MCP** | Live inspection, performance |
| **Mobile MCP** | iOS & Android automation |
| **Sentry MCP** | Full issue context, no copy-paste |

::right::

<img src="/b3-s1-stack.png" class="rounded-xl mt-4 w-full" />

<!--
Є кілька серверів, які формують базовий стек для web і mobile команди у 2026. Давайте пройдемось по кожному.
-->

---
layout: two-cols
---

# GitHub MCP

**Official. Rewritten in Go. Production-ready.**

- Review PRs, read CI logs, rerun failed workflows — all from the agent
- OAuth support in VS Code
- Start here — it's the first server most teams connect

<div class="mt-4 p-3 bg-green-900/30 rounded-lg border border-green-500/30 text-sm">
No more switching between IDE and GitHub in the browser.
</div>

::right::

<img src="/b3-s2-github.png" class="rounded-xl mt-4 w-full" />

<!--
GitHub MCP — офіційний, переписаний на Go. Агент може переглядати PR, читати логи CI, перезапускати workflow'и. З нього починає більшість команд.
-->

---
layout: two-cols
---

# Context7: Kill Hallucinations

**9,000+ libraries. Always version-correct.**

- Fetches **live, version-specific docs** into the model's context
- Trigger: add `use context7` to any prompt
- No API key — `npx`-installable in seconds

<div class="mt-4 font-mono text-sm bg-gray-800/60 rounded p-3">
resolve-library-id → get-library-docs → injected into context
</div>

::right::

<img src="/b3-s3-context7.png" class="rounded-xl mt-4 w-full" />

<!--
Context7 від Upstash вирішує одну проблему: LLM галюцинує на версіях API. Тягне актуальну документацію прямо в контекстне вікно. Можна додати в Cursor Rules — і він буде активним завжди.

Чому це взагалі проблема? Модель навчена на даних минулого року. Вона може написати код для React 18, коли у тебе React 19, або використати старий синтаксис Next.js App Router. Ти витрачаєш час на дебаг помилок, яких не існує в актуальній версії — бо модель просто не знала про зміни.

Як Context7 це вирішує: два виклики — спочатку `resolve-library-id` (знаходить бібліотеку по назві), потім `get-library-docs` (тягне актуальні доки для конкретної версії). Все це потрапляє в контекст перед тим як модель починає генерувати код. 9000+ бібліотек уже проіндексовані.

Практичний лайфхак: замість того щоб щоразу писати "use context7" у промпті — додай `use context7` один раз у Cursor Rules або системний промпт. Тоді він працює автоматично для всього проєкту без будь-яких додаткових дій від розробника.
-->

---
layout: two-cols
---

# Figma Dev Mode MCP

**Agent reads the real design — not a screenshot.**

- Live layer structure, design tokens, component variants
- Generates React / Vue code **~95% on first iteration**

<div class="mt-4 p-3 bg-amber-900/30 rounded-lg border border-amber-500/30 text-sm">
⚠️ <strong>Rate limits are a financial decision:</strong><br/>
Starter: 6/month · Organization: 200/day · Enterprise: 600/day
</div>

::right::

<img src="/b3-s4-figma.png" class="rounded-xl mt-4 w-full" />

<!--
Агент читає справжнє дерево шарів, токени дизайн-системи, варіанти компонентів. Результат: код, який відповідає дизайну на 95%. Rate limits — перевірте план перед підключенням для команди.
-->

---
layout: two-cols
class: text-sm
---

# Playwright vs Chrome DevTools

> "Playwright drives. Chrome DevTools debugs." — Steve Kinney

| | Playwright MCP | Chrome DevTools MCP |
|---|---|---|
| **Purpose** | Drive the browser | Inspect the live page |
| **Actions** | Click, fill, navigate | LCP, network, performance |
| **Testing** | E2E, cross-browser | A11y audits, debugging |
| **Under the hood** | Playwright engine | Puppeteer |

<div class="mt-3 text-amber-400 text-sm">⚠️ Pick one per job — don't install both "just in case".</div>

::right::

<img src="/b3-s5-playwright-devtools.png" class="rounded-xl mt-4 w-full" />

<!--
Playwright веде браузер. Chrome DevTools дебажить живу сторінку. Для аутсорс-команд Playwright найбільш цінний для автоматичної генерації E2E-тестів і A11y-аудитів.
-->

---
layout: two-cols
---

# Mobile MCP

**Describe the scenario. The agent handles iOS & Android.**

- Works on simulators, emulators, **real devices**
- Uses **native accessibility tree** first — no computer vision required
- Falls back to screenshot analysis when a11y labels are missing

```
mobile_list_elements_on_screen
mobile_click_on_screen_at_coordinates
mobile_swipe_on_screen
mobile_take_screenshot
```

::right::

<img src="/b3-s6-mobile.png" class="rounded-xl mt-4 w-full" />

<!--
Навіщо це взагалі? Мобільна автоматизація завжди була болючою темою. Appium складний у налаштуванні, XCUITest і Espresso — різні мови і різні підходи для iOS та Android, запис UI-тестів через UI Recorder дає крихкі тести прив'язані до конкретних координат. В результаті більшість команд або зовсім не автоматизує mobile QA, або має мінімальний набір тестів які ламаються при кожному редизайні. Mobile MCP міняє підхід: замість написання тестового коду — ти описуєш що має відбутись, і агент сам будує послідовність дій.

Описуєш сценарій природною мовою. Ключовий інсайт: використовує нативне дерево доступності — структуровані дані, не картинка. Побічний ефект: якщо Mobile MCP не може знайти кнопку — у вашого додатку проблеми з accessibility.

Як це виглядає на практиці: пишеш "натисни кнопку логін, введи тестові дані, перевір що відкрився дашборд" — і агент сам розбирається де ця кнопка знаходиться на iOS і де вона на Android. Не треба писати XPath, не треба знати координати, не треба розбиратись в UIAutomator.

Під капотом: для iOS — WebDriverAgent (той самий що використовує XCUITest), для Android — ADB + UIAutomator. Спочатку сервер викликає `mobile_list_elements_on_screen` і отримує структуроване дерево елементів з координатами — як JSON, не скріншот. Комп'ютерний зір підключається тільки якщо у елемента немає accessibility-мітки. Саме тому автоматизація детермінована і швидка.

Несподіваний бонус для команди: якщо Mobile MCP не може натиснути на кнопку через accessibility tree — значить у вашого додатку проблеми з доступністю. Тобто ти отримуєш безкоштовний a11y-аудит кожного разу коли запускаєш тести. Для аутсорс-команд, де accessibility часто виявляється в кінці — це рання сигналізація.
-->

---
layout: two-cols
---

# Sentry MCP

**No more copy-pasting stack traces.**

- Agent pulls the **full issue**: stack trace + breadcrumbs + environment
- Tools: `search_issues`, `get_issue_details`, `search_events`
- "root cause this issue" → agent fetches everything needed

::right::

<img src="/b3-s7-sentry.png" class="rounded-xl mt-4 w-full" />

<!--
Ти більше не копіюєш stack trace в чат. Агент підключається до Sentry, сам тягне повний issue. Для аутсорс-команд, де QA і девелопер в різних таймзонах — агент робить первинну діагностику до того, як розробник прокинувся.
-->

---
layout: center
class: text-center
---

# You don't need custom integrations for 80% of typical needs.

<div class="text-gray-300 mt-4 mb-8">The servers already exist. The question is which ones to connect first.</div>

<div class="grid grid-cols-4 gap-4">
  <div class="p-4 bg-gray-800/60 rounded-xl border border-green-500/30">
    <div class="font-bold text-green-400">GitHub MCP</div>
  </div>
  <div class="p-4 bg-gray-800/60 rounded-xl border border-green-500/30">
    <div class="font-bold text-green-400">Context7</div>
  </div>
  <div class="p-4 bg-gray-800/60 rounded-xl border border-green-500/30">
    <div class="font-bold text-green-400">Playwright</div>
  </div>
  <div class="p-4 bg-gray-800/60 rounded-xl border border-green-500/30">
    <div class="font-bold text-green-400">Sentry MCP</div>
  </div>
</div>

<div class="mt-6 text-gray-400 text-sm">Starter stack for web teams</div>

<!--
Для більшості типових потреб web і mobile команди — все вже є. Базовий стек, з якого можна почати: GitHub MCP, Context7, Playwright і Sentry. Figma і Mobile MCP — наступний крок.
-->

---
layout: center
class: text-center
transition: fade
---

# Delivery / PM / Observability

<div class="text-gray-400 mt-4">Block 4 of 8</div>

<img src="/b4-s0-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
Цей блок — для PM'ів, delivery leads і тих, хто хоче показати цінність MCP не технічній аудиторії. Менше перемикань між вкладками. Більше часу на реальні рішення.
-->

---

# The Core Idea

**MCP works best as context federation — not autonomous execution.**

<img src="/b4-s1-core-idea.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

<div class="mt-4 p-4 bg-blue-900/30 rounded-xl border border-blue-500/30 text-lg">
The real value: AI that sees <strong>ticket + design + code + CI + incidents</strong> simultaneously — not just the file you have open.
</div>

<div class="grid grid-cols-3 gap-4 mt-6">
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm">
    <div class="text-blue-400 font-bold mb-1">Reduces role friction</div>
    Less tab switching between Jira, Sentry, GitHub, Figma
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm">
    <div class="text-teal-400 font-bold mb-1">Synthesis work</div>
    Summarizing, correlating, triaging
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm">
    <div class="text-purple-400 font-bold mb-1">Not a replacement</div>
    Reduces "management minutes" per day
  </div>
</div>

<!--
Головна ідея: MCP найкраще працює не як автономний агент, а як федерація контексту. Цінність не в тому, що AI щось робить замість людини — а в тому, що він бачить усе одночасно. Людина теж може зібрати — але витратить 40 хвилин на перемикання між вкладками.
-->

---

# Three Scenarios That Actually Work

<img src="/b4-s2-scenarios.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

<div class="space-y-4 mt-2">

<div class="p-4 bg-gray-800/60 rounded-xl border border-gray-600/40">
<div class="font-bold text-blue-400 mb-1">🐛 Bug triage</div>
<div class="font-mono text-sm text-gray-300">"root cause this Sentry issue: &lt;link&gt;"</div>
<div class="text-sm mt-1">→ stack trace + breadcrumbs + release info → hypothesis + fix → GitHub issue created</div>
</div>

<div class="p-4 bg-gray-800/60 rounded-xl border border-gray-600/40">
<div class="font-bold text-teal-400 mb-1">📋 Sprint planning</div>
<div class="font-mono text-sm text-gray-300">"read Figma file &lt;link&gt; and break into Jira epics and tasks"</div>
<div class="text-sm mt-1">→ structured backlog in minutes ⚠️ needs review — AI doesn't know your estimates</div>
</div>

<div class="p-4 bg-gray-800/60 rounded-xl border border-gray-600/40">
<div class="font-bold text-purple-400 mb-1">🔍 Release investigation</div>
<div class="font-mono text-sm text-gray-300">"correlate production errors with last 3 releases, find hotfix candidates"</div>
<div class="text-sm mt-1">→ Sentry MCP + GitHub MCP → suspect PRs surfaced</div>
</div>

</div>

<!--
Три сценарії, де це вже реально працює. Bug triage — від посилання на Sentry до GitHub issue за одну LLM-сесію. Sprint planning — Figma-файл → Jira backlog. Але AI не знає ваші estimates — це відправна точка. Release investigation — агент корелює помилки з релізами.
-->

---

# High-ROI Use Cases for Outsource Teams

<img src="/b4-s3-use-cases.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

| Use Case | What AI does | Measurable KPI |
|---|---|---|
| **Engineering copilot** | Ticket + design + code + CI in one workflow | Onboarding time ↓ |
| **QA triage copilot** | Duplicate clustering, regression suspects, probable owner | Triage time ↓ |
| **Delivery / PM copilot** | Weekly risk digest, sprint summaries | Summary prep time ↓ |
| **Presales / estimation** | Compare RFP against similar past cases | Proposal prep time ↓ |

<!--
Для аутсорс-команди є чотири use case'и з реальним ROI. Engineering copilot — особливо корисний для онбордингу: новий розробник отримує контекст проекту за годину. QA triage — зменшує toil. Delivery copilot — найсильніший ефект у PM'ів. Presales — порівняння нового RFP зі схожими проектами.
-->

---
layout: two-cols
---

# Observability Layer

<img src="/b4-s4-observability.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**Sentry AI Monitoring — track your agents in production.**

- Monitors: tool calls, LLM interactions, token usage, latency
- Full trace of what the agent did and why
- If you're putting agents in production — **this is the first thing you deploy**

<div class="mt-4 p-3 bg-green-900/30 rounded-lg border border-green-500/30 text-sm">
Sellable story: <em>"your bug → triage → assigned → PR — in one LLM session, with full audit trail"</em>
</div>

::right::

<img src="/b4-s4-trace.png" class="rounded-xl mt-4 w-full" />

<!--
Sentry AI Monitoring у 2026-му сам відстежує agent workflows. Якщо ви ставите агентів у production для клієнта — це перше, що треба налаштувати. Без цього ви сліпі.
-->

---
layout: two-cols
---

# Anti-patterns

**Don't say:**
- ~~"This will replace senior developers"~~
- ~~"We can fully automate QA with MCP agents"~~
- ~~"PM gets a single source of truth automatically"~~

**Don't do:**
- Allow branch mutations without human approval
- Trust AI-generated severity without validation
- Ask AI for delivery status without access to primary artifacts

::right::

<div class="mt-8 space-y-3">
  <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm line-through text-gray-400">Replace senior developers</div>
  <div class="p-3 bg-green-900/30 rounded-lg border border-green-500/30 text-sm text-green-300">Reduce toil &amp; context switching</div>
  <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm line-through text-gray-400">Automate QA fully</div>
  <div class="p-3 bg-green-900/30 rounded-lg border border-green-500/30 text-sm text-green-300">Accelerate triage &amp; coverage</div>
  <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm line-through text-gray-400">Auto single source of truth</div>
  <div class="p-3 bg-green-900/30 rounded-lg border border-green-500/30 text-sm text-green-300">Synthesize from aligned sources</div>
</div>

<!--
Кілька речей, яких варто уникати. "Замінить senior dev" — ні. "Автоматизуємо QA повністю" — ні, зменшуємо toil. "PM отримає single source of truth" — тільки якщо джерела вже узгоджені між собою.
-->

---

# Frame Pilots as Workflow Augmentation

<img src="/b4-s6-takeaway.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**Start with read-heavy flows. Write actions only through approval.**

| KPI | Before | After |
|---|---|---|
| Triage time | 2h per bug | 20 min |
| Onboarding time | 5 days | 1 day |
| Summary prep time | 60 min / week | 5 min |
| Proposal prep time | 3h per RFP | 45 min |

<div class="mt-4 text-gray-400 text-sm">
Pick a use case with a measurable baseline. Not "it will be better" — specific numbers.
</div>

<!--
Як правильно підходити до пілоту. Починати з read-heavy flows — читати, шукати, агрегувати, резюмувати. Ніяких write-дій без підтвердження людини на першому етапі. Вибрати use case, де є вимірюваний baseline.
-->

---
layout: center
class: text-center
transition: fade
---

# Token Bloat

<div class="text-gray-400 mt-4">Block 5 of 8</div>

<img src="/b5-s0-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
Є одна проблема, яка вдарить по вашому бюджету і швидкості швидше, ніж будь-яка security-вразливість. І більшість команд стикається з нею щойно підключає більше двох-трьох серверів.
-->

---

# You Haven't Typed a Word Yet.

**A third of your context is already gone.**

| Setup | Token overhead |
|---|---|
| GitHub MCP (94 tools) | ~17,600 tokens |
| Atlassian MCP (Jira + Confluence) | ~10,000 tokens |
| GitHub + Jira + AWS | **30,000+ tokens per request** |
| MySQL server (106 tools) | ~54,600 tokens |
| 8 production servers (224 tools) | **~66,000 tokens = 33% of Claude Sonnet's context** |

<div class="mt-4 p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm">
Cost: <strong>$0.01–0.10 per request</strong> just for tool schema overhead, before any actual work.
</div>

<!--
GitHub MCP сам по собі — 17 600 токенів щоразу. GitHub + Jira + AWS — 30 тисяч токенів до першого слова. 8 серверів — третина контекстного вікна Claude Sonnet. Від 1 до 10 центів за запит лише на описи інструментів.
-->

---
layout: two-cols
---

# Two Types of Bloat

**Schema bloat** — token cost of loading tool *definitions*
<div class="p-3 bg-gray-800/60 rounded-lg text-sm mb-4">
Happens upfront, every request. Predictable and measurable.
</div>

**Response bloat** — token cost of tool *outputs*
<div class="p-3 bg-gray-800/60 rounded-lg text-sm">
A single HRIS or CRM call can return hundreds of thousands of characters of raw JSON.<br/>
<span class="text-amber-400">Gets less attention — but often consumes more context than schemas.</span>
</div>

::right::

<div class="mt-8 space-y-4">
  <div class="p-4 bg-blue-900/20 rounded-xl border border-blue-500/30">
    <div class="font-bold text-blue-400 mb-2">Schema Bloat</div>
    <div class="font-mono text-xs text-gray-300">tool_1: { name, desc, params... }<br/>tool_2: { name, desc, params... }<br/>tool_3: { name, desc, params... }<br/><span class="text-red-400">... × 94 tools</span></div>
  </div>
  <div class="p-4 bg-red-900/20 rounded-xl border border-red-500/30">
    <div class="font-bold text-red-400 mb-2">Response Bloat</div>
    <div class="font-mono text-xs text-gray-300">{"users":[{"id":1,"name":"...",<br/>"email":"...","dept":"...",<br/><span class="text-red-400">...500 more fields × 10,000 rows}</span></div>
  </div>
</div>

<!--
Є два типи token bloat. Schema bloat — вартість завантаження описів інструментів на початку. Response bloat — коли інструмент повертає відповідь. Один виклик до HRIS або CRM може повернути сотні тисяч символів сирого JSON.
-->

---

# CLI Solved This 50 Years Ago

<img src="/b5-s3-cli.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

> *"You connected 8 MCP servers. 33% of your context is gone. CLI tools solved the exact same constraint decades ago."* — Miguel MS

<div class="grid grid-cols-2 gap-8 mt-4">
  <div class="p-4 bg-green-900/20 rounded-xl border border-green-500/30">
    <div class="font-bold text-green-400 mb-3">CLI — hierarchical discovery</div>
    <div class="font-mono text-sm">git --help<br/>→ list of command groups<br/><br/>git commit --help<br/>→ details for that command</div>
    <div class="text-sm text-green-300 mt-2">Pay only for what you explore</div>
  </div>
  <div class="p-4 bg-red-900/20 rounded-xl border border-red-500/30">
    <div class="font-bold text-red-400 mb-3">MCP today — dump everything</div>
    <div class="font-mono text-sm">startup →<br/>ALL 94 tool schemas<br/>loaded into context<br/>before first message</div>
    <div class="text-sm text-red-300 mt-2">Pay for everything upfront</div>
  </div>
</div>

<!--
CLI-інструменти вирішили точно таку ж проблему 50 років тому: ієрархічний discovery. Ти платиш тільки за те, що реально досліджуєш. MCP поки що робить навпаки: викидає весь каталог інструментів у контекст при старті.
-->

---

# Four Approaches Compared

<img src="/b5-s4-approaches.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

| Approach | Solves | Token savings | Effort |
|---|---|---|---|
| **Schema compression** | Schema bloat | 50–97% | Low |
| **Search-first discovery** | Schema bloat | 85–97% | Medium |
| **Response filtering** | Response bloat | Varies | Medium |
| **Code-based execution** | Both | Max | High |

**Ready-to-use proxies today:**

- **mcp-compressor** *(Atlassian Labs)* — 70–97%, drop-in wrapper
- **mcp-lazy-proxy** — 6.5–6.7× reduction
- **Claude Code** *(v2.1.7+)* — auto-activates when tools exceed 10% of context

<!--
Є чотири підходи. Schema compression — стискаємо описи. Search-first — модель бачить тільки два мета-інструменти. Response filtering — фільтруємо що повертає інструмент. Code-based — найрадикальніший. mcp-compressor від Atlassian Labs — найзрілніший готовий інструмент.
-->

---

# mcp-compressor in Depth

<img src="/b5-s5-compressor.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**Atlassian Labs, open source. Drop-in wrapper for any MCP server.**

Exposes **two meta-tools** instead of all schemas upfront:
- `get_tool_schema(tool_name)` — fetch full schema on demand
- `invoke_tool(tool_name, input)` — execute it

| Level | What model sees | Use case |
|---|---|---|
| `low` | Full descriptions | Unusual tools |
| `medium` *(default)* | First sentence only | Most servers |
| `high` | Tool name + param names | Large toolsets |
| `max` | `list_tools()` only | Rarely-used servers |

<div class="text-sm text-gray-400 mt-2">Works with stdio, HTTP, SSE. Python and TypeScript. Zero functionality loss.</div>

<!--
mcp-compressor стає проксі між вашим клієнтом і існуючим MCP-сервером. Модель бачить тільки два інструменти. Детальна схема підтягується тільки у момент, коли модель реально збирається щось викликати. Функціональність не змінюється.
-->

---
layout: center
class: text-center
---

# Token Bloat: Key Takeaway

<img src="/b5-s6-takeaway.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

<div class="grid grid-cols-2 gap-6 mt-8 text-left">
  <div class="p-4 bg-blue-900/30 rounded-xl border border-blue-500/30">
    <div class="font-bold text-blue-400 mb-2">Schema Bloat</div>
    <div class="text-sm">Predictable — measure it before connecting servers to production</div>
  </div>
  <div class="p-4 bg-red-900/30 rounded-xl border border-red-500/30">
    <div class="font-bold text-red-400 mb-2">Response Bloat</div>
    <div class="text-sm">Often bigger — don't ignore tool outputs</div>
  </div>
</div>

<div class="mt-6 p-4 bg-teal-900/30 rounded-xl border border-teal-500/30 text-left">
  <div class="font-bold text-teal-400 mb-1">Rule of thumb</div>
  <div class="text-sm">If tool schemas exceed 10% of context window → switch to search-first or compression.<br/>
  Start with <strong>mcp-compressor</strong> on any server with 20+ tools.</div>
</div>

<div class="mt-4 text-sm text-gray-400">Claude Code handles this automatically. Cursor, Claude Desktop — don't.</div>

<!--
Практичний висновок. Виміряйте обидва типи bloat перед тим, як підключати сервери в production. mcp-compressor — ваш перший інструмент для будь-якого сервера з 20+ тулзами. Важлива відмінність між клієнтами: Claude Code вирішує schema bloat автоматично. Cursor та інші — ні.
-->

---
layout: center
class: text-center
transition: fade
---

# Security

<div class="text-gray-400 mt-4">Block 6 of 8</div>

<img src="/b6-s0-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
MCP має системні вразливості, і вендор офіційно каже "це не баг". Важливо розуміти threat model перш ніж підключати будь-який сервер.
-->

---

# The OX Security Incident — April 2026

<img src="/b6-s1-incident.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**Architectural RCE in official Anthropic MCP SDKs.**

<div class="grid grid-cols-2 gap-4 mt-4">
  <div class="space-y-2">
    <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm">
      <strong>Affected:</strong> Python, TypeScript, Java, Rust SDKs
    </div>
    <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm">
      <strong>Scale:</strong> ~200,000 vulnerable instances, 150M+ downloads
    </div>
    <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm">
      <strong>Clients:</strong> Cursor, VS Code, Windsurf, Claude Code, Gemini CLI
    </div>
    <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm">
      <strong>CVE-2026-30615</strong> (Windsurf) — zero-click, no user interaction
    </div>
  </div>
  <div class="p-4 bg-gray-900 rounded-xl border border-red-500/50">
    <div class="text-red-400 font-bold mb-2">Anthropic's response:</div>
    <div class="text-gray-300 italic text-sm">"This is expected behavior — STDIO was designed for this."</div>
    <div class="mt-4 text-amber-400 text-sm font-bold">The protocol won't fix this.<br/>That's an official position.</div>
  </div>
</div>

<!--
У квітні 2026 дослідники OX Security опублікували архітектурну RCE-вразливість в офіційних MCP SDK від Anthropic — у всіх чотирьох мовах. 200 тисяч вразливих інстансів. Найважливіше тут — відповідь Anthropic: "це очікувана поведінка". Це політична позиція, не технічна.
-->

---

# Attack Classes

<img src="/b6-s2-attacks.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

| Attack | How it works |
|---|---|
| **Prompt injection** | AI reads malicious content (issue, PDF, README) and executes hidden instructions |
| **Tool poisoning** | Attacker changes tool description metadata — user never sees it, LLM acts on it |
| **Rug pull** | Server behaves safely on install, changes behavior days later |
| **Cross-tool hijacking** | One server intercepts calls addressed to another |
| **Supply chain** | STDIO server inherits your user permissions — reads `.env`, SSH keys, shell history |

<!--
П'ять класів атак. Prompt injection — AI читає заражений контент. Tool poisoning — атакувальник змінює опис інструмента. Rug pull — сервер безпечний у день установки. Cross-tool hijacking — перехоплення викликів. Supply chain — STDIO успадковує ваші user permissions.
-->

---
layout: two-cols
---

# The Lethal Trifecta

<img src="/b6-s3-trifecta.png" class="absolute inset-0 w-full h-full object-cover opacity-10 -z-10" />

**Simon Willison's model — attack becomes near-inevitable when agent has all three:**

<div class="space-y-3 mt-4">
  <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30">
    <span class="font-bold text-red-400">1.</span> Access to <strong>private data</strong>
  </div>
  <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30">
    <span class="font-bold text-red-400">2.</span> <strong>Untrusted input</strong> (web pages, user files, external APIs)
  </div>
  <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30">
    <span class="font-bold text-red-400">3.</span> An <strong>exfiltration channel</strong> (outbound HTTP, email, any write tool)
  </div>
</div>

::right::

<div class="mt-4 space-y-2">
  <div class="font-bold text-amber-400 mb-3">Recent CVEs in the wild:</div>
  <div class="p-2 bg-gray-800/60 rounded text-xs font-mono">CVE-2025-49596 — MCP Inspector RCE<br/><span class="text-gray-400">CVSS 9.4 — just by visiting a malicious site</span></div>
  <div class="p-2 bg-gray-800/60 rounded text-xs font-mono">CVE-2026-25536 — TypeScript SDK<br/><span class="text-gray-400">cross-client data leak via Streamable HTTP</span></div>
  <div class="p-2 bg-gray-800/60 rounded text-xs font-mono">CVE-2025-68143/144/145<br/><span class="text-gray-400">prompt injection via mcp-server-git README</span></div>
</div>

<!--
Simon Willison описав "смертельне тріо": коли агент одночасно має доступ до приватних даних, отримує недовірений ввід і має канал для ексфільтрації — атака майже гарантовано спрацює. CVE-2025-68143: достатньо щоб агент прочитав шкідливий README.
-->

---

# Team Checklist

<img src="/b6-s4-checklist.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

<div class="grid grid-cols-2 gap-3 mt-2">
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> Install from <strong>official MCP Registry</strong> or verified vendors only
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> Untrusted servers — <strong>Docker / sandbox only</strong>
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> GitHub MCP: use <code>--toolsets</code> scope, never <code>--all</code>
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> <strong>Human-in-the-loop</strong> on all destructive actions — always
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> Short-lived, <strong>scoped tokens</strong> — no over-permissioned accounts
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> <strong>Monitor tool invocations</strong> — know what the agent called
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> <strong>Audit trail at gateway level</strong> — required for enterprise delivery
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> Treat external config as <strong>untrusted input</strong> — user-influenced MCP config = RCE vector
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2">
    <span class="text-green-400">✅</span> Use <strong>OWASP MCP Top 10</strong> as your security review checklist
  </div>
</div>

<!--
Практичний чекліст для команди. Ставити тільки з офіційного MCP Registry. Недовірені сервери — тільки в Docker. GitHub MCP підтримує --toolsets. Human-in-the-loop на будь-яку деструктивну дію. Audit trail на рівні gateway — обов'язковий для enterprise клієнта.
-->

---
layout: two-cols
class: text-sm
zoom: 0.85
---

# OWASP MCP Top 10

<img src="/b6-s5-owasp.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**The living security standard for MCP — beta pilot 2026.**

| # | Category |
|---|---|
| 1 | Prompt Injection |
| 2 | Input Validation |
| 3 | Auth Failures |
| 4 | Session Management |
| 5 | Integrity Issues |
| 6 | Trust Model |
| 7 | Credentials |
| 8 | Network Security |
| 9 | Command Injection |
| 10 | Supply Chain |

::right::

<div class="mt-4 p-4 bg-gray-800/60 rounded-xl">
  <div class="font-bold text-amber-400 mb-3">Pay attention to #10:</div>
  <div class="text-sm text-gray-300">Supply chain — package signatures and dependency monitoring. This is what could have caught the April 2026 CVEs earlier.</div>
</div>

<div class="mt-4 p-4 bg-blue-900/30 rounded-xl border border-blue-500/30">
  <div class="text-sm">Use this as your internal checklist before delivering any MCP-enabled product to a client.</div>
</div>

<!--
OWASP випустили MCP Top 10 — живий документ, beta pilot у 2026. Те саме, що OWASP Top 10 для веб, але для MCP. Зверніть увагу на №10: supply chain — підписи пакетів і моніторинг залежностей.
-->

---
layout: center
class: text-center
---

# The Protocol Cannot Fix This — And That's Official.

<img src="/b6-s6-takeaway.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

<div class="mt-6 p-4 bg-gray-800/60 rounded-xl max-w-xl mx-auto text-left">
  <div class="text-gray-300 text-sm">MCP's security model relies on host implementations and operational policy — not the protocol itself.</div>
</div>

<div class="grid grid-cols-3 gap-4 mt-8">
  <div class="p-4 bg-blue-900/30 rounded-xl border border-blue-500/30">
    <div class="font-bold text-blue-400">Host Implementation</div>
  </div>
  <div class="p-4 bg-teal-900/30 rounded-xl border border-teal-500/30">
    <div class="font-bold text-teal-400">Operational Policy</div>
  </div>
  <div class="p-4 bg-purple-900/30 rounded-xl border border-purple-500/30">
    <div class="font-bold text-purple-400">Audit Trail</div>
  </div>
</div>

<div class="mt-6 text-gray-400 text-sm">Clients will ask about MCP security. You need an answer — not "that's Anthropic's problem".</div>

<!--
Мета-інсайт, яким варто завершити цей блок: сам протокол ці проблеми не вирішує — і це офіційна позиція. Це не баги реалізації, це design trade-offs. Anthropic свідомо залишили це на відповідальності host implementations і операційних політик. Ми не чекаємо патча — ми самі закриваємо це на рівні того, як налаштовуємо і деплоїмо.

Для аутсорс-компанії це означає одне: клієнти будуть питати про безпеку MCP, і ви маєте мати відповідь — не "це проблема Anthropic".
-->

---
layout: center
class: text-center
transition: fade
---

# Now & Future

<div class="text-gray-400 mt-4">Block 7 of 8</div>

<img src="/b7-s0-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
Фінальний блок — про те, що робити з усім цим прямо зараз. Що пілотити цього місяця, що зачекати, і куди весь цей стек рухається далі.
-->

---

# Where We Are — April 2026

<img src="/b7-s1-hype-cycle.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**Past the peak of inflated expectations.**

<div class="grid grid-cols-3 gap-4 mt-6">
  <div class="p-4 bg-gray-800/60 rounded-xl border border-gray-600/40">
    <div class="text-red-400 font-bold mb-2">Faded away</div>
    <div class="text-sm">Complex multi-agent orchestrators without standardized interfaces</div>
  </div>
  <div class="p-4 bg-green-900/30 rounded-xl border border-green-500/30">
    <div class="text-green-400 font-bold mb-2">What remained</div>
    <div class="text-sm">Reliable, simple MCP — infrastructure, not experiment</div>
  </div>
  <div class="p-4 bg-amber-900/30 rounded-xl border border-amber-500/30">
    <div class="text-amber-400 font-bold mb-2">Still mostly demos</div>
    <div class="text-sm">Multi-agent autonomous workflows in production</div>
  </div>
</div>

<div class="mt-6 p-3 bg-blue-900/30 rounded-lg border border-blue-500/30 text-sm">
Investing in MCP today = investing in what's already stable, not what might change tomorrow.
</div>

<!--
Де ми знаходимось у квітні 2026. Ми пройшли пік завищених очікувань. Складні мульти-агентні оркестратори відійшли на задній план. Залишилось те, що реально працює: надійний і простий MCP.
-->

---

# MCP Is Not Alone: The Protocol Stack

<img src="/b7-s2-protocol-stack.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**Four layers, four jobs.**

| Protocol | Layer | Answers |
|---|---|---|
| **MCP** | Agent ↔ Tool / Data | How does an LLM call a tool? |
| **A2A** *(Google, Apr 2025)* | Agent ↔ Agent | How does one agent delegate to another? |
| **ACP** | Agent ↔ Editor | IDE-native communication (Zed, JetBrains, Neovim) |
| **WebMCP** | Agent ↔ Web | Early stage, under AAIF |

<div class="mt-4 p-3 bg-gray-800/60 rounded-lg text-sm text-gray-300">
All four are under or moving toward Linux Foundation. They <strong>complement</strong>, not compete.
</div>

<!--
MCP — не єдиний протокол у стеку. Є чотири рівні. MCP — як LLM викликає інструмент. A2A від Google — як один агент делегує задачу іншому. ACP — всередині редакторів. WebMCP — як агент взаємодіє з вебом. Все під Linux Foundation.
-->

---
layout: two-cols
---

# A2A: When It Becomes Relevant

<img src="/b7-s3-a2a.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

**A2A v1.0 — Q1 2026. 150+ organizations in production.**

Already supported: Microsoft, AWS, Salesforce, SAP, ServiceNow, Google ADK, LangGraph, CrewAI, LlamaIndex.

**How it works:**
- Each agent publishes an **Agent Card** at `/.well-known/agent-card.json`
- Capability negotiation + task lifecycle between agents

> *"MCP first for sharing context; then A2A for dynamic interaction."* — ISG analyst

::right::

<div class="mt-4 space-y-3">
  <div class="font-bold text-amber-400 mb-2">When it's relevant:</div>
  <div class="p-3 bg-green-900/30 rounded-lg border border-green-500/30 text-sm">
    ✅ Integrating multi-vendor enterprise (Salesforce ↔ ServiceNow ↔ SAP agents)
  </div>
  <div class="p-3 bg-red-900/30 rounded-lg border border-red-500/30 text-sm">
    ❌ When you're still building your first MCP setup
  </div>
</div>

<!--
A2A вже в production у 150+ організаціях. Практична порада: спочатку MCP для sharing context, потім A2A для dynamic interaction. Для аутсорс-команди у 2026 році — 95% use cases закриваються MCP.
-->

---

# Pilot This Month

<img src="/b7-s4-pilots.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

**Start small. Measure one metric. Prove it.**

| Pilot | Setup | Measure |
|---|---|---|
| **GitHub + Figma + Jira + Sentry** | 1 tech lead + 2 devs | Time per ticket before vs after |
| **Playwright MCP** on project without E2E | 5 user flows | Coverage vs time budget |
| **Expo MCP** on React Native project | 1 RN project | Developer friction on device testing |

<div class="mt-4 p-3 bg-green-900/30 rounded-lg border border-green-500/30 text-sm">
Small scope · Measurable baseline · One team · One metric<br/>
That's enough to validate or kill the idea in <strong>2–3 weeks</strong>.
</div>

<!--
Що пілотувати вже цього місяця. Перший — підключити GitHub, Figma, Jira і Sentry в IDE одної невеликої команди. Другий — Playwright MCP на проєкті без E2E. Третій — Expo MCP якщо є React Native. Маленький скоуп, вимірюваний baseline, одна метрика.
-->

---

# Wait 2–4 Quarters

<img src="/b7-s5-wait.png" class="absolute inset-0 w-full h-full object-cover opacity-15 -z-10" />

**Technically possible. Not yet production-safe.**

<div class="space-y-2 mt-4">
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2 items-start">
    <span class="text-red-400 mt-0.5">⏸</span>
    <div><strong>Self-hosted remote MCP servers for clients</strong> — stateful HTTP doesn't scale yet</div>
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2 items-start">
    <span class="text-red-400 mt-0.5">⏸</span>
    <div><strong>Publishing client API as a public MCP server</strong> — security surface too large</div>
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2 items-start">
    <span class="text-red-400 mt-0.5">⏸</span>
    <div><strong>Multi-agent A2A workflows</strong> — expensive to maintain, mostly demos</div>
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2 items-start">
    <span class="text-red-400 mt-0.5">⏸</span>
    <div><strong>MCP as core architecture of a client-facing product</strong> — fine for internal, not consumer-facing</div>
  </div>
  <div class="p-3 bg-gray-800/60 rounded-lg text-sm flex gap-2 items-start">
    <span class="text-red-400 mt-0.5">⏸</span>
    <div><strong>Replacing QA teams with AI agents</strong> — the economics don't add up</div>
  </div>
</div>

<!--
Що не варто робити прямо зараз. Self-hosted remote MCP сервери — stateful HTTP поки погано масштабується. Публікація клієнтського API як MCP-сервера — security surface завеликий. Multi-agent A2A — дорого підтримувати. Заміна QA-команди — не спрацьовує.
-->

---
layout: center
class: text-center
---

# MCP Is the Foundation of a New Engineering Discipline.

<img src="/b7-s6-takeaway.png" class="absolute inset-0 w-full h-full object-cover opacity-20 -z-10" />

<div class="text-gray-300 mt-4 max-w-2xl mx-auto">
Companies that master building <strong>secure, efficient, and scalable context networks</strong> will lead in the era of autonomous AI.
</div>

<div class="grid grid-cols-3 gap-4 mt-8 text-left">
  <div class="p-4 bg-blue-900/30 rounded-xl border border-blue-500/30">
    <div class="font-bold text-blue-400 mb-1">Standardize</div>
    <div class="text-sm">Use MCP servers instead of custom sync scripts</div>
  </div>
  <div class="p-4 bg-teal-900/30 rounded-xl border border-teal-500/30">
    <div class="font-bold text-teal-400 mb-1">Secure</div>
    <div class="text-sm">Every mcp.json entry deserves the same review as a new PR</div>
  </div>
  <div class="p-4 bg-purple-900/30 rounded-xl border border-purple-500/30">
    <div class="font-bold text-purple-400 mb-1">Automate routines</div>
    <div class="text-sm">Figma MCP + Playwright MCP can cut the most routine stages in half</div>
  </div>
</div>

<div class="mt-8 text-xl text-yellow-400 font-bold">From selling hours → to selling outcomes.</div>

<!--
MCP у 2026-му — це фундамент нової інженерної дисципліни. Для аутсорс-бізнесу: від продажу годин до продажу результату. Не "ми витратили 40 годин", а "ось результат, досягнутий за допомогою системи". Це конкурентна перевага.
-->

---
layout: center
class: text-center
transition: fade
---

# Sources & Further Reading

<div class="text-gray-400 mt-4">Block 8 of 8</div>

<img src="/b8-s0-title.png" class="absolute inset-0 w-full h-full object-cover opacity-25 -z-10" />

---

# Official Docs & Tools

<div class="grid grid-cols-2 gap-3 mt-2 text-sm">
  <a href="https://modelcontextprotocol.io" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">modelcontextprotocol.io</div>
    <div class="text-gray-400">MCP Official Documentation</div>
  </a>
  <a href="https://github.com/modelcontextprotocol" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">github.com/modelcontextprotocol</div>
    <div class="text-gray-400">MCP Specification Repo</div>
  </a>
  <a href="https://mcpservers.org" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">mcpservers.org</div>
    <div class="text-gray-400">Awesome MCP Servers directory</div>
  </a>
  <a href="https://owasp.org/www-project-mcp-top-10/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">OWASP MCP Top 10</div>
    <div class="text-gray-400">Security checklist — 2026 beta</div>
  </a>
  <a href="https://vulnerablemcp.info" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">vulnerablemcp.info</div>
    <div class="text-gray-400">Live CVE database for MCP</div>
  </a>
  <a href="https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">MCP 2026 Roadmap</div>
    <div class="text-gray-400">blog.modelcontextprotocol.io</div>
  </a>
  <a href="https://www.linuxfoundation.org/projects/agentic-ai-foundation" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">Agentic AI Foundation (AAIF)</div>
    <div class="text-gray-400">Linux Foundation · 146 members</div>
  </a>
</div>

---

# Deep Dives & Videos

<div class="grid grid-cols-2 gap-3 mt-2 text-sm">
  <a href="https://newsletter.pragmaticengineer.com/p/mcp-deepdive" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">Pragmatic Engineer — MCP Deep Dive</div>
    <div class="text-gray-400">Best real-world overview</div>
  </a>
  <a href="https://blog.sentry.io/monitoring-mcp-server-sentry/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">Sentry Blog — Monitoring MCP</div>
    <div class="text-gray-400">Observability deep dive</div>
  </a>
  <a href="https://www.figma.com/blog/introducing-figma-mcp-server/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">Figma Blog — Dev Mode MCP</div>
    <div class="text-gray-400">Figma → code workflow</div>
  </a>
  <a href="https://truto.one/blog/what-is-mcp-model-context-protocol-the-2026-guide-for-saas-pms" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">Truto — 2026 Guide for SaaS PMs</div>
    <div class="text-gray-400">PM perspective on MCP</div>
  </a>
  <a href="https://www.youtube.com/watch?v=kQmXtrmQ5Zg" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">MCP Deep Dive Workshop — Anthropic</div>
    <div class="text-gray-400">YouTube · Mahesh Murag</div>
  </a>
  <a href="https://www.youtube.com/watch?v=CQywdSdi5iA" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">The Model Context Protocol</div>
    <div class="text-gray-400">YouTube · Official Anthropic Intro</div>
  </a>
</div>

---

# Security & Critical Reading

<div class="grid grid-cols-2 gap-3 mt-2 text-sm">
  <a href="https://www.ox.security/blog/the-mother-of-all-ai-supply-chains-critical-systemic-vulnerability-at-the-core-of-the-mcp/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-red-400">OX Security — "Mother of All AI Supply Chains"</div>
    <div class="text-gray-400">Apr 2026 · The RCE incident</div>
  </a>
  <a href="https://vulnerablemcp.info" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-red-400">vulnerablemcp.info</div>
    <div class="text-gray-400">Live CVE database for MCP</div>
  </a>
  <a href="https://unit42.paloaltonetworks.com/model-context-protocol-attack-vectors/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-red-400">Palo Alto Unit 42 — Attack Vectors</div>
    <div class="text-gray-400">Dec 2025 · Sampling attack analysis</div>
  </a>
  <a href="https://blog.sshh.io/p/everything-wrong-with-mcp" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-amber-400">Shrivu Shankar — Everything Wrong with MCP</div>
    <div class="text-gray-400">Critical perspective</div>
  </a>
  <a href="https://newsletter.victordibia.com/p/no-mcps-have-not-won-yet" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-amber-400">Victor Dibia — No, MCPs have not won (Yet)</div>
    <div class="text-gray-400">Reality check</div>
  </a>
  <a href="https://www.tigerdata.com/blog/three-tigerdata-engineers-told-us-the-truth-about-mcp-security-is-its-achilles-heel" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-amber-400">TigerData — Engineers on MCP Security</div>
    <div class="text-gray-400">Security is its Achilles heel</div>
  </a>
  <a href="https://thehackernews.com/2026/04/critical-mcp-rce-anthropic.html" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-red-400">The Hacker News — Critical MCP RCE analysis</div>
    <div class="text-gray-400">Apr 2026 · OX Security coverage</div>
  </a>
  <a href="https://www.theregister.com/2026/04/15/anthropic_mcp_design_flaw/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-amber-400">The Register — Anthropic won't fix MCP design flaw</div>
    <div class="text-gray-400">Apr 2026 · Official Anthropic position</div>
  </a>
</div>

---

# Courses & Repositories

<div class="grid grid-cols-2 gap-3 mt-2 text-sm">
  <a href="https://anthropic.skilljar.com/introduction-to-model-context-protocol" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">Anthropic Academy — Intro to MCP</div>
    <div class="text-gray-400">Free course</div>
  </a>
  <a href="https://anthropic.skilljar.com/model-context-protocol-advanced-topics" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">Anthropic Academy — Advanced MCP</div>
    <div class="text-gray-400">Free course</div>
  </a>
  <a href="https://learn.deeplearning.ai/courses/mcp-build-rich-context-ai-apps-with-anthropic/" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">DeepLearning.AI — MCP Course</div>
    <div class="text-gray-400">Free · Build rich-context AI apps</div>
  </a>
  <a href="https://huggingface.co/learn/mcp-course" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-teal-400">Hugging Face — MCP Course</div>
    <div class="text-gray-400">Free · by Anthropic</div>
  </a>
  <a href="https://github.com/github/github-mcp-server" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">github/github-mcp-server</div>
    <div class="text-gray-400">Official GitHub MCP Server</div>
  </a>
  <a href="https://github.com/microsoft/playwright-mcp" class="p-3 bg-gray-800/60 rounded-lg hover:bg-gray-700/60 transition">
    <div class="font-bold text-blue-400">microsoft/playwright-mcp</div>
    <div class="text-gray-400">Official Playwright MCP (Microsoft)</div>
  </a>
</div>
