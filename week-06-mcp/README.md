# Multi-Agent Trading Floor Simulation (MCP)

## 🚀 Project Overview
This project is an autonomous, multi-agent AI simulation where distinct Large Language Model (LLM) personas act as traders in a simulated stock market. 

Built using the **Model Context Protocol (MCP)**, the system decouples the AI reasoning engine from its tools. Instead of hardcoding functions into the agent loop, the agents communicate with independent MCP servers to conduct web research, fetch real-time stock data, and execute trades. The entire simulation runs on a scheduled loop and is visualized through a live Gradio web dashboard.

The simulation features four AI agents, each strictly adhering to a unique investment philosophy:
* **Warren (Value):** Focuses on fundamental analysis, steady cash flows, and long-term value holding.
* **George (Macro):** An aggressive macro trader looking for large-scale economic mispricings.
* **Ray (Systematic):** Focuses on risk parity, macroeconomic indicators, and broad diversification.
* **Cathie (Innovation):** Pursues high-growth, disruptive innovation, specifically focusing on Crypto ETFs.

---

## 📂 File Architecture

Here is a detailed breakdown of what every file in this directory does:

### Core Agent Logic
* **`traders.py`**: The "brain" of the operation. It initializes the AI agents using different LLM providers (GPT-4, DeepSeek, Grok, Gemini), binds them to the MCP tools, and manages the execution loop for trading and portfolio rebalancing.
* **`templates.py`**: Contains the system instructions, personas, and dynamic prompt templates. This file gives the agents their unique behaviors and structures the data they receive before making a decision.
* **`tracers.py`**: Implements custom tracing (`LogTracer`) to monitor the agents' thought processes, logging exactly when an agent starts reasoning, calls a tool, or finishes a task.

### The Trading Floor
* **`trading_floor.py`**: The main asynchronous scheduler. It checks if the market is open and triggers the agents to analyze and trade at set intervals (e.g., every 60 minutes).
* **`reset.py`**: A utility script used to initialize or reset the simulation. It wipes current portfolios, restores the starting balances ($10,000), and injects the base strategies into the database for each agent.

### Model Context Protocol (MCP) Servers
* **`mcp_params.py`**: The configuration hub for the MCP servers. It defines the shell commands and environment variables required to spin up the local servers (Accounts, Push) and remote servers (Brave Search, Fetch, SQLite Memory).
* **`accounts_server.py`**: An MCP server that exposes the portfolio management tools to the AI agents. It allows them to check their balance, view holdings, and execute buy/sell orders.
* **`market_server.py`**: An MCP server that exposes stock market data tools, allowing agents to look up the latest share prices.
* **`push_server.py`**: An MCP server that provides a tool for agents to send push notifications (via Pushover) summarizing their latest trades and portfolio health.

### Data & Backend
* **`accounts.py`**: The backend logic for portfolio management. It defines the `Account` and `Transaction` classes, calculates profit/loss, validates if an agent has enough funds to buy a stock, and records transactions.
* **`accounts_client.py`**: A small client script used by the core agent engine to quickly fetch an agent's current portfolio state and strategy over the MCP connection before prompting the LLM.
* **`market.py`**: Handles the connection to the Polygon.io API. It fetches real-time or end-of-day stock prices and checks if the stock market is currently open.
* **`database.py`**: Manages the SQLite database (`accounts.db`). It contains the SQL queries for saving and reading account states, logging agent thought processes, and caching historical market data.

### User Interface
* **`app.py`**: The frontend Gradio application. It creates the live dashboard that displays the agents' portfolio values, dynamic charts (via Plotly), current holdings, transaction history, and live execution logs.
* **`util.py`**: Contains the CSS styling, JavaScript for enforcing dark mode, and color mappings to ensure the Gradio dashboard looks polished and professional.
