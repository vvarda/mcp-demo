---
theme: default
title: "MCP & The State of AI Tooling in 2026"
info: Block 1 — MCP Presentation
colorSchema: dark
highlighter: shiki
transition: slide-left
fonts:
  sans: Inter
  mono: Fira Code
---

# MCP & The State of AI Tooling in 2026

<div class="pt-12 text-gray-400 text-sm">Block 1 of 3</div>

<img src="/slide-00-title.png" class="absolute inset-0 w-full h-full object-cover opacity-30 -z-10" />

<!--
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

# The N×M Problem

<img src="/slide-02-nxm-problem.png" class="w-full rounded-xl mt-2" style="max-height: 380px; object-fit: cover;" />

<div class="grid grid-cols-2 gap-8 mt-4 text-sm">
  <div class="text-red-400">N tools × M clients = <strong>N×M</strong> custom integrations</div>
  <div class="text-green-400">N tools + M clients = <strong>N+M</strong> standard connectors</div>
</div>

<!--
До MCP кожна пара "інструмент + клієнт" потребувала окремої інтеграції. Якщо у вас 10 інструментів і 10 клієнтів — це 100 з'єднань. MCP зводить це до 20. Саме цю проблему він і вирішував з першого дня.
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
-->

---
layout: two-cols
---

# Ecosystem Growth

| Metric | Nov 2024 | Apr 2026 |
|---|---|---|
| Public servers | ~50 | 1,000+ |
| SDK downloads / mo | < 1M | 150M+ |
| Standardization | Proprietary | Linux Foundation |
| Transport | STDIO | Streamable HTTP |

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

Окремо варто сказати про A2A — Agent-to-Agent протокол від Google, анонсований у квітні 2025. Якщо MCP вирішує задачу "агент ↔ інструмент або дані", то A2A — це про те, як один агент може делегувати задачу іншому агенту. Обидва протоколи зараз під Linux Foundation, вони доповнюють, а не замінюють один одного.
-->
