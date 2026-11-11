<div align="center">

# ✅ Enjazi | إنجــازي
**مساعدك الذكي لتنظيم مهامك وإنجاز يومك**

*Write naturally. Get organized. Know what matters.*

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/enjazi-ai/blob/main/Enjazi_AI.ipynb)
![Model](https://img.shields.io/badge/Model-Qwen2.5-6366F1)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Languages](https://img.shields.io/badge/العربية-English-8B5CF6)

</div>

---

## The idea

Most to-do apps ask you to work for them: pick a category, set a priority, choose a date, fill a form.
**Enjazi flips that.** Just write the way you'd text a friend:

> *"عندي اختبار الأسبوع الجاي ولازم أخلص المشروع بكرة"*
> *"Remind me to submit the report tomorrow at 3"*

Enjazi reads it, turns it into clear tasks, and tells you what deserves your attention first, and why.

## Run it (free, about 5 minutes the first time)

1. Click **Open in Colab** above.
2. `Runtime → Change runtime type → T4 GPU`.
3. Run the cells from top to bottom (the first run downloads the model, a few minutes).
4. The last cell prints a **public link** (`https://xxxx.gradio.live`). Open it.

The app uses **one model only: Qwen2.5-3B-Instruct** (free, open). No API key and no database needed. On a machine without a GPU it falls back to the 1.5B model (slower, less accurate). You can switch to `Qwen/Qwen2.5-7B-Instruct` on a bigger GPU.

## The five pages

| Page | What it is for |
|---|---|
| ✍️ **Add tasks** | Write a free-form note in Arabic, English, or both. Qwen splits it into separate tasks and extracts the **date, time, category, importance and estimated duration**. One-tap examples are provided. |
| 🎯 **Priorities** | A big card answers *"What should I do first?"* with a plain reason (e.g. *"Because it is due tomorrow and it matters"*). All other tasks follow, ranked **Critical / High / Medium / Low**. You can override any priority and the app respects your choice. |
| ✅ **Check** | Tick one or more tasks, then mark them **Done**, **move them to tomorrow**, or **delete** them. Finished tasks are listed underneath. |
| 💬 **Assistant** | Chat in your own words: *"What should I focus on today?"*, *"I have two free hours"*, *"move the project to day after tomorrow"*, *"I finished the report"*, *"Remind me to call Sara tomorrow at 4"*. Qwen decides the action and the app performs it. |
| 📊 **Dashboard** | Totals, completion rate and progress bar, **workload for the next 7 days**, tasks **by category**, and open tasks **by priority**. It updates the moment you add or finish a task. |

**Language:** use the switch under the banner (English / العربية). The whole interface changes to **one language at a time**, with right-to-left text for Arabic. Your notes are never translated.

## How it works

```mermaid
flowchart LR
    A[✍️ Note<br/>Arabic / English] --> B[🤖 Qwen2.5<br/>strict JSON]
    B --> C[Python validates<br/>and computes dates]
    C --> D[📋 Tasks]
    D --> E[🎯 Priority score + reason]
    E --> F[✅ Check]
    E --> G[📊 Dashboard]
    H[💬 Chat message] --> I[🤖 Qwen2.5<br/>picks an action] --> D
```

- **Qwen returns "days from today"**, and Python computes the real date. Small models are weak at calendar math, so this keeps dates reliable. The prompt also includes a "days until each weekday" table.
- **Priority score** = urgency (past / today / tomorrow / this week / later) + importance × 8, mapped to Critical, High, Medium, Low.
- **Never breaks:** if Qwen returns something unreadable, the note is saved as one plain task and you are told.
- **No database:** everything lives in the browser session. Refreshing the page starts clean.

## 🧪 Test scenarios (also good for a demo video)

Dates below assume Qwen reads the note correctly; wording of reasons may vary slightly.

| # | Page | Do this | Expect |
|---|---|---|---|
| 1 | Add tasks | Click the first example (*"عندي اختبار الأسبوع الجاي ولازم أخلص المشروع بكرة"*) → **Organize** | 2 task cards: **اختبار** (due in 7 days, Study, importance 5) and **أخلص المشروع** (due tomorrow, Work) |
| 2 | Add tasks | Example 2 (*"Remind me to submit the report tomorrow at 3pm"*) | 1 card, due tomorrow, time **15:00**, category Work |
| 3 | Add tasks | Example 3 (*"buy groceries today, call the dentist on Thursday, maybe read a book"*) | 3 cards. "Read a book" has no date and low importance |
| 4 | Add tasks | Type your own messy note, e.g. *"gym tonight and email the client Sunday"* | Tasks split correctly, wording kept as you wrote it |
| 5 | Priorities | Open the page | Top card **"Do this first"** with a reason; the rest ranked below |
| 6 | Priorities | Pick "Read a book", set **Critical**, press **Set priority** | It jumps to the top, reason says *"you chose this priority"* |
| 7 | Check | Tick two tasks → **Done** | They disappear from the list, appear under **Finished**, a 🎉 message shows |
| 8 | Check | Tick a task → **Move to tomorrow** | Its date becomes tomorrow; priorities update |
| 9 | Assistant | *"What should I focus on today?"* | Names the first task and the reason, then the next two |
| 10 | Assistant | *"I have two free hours, what fits?"* | A list of tasks whose estimated minutes fit within 120 |
| 11 | Assistant | *"I finished the report"* | Report is marked done |
| 12 | Assistant | *"Move the project to day after tomorrow"* | Project date moves two days ahead |
| 13 | Assistant | *"Remind me to call Sara tomorrow at 4"* | A new task is added |
| 14 | Assistant | *"Delete the groceries"* | Task removed |
| 15 | Dashboard | Open after scenarios 1-14 | KPIs, completion %, 7-day workload bars, category donut, priority bars |
| 16 | Language | Switch to **العربية** | Every label, card, reason, chart and reply turns Arabic (right-to-left). Try *"وش أبدأ فيه اليوم؟"* and *"عندي ساعتين فاضية"* |
| 17 | Edge | Press **Organize** with an empty box, or press **Done** with nothing ticked | A friendly warning, nothing breaks |

## Repository layout

```
enjazi-ai/
├── Enjazi_AI.ipynb     # the whole app (run it in Colab)
├── requirements.txt    # for running locally with a GPU
└── README.md
```

## 🛣️ Future work

Not built yet in this prototype, planned for the full product:

- 📱 **Mobile app** (iOS and Android), with the same natural-language notes
- ⏰ **Push reminders** before deadlines, with quiet hours so it never nags at night
- 🗓️ **Smart day plan**: a realistic schedule that fits your working hours and never overloads the day
- 🔁 **Routines**: say "gym every evening" once and it repeats automatically
- 🧠 **Habit learning**: learn your peak focus hours and typical task lengths, visible and editable
- 📊 **Progress history**: daily, weekly, monthly and yearly views, streaks and a consistency map (needs stored history)
- 💾 **Accounts without email**: a private key to keep tasks across sessions and devices, with a real database
- 🔎 **Cross-language search**: search *project* and find *مشروع*
- 🎙️ **Voice notes** in Arabic and English
- 👥 **Shared lists** for family and teams
- 🌐 **Public web version** (not tied to Colab sessions)
- ✅ **Evaluation**: a test set of real Arabic notes to measure and improve Qwen's accuracy (and try larger or fine-tuned models)

## 📄 License

MIT.

---

<div align="center">

**Enjazi: your tasks, intelligently organized.**

</div>
