# 📊 Automated Business Reporter

## Overview
An automated reporting tool that connects to any PostgreSQL database, analyzes KPIs using Claude AI, and delivers a professional report via email — with zero manual intervention.

The tool reads KPIs from a simple text file, generates SQL queries automatically, interprets the results, and sends:
- **Email body** — concise executive summary with key insights
- **Email attachment** — full detailed `.docx` report

[📧 View Email Screenshot](examples/Screenshot_email.pdf)
---

## The Problem It Solves
Business reporting is often manual and time-consuming. An analyst has to run queries, compile results, write commentary, and distribute the report — every week. This tool automates the entire process end-to-end.

---

## How It Works
```
Reads KPIs from kpis.txt
        ↓
Connects to PostgreSQL and reads schema automatically
        ↓
Claude generates SQL queries for each KPI
        ↓
Queries are executed on the database
        ↓
Claude writes an executive summary + full report
        ↓
Report saved as .docx
        ↓
Email sent automatically with summary + attachment
```

---

## Example Output

The `examples/` folder contains a sample report generated from a real e-commerce database (`amazon_db`) with the following KPIs:

- Total revenue across all transactions
- Revenue and profit breakdown by product category
- Top 5 best selling products by units sold
- Top 3 sellers by total revenue
- Order cancellation rate

> Note: When a KPI cannot be calculated due to missing data, the report explicitly flags it with a recommendation — providing actionable insight for data quality improvement.

---

## Features
- Connects to any PostgreSQL database automatically
- Reads schema dynamically — no hardcoded queries
- KPIs defined in plain English in `kpis.txt` — no code changes needed
- Handles missing data gracefully with clear recommendations
- Executive summary formatted for mobile reading
- Full detailed report as `.docx` attachment
- Schedulable with cron for fully automated weekly delivery

---

## Tech Stack

| Component | Technology |
|---|---|
| AI Model | Anthropic Claude (`claude-sonnet-4-6`) |
| Database | PostgreSQL via psycopg2 |
| Report Generation | python-docx + html2docx |
| Email | smtplib + Gmail SMTP |
| Scheduling | cron (macOS/Linux) |
| Language | Python 3.12 |

---

## Setup

**1. Clone the repository**
```bash
git clone https://github.com/EdoChiari/automated-reporter.git
cd automated-reporter
```

**2. Create virtual environment**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

**3. Install dependencies**
```bash
pip install anthropic psycopg2-binary python-dotenv python-docx markdown html2docx
```

**4. Configure credentials**

Create a `.env` file:
```
ANTHROPIC_API_KEY=your-anthropic-api-key
DB_HOST=localhost
DB_PORT=5432
DB_NAME=your-database
DB_USER=your-username
DB_PASSWORD=your-password
EMAIL_SENDER=your@gmail.com
EMAIL_PASSWORD=your-gmail-app-password
EMAIL_RECIPIENT=recipient@email.com
```

**5. Define your KPIs**

Edit `kpis.txt` with one KPI per line in plain English:
```
Total revenue across all transactions
Revenue breakdown by product category
Top 5 best selling products
```

---

## Running the Script
```bash
python report.py
```

## Scheduling with Cron (optional)

To run automatically every Monday at 8:00 AM:
```bash
crontab -e
```

Add this line:
```
0 8 * * 1 cd /path/to/automated-reporter && /path/to/.venv/bin/python report.py
```

---

## Output

The script generates a `.docx` report saved locally and sent via email:
```
📁 automated-reporter/
    ├── report.py          ← main script
    ├── kpis.txt           ← KPIs in plain English
    ├── .env               ← credentials (never committed)
    └── examples/
            └── report_2026-03-23.docx  ← sample output
```

---

## Roadmap
- [x] Dynamic SQL generation from natural language KPIs
- [x] Automatic schema discovery
- [x] Executive summary for email body
- [x] Full report as .docx attachment
- [x] Graceful handling of missing data with recommendations
- [x] Support for multiple recipients
- [x] HTML email template for richer formatting
- [ ] Slack integration for report delivery
