# School Tracker Web 🌐

A Flask web interface for the [School Fees & Results Tracker](https://github.com/aloismutharimi/school_tracker-sys) — the same engine, browser-based, for administrators who don't use the terminal.

---

## What It Does

- **Dashboard** — school-wide stats at a glance: total billed, collected, outstanding, arrears, top performers
- **Students** — list every student with fee status. Add new students through a form.
- **Payments** — record fee payments with student dropdown, amount, method, and date
- **Scores** — record exam scores per subject
- **Reports** — generate any report mode in the browser, then download CSV or PDF
- **Downloads** — CSV opens in Excel, PDF is print-ready for guardians

No database, no login system. It's a thin web layer over the CLI package.

---

## How It Works

The web app imports from the `school_tracker` package — the exact same functions the CLI uses. One source of truth for balances, rankings, and reports.

```
CLI Engine (dependency)          Web Layer (this repo)
├── storage.py                   ├── app.py         ← Flask routes
├── tracker.py                   ├── templates/     ← HTML
├── report.py                    └── static/        ← CSS
├── pdf_report.py
└── cli.py
```

Every page either calls a tracker function, renders what a report function returns, or handles a form submission. There's almost no logic in the web layer itself.

---

## Install and Run

### 1. Clone the repo

```bash
git clone https://github.com/aloismutharimi/school_tracker-web.git
cd school_tracker-web
```

### 2. Create a virtual environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

This installs Flask and pulls `school_tracker` from GitHub automatically.

### 4. Run

```bash
python app.py
```

Open `http://localhost:5000` in your browser.

The data folder is created automatically in the current working directory on first run, seeded with empty files and a starter fee schedule.

### Configure data location (optional)

By default, data lives in `./data/`. To use a different location:

```bash
export SCHOOL_TRACKER_DATA=/path/to/your/data      # macOS/Linux
set SCHOOL_TRACKER_DATA=D:\school_data             # Windows
```

---

## Pages

| Page | Route | Purpose |
|---|---|---|
| Dashboard | `/` | School overview with stats and top performers |
| Students | `/students` | List all students with fee status |
| Add Student | `/students/new` | Form to register a new student |
| Record Payment | `/payments/new` | Form to log a fee payment |
| Record Score | `/scores/new` | Form to log an exam score |
| Reports | `/reports` | Generate and download reports |

---

## Reports

Every report mode from the CLI is available in the browser:

| Mode | Filter |
|---|---|
| Full | All classes, fees + grades |
| Fees only | Balances and arrears |
| Grades only | Rankings and averages |

Filters:
- **Class** — narrow to one class
- **Student** — solo student report with subject breakdown

After generating, click **Download CSV** or **Download PDF** — the file reflects whatever filters are active.

---

## Architecture

```
school_tracker-web/
├── app.py                   # Flask app — routes and form handling
├── templates/               # Jinja2 HTML templates
│   ├── base.html            # Shared layout, nav, flash messages
│   ├── home.html            # Dashboard
│   ├── students.html        # Students list
│   ├── add_student.html     # Add student form
│   ├── add_payment.html     # Payment form
│   ├── add_score.html       # Score form
│   ├── reports.html         # Reports page
│   └── placeholder.html     # Fallback for unbuilt routes
├── static/
│   └── style.css            # Dark theme, matches portfolio
├── requirements.txt
├── pyproject.toml
└── README.md
```

---

## Stack

`Python 3.10+` · `Flask` · `Jinja2` · `school-tracker` (CLI package as dependency)

---

## Related Projects

- **[school_tracker-sys](https://github.com/aloismutharimi/school_tracker-sys)** — The CLI engine this web app builds on. Same logic, terminal interface.

---

## License

MIT
