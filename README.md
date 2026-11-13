# JIRA AI Bridge

> **Stop logging your work twice.** JIRA AI Bridge watches your AI-assisted dev
> activity (git commits, Claude Code sessions, manual notes), creates + logs the
> JIRA tasks for you, and shows a dashboard of *"what I did today / this week."*

The elevated version of the CodeAlpha **Task Automation** task — it automates the
most tedious chore in AI-assisted development: **manual JIRA bookkeeping**. Runs
fully offline against a local mock JIRA when no credentials are configured.

---

## 🎬 Demo Video

A short walkthrough of the dashboard — summary stats, the work timeline, and
one-click **Sync to JIRA**. Click the poster to play
[`screenshots/demo.mp4`](screenshots/demo.mp4):

[![Watch the demo](screenshots/demo-thumbnail.png)](screenshots/demo.mp4)

---

## 📸 Screenshots

### Dashboard overview

Summary stats (tasks today / this week / total, time logged, pending sync),
**by-source** and **by-project** breakdowns, MOCK/real mode, and dark-mode toggle.

![Dashboard overview](screenshots/screenshot-dashboard.png)

### Work timeline

Each captured work event as a card: title, **source badge** (git / claude / cli),
inferred **issue type** (Story / Bug / Task), files changed, time, JIRA key, and
status.

![Work timeline](screenshots/screenshot-timeline.png)

### One-click sync

Pending events show a **Sync to JIRA** button; syncing assigns a key
(e.g. `ENG-22`) and flips the status to **synced** (toast confirmation shown).

![Sync to JIRA](screenshots/screenshot-sync.png)

---

## ▶️ Run it

```bash
pip install -r requirements.txt
python demo_seed.py               # seed ~12 sample events for an instant demo
python -m jira_bridge dashboard   # dashboard at http://127.0.0.1:5050
```

Runs fully offline against a local mock JIRA by default. To point it at a real
JIRA instance, copy `.env.example` to `.env` and fill in your credentials.

## 📁 Project Structure

```
jira-autopilot/
├─ src/jira_bridge/
│  ├─ core.py            # WorkEvent + BridgeEngine (persist, dedup, infer, sync)
│  ├─ jira_client.py     # JiraClient ABC, RealJiraClient, MockJiraClient, get_client()
│  ├─ config.py          # .env loading + Settings
│  ├─ report.py          # text "what I did" reports
│  ├─ cli.py             # argparse CLI
│  ├─ __main__.py        # python -m jira_bridge
│  ├─ watchers/
│  │  ├─ base.py         # BaseWatcher interface
│  │  ├─ git_watcher.py  # polls git log / status
│  │  ├─ claude_hook.py  # reads Claude Code hook payload from stdin
│  │  └─ cli_watcher.py  # manual one-shot trigger
│  └─ web/
│     ├─ app.py          # Flask app factory + JSON API
│     ├─ templates/dashboard.html
│     └─ static/style.css, app.js
├─ tools/
│  └─ icontool.py        # generates web/templates/_icons.html from the lucide package
├─ tests/                # pytest suite (no network / git required)
├─ screenshots/
├─ demo_seed.py          # seed ~12 sample events for an instant great demo
├─ requirements.txt
├─ pyproject.toml
└─ .env.example
```
