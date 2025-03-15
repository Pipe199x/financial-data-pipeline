# Financial Data Pipeline

This project is an **Azure-based financial data pipeline** that ingests financial news and stock price data, processes it, and stores it for further analysis.

## 📌 Features
- **Ingest financial news** from NewsAPI & Google News.
- **Fetch stock price data** from Yahoo Finance.
- **Process and store data** in Azure Blob Storage.
- **Scheduled execution** using Azure Functions & Timer Triggers.
- **Integration with Azure Data Factory** (optional).

---

## 🚀 Deployment Steps

### 1️⃣ **Set Up Azure Resources**

1. **Create a Resource Group**  
   ```sh
   az group create --name financial-project-rg --location eastus
   ```
2. **Create an Azure Storage Account**  
   ```sh
   az storage account create --name financialprojectrgb8f9 --resource-group financial-project-rg --location eastus --sku Standard_LRS
   ```

### 2️⃣ **Create Azure Function Apps**
- **Go to Azure Portal** → Search for **"Azure Functions"** → Click **"Create"**  
- Create **two Function Apps**:  
  - `ingest-financial-news`
  - `ingest-stock-data`  

Use the following configurations:
  - **Runtime:** Python 3.10
  - **Region:** East US
  - **Hosting Plan:** Consumption (Serverless)
  - **Storage Account:** Use the one created earlier

### 3️⃣ **Initialize Function Apps Locally**
Ensure you have **Python 3.10** and **Azure Functions Core Tools** installed.

```sh
# Create virtual environment
py -3.10 -m venv venv
source venv/bin/activate  # For macOS/Linux
venv\Scriptsctivate  # For Windows

# Install dependencies
pip install azure-functions
```

### 4️⃣ **Create Function for Financial News Ingestion**
```sh
func init financial-data-ingest --worker-runtime python
cd financial-data-ingest
func new --name ingest-financial-news --template "TimerTrigger"
```

Replace the `__init__.py` content with:

```python
import requests
import json
import datetime
from azure.storage.blob import BlobServiceClient

NEWS_API_KEY = "YOUR_NEWSAPI_KEY"
NEWS_API_URL = f"https://newsapi.org/v2/everything?q=stocks&apiKey={NEWS_API_KEY}"
AZURE_STORAGE_CONNECTION_STRING = "YOUR_STORAGE_CONNECTION_STRING"
CONTAINER_NAME = "news-data"

def fetch_news():
    response = requests.get(NEWS_API_URL)
    return response.json()["articles"] if response.status_code == 200 else []

def save_to_blob(data):
    blob_service_client = BlobServiceClient.from_connection_string(AZURE_STORAGE_CONNECTION_STRING)
    blob_client = blob_service_client.get_blob_client(container=CONTAINER_NAME, blob=f"news-{datetime.datetime.now().date()}.json")
    blob_client.upload_blob(json.dumps(data), overwrite=True)

def main(mytimer):
    news_data = fetch_news()
    save_to_blob(news_data)
    print("News data saved to Azure Storage")
```

### 5️⃣ **Deploy to Azure**
```sh
func azure functionapp publish ingest-financial-news
```

### 6️⃣ **Schedule Execution**
- Go to Azure Portal → **Function App** → **TimerTrigger**  
- Set **Cron Expression:** `0 0 * * * *` (Runs every hour)

---

## 🛠 **Technologies Used**
- **Azure Functions**
- **Python (Azure Functions Core Tools)**
- **Azure Blob Storage**
- **Azure Data Factory** (optional)
- **NewsAPI & Yahoo Finance API**

## 📌 **To-Do**
- ✅ Implement data processing pipeline
- ✅ Automate data ingestion & storage
- ⏳ Add sentiment analysis (FinBERT)
- ⏳ Deploy frontend (Angular / React)

---

## 📄 License
This project is licensed under the **MIT License**.
