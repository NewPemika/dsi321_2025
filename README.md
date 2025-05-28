
# EGAT Real-time Power Data Pipeline

This project is designed to extract real-time electricity generation data from the Electricity Generating Authority of Thailand (EGAT) website (https://www.sothailand.com/sysgen/egat/). It leverages Selenium to interact with the website and capture data from the browser’s Console Log, where dynamic updates are logged. The collected data is continuously saved to a CSV file at defined intervals. To ensure data versioning and integrity, the dataset is stored in LakeFS. The entire workflow is orchestrated using Prefect and executed automatically through Docker Compose.

---

##  Key Features

- **Real-time Data Extraction:** Uses Selenium to open the website and extract MW and temperature data from the browser console log.
- **Workflow Orchestration with Prefect:** Prefect is used to manage tasks and flows in the pipeline.
- **Containerized Operation:** Supports execution via Docker Compose for ease of deployment and reproducibility.
- **Versioned Data Storage in LakeFS:** Stores the extracted data in Parquet format in an S3 bucket managed by LakeFS for version control.
- **Automated Commits:** Automatically commits new data into LakeFS whenever new records are available.

---

## Project Structure

```

DSI321\_2025-MAIN/
├── README.md                         # Project documentation
├── docker-compose.yml                # Runs the multi-container setup
├── prefect.yaml                      # Prefect deployment configuration
├── egat\_pipeline.py                  # Prefect pipeline for scraping and storing dat
│
├── **pycache**/                      # Auto-generated compiled Python files
│   └── egat\_pipeline.cpython-312.pyc
│
├── jupyter\_app/                      # Jupyter-based testing of pipeline
│   ├── Dockerfile                    # Docker config for notebook container
│   ├── requirements.txt              # Python dependencies for the notebook
│   ├── run\_scraper\_and\_save\_to\_lakefs.ipynb           # Main notebook
│   └── .ipynb\_checkpoints/
│       └── run\_scraper\_and\_save\_to\_lakefs-checkpoint.ipynb # Auto checkpoint
│
├── parquet/                          # Versioned Parquet data storage
│   └── egat\_realtime\_power\_history.parquet
│
└── UI/                               # Additional UI components
└── streamlit\_app.py

````

---

## Prerequisites

- Python 3.7+
- Google Chrome installed
- Docker and Docker Compose
- Prefect and LakeFS installed and configured

---

## How to Use

### 1. Install Dependencies

```bash
pip install -r requirements.txt
````

If `requirements.txt` is missing, install manually:

```bash
pip install pandas selenium webdriver-manager prefect lakefs-client pyarrow
```

### 2. Set Environment Variables (via `.env` or export)

```env
LAKEFS_ACCESS_KEY_ID=your_access_key
LAKEFS_SECRET_ACCESS_KEY=your_secret_key
LAKEFS_ENDPOINT_URL=http://localhost:8001/
```

### 3. Run the Prefect Flow (Locally)

```bash
python egat_pipeline.py
```

---

## Run with Docker Compose

```bash
docker-compose up --build
```

> Ensure that both the Prefect agent and LakeFS container are running before starting the flow.

---

## Data Fields

* `display_date_id`: Date shown on source system
* `current_value_MW`: Power production in megawatts
* `temperature_C`: Temperature in Celsius
* `scrape_timestamp_utc`: UTC timestamp of data extraction

---

##  Project Report

### 1. Data Visualization

* Streamlit dashboard displays:

  * Line charts for power and temperature
  * Table of latest 10 records
  * Key metrics (e.g., peak, average, anomaly rate)
  * Anomaly status indicator

### 2. Machine Learning Application

* Uses `IsolationForest` to detect anomalies in power data
* Anomaly sensitivity is adjustable via sidebar
* Displays status: Normal or Anomaly

---

## Streamlit Dashboard Example

![image](https://github.com/user-attachments/assets/4f0202d2-6481-4f03-8f96-f7149cf8fa8d)

![image](https://github.com/user-attachments/assets/970bbb11-7b35-4e41-b96f-768c9b670a64)

```
