# 📊 Automated Business Report Engine

A tool that takes any raw CSV file of business data and automatically generates a clean PDF report — summary statistics, charts, and key insights — without the user needing to do any data analysis themselves.

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg?style=flat-square&logo=python)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Framework-Streamlit-FF4B4B.svg?style=flat-square&logo=streamlit)](https://streamlit.io/)
[![Pandas](https://img.shields.io/badge/Data-Pandas-150458.svg?style=flat-square&logo=pandas)](https://pandas.pydata.org/)

🔗 **Live demo:** Coming soon
📹 **Demo video:** Coming soon

---

## What it does

Upload a CSV. The tool automatically detects what kind of data is in each column, computes relevant statistics, generates appropriate charts, and compiles everything into a downloadable PDF report.

**This project intentionally contains no AI, LLM, or machine learning model anywhere.** It's a deterministic data-processing tool — Pandas for analysis, Matplotlib for charts, ReportLab for the PDF. For an AI/agentic project, see [Nexus — Multi-Source Agentic AI System](link-to-nexus-repo).

---

## How it works

1. **Column detection** — each column is automatically classified as numeric, categorical, date, or a unique identifier, using Pandas' inferred data types and a date-parsing attempt (no manual configuration needed)
2. **Statistics generation** — numeric columns get totals/averages/min/max; categorical columns get value counts and the most frequent value(s); date columns get a date range and monthly totals
3. **Chart selection** — the tool only generates charts that make sense for the data it finds (e.g. it won't try to build a time-series chart if there's no date column)
4. **PDF compilation** — a cover page, statistics summary, and charts are assembled into one report using ReportLab

---

## A real bug I found and fixed

While testing, I found that the "most frequent category" calculation could silently report a misleading answer. If three categories were tied at the same count, the original code (using Pandas' `.mode()`) would pick one and report it as the single winner — with nothing indicating it was actually a three-way tie.

Since the entire point of this tool is giving people trustworthy automated insights, a confidently-wrong answer is worse than no answer. I changed `most_frequent` to return a list of all tied values plus an explicit `is_tie` flag, so the report shows the tie honestly instead of picking an arbitrary winner.

---

## Tech stack

| Component | Technology |
|---|---|
| Data processing | Pandas |
| Charts | Matplotlib (static images, embedded into the PDF) |
| PDF generation | ReportLab |
| Frontend | Streamlit |
| Deployment | Streamlit Cloud |

---

## Known limitations

I'd rather state these clearly than have them discovered later:

- **Date detection only works for YYYY-MM-DD format.** Columns with dates in a different format (e.g. MM/DD/YYYY) will be misclassified as categorical text rather than recognized as dates. Fixing this would mean trying multiple date formats during detection instead of one fixed format.
- **Monthly trend grouping has only been tested with data spanning a single month.** The logic should generalize to multi-month data based on how it's built, but this hasn't yet been verified with a dataset that actually spans multiple months.

---

## Project structure

```
business-report-tool/
├── data_processing/
│   ├── loader.py             # CSV loading with error handling
│   ├── column_detector.py    # Automatic column type classification
│   └── stats_generator.py    # Statistics and trend computation
├── visualization/
│   └── chart_generator.py    # Bar, pie, and line chart generation
├── report/
│   └── pdf_builder.py        # PDF assembly with ReportLab
├── sample_data/               # Synthetic test CSVs
├── assets/screenshots/        # Example chart outputs
├── app.py                     # Streamlit app entry point
└── requirements.txt
```

---

## Run it locally

```bash
git clone https://github.com/mohanapriyaramesh23-arch/business-report-tool.git
cd business-report-tool
python -m venv report-env
report-env\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

---

*Built by Mohana Priya — B.E. ECE, Rajalakshmi Engineering College*
