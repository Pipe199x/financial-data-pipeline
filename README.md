# Financial Data Pipeline

## Overview
This project is an **end-to-end financial data analysis pipeline** that ingests stock market data and financial news, performs sentiment analysis, and serves the insights through a web-based dashboard. The architecture is built on **Azure Cloud Services**.

## Architecture
The solution follows a **four-stage architecture**:
1. **Ingest**: Fetch stock prices from **Yahoo Finance** and financial news from **NewsAPI/Google News** using **Azure Functions**.
2. **Store**: Save the ingested data to **Azure Blob Storage** and process it via **Azure Data Factory**.
3. **Analyze & Model**: Perform sentiment analysis using **FinBERT** (Azure ML) and store results in **Azure CosmosDB**.
4. **Serve**: Expose the processed data via **Azure SQL Data Warehouse** and serve insights in a **web app (Angular + FastAPI)**.

![Architecture](./images/architecture-diagram.png)

## Features
- **Stock Price Tracking**: Retrieves 7-day price history from Yahoo Finance.
- **Sentiment Analysis**: Classifies financial news as **positive, negative, or neutral**.
- **Automated Data Pipeline**: Uses Azure Data Factory for scheduling ingestion.
- **Web Dashboard**: Built with Angular & FastAPI for real-time visualization.

---
## Deployment Guide

### Prerequisites
Ensure you have:
- **Azure Subscription** (Azure for Students or Pay-As-You-Go)
- **Python 3.10+**
- **Node.js & Angular CLI** (for frontend)
- **Azure CLI & Function Core Tools** installed
- **GitHub Account** (for source control)

### 1️⃣ Set Up Azure Environment
#### **Create Required Azure Resources**
1. Log in to **Azure Portal**
2. Create a **Resource Group** (Example: `financial-project-rg`)
3. Create **Azure Storage Account** (Example: `financialstorage`)
4. Create **Azure Function Apps**:
   - `ingest-financial-news`  
   - `ingest-stock-data`
5. Enable **Application Insights** (for monitoring logs)
6. Deploy **Azure Data Factory** for pipeline orchestration
7. Create **Azure CosmosDB** (for sentiment results)
8. Deploy **Azure SQL Data Warehouse** (for structured stock data)

### 2️⃣ Set Up and Deploy Azure Functions
#### **Install Required Dependencies**
```bash
pip install azure-functions requests azure-storage-blob yfinance transformers
```
#### **Initialize Function App**
```bash
func init financial-data-ingest --worker-runtime python
cd financial-data-ingest
func new --name ingest-financial-news --template "TimerTrigger"
func new --name ingest-stock-data --template "TimerTrigger"
```
#### **Deploy to Azure**
```bash
func azure functionapp publish ingest-financial-news
func azure functionapp publish ingest-stock-data
```
### 3️⃣ Configure Azure Data Factory
1. Open **Azure Data Factory Studio**
2. Create Linked Services:
   - **Azure Blob Storage** (for news & stock data)
   - **Azure Data Lake Storage Gen2**
3. Define **Copy Activity**:
   - **Source**: Azure Storage (`news-data`, `stock-data`)
   - **Destination**: Azure Data Lake Gen2
4. Schedule ingestion **every hour**

### 4️⃣ Train and Deploy Sentiment Analysis Model
#### **Azure Machine Learning Setup**
```bash
az ml compute create -n finbert-cluster --size Standard_NC6
```
#### **Train FinBERT on Financial News**
```python
from transformers import pipeline
sentiment_pipeline = pipeline("sentiment-analysis", model="yiyanghkust/finbert-tone")
result = sentiment_pipeline("Apple stock surges 10% after strong earnings.")
print(result)
```
#### **Deploy Model to Azure Kubernetes Service (AKS)**
```bash
az ml model deploy -n finbert-service --model finbert.pkl --compute-type aks
```

### 5️⃣ Deploy Web Dashboard
#### **Frontend Setup (Angular)**
```bash
cd frontend
npm install
ng build --prod
```
#### **Deploy to Azure Static Web Apps**
```bash
az staticwebapp create --name sentiment-dashboard --resource-group financial-project-rg --source .
```
#### **Backend API (FastAPI)**
```bash
cd backend
uvicorn app:app --host 0.0.0.0 --port 8000
```

---
## Usage
### **1️⃣ Viewing Stock & Sentiment Analysis**
1. Open **sentiment-dashboard** in your web browser.
2. Search for a stock symbol (AAPL, TSLA, AMZN, etc.).
3. View **historical prices** and sentiment analysis.

### **2️⃣ Monitoring & Troubleshooting**
- **Check Azure Logs**: Azure Portal → Function Apps → Monitor
- **Verify Data in CosmosDB**: Query sentiment data
- **Check API Responses**: `curl http://your-api-endpoint/stocks/AAPL`

---
## Contributing
1. Fork the repository on GitHub.
2. Clone the repo:
```bash
git clone https://github.com/Pipe199x/financial-data-pipeline.git
```
3. Create a new branch:
```bash
git checkout -b feature-branch
```
4. Commit & Push:
```bash
git add .
git commit -m "Added new feature"
git push origin feature-branch
```
5. Create a Pull Request.

---
## License
This project is licensed under the **MIT License**.

