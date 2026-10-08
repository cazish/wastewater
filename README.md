# wastewater
A deployable Chicago Wastewater Monitor for GitHub Actions.

It includes:

A daily check at 8:17 AM Chicago time

Monitoring for COVID-19, influenza A/B, and RSV

Detection of newly published Chicago wastewater datasets

Email alerts whenever reported readings change

A responsive GitHub Pages dashboard with dark mode, historical trends, and CSV export

Setup instructions for GitHub and SMTP email

Baseline data from Chicago’s official Socrata API

GitHub scheduled workflows may occasionally run late, especially during high-load periods, which is why the checker runs at minute 17 rather than at the top of the hour. Email credentials are stored as encrypted GitHub repository secrets rather than committed in the code.

Download the project above, unzip it, and follow its README.md
