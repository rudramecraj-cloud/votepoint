# VotePoint – Digital Voting Portal

VotePoint is a web-based digital voting application built with **Python, Flask, and SQLite**. It was developed for the 2026 school election at **SBOA Public (Sr. Sec.) School, South Chittoor**.

The portal lets eligible voters enter a unique voting key, select one candidate in each election category, and submit their ballot. Administrators can view vote totals and leading candidates in a dashboard and export the recorded results to an Excel workbook.

**Live deployment:** [VotePoint – Digital Voting Portal](https://digital-voting-portal.onrender.com/start)

> The application’s voting flow begins at `/voting` (or `/`). The `/start` endpoint accepts a POST request containing a voting key; opening that endpoint directly in a browser may not display the voting form.

## Features

### Voter experience
- Simple, responsive voting interface.
- Voting-key entry before accessing the ballot.
- Candidate selection using required radio-button choices.
- One selection per category.
- Duplicate-key check before displaying the ballot and again before recording a vote.
- Confirmation screen after successful submission.

### Election administration
- Password-protected administrator area.
- Results dashboard showing totals by category and candidate.
- Leading candidate or tie indication for each category.
- Vote counts and percentage breakdowns.
- Excel export of the recorded ballots.

## Election categories and candidates

The current candidate list is configured in `app.py`:

| Category | Candidates |
|---|---|
| President | Emma, Charlotte |
| Vice President | James, Chloe |
| Secretary | Abigail, Ethan, Ruby |
| Executive | Daniel, Lucas |

To change the categories or candidates, edit the `CATEGORIES` dictionary in `app.py`.

## Technology stack

- **Python** – application language
- **Flask 3.1.2** – web framework and template rendering
- **SQLite** – vote storage (`database.db`)
- **OpenPyXL 3.1.5** – Excel workbook generation
- **Gunicorn 23.0.0** – production WSGI server
- **HTML and CSS** – user interface, with Jinja templates

## Project structure

```text
votepoint-main/
├── app.py
├── database.db
├── Election_Results.xlsx
├── Procfile
├── requirements.txt
├── static/
│   └── logo.png
└── templates/
    ├── admin_login.html
    ├── ballot.html
    ├── error.html
    ├── index.html
    ├── landing.html
    ├── result.html
    └── success.html
```

| File or directory | Purpose |
|---|---|
| `app.py` | Flask application, routes, election configuration, database operations, and result export |
| `database.db` | SQLite database containing submitted votes; created/initialized by the application if needed |
| `templates/` | Jinja HTML templates for voter and administrator pages |
| `static/` | Static assets, including the school logo |
| `requirements.txt` | Python dependencies |
| `Procfile` | Gunicorn start command used by platforms such as Render |
| `Election_Results.xlsx` | Workbook included in the repository; the export route generates a fresh workbook when requested |

## Run locally

### 1. Requirements

- Python 3.10 or later (a recent supported Python version is recommended)
- `pip`

### 2. Get the source code

Clone the repository, or extract the project ZIP:

```bash
git clone <YOUR_REPOSITORY_URL>
cd votepoint-main
```

If you are using the ZIP file, extract it and open a terminal in the `votepoint-main` directory.

### 3. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 4. Install dependencies

```bash
pip install -r requirements.txt
```

### 5. Start the application

```bash
python app.py
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser. The database and `votes` table are initialized when the application starts.

For a production-style local run, use:

```bash
gunicorn app:app
```

## How to use

### Cast a vote

1. Open the portal.
2. Select **Start Voting**.
3. Enter the voter's unique key.
4. Choose one candidate in each category.
5. Submit the ballot.
6. The portal displays a confirmation page when the vote is recorded.

A key already present in the votes database is rejected. The current implementation records one row per category for each submitted ballot.

### View election results

1. Open `/admin`.
2. Enter the administrator password configured in `app.py`.
3. The application redirects authenticated administrators to `/results`.
4. Review category totals, candidate counts, percentages, and the current leader.
5. Select **Download Excel** to export the results.
6. Select **Logout** to clear the administrator session.

The Excel export contains a timestamp and the selected candidate for each category. It does not include the voting key in the exported columns.

## Application routes

| Route | Method | Purpose |
|---|---|---|
| `/` | GET | Redirects to the voting page |
| `/begin` | GET | Redirects to the voting page |
| `/voting` | GET | Displays the voting-key form |
| `/start` | POST | Validates a key and displays the ballot |
| `/vote` | POST | Validates and records the submitted ballot |
| `/admin` | GET, POST | Displays administrator login and checks the password |
| `/results` | GET | Displays the protected results dashboard |
| `/download-results` | GET | Downloads the Excel results workbook (administrator only) |
| `/logout` | GET | Clears the administrator session |
| `/reset-election` | GET | Deletes all recorded votes (administrator session required) |
