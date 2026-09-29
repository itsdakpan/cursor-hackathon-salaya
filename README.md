# Care Companion

A tablet for someone who needs care, and a control panel for the family who looks after them. The tablet is a quiet photo frame until the family has something to say. Then it asks a simple question, and if nobody answers, the family finds out.

**Live demo:** https://itsdakpan.github.io/cursor-hackathon-salaya/

## How to try it

The demo opens Margaret's photo frame and her family's panel side by side. It takes about a minute.

1. Press **Send test check-in** at the top. A photo and a question appear on Margaret's frame.
2. Tap an answer, or wait 10 seconds. An unanswered check-in gets one reminder, then the family panel shows an alert.
3. Press **Send medication reminder**. Select each item on the frame, then confirm.
4. Check **Today's activity** on the family panel. Everything that happened is logged there.

Everything runs in your browser. There is no account and nothing is stored online.

![The live demo: Margaret's frame on a tablet and the family panel side by side](docs/screenshots/demo.jpg)

## What it does

- **Photo check-ins.** The family queues photos with a caption. At set times one becomes a check-in: the tablet reads it aloud and asks "How are you feeling today?" with large answers: Good, I'm OK, Need help, or a spoken reply.
- **Medication reminders.** The tablet shows each medication with a picture. Margaret selects each one, then confirms. The family sees what she marked, which is a record of her answer rather than proof that a dose was taken.
- **Reminders and routines.** A reminder for Margaret plus a named family member who is responsible for it, like a lift to the doctor.
- **Missed check-ins get escalated.** No answer, the tablet reminds her again. Still no answer, the family's panel raises an alert and plays a sound.
- **Activity log.** Every answer, reminder and alert shows up on the family's panel with a time.

| Check-in arrives | Pills | Family panel after a missed check-in |
|---|---|---|
| ![Check-in takeover](docs/screenshots/tablet-checkin.jpg) | ![Pill reminder](docs/screenshots/tablet-pills.jpg) | ![Caretaker alert](docs/screenshots/caretaker-alert.jpg) |

## How the reminder engine works

Check-ins, pills and errands all run through the same loop:

1. The family sends or schedules an item.
2. It takes over the tablet with a sound and a spoken prompt.
3. If Margaret opens it, the countdown stops and her answer goes to the family.
4. If she doesn't, the tablet reminds her once more.
5. If there is still no answer, the family panel shows an alert and logs it.

For the demo the retry and the alert fire after 5 seconds each, so the miss path can be shown live.

The resident's name lives in one place, `frontend/assets/household.js`.

## Built with

HTML, CSS and JavaScript with no build step. The tablet and the family panel talk to each other through the browser's `BroadcastChannel` and `localStorage`, so both screens stay in sync on one machine. Speech uses the browser's built-in text to speech. Icons are Lucide. Fonts are Lexend and Atkinson Hyperlegible, which was designed for readers with low vision.

There is also a Django, Postgres and Redis backend scaffold in `backend/` with a health check. The demo does not use it yet.

## Run it locally

```bash
git clone https://github.com/itsdakpan/cursor-hackathon-salaya.git
cd cursor-hackathon-salaya/frontend
python3 -m http.server 8000
```

Open http://localhost:8000 and follow the steps in the "How to try it" panel at the top.

Use a local server rather than opening the files directly, because the two screens need to share an origin to talk to each other.

To run the backend scaffold: `cd backend && docker compose up`, then visit http://localhost:8001/health/.

## Background

I built this with a small team in one day at the Cursor hackathon in Salaya, Thailand, in August 2026. I worked on the idea, the screen designs, the pitch and parts of the front end, and presented the demo. The original team repo is [AyazYakupov/cursor-hackathon-salaya](https://github.com/AyazYakupov/cursor-hackathon-salaya).

Afterwards I redesigned the front end for older eyes, fixed the missed check-in alerts, and made the demo easier to try.

Photos: StockSnap (CC0) and Eric Oliveira on Unsplash.
