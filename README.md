# 🚀 Financial Data Pipeline  

## 📌 Overview  
This project is an **automated financial data pipeline** that **fetches stock market data and financial news**, processes them for **sentiment analysis**, and stores results for visualization.  

It is built using **Azure Functions, Azure Data Factory, CosmosDB, SQL Data Warehouse, and FinBERT**. The goal is to analyze **market trends by comparing numerical stock data with financial news sentiment**.  

## 📌 Features  
✅ **Real-time financial news ingestion** (NewsAPI, Google News).  
✅ **Stock market data extraction** (Yahoo Finance API).  
✅ **Sentiment analysis using FinBERT**.  
✅ **Data storage in Azure Data Lake, SQL, and CosmosDB**.  
✅ **Automated deployment via GitHub Actions**.  
✅ **Web dashboard with Angular (coming soon)**.  

## 📌 Setup & Deployment  

### **1️⃣ Clone the Repository**  
```bash
git clone https://github.com/YOUR_USERNAME/financial-data-pipeline.git
cd financial-data-pipeline

### **2 Install Dependencies** 
```bash
pip install -r requirements.txt

### **3 Configure Azure Credentials**
```bash
export AZURE_STORAGE_CONNECTION_STRING="your_connection_string"
export NEWS_API_KEY="your_news_api_key"
export YAHOO_API_KEY="your_yahoo_api_key"

### **4 Deploy to Azure Functions**
```bash
func azure functionapp publish financial-data-functions


📌 Contributing
We welcome contributions! 🚀

📌 License
This project is licensed under the MIT License.
