# wastewater
Chicago Wastewater Monitor


Automated daily surveillance of respiratory-disease signals in Chicago wastewater

A lightweight Python monitor that checks Chicago's public wastewater data, records historical readings, publishes a responsive dashboard, and sends email alerts when reported activity changes.

[!IMPORTANT]
Wastewater measurements are community-level indicators. They are not individual diagnoses, exact case counts, or drinking-water safety measurements.

Features
Checks Chicago's wastewater-surveillance data every day

Tracks SARS-CoV-2, influenza A, influenza B, and RSV

Records activity categories, activity values, concentrations, and reporting weeks

Preserves historical observations in JSON and CSV formats

Emails when readings change or a new pathogen appears

Searches Chicago's data catalog for newly published wastewater datasets

Publishes a mobile-friendly dashboard through GitHub Pages

Supports light and dark display modes

Runs without a dedicated server or paid hosting service

Dashboard
After deployment, the dashboard provides:

Current citywide activity cards

Recent concentration trends by pathogen

A detailed table of the latest readings

Downloadable historical CSV data

A discovery list of related Chicago wastewater datasets

Source and last-checked timestamps

Your dashboard will be available at:

text
https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/
Replace YOUR-USERNAME and YOUR-REPOSITORY with your GitHub account and repository names.

How It Works



The workflow runs once daily, retrieves the latest citywide observations, compares them with the stored state, updates the dashboard data, and commits genuine changes back to the repository. If the configured notification conditions are met, it sends an SMTP email.

Project Structure
text
chicago-wastewater-monitor/
├── .github/
│   └── workflows/
│       └── daily-check.yml
├── docs/
│   ├── data/
│   │   ├── history.csv
│   │   ├── history.json
│   │   └── latest.json
│   ├── app.js
│   ├── chicago-wastewater-monitor.html
│   ├── index.html
│   └── styles.css
├── .gitignore
├── monitor.py
├── monitor_state.json
└── README.md
File	Purpose
monitor.py	Retrieves data, compares readings, updates history, discovers datasets, and sends email
.github/workflows/daily-check.yml	Schedules the monitor and deploys GitHub Pages
monitor_state.json	Stores the previous snapshot used for change detection
docs/data/latest.json	Supplies the newest readings to the dashboard
docs/data/history.json	Supplies historical observations to the trend chart
docs/data/history.csv	Provides a downloadable tabular history
docs/chicago-wastewater-monitor.html	Main dashboard interface
docs/app.js	Loads data and renders cards, tables, and charts
docs/styles.css	Responsive light/dark dashboard styling
Quick Start
Prerequisites
A GitHub account

Git installed locally, or permission to upload files through GitHub

An SMTP-enabled email account if you want notifications

Python 3.12 only if you want to run the monitor locally

Create the Repository
Create a new repository on GitHub.

Upload or push all project files, including the hidden .github directory.

Confirm that .github/workflows/daily-check.yml appears in the repository.

Using Git from a terminal:

bash
git init
git add .
git commit -m "Add Chicago wastewater monitor"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
git push -u origin main
Enable Workflow Access
Open Settings → Actions → General.

Find Workflow permissions.

Select Read and write permissions.

Save the setting.

The workflow needs write access because it commits updated data files to the repository.

Enable GitHub Pages
Open Settings → Pages.

Under Build and deployment, select GitHub Actions as the source.

Open the repository's Actions tab.

Select Daily Chicago wastewater check.

Choose Run workflow.

The first successful run generates the current data files and deploys the dashboard.

Email Notifications
Email is optional. Without SMTP secrets, daily collection and dashboard deployment still work.

Add secrets under:

Settings → Secrets and variables → Actions → New repository secret

Secret	Required	Description	Example
SMTP_HOST	Yes	SMTP server hostname	smtp.gmail.com
SMTP_PORT	Yes	SMTP port	465
SMTP_USER	Yes	SMTP login and sending account	name@example.com
SMTP_PASSWORD	Yes	SMTP or application password	Stored only as a secret
EMAIL_TO	Yes	Alert recipient	name@example.com
EMAIL_FROM	No	Sender address	Defaults to SMTP_USER
Gmail Example
For Gmail, use:

text
SMTP_HOST=smtp.gmail.com
SMTP_PORT=465
SMTP_USER=your-address@gmail.com
SMTP_PASSWORD=your-app-password
EMAIL_TO=your-address@gmail.com
Use a Google app password, not your normal account password. App passwords generally require two-step verification to be enabled.

[!CAUTION]
Never place passwords, SMTP credentials, or email secrets directly in monitor.py, the workflow YAML, or any committed file.

Configure with GitHub CLI
If you use the GitHub CLI, secrets can be added from the terminal:

bash
gh secret set SMTP_HOST
gh secret set SMTP_PORT
gh secret set SMTP_USER
gh secret set SMTP_PASSWORD
gh secret set EMAIL_TO
gh secret set EMAIL_FROM
The CLI securely prompts for each value.

Alert Rules
The monitor sends an email when it detects one or more of these conditions:

A pathogen's activity category changes

A newly reported week contains a different concentration

A new pathogen appears in the citywide dataset

A new Chicago wastewater-related dataset is discovered

The initial run creates a baseline and does not send an alert by default. To send a test notification on the first run, add this variable to the monitor step in the workflow:

text
env:
  EMAIL_ON_FIRST_RUN: "true"
Remove it or change it to "false" after testing.

Schedule
The included workflow runs daily at 8:17 AM America/Chicago:

text
on:
  schedule:
    - cron: "17 8 * * *"
      timezone: "America/Chicago"
  workflow_dispatch:
workflow_dispatch keeps the manual Run workflow button available. Change the cron expression if you want a different schedule.

[!NOTE]
A daily check does not mean Chicago publishes a new sample every day. The dashboard may show the same reporting week until the source dataset is updated.

Run Locally
No third-party Python packages are required.

bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY
python monitor.py
Start a local web server for the dashboard:

bash
python -m http.server 8000 --directory docs
Open:

text
http://localhost:8000/chicago-wastewater-monitor.html
Opening the HTML file directly with a file:// URL may prevent the browser from loading the JSON data. Use the local web server instead.

Local Email Test
Set the SMTP variables in your shell before running the monitor.

macOS or Linux
bash
export SMTP_HOST="smtp.gmail.com"
export SMTP_PORT="465"
export SMTP_USER="your-address@gmail.com"
export SMTP_PASSWORD="your-app-password"
export EMAIL_FROM="your-address@gmail.com"
export EMAIL_TO="your-address@gmail.com"
export EMAIL_ON_FIRST_RUN="true"
python monitor.py
PowerShell
powershell
$env:SMTP_HOST = "smtp.gmail.com"
$env:SMTP_PORT = "465"
$env:SMTP_USER = "your-address@gmail.com"
$env:SMTP_PASSWORD = "your-app-password"
$env:EMAIL_FROM = "your-address@gmail.com"
$env:EMAIL_TO = "your-address@gmail.com"
$env:EMAIL_ON_FIRST_RUN = "true"
python monitor.py
Data Source
The primary source is the City of Chicago's Respiratory Virus Wastewater Surveillance dataset:

Dataset page: <https://data.cityofchicago.org/Health-Human-Services/Respiratory-Virus-Wastewater-Surveillance/4tzt-ir6h>

Socrata API: <https://data.cityofchicago.org/resource/4tzt-ir6h.json>

Dataset ID: 4tzt-ir6h

The monitor requests citywide records where sitename is All sites. It also searches Chicago's Socrata catalog for related wastewater datasets.

Data Interpretation
Field	Meaning
activity_category	Qualitative activity level, such as Minimal, Low, Moderate, High, or Very High
activity_value	Numeric viral activity value associated with the category
concentration	Average measured concentration for the pathogen and reporting week
week_end	Ending date of the source reporting week
site	Geographic reporting level; this project uses All sites
Concentrations should be compared with earlier observations for the same pathogen. Raw concentration values should not be used to rank different pathogens against one another.

Bacterial Dataset Discovery
The project watches Chicago's data catalog for new wastewater-related datasets. Discovery is intentionally conservative:

A matching catalog result is reported as a newly discovered dataset.

The project does not automatically assume the dataset measures bacteria.

A human should inspect the dataset's fields, units, sampling sites, and update pattern before adding it to routine monitoring.

The current dashboard focuses on the validated respiratory-virus dataset.

This avoids presenting unrelated change notices, historical datasets, or differently structured measurements as current bacterial surveillance.

Troubleshooting
Workflow Cannot Push Changes
Confirm Settings → Actions → General → Workflow permissions is set to Read and write permissions.

Dashboard Shows No Data
Run the workflow manually from the Actions tab.

Confirm docs/data/latest.json exists after the run.

Review the failed workflow step for an API or permissions error.

Confirm GitHub Pages uses GitHub Actions as its source.

Email Is Skipped
Check that all required secrets exist and that the names match exactly:

text
SMTP_HOST
SMTP_PORT
SMTP_USER
SMTP_PASSWORD
EMAIL_TO
Also verify that the SMTP provider allows application access. Some providers require an app password or SMTP-specific credential.

Scheduled Run Did Not Start Exactly on Time
GitHub schedules are not real-time job schedulers. Runs may begin later during periods of heavy demand. Use Run workflow when an immediate check is needed.

Dashboard Works Locally but Not on Pages
Check browser developer tools for missing files. The dashboard expects these relative paths:

text
./data/latest.json
./data/history.json
./data/history.csv
Preserve the docs/data directory structure when moving files.

Security
Keep all credentials in GitHub Actions secrets.

Do not commit .env files or SMTP passwords.

Use an application-specific password where supported.

Review third-party action versions before upgrading them.

Keep workflow permissions limited to what the project needs.

Treat generated measurements as public data; do not add personal health information.

Limitations
Wastewater signals cannot determine how many individual people are infected.

Source reporting can lag sample collection.

Daily checks may repeatedly find the same most-recent reporting week.

Concentrations from different pathogens are not directly comparable.

Catalog discovery can identify related datasets that require manual validation.

Email delivery depends on the selected SMTP provider.

GitHub may delay scheduled workflows during heavy demand.

Roadmap
Potential improvements include:

Separate alerts for category changes versus concentration-only changes

Configurable alert thresholds

Neighborhood or sewershed-level views

Validated bacterial and gastrointestinal pathogen sources

Multiple email recipients

Microsoft Teams, Slack, or Discord notifications

Automated data-quality checks

Accessible downloadable chart images

Contributing
Issues and pull requests are welcome.

Fork the repository.

Create a branch: git checkout -b feature/your-change.

Make and test your changes.

Commit: git commit -m "Describe your change".

Push the branch and open a pull request.

When adding a pathogen or dataset, document its source, units, geographic scope, reporting frequency, and interpretation limitations.

Disclaimer
This project is an independent informational tool and is not affiliated with the Chicago Department of Public Health, the City of Chicago, the Illinois Department of Public Health, or the Centers for Disease Control and Prevention.

Do not use this dashboard as a substitute for professional medical advice, clinical testing, official emergency guidance, or drinking-water safety information. Consult official public-health sources for decisions affecting health or safety.

Acknowledgments
Chicago Department of Public Health

City of Chicago Data Portal

GitHub Actions

GitHub Pages

<div align="center">

Built for clear, practical monitoring of Chicago's public wastewater data.

</div>
