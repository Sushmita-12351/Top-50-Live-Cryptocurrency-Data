# Top-50-Live-Cryptocurrency-Data
This project fetches real-time data for the top 50 cryptocurrencies from the CoinGecko API using Python. It processes and analyzes the data, then updates it in an Excel file for further visualization and reporting. The script runs at regular intervals to keep the data up to date.
Features
✅ Live Data Fetching – Retrieves real-time cryptocurrency prices, market cap, and volume
✅ Top 50 Cryptocurrencies – Fetches the most valuable digital assets ranked by market cap
✅ Automated Data Updates – Refreshes and updates the Excel file at set intervals
✅ Excel Integration – Saves structured data in an easy-to-read spreadsheet
✅ Data Analysis – Prepares data for further insights and visualization
Technologies & Libraries Used
Python 3.x – The core programming language
Requests – For making API calls to fetch live data
Pandas – For data manipulation and structuring
OpenPyXL – For writing and updating Excel files
How It Works
The script fetches live cryptocurrency data from CoinGecko API.
It extracts key metrics: name, symbol, price, market cap, total volume, etc.
The data is processed using Pandas and stored in a structured format.
The OpenPyXL library updates the Excel sheet with the latest data.
The script runs at regular intervals to maintain fresh data.

import requests
import pandas as pd
#Function to fetch top 5 cryptocurrencies
def get_top_5_crypto():
    url = "https://api.coingecko.com/api/v3/coins/markets"
    params = {
        "vs_currency": "USD",          # Prices in USD
        "order": "market_cap_desc",     # Sort by market cap
        "per_page": 5,                 # Get top 50 cryptocurrencies
        "page": 1,                      # First page
        "sparkline": False              # Exclude sparkline data
    }
    response = requests.get(url, params=params)

    if response.status_code == 200:

        data = response.json()
        df = pd.DataFrame(data, columns =["id", "symbol", "name", "current_price", "market_cap", "total_volume"])
        return df
    else:
        print("Error fetching data:", response.status_code)
        return None
#Fetch and display the top 5 cryptocurrencies
top_5_crypto = get_top_5_crypto()
print(top_5_crypto)
#Function to fetch top 50 cryptocurrencies
def get_top_50_crypto():
    url = "https://api.coingecko.com/api/v3/coins/markets"
    params = {
        "vs_currency": "usd",          # Prices in USD
        "order": "market_cap_desc",     # Sort by market cap
        "per_page": 50,                 # Get top 50 cryptocurrencies
        "page": 1,                      # First page
        "sparkline": False              # Exclude sparkline data
    }
    response = requests.get(url, params=params)

    if response.status_code == 200:

        data = response.json()
        df = pd.DataFrame(data,columns =["id","symbol","name","current_price","market_cap","total_volume"])
        return df
    else:
        print("Error fetching data:", response.status_code)
        return None
#Fetch and display the top 50 cryptocurrencies
top_50_crypto = get_top_50_crypto()
print(top_50_crypto)
import requests
import pandas as pd
from openpyxl import Workbook
from openpyxl.styles import Font, Alignment

# Function to fetch live crypto data
def fetch_crypto_data():
    url = "https://api.coingecko.com/api/v3/coins/markets"
    params = {
        "vs_currency": "usd",
        "order": "market_cap_desc",
        "per_page": 10,  # Fetch top 10 cryptocurrencies
        "page": 1,
        "sparkline": False
    }
    
    response = requests.get(url, params=params)
    if response.status_code == 200:
        data = response.json()

        # Ensure data exists before creating a DataFrame
        if data:
            df = pd.DataFrame(data)[["id", "symbol", "current_price", "market_cap", "total_volume"]]

            # Renaming columns for better readability
            df.columns = ["Coin Name", "Symbol", "Price (USD)", "Market Cap (USD)", "Total Volume"]

            # Rounding numerical values
            df["Price (USD)"] = df["Price (USD)"].round(2)
            df["Market Cap (USD)"] = df["Market Cap (USD)"].apply(lambda x: f"${x:,.2f}")
            df["Total Volume"] = df["Total Volume"].apply(lambda x: f"${x:,.2f}")

            return df  # ✅ Correctly indented inside the function

    print(f"Failed to fetch data. Status Code: {response.status_code}")
    return None

# Function to save data to Excel with formatting
def save_to_excel(df, filename="Live_Crypto_Data.xlsx"):
    writer = pd.ExcelWriter(filename, engine="openpyxl")
    df.to_excel(writer, index=False, sheet_name="Crypto Data")

    # Load the workbook and sheet
    workbook = writer.book
    sheet = writer.sheets["Crypto Data"]

    # Format header row
    for col in range(1, len(df.columns) + 1):
        cell = sheet.cell(row=1, column=col)
        cell.font = Font(bold=True)
        cell.alignment = Alignment(horizontal="center")

    # Auto-adjust column widths
    for col_idx, col in enumerate(df.columns, start=1):
        max_length = max(df[col].astype(str).map(len).max(), len(col))
        sheet.column_dimensions[sheet.cell(row=1, column=col_idx).column_letter].width = max_length + 2

    writer._save()  # Save Excel file

# Fetch and save data
crypto_df = fetch_crypto_data()
if crypto_df is not None:
    save_to_excel(crypto_df)
    print("✅ Live crypto data saved in an organized Excel file: Live_Crypto_Data.xlsx")
    
📌 Summary of Features
✅ Fetches live cryptocurrency data (top 10 by market cap)
✅ Uses Pandas to structure the data
✅ Formats and saves the data into an Excel file
✅ Enhances readability with styling & column width adjustments

📌 Possible Improvements
🔹 Increase the number of cryptocurrencies fetched ("per_page": 50)
🔹 Add error handling to check if the API is down
🔹 Include price changes (e.g., 24-hour or 7-day % change)
🔹 Allow users to specify currency (USD, EUR, etc.)
