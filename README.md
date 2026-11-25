# 🤖 AI Data Agent

An Agentic AI Data Agent built using Python, LangGraph, and PostgreSQL. This project uses specialized AI agents to process natural language queries and perform SQL operations, data extraction, and data transformation.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Usage](#usage)
- [Agent Descriptions](#agent-descriptions)
- [Examples](#examples)
- [Security](#security)
- [Troubleshooting](#troubleshooting)
- [Author](#author)

---

## 🎯 Overview

AI Data Agent is a multi-agent system designed to understand natural language instructions and execute data-related tasks.

The main Data Agent acts as a router. It analyzes user requests and delegates them to specialized agents:

- SQL Analyst Agent: Handles database queries and SQL operations.
- ETL Analyst Agent: Handles data extraction, transformation, and loading operations.

The system uses LangGraph for agent orchestration and integrates with PostgreSQL for database operations.

## 🏗️ Architecture

The project follows a hierarchical multi-agent architecture.

```text
                  User Query
                      |
                      v
               Data Agent Router
                      |
              +-------+-------+
              |               |
              v               v
         SQL Analyst       ETL Analyst
            Agent              Agent
              |                 |
       +------+------+    +-----+------+
       |      |      |    |            |
       v      v      v    v            v
     SQL   Safety  Query  Extract   Transform
   Generate Check Execute  Data       Data
       |      |      |      |            |
       +------+------+      +------------+
              |                    |
              v                    v
          SQL Result           ETL Result
```

### Workflow

1. User submits a natural language query.
2. The Data Agent analyzes the request.
3. The Router classifies the task as SQL or ETL.
4. The appropriate specialized agent processes the request.
5. The agent executes the required operation.
6. The system returns the result.

## ✨ Features

### Intelligent Query Routing
- Automatically classifies user requests.
- Routes tasks to specialized agents.
- Uses LangGraph for workflow management.

### SQL Analyst Agent
- Converts natural language into SQL queries.
- Retrieves database schema information.
- Validates SQL query safety.
- Executes queries using PostgreSQL.
- Generates answers from query results.

### ETL Analyst Agent
- Extracts data from API endpoints.
- Transforms data using Pandas.
- Supports CSV, JSON, and Parquet formats.
- Generates transformation code based on user instructions.
- Loads processed data into storage.

### Multi-LLM Integration
- Supports OpenAI through LangChain.
- Supports Anthropic through LangChain.
- Selects an LLM based on the configured complexity level.

### Safety and Validation
- SQL query safety validation.
- Protection against destructive SQL operations.
- Input validation.
- Structured output validation using Pydantic.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| LangChain | LLM integration |
| LangGraph | Multi-agent orchestration |
| OpenAI | Language model integration |
| Anthropic | Claude integration |
| PostgreSQL | Database management |
| Pandas | Data processing |
| Pydantic | Data validation |
| psycopg2 | PostgreSQL connectivity |

## 📦 Prerequisites

Before installing the project, make sure you have:

- Python 3.12 or above
- PostgreSQL
- Git
- Visual Studio Code (recommended)
- API key for the selected LLM provider

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Vasanthkumar1718/AI-Data-Agent.git
```

Navigate to the project directory:

```bash
cd AI-Data-Agent
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

Activate the virtual environment on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root directory.

Add the following configuration:

```env
# LLM Configuration
OPENAI_API_KEY=your_openai_api_key
ANTHROPIC_API_KEY=your_anthropic_api_key

# PostgreSQL Configuration
host=localhost
port=5432
user=postgres
password=your_postgresql_password
database=data_agent
```

Replace the placeholder values with your own credentials.

Do not upload your `.env` file to GitHub.

### 5. Set Up PostgreSQL

1. Install and start PostgreSQL.
2. Open pgAdmin.
3. Create a database named `data_agent`.
4. Configure the database connection in `utils/database.py`.
5. Run the database initialization script:

```bash
python feed_db.py
```

This script loads the sample datasets into PostgreSQL.

## 📁 Project Structure

```text
AI-Data-Agent/
│
├── agents/
│   ├── __init__.py
│   ├── data_agent.py
│   ├── sql_analyst.py
│   └── etl_analyst.py
│
├── Models/
│   ├── __init__.py
│   └── schema.py
│
├── utils/
│   ├── __init__.py
│   ├── database.py
│   ├── etl_tools.py
│   └── llm_pick.py
│
├── data/
│   ├── extract/
│   ├── transform/
│   ├── payments.csv
│   ├── ratings.csv
│   ├── rides.csv
│   ├── users.csv
│   └── vehicles.csv
│
├── main.py
├── feed_db.py
├── requirements.txt
├── pyproject.toml
├── .env.example
├── .gitignore
└── README.md
```

## ⚙️ Configuration

### LLM Selection

The project uses the `pick_llm()` function in `utils/llm_pick.py` to select a language model based on task complexity.

Available levels in the current implementation:

- `low`: OpenAI
- `medium`: OpenAI
- `high`: OpenAI
- `claude`: Anthropic

The selected provider requires a valid API key and an available model.

### Database Configuration

The project uses PostgreSQL for storing and retrieving data.

Default connection settings:

```python
{
    "host": "localhost",
    "port": 5432,
    "user": "postgres",
    "dbname": "data_agent"
}
```

Configure your PostgreSQL password in the database connection settings.

## 💻 Usage

### Run the Data Agent

Activate your virtual environment and execute:

```bash
python main.py
```

The application starts the main Data Agent workflow.

### Database Initialization

To load the sample datasets into PostgreSQL:

```bash
python feed_db.py
```

### Example Queries

SQL operations:

```text
Show me the top 5 users with the highest ratings.
```

```text
Calculate the average rating for each vehicle type.
```

ETL operations:

```text
Extract data from an API endpoint and save it as CSV.
```

```text
Transform rides.csv by filtering records with rating greater than 4.5 and save the result as JSON.
```

## 🤖 Agent Descriptions

### 1. Data Agent

File: `agents/data_agent.py`

Responsibilities:

- Receives user queries.
- Classifies queries into SQL or ETL tasks.
- Routes requests to specialized agents.
- Coordinates the overall workflow.

### 2. SQL Analyst Agent

File: `agents/sql_analyst.py`

Responsibilities:

- Generates SQL queries.
- Retrieves database schema information.
- Validates SQL queries.
- Executes approved queries.
- Generates responses from database results.

### 3. ETL Analyst Agent

File: `agents/etl_analyst.py`

Responsibilities:

- Extracts data from APIs.
- Transforms data using Pandas.
- Loads processed data into storage.
- Selects appropriate ETL tools.

## 🔐 Security

The project includes security-related features:

- SQL query validation.
- Protection against destructive SQL operations.
- Input validation.
- Environment-based credential configuration.

Always keep your API keys and database credentials private.

Never commit `.env`, passwords, or other sensitive information to GitHub.

## 🚨 Troubleshooting

### Database Connection Failed

Check whether PostgreSQL is running and verify your connection settings.

### API Key Error

Verify that your selected LLM provider has a valid API key.

### Module Not Found

Activate the virtual environment and install dependencies again:

```bash
pip install -r requirements.txt
```

### Database Does Not Exist

Create the `data_agent` database in PostgreSQL before running `feed_db.py`.

## 🤝 Contributing

Contributions and suggestions are welcome.

1. Fork the repository.
2. Create a new branch.
3. Make your changes.
4. Commit your changes.
5. Submit a pull request.

## 👨‍💻 Author

**Vasanthkumar HS**

GitHub: https://github.com/Vasanthkumar1718

Project Repository: https://github.com/Vasanthkumar1718/AI-Data-Agent

---

**Version:** 0.1.0

**Project:** AI Data Agent