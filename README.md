# My Daily Progress — Telegram AI Agent

> A Telegram bot where I report what I did during the day. An AI agent matches each message to my daily routine, confirms it with me, logs it to Google Sheets, and sends an end-of-day report of what was done and what was missed.

**🌐 English** · [العربية](#-النسخة-العربية)

> **Status: in progress.** The design is complete and the Telegram bot is created; the n8n workflows are being built.

---

## 🎯 The Problem

I have a fixed daily routine, but at the end of the day it is hard to see honestly what I actually did and what I skipped. Writing it down manually is easy to forget; a quick chat message is not.

---

## 🗓️ The Routine (the only allowed tasks)

| Task | Days | Time |
|---|---|---|
| Wake up | Every day | 08:00 |
| Women's Affairs | Saturday, Monday, Wednesday | 10:00 – 15:00 |
| Lunch | Every day | After returning home |
| Laptop work | Every day | — |
| Fajr · Dhuhr · Asr · Maghrib · Isha | Every day | Five separate tasks |
| Sleep | Every day | 22:00 |

---

## ⚙️ Design

**1. Chat workflow (n8n)**

```text
Telegram Trigger → AI Agent → Telegram Send Message
                     ├─ Chat model
                     ├─ Simple Memory (session = chat id)
                     ├─ Google Sheets: Log Task  (append or update, match on Key)
                     └─ Google Sheets: Read Log  (today's rows)
```

**2. End-of-day report (no AI — a deterministic comparison)**

```text
Schedule Trigger → Google Sheets (today) → Code: plan vs logged → Telegram message
```

**Log sheet:** `Key | Date | Day | Task | Status | Time | Notes` — `Key = Date | Task` prevents duplicate rows when the same task is confirmed twice.

---

## 🧠 Why an Agent — and Its Boundaries

The agent must **collect, validate, confirm, then execute**:

- Extract several tasks from one free-text message (Arabic, Levantine dialect).
- Ask when something is unclear (*"I prayed"* — which prayer?), never guess.
- Flag a task that is not scheduled today (Women's Affairs on a Tuesday).
- Show a summary and log **only after explicit confirmation**.
- Never claim a task was saved unless the Sheets tool succeeded.

The daily report is calculated in code, not by the model, so it cannot hallucinate.

---

## 🧪 Planned Tests

Multiple tasks in one message · unclear prayer · task not scheduled today · task outside the routine · correction before saving · no confirmation → nothing saved · confirmation → rows saved · tool failure → "not saved" · duplicate confirmation → one row · memory · end-of-day report.

---

## 🛠️ Tools

Telegram (BotFather) · n8n (AI Agent, Simple Memory, Schedule, Code) · Google Sheets

**Why Telegram instead of WhatsApp:** the course project used WhatsApp; I do not use WhatsApp, so I built the same agent pattern on Telegram.

---

## 👩‍💻 Author

**Dina Albaghdadi** — AI Automation

[LinkedIn](https://www.linkedin.com/in/dina-albaghdadi-79bb9a25a/) · [GitHub](https://github.com/Dina-Albaghdadi)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

> بوت تيليجرام أخبره بما أنجزته خلال اليوم، فيطابق الـAI Agent كل رسالة مع روتيني، ويؤكدها معي، ويسجلها في Google Sheets، ويرسل في نهاية اليوم تقريرًا بما أُنجز وما لم يُنجز.

> **الحالة: قيد التنفيذ.** التصميم مكتمل والبوت مُنشأ، ويجري بناء الـWorkflows في n8n.

### 🧠 لماذا Agent وما حدوده

يجمع ويتحقق ويؤكد ثم ينفذ: يستخرج عدة مهام من رسالة واحدة، ويسأل عند الغموض، وينبّه للمهمة غير المجدولة اليوم، ولا يسجل إلا بعد تأكيد صريح، ولا يدّعي الحفظ إلا إذا نجحت الأداة. والتقرير اليومي يُحسب بالكود وليس بالنموذج.

**لماذا تيليجرام وليس واتساب:** مشروع المحاضرة استخدم واتساب، وأنا لا أستخدمه، فبنيت نفس النمط على تيليجرام.

</div>
