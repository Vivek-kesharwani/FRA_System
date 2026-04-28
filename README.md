AI-Based FRA Diagnostic System
Automated Frequency Response Analysis (FRA) for power transformers: parsing, feature extraction, machine learning, anomaly detection, expert rules, interactive web dashboard, and PDF reports.

Table of contents
Overview
Problem and approach
Features
Architecture
Project structure
Requirements
Installation
Running the application
Usage
Web routes
Sample diagnostic output
Tech stack
Roadmap
Contributing
License
Overview
This platform reduces reliance on manual expert interpretation of FRA sweeps by combining:

Multi-format ingestion (CSV / Excel)
Signal features and statistical summaries
Supervised fault classification (e.g. Random Forest)
Unsupervised anomaly scoring (Isolation Forest)
Rule-based expert interpretation and maintenance-style recommendations
A dark-themed Flask UI with Chart.js visualizations and optional PDF export
Problem and approach
Context: FRA is used to detect faults such as winding deformation, core displacement, and insulation issues. Interpretation is often slow, format-dependent, and inconsistent.

Approach: A single pipeline normalizes inputs, extracts features, runs ML + anomaly models, merges results with expert rules, and presents a unified diagnosis with plots and a downloadable report.

Features
Area	Description
Parsing	CSV and Excel; frequency / magnitude columns auto-detected where possible
Features	Peaks, band energies, statistics
ML	Fault classification with confidence
Anomaly	Isolation Forest–style scoring for unusual patterns
Expert system	Rules for fault type, severity, recommendations
Visualization	Log-scale FRA, comparison, difference; interactive charts + Matplotlib exports
UI	Responsive dashboard (upload → analyze → results → report)
Reports	PDF generation (ReportLab) when the pipeline completes successfully
Architecture
FRA file upload
      ↓
Parse & preprocess
      ↓
Feature extraction
      ↓
ML classification + anomaly detection
      ↓
Expert rules & unified diagnosis
      ↓
Plots (static + chart data) + optional PDF
      ↓
Web UI (results page)
Project structure
Repository root contains this README. Application code lives under FRA_AI_Data/:

FRA_Datas/
├── README.md                 ← This file
└── FRA_AI_Data/
    ├── app/
    │   ├── app.py            # Flask app, routes, upload/report paths
    │   ├── static/           # css/, js/, images/, plots/
    │   └── templates/        # landing, index (upload), result, history, about
    ├── src/
    │   ├── parser/           # CSV/Excel/universal parsing
    │   ├── features/         # Feature extraction & FRA signal features
    │   ├── models/           # Training, prediction, anomaly
    │   ├── expert/           # Rule engine
    │   ├── utils/            # Plotting, PDF reports
    │   ├── pipeline.py       # End-to-end process_fra
    │   └── analyzer.py       # DataFrame-level analysis helper
    ├── data/                 # Uploads & datasets (see data/raw for reference e.g. fra_healthy.csv)
    ├── reports/              # Generated PDFs
    ├── requirements.txt
    ├── main.py
    └── notebooks/            # Experiments (optional)
Requirements
Python 3.10+ recommended
Dependencies are listed in FRA_AI_Data/requirements.txt (Flask, pandas, NumPy, SciPy, matplotlib, scikit-learn, openpyxl, ReportLab, joblib).
Installation
git clone <your-repository-url>
cd FRA_Datas/FRA_AI_Data
Create and activate a virtual environment:

# Windows (PowerShell)
python -m venv venv
.\venv\Scripts\Activate.ps1

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
Install dependencies:

pip install -r requirements.txt
Running the application
From the FRA_AI_Data directory:

python app/app.py
Open a browser at:

http://127.0.0.1:5000/

Usage
Open Home, then go to Diagnostics (or open /analysis directly).
Upload an FRA file (CSV or Excel).
Review interactive charts, fault type, confidence, severity, features, and insights.
Download the PDF report when generation succeeds (link on the results page).
Reference curve: If data/raw/fra_healthy.csv exists, it is used as the baseline for comparison. Otherwise the pipeline falls back according to src/pipeline.py logic.

Web routes
Route	Method	Description
/	GET	Landing page
/analysis	GET	Upload / diagnostics entry
/analyze	POST	Upload file and run pipeline; returns results page
/history	GET	History UI (CSV listing)
/about	GET	About page
/download-report	GET	PDF download (?f=<filename.pdf>, basename only)
Sample diagnostic output
Structured result fields (illustrative) returned to the UI include:

{
  "fault": "Winding deformation",
  "confidence": 87,
  "severity": "High",
  "anomaly_score": 72.5,
  "recommendation": "Schedule detailed inspection per utility procedure."
}
Exact keys and types match the unified diagnosis object produced in src/pipeline.py and templates.

Tech stack
Layer	Technologies
Backend	Python, Flask
ML / stats	scikit-learn, NumPy, SciPy, pandas
Plots	Matplotlib (exports), Chart.js (interactive UI)
Reports	ReportLab
Frontend	HTML, CSS, JavaScript (Jinja2 templates)
Roadmap
Broader real-world FRA dataset coverage
Optional deep-learning models for waveform classification
Multi-file / multi-epoch comparison workflows
Cloud-friendly deployment packaging
Contributing
Contributions are welcome: fork the repository, open a branch, and submit a pull request with a clear description of changes and any new dependencies.

License
This project is intended for academic and research use unless otherwise stated by the repository owner.
