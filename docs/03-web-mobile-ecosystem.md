# Block 3: Web & Mobile Ecosystem

---

## Slide 0 — Title Slide

**Web & Mobile Ecosystem**

> 🗣 *Цей блок — про конкретні MCP-сервери, які вже зараз змінюють щоденну роботу web і mobile команд. Не теорія — інструменти, які можна поставити сьогодні.*

> `[image prompt]` 🖼 *A grid of recognizable dev tool logos (Figma, GitHub, Playwright, Sentry, Chrome DevTools) connected by glowing MCP lines to a central AI agent node. Dark background, ecosystem map aesthetic.*

---

## Slide 1 — The Stack at a Glance

| Server | What it unlocks |
|--------|----------------|
| **GitHub MCP** | PR review, CI logs, rerun workflows |
| **Context7** | Live, version-specific docs — kills hallucinations |
| **Figma Dev Mode MCP** | Real design structure → code, not screenshots |
| **Playwright MCP** | Browser automation — E2E tests, A11y audits |
| **Chrome DevTools MCP** | Live page inspection, performance traces |
| **Mobile MCP** | Native iOS & Android automation |
| **Sentry MCP** | Full issue context with breadcrumbs — no copy-paste |

> 🗣 *Є кілька серверів, які формують базовий стек для web і mobile команди у 2026. Давайте пройдемось по кожному.*

> `[image prompt]` 🖼 *Clean dark-mode table or card grid showing 7 tools with their logos and one-line descriptions. Minimal, information-dense layout.*

---

## Slide 2 — GitHub MCP

**Official. Rewritten in Go. Production-ready.**

- Review PRs, read CI logs, rerun failed workflows — all from the agent
- OAuth support in VS Code
- Start here — it's the first server most teams connect

> 🗣 *GitHub MCP — офіційний, переписаний на Go. Агент може переглядати PR, читати логи CI, перезапускати workflow'и. З нього починає більшість команд — і правильно роблять. Він безпечний, добре підтримується і вирішує негайну проблему: більше не треба перемикатись між IDE і GitHub у браузері.*

> `[image prompt]` 🖼 *GitHub Octocat logo connected to an AI agent with arrows labeled "PR review", "CI logs", "rerun workflow". Dark background, GitHub green accent.*

---

## Slide 3 — Context7: Kill Hallucinations

**9,000+ libraries. Always version-correct.**

- Fetches **live, version-specific docs** directly into the model's context
- Trigger: add `use context7` to any prompt
- No API key required — `npx`-installable in seconds
- Zero secrets, zero setup friction

> How it works: `resolve-library-id` → `get-library-docs` → injected into context window

> 🗣 *Context7 від Upstash вирішує одну конкретну проблему: LLM галюцинує на версіях API. Модель навчена на даних минулого року — і пише код для React 18, коли у вас React 19, або для старого Next.js App Router. Context7 тягне актуальну документацію прямо в контекстне вікно перед генерацією. Два виклики: резолвить назву бібліотеки в ID, потім тягне доки. Запустити — один рядок в терміналі, без жодних ключів.*
>
> 🗣 *Непомітний інсайт: Context7 можна додати в системний промт або Cursor Rules — і він буде активним завжди, без "use context7" щоразу. Для команди це означає, що всі розробники автоматично отримують актуальні доки, незалежно від того, чи пам'ятають вони про нього.*

> `[image prompt]` 🖼 *Split comparison: left "LLM training data" with old/crossed-out API code; right "Context7" with fresh docs injected into context, accurate code output. Dark mode, red vs green visual contrast.*

---

## Slide 4 — Figma Dev Mode MCP

**Agent reads the real design — not a screenshot.**

- Access to live layer structure, design tokens, auto-layout, component variants
- Generates React / Vue code that matches the design **~95% on first iteration**
- ⚠️ **Rate limits are a financial decision, not a technical one:**

| Plan | Calls/day |
|------|-----------|
| Starter | 6 / month |
| Organization | 200 / day |
| Enterprise | 600 / day |

> 🗣 *Figma Dev Mode MCP — один з найбільших зрушень для дизайн-команд. До цього агент дивився на скріншот і вгадував структуру. Тепер він читає справжнє дерево шарів, токени дизайн-системи, варіанти компонентів. Результат: код, який з першої ітерації відповідає дизайну на 95%.*
>
> 🗣 *Але є критичний практичний момент — rate limits. На Starter плані це 6 викликів на місяць. Це не помилка — шість на місяць. Organization дає 200 на день, Enterprise — 600. Тобто перш ніж підключити Figma MCP для команди, потрібно перевірити план. Це фінансове рішення, а не технічне.*

> `[image prompt]` 🖼 *Figma file tree with layers, tokens, and variants on the left; clean generated React component code on the right. Arrow between them labeled "MCP". Dark mode, Figma purple accent.*

---

## Slide 5 — Playwright vs Chrome DevTools

> "Playwright drives. Chrome DevTools debugs." — Steve Kinney

| | Playwright MCP *(Microsoft)* | Chrome DevTools MCP *(Google)* |
|-|------------------------------|-------------------------------|
| **Purpose** | Drive the browser | Inspect the live page |
| **Actions** | Click, fill, navigate, user flows | LCP, network traces, performance |
| **Testing** | E2E, cross-browser (Chromium + WebKit + Firefox) | A11y audits, debugging |
| **Under the hood** | Playwright engine | Puppeteer |

> ⚠️ Common mistake: teams install both "just in case" — pick one per job.

> 🗣 *Часта помилка — ставити обидва "про всяк випадок". Є чітке розмежування: Playwright веде браузер — клікає, заповнює форми, запускає user flow'и, тестує в трьох движках. Chrome DevTools дебажить живу сторінку — LCP, network, перформанс, accessibility. Цитата від Steve Kinney, яка добре запам'ятовується: "Playwright drives. Chrome DevTools debugs."*
>
> 🗣 *Для аутсорс-команд Playwright MCP найбільш цінний для автоматичної генерації E2E-тестів і A11y-аудитів — це два типових deliverable, які з'являються в кожному другому проекті.*

> `[image prompt]` 🖼 *Two-lane road metaphor: left lane "Playwright — drives the browser" (steering wheel icon, Chromium/WebKit/Firefox logos), right lane "Chrome DevTools — debugs" (magnifying glass, performance graph). Dark background.*

---

## Slide 6 — Mobile MCP

**Describe the scenario. The agent handles iOS & Android.**

- Works on: simulators, emulators, **real devices**
- No computer vision required — uses **native accessibility tree** first
- Falls back to screenshot analysis (Visual Sense) when a11y labels are missing
- Under the hood: WebDriverAgent (iOS), ADB + UIAutomator (Android)

```
mobile_list_elements_on_screen   → structured UI tree with coordinates
mobile_click_on_screen_at_coordinates
mobile_swipe_on_screen
mobile_take_screenshot
```

> 🗣 *Mobile MCP — прорив для мобільних команд. Ти описуєш сценарій природною мовою: "натисни кнопку логін, введи тестові дані, перевір що відкрився дашборд". Сервер сам розбирається, де ця кнопка на iOS і де вона на Android.*
>
> 🗣 *Ключовий інсайт у архітектурі: він першочергово використовує нативне дерево доступності — це структуровані дані, не картинка. Комп'ютерний зір не потрібен. Тільки якщо a11y-мітки відсутні — переходить до аналізу скріншота. Це робить автоматизацію детермінованою і швидкою. Побічний ефект: якщо Mobile MCP не може натиснути на кнопку через accessibility tree — значить у вашого додатку проблеми з доступністю. Це вже не просто тест, це a11y-аудит безкоштовно.*

> `[image prompt]` 🖼 *An iPhone and Android phone side by side, with an AI agent node above connected to both via arrows. Accessibility tree data structure visible as overlay on both screens. Dark background, clean technical aesthetic.*

---

## Slide 7 — Sentry MCP

**No more copy-pasting stack traces.**

- Agent pulls the **full issue**: stack trace + breadcrumbs + environment context
- Tools: `search_issues`, `get_issue_details`, `search_events`
- User types "root cause this issue" → agent fetches everything needed

> 🗣 *Sentry MCP вирішує одну маленьку але болючу проблему: ти більше не копіюєш stack trace в чат. Агент підключається до Sentry, сам тягне повний issue з breadcrumbs, environment variables, версією релізу. Написав "знайди root cause цього issue" — отримав відповідь. Для аутсорс-команд, де QA і девелопер часто в різних таймзонах, це означає, що агент може зробити первинну діагностику до того, як розробник прокинувся.*

> `[image prompt]` 🖼 *Split: left shows messy copy-pasted stack trace in chat (red X); right shows Sentry MCP agent automatically fetching full issue context with breadcrumbs (green checkmark). Dark background.*

---

## Slide 8 — Key Takeaway

**You don't need to build custom integrations for 80% of typical needs.**

The servers already exist. The question is which ones to connect first.

> **Starter stack for web teams:** GitHub MCP + Context7 + Playwright MCP + Sentry MCP

> 🗣 *Головний висновок цього блоку: для більшості типових потреб web і mobile команди — все вже є. Не треба нічого писати з нуля. Базовий стек, з якого можна почати: GitHub MCP, Context7, Playwright і Sentry. Це чотири сервери, які дають відчутний результат вже в перший день. Figma і Mobile MCP — наступний крок, коли є необхідність.*

> `[image prompt]` 🖼 *Four server logos (GitHub, Context7, Playwright, Sentry) arranged as a 2x2 grid with "Starter Stack" label above. Clean, dark background, subtle green "ready to use" indicator on each.*
