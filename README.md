Trading Bot Project README
Table of Contents
Introduction
Project Structure
Environment Setup
Prerequisites
Virtual Environment
Installing Dependencies
Configuration
Setting Up config.ini
Database Initialization
Running the Scripts
Fetching Data
Executing Trading Strategies
Project Components
database.py
models.py
macro_data.py
trading_logic.py
fetch_data.py
execute_strategy.py
Logging
Additional Notes
Troubleshooting
Resources
Introduction
This project is a comprehensive trading bot designed to fetch financial and macroeconomic data, store it in a SQLite database, and execute trading strategies based on that data. The bot integrates stock price data with macroeconomic indicators such as the Federal Funds Rate, GDP, employment data, and yield curve data to make informed trading decisions.

Project Structure
lua
Copy code
TradingBot/
├── config.ini
├── data/
│   └── market_data.db
├── logs/
│   └── fetch_data.log
├── scripts/
│   ├── database.py
│   ├── models.py
│   ├── macro_data.py
│   ├── trading_logic.py
│   ├── fetch_data.py
│   └── execute_strategy.py
├── README.md
└── requirements.txt
Environment Setup
Prerequisites
Python 3.8 or higher: Ensure you have Python installed. You can download it from python.org.
Virtual Environment
It's recommended to use a virtual environment to manage dependencies.

Create a virtual environment:

bash
Copy code
python -m venv venv
Activate the virtual environment:

On Windows:

bash
Copy code
venv\Scripts\activate
On Unix or MacOS:

bash
Copy code
source venv/bin/activate
Installing Dependencies
Install the required Python packages using pip:

bash
Copy code
pip install -r requirements.txt
Contents of requirements.txt:

Copy code
alpaca-trade-api
pandas
sqlalchemy
yfinance
fredapi
configparser
Configuration
Setting Up config.ini
Create a config.ini file in the root directory of the project to store configuration settings.

Example config.ini:

ini
Copy code
[database]
DB_PATH = data/market_data.db

[alpaca]
API_KEY = your_alpaca_api_key
SECRET_KEY = your_alpaca_secret_key

[fred]
API_KEY = your_fred_api_key
DB_PATH: Path to your SQLite database file.
Alpaca API Keys: Replace with your actual Alpaca API credentials.
FRED API Key: Obtain a free API key from FRED.
Database Initialization
Before running any scripts, ensure that the database and its tables are initialized.

Create the Database Directory:

bash
Copy code
mkdir data
Initialize the Database:

The tables will be created automatically when you run the data fetching script for the first time, as long as Base.metadata.create_all(engine) is called in database.py.

Running the Scripts
Fetching Data
Use fetch_data.py to fetch and store both stock price data and macroeconomic data.

Command:

bash
Copy code
python scripts/fetch_data.py
Functionality:
Fetches historical stock data for a list of predefined symbols.
Retrieves macroeconomic data from FRED, including Federal Funds Rate, GDP, employment data, and yield curve data.
Stores all data in the SQLite database.
Executing Trading Strategies
Use execute_strategy.py to run your trading strategies based on the data in the database.

Command:

bash
Copy code
python scripts/execute_strategy.py
Functionality:
Reads data from the database.
Applies trading logic defined in trading_logic.py.
Executes trades using Alpaca's API (ensure you have your API keys configured).
Project Components
database.py
Purpose: Sets up the connection to the SQLite database and provides a session factory for interacting with the database.
Key Components:
Engine Creation: Establishes the connection to the database using SQLAlchemy's create_engine.
Session Factory: Provides get_session() to create new database sessions.
Table Creation: Calls Base.metadata.create_all(engine) to create tables defined in models.py.
models.py
Purpose: Defines the database schema using SQLAlchemy's ORM.
Key Models:
Price: Stores historical stock price data.
Fields: symbol, timestamp, open, high, low, close, volume.
FedFundsRate: Stores the Federal Funds Rate data.
Fields: date, rate.
GDP: Stores GDP data.
Fields: date, gdp.
EmploymentData: Stores employment-related data.
Fields: date, unemployment_rate, nonfarm_payrolls.
YieldCurve: Stores yield curve data for various maturities.
Fields: date, _1_month, _3_month, _2_year, _10_year, _30_year.
macro_data.py
Purpose: Contains functions to fetch macroeconomic data from FRED and store it in the database.
Key Functions:
fetch_fed_funds_rate: Fetches the Federal Funds Rate.
fetch_gdp_data: Fetches GDP data.
fetch_employment_data: Fetches unemployment rate and nonfarm payrolls data.
fetch_yield_curve_data: Fetches Treasury yields for various maturities to construct the yield curve.
trading_logic.py
Purpose: Contains the trading strategies and logic used by the bot.
Key Functions:
populate_historical_data: (If present) Populates the database with historical stock data.
should_trade_based_on_interest_rate: Example function that decides whether to trade based on the Federal Funds Rate.
Strategy Functions: Define your trading strategies here, utilizing both stock and macroeconomic data.
fetch_data.py
Purpose: Orchestrates the data fetching process.
Functionality:
Reads configuration from config.ini.
Fetches stock price data for predefined symbols using yfinance.
Calls functions from macro_data.py to fetch macroeconomic data.
Logs the data fetching process to logs/fetch_data.log.
execute_strategy.py
Purpose: Runs the trading strategies and executes trades via Alpaca's API.
Functionality:
Establishes a database session.
Checks macroeconomic conditions (e.g., via should_trade_based_on_interest_rate).
Executes trading strategies defined in trading_logic.py.
Closes the database session after execution.
Logging
Log Files Location: logs/ directory.
Logging Configuration: Set up in fetch_data.py and execute_strategy.py using Python's logging module.
Log File: fetch_data.log records information, warnings, and errors during data fetching.
Additional Notes
Data Directory: Ensure the data/ directory exists to store the SQLite database.
Logs Directory: Ensure the logs/ directory exists to store log files.
Alpaca API: You need a valid Alpaca API key and secret to execute trades. Sign up at Alpaca if you don't have an account.
FRED API Key: Obtain a free API key from FRED to fetch macroeconomic data.
Troubleshooting
ModuleNotFoundError or ImportError:
Ensure all dependencies are installed in your virtual environment.
Verify that the scripts are in the correct directories and that paths are correctly set.
Database Errors:
Check that your database tables are correctly set up.
Ensure Base.metadata.create_all(engine) is called to create tables before inserting data.
API Errors:
Confirm that your API keys are valid and correctly specified in config.ini.
Check for any API rate limits or connectivity issues.
Data Fetching Issues:
Verify internet connectivity.
Ensure that the FRED series IDs are correct and that the data is available.
Logs Not Appearing:
Confirm that the logging configuration is correctly set up.
Ensure that the logs/ directory exists and that you have write permissions.
Resources
Python Official Documentation: https://docs.python.org/3/
SQLAlchemy Documentation: https://docs.sqlalchemy.org/en/14/
Pandas Documentation: https://pandas.pydata.org/docs/
Alpaca API Documentation: https://alpaca.markets/docs/api-references/trading-api/
FRED API Documentation: https://fred.stlouisfed.org/docs/api/fred/
YFinance Documentation: https://pypi.org/project/yfinance/
Fredapi Library: https://github.com/mortada/fredapi
Disclaimer: This trading bot is for educational purposes only. Trading involves risk, and past performance is not indicative of future results. Always conduct your own research or consult a professional before making investment decisions.
