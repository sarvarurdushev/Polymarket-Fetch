# Polymarket Market Data Analysis Tool 📊

## Project Overview

This project is a Python-based utility designed to programmatically interact with Polymarket's public APIs. Its core purpose is to retrieve comprehensive, real-time, and historical data about prediction markets, facilitating advanced analysis, tracking market sentiment, and monitoring trading activity.

The tool provides the necessary functions to bypass manual data collection, allowing users to build automated feeds for dashboards, trading strategy backtesting, or in-depth statistical research on market behavior.

---

## What It Does (Functionality)

The utility performs three primary data fetching functions for any given Polymarket event:

1.  **Retrieves Market Details:** Fetches high-level information about a specific event, including the market title, description, resolution criteria, current **outcome prices (probabilities)**, and total **volume** traded.
2.  **Gathers Historical Price Data:** Collects time-series data detailing how the probability of a specific outcome has changed over time. This data is aggregated by user-defined time intervals (e.g., daily or hourly) and is crucial for **trend analysis** and volatility assessment.
3.  **Fetches Trade History:** Pulls a detailed log of all transactions (trades) that have occurred in the market. This includes transaction hashes, the direction of the trade (**side**), the price at which the trade was executed, and the quantity (**size**), allowing for detailed tracking of trader activity and market depth.

---

## How It Works (Methodology)

The project leverages different Polymarket API endpoints, each specialized for a particular type of data:

### 1. Market Details

* **API Used:** The dedicated **Gamma API**.
* **Method:** A standard HTTP `GET` request is made to the API, using the market's unique URL **slug** (the text identifier found in the URL) as a parameter. The API returns a JSON object containing the overall event structure and the current state of its associated markets.

### 2. Historical Price Data

* **API Used:** The **Central Limit Order Book (CLOB) API**'s `prices-history` endpoint.
* **Method:** This endpoint requires a unique identifier called the **ClobTokenId**, which corresponds specifically to a market's "Yes" or "No" outcome. The tool passes this token ID along with the desired time **interval** (e.g., '1d' for 1 day) and data **fidelity** (number of data points) to the API. The response is a time series of price points suitable for charting and analysis.

### 3. Trade History

* **API Used:** The **Data API**'s `trades` endpoint.
* **Method:** This function uses the market's unique **condition\_id** (a smart contract identifier) to filter the global trade log. It allows for setting a maximum **limit** on the number of trades returned and can be adapted to support **pagination** (fetching trades in sequential blocks) to retrieve the full, unpaginated history for highly active markets.
