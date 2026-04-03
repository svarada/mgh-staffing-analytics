# mgh-staffing-analytics
End-to-end SQL/Python pipeline for analyzing nursing labor variance on macOS M5.

## MGH CASE STUDY: Nursing Staffing Analytics & Labor Variance
Author: Saipreeth Varada, MSBA

Tech Stack: Python (Pandas, SQLAlchemy, Plotly), Docker, Azure SQL Edge (T-SQL), macOS M5 Silicon

1. Project Overview:
This project addresses a critical challenge in healthcare operations: balancing patient safety (staffing levels) with fiscal responsibility (budget management). Using a synthetic dataset modeled after Massachusetts General Hospital (MGH) staffing metrics, I built an end-to-end pipeline to identify "Labor Variance" —the gap between planned nursing hours and actual hours worked.

2. Technical Architecture:
To simulate an enterprise environment on a local ARM64 (M5) architecture, I deployed:
  - **Database**: A containerized Azure SQL Edge instance via Docker.
  - **Pipeline**: A Python-based ETL (Extract, Transform, Load) process using SQLAlchemy to migrate flat-file operational data into a relational SQL environment.
  - **Connectivity**: Configuration of Microsoft ODBC Driver 18 and unixODBC to facilitate high-speed data transfer between the macOS kernel and the SQL container.

3. Data Engineering & Troubleshooting:
There were significant technical hurdles that were overcome to ensure data integrity:
  - **CLR Limitations**: Encountered a Common Language Runtime (CLR) restriction in the SQL Edge environment which blocked the FORMAT() function. I refactored the codebase to use native _CONVERT_ and _LEFT_ string slicing for time-series aggregation, optimizing performance for a containerized environment.
  - **Type Casting**: Resolved Data Type Mismatches (Error 8116) by implementing _CAST_ operations during the T-SQL aggregation phase to ensure string-based dates were correctly processed as temporal objects.

4. Key Findings & Analysis:
The analysis focused on **HPPD** (Hours Per Patient Day) and **total monthly variance**.
  - **The "See-Saw" Effect**: The visualization revealed a cyclical variance pattern. Red bars (Over-Budget) coincided with high-intensity census periods, while Green bars (Under-Budget) indicated potential staffing shortages or successful cost-containment.
  - **Operational Insights**: By using TOP (N) WITH TIES, I identified that the unit reached maximum capacity (30 patients) on 29 separate occasions throughout the year, suggesting that current "Budgeted Hours" may not account for the recurring nature of patient surges.

5. Visualizations Included:
  - **Daily Staffing Trend**: An interactive line chart comparing Budgeted vs. Actual hours to show daily volatility.
  - **Monthly Variance Bar Chart**: A color-coded financial report (Red/Green) designed for executive-level review of labor expenditures.

### Project Structure
- **nursing_staffing_variance_analysis.ipynb**: The primary Python notebook that contains data cleaning, SQL queries, and visualizations.
- **mgh_staffing_data.csv**: The source dataset contains a full year (365 days) of nursing operational metrics.
- **docker-compose.yml**: The configuration file that instantly deploys the Azure SQL Edge environment.
- **requirements.txt**: The text document that lists all of the Python dependencies (Pandas, SQLAlchemy, Plotly, etc). 
