🚦 Accident Intelligence Dashboard: Road & Rail

An end-to-end data engineering and analytics project covering 2.2 million road and railway accident records across India (2018–2022), built with Python, Google BigQuery, and Power BI.

<img width="1072" height="609" alt="image" src="https://github.com/user-attachments/assets/5c5d4528-0b3e-468b-a040-a867b265c088" />

📌 Project Overview
This project delivers a live, interactive Power BI dashboard that analyses road and railway accident patterns across all Indian states and union territories. It covers fatality trends, vehicle involvement, road type breakdown, accident causes, and urban vs rural split — helping surface actionable safety insights from government accident data.
Key highlights:

2,205,533 total accident records analysed
36 states and union territories covered
5 years of data: 2018 to 2022
Dual focus: road accidents (2,087,360) and railway accidents (109,780 + 8,393 crossing incidents)

<img width="1013" height="676" alt="image" src="https://github.com/user-attachments/assets/4ce48755-6c10-4e7f-bbf8-c395aed432d0" />

🏗️ Architecture & Data Pipeline
Raw Dataset          Python ETL            Google BigQuery        Power BI
(Neon DB Cloud)  →  (Extract → Save   →   Data Warehouse     →  Live Dashboard
                     Transform → Load)
                          ↑
                   Python Scheduler
                (Automates ETL at
                 scheduled intervals)
                          ↓
                   PKL Pickle File
                 (temp local storage
                  in binary format)
Pipeline steps

Extract — Raw accident data pulled from Neon DB Cloud (PostgreSQL-compatible serverless database)
Save — Data saved locally as a .pkl (Python Pickle) binary file for intermediate storage
Transform — Data cleaned, typed, and restructured using Python (Pandas)
Load — Transformed data loaded into Google BigQuery data warehouse
Schedule — Python Scheduler automates the entire ETL at defined time intervals
Visualise — Power BI connects live to BigQuery and renders the dashboard


🛠️ Tech Stack
LayerToolSource databaseNeon DB Cloud (PostgreSQL)ETL scriptingPython (Pandas, SQLAlchemy)Intermediate storagePKL Pickle fileSchedulingPython Scheduler (APScheduler / schedule library)Data warehouseGoogle BigQueryVisualisationMicrosoft Power BI (Live connection)

📊 Dashboard Pages
Page 1 — Outlook
High-level overview of accident patterns across India.
VisualTypeInsightTotal accidents KPI cardsCard tiles2.2M total, split by road / rail / crossingMonthly accident volumeDecomposition treeJanuary highest (197,422), April lowest (151,677)Death growth 2020–2022Clustered barYoY % change per state across 3 yearsDeaths by road type per stateStacked barNational Highways and Other Roads dominate fatalitiesState with most accidentsKPI cardUttar Pradesh (UP)Vehicle with most accidentsKPI cardFour Wheelers
Page 2 — Deep Dive
Granular analysis of fatality rates, place types, and vehicle breakdown.
VisualTypeInsightFatality by road causeLine chartAnimal Crossing has highest fatality rate (2.81)Increase/decrease by place typeArea chartUrban pedestrian crossings spiked in 2021 (+2.18)YoY urban and rural decomp treeDecomposition treeDrill into place type → year → rural/urban splitVehicle share by typeHorizontal barFour Wheelers 34.50%, Trucks 23.84%, Bus 17.66%Most accident-prone timeKPI calloutRoad: Noon–Early Afternoon / Rail: Midnight–Late NightHighest fatality causeKPI calloutRoad: Animal Crossing / Rail: Collision

🔍 Key Insights
<img width="1070" height="602" alt="image" src="https://github.com/user-attachments/assets/a1745a6a-1e54-42e0-b61b-20161f0408de" />

UP accounts for the highest accident volume across all years and road types
January is consistently the most accident-prone month — likely due to fog and low visibility in North India
Animal crossing is the deadliest road accident cause with a fatality rate of 2.81 (deaths per accident)
Collision is the top fatality cause on railways
Four Wheelers are involved in 34.5% of all accidents — the single largest vehicle category
Urban pedestrian crossings saw a 2.18x spike in 2021 — likely post-lockdown traffic surge
National Highways and Other Roads contribute the most deaths across high-accident states
Fatality rates have generally declined from 2018 to 2022, suggesting improving emergency response or road safety measures


📁 Repository Structure
├── etl/
│   ├── extract.py          # Pull data from Neon DB
│   ├── transform.py        # Clean and restructure data
│   ├── load.py             # Load to Google BigQuery
│   └── scheduler.py        # Automate ETL at intervals
├── data/
│   └── temp.pkl            # Intermediate pickle storage (gitignored)
├── dashboard/
│   └── Road_Rail_Dashboard.pbix   # Power BI dashboard file
├── docs/
│   ├── data_flow.png       # Architecture diagram
│   └── dashboard_preview.png
├── requirements.txt
└── README.md

⚙️ Setup & Run
Prerequisites

Python 3.9+
Google Cloud account with BigQuery enabled
Power BI Desktop
Neon DB connection string

Installation
bashgit clone https://github.com/yourusername/accident-intelligence-dashboard.git
cd accident-intelligence-dashboard
pip install -r requirements.txt
Configure environment variables
bash# Create a .env file
NEON_DB_URL=postgresql://user:password@your-neon-host/dbname
BIGQUERY_PROJECT_ID=your-gcp-project-id
BIGQUERY_DATASET=accident_data
GOOGLE_APPLICATION_CREDENTIALS=path/to/service-account.json
Run the ETL pipeline
bash# Run once
python etl/extract.py
python etl/transform.py
python etl/load.py

# Run on schedule
python etl/scheduler.py
Open the dashboard

Open dashboard/Road_Rail_Dashboard.pbix in Power BI Desktop
Update the BigQuery data source credentials
Click Refresh to pull latest data


📈 DAX Measures Used
dax-- Year-over-Year Death Growth %
YoY Death Growth % =
VAR CurrentYear = SUM(accidents[died])
VAR PreviousYear = CALCULATE(SUM(accidents[died]), SAMEPERIODLASTYEAR(dates[date]))
RETURN DIVIDE(CurrentYear - PreviousYear, PreviousYear)

-- Fatality Rate per Accident
Fatality Rate =
DIVIDE(SUM(accidents[died]), SUM(accidents[total_accidents]))

-- Vehicle Share %
Vehicle Share % =
DIVIDE(SUM(accidents[vehicle_count]), CALCULATE(SUM(accidents[vehicle_count]), ALL(accidents[vehicle_type])))

🗂️ Data Schema (BigQuery)
ColumnTypeDescriptionstate_nameSTRINGIndian state or union territoryyearINTEGERAccident year (2018–2022)monthSTRINGMonth of accidentaccident_typeSTRINGRoad / Railway / Railway Crossingroad_typeSTRINGExpressway / NH / State Highway / Othervehicle_typeSTRINGFour Wheeler / Truck / Bus etc.causeSTRINGAccident cause categoryplace_typeSTRINGUrban / Rural place classificationdiedINTEGERNumber of fatalitiesinjuredINTEGERNumber of injuriestotal_accidentsINTEGERTotal accident count

🙏 Acknowledgements

Data sourced from Ministry of Road Transport and Highways (MoRTH), Government of India
Railway data from National Crime Records Bureau (NCRB) accident statistics


📄 License
This project is for educational and portfolio purposes.
