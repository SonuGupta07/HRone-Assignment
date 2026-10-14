# Employee Attendance & Analytics API

A REST API built using **Python, FastAPI, and MongoDB** to manage employee attendance, handle punch-in and punch-out operations, process attendance corrections, and generate attendance analytics.

## Tech Stack

- Python 3.11+
- FastAPI
- MongoDB 6.0+
- PyMongo
- Pydantic

## Features

- Employee creation and employee listing with pagination and filters
- Attendance punch-in and punch-out
- Attendance listing with filtering and pagination
- Manual attendance regularization with an audit trail
- Employee monthly attendance analytics
- Department-wise attendance summaries
- Late-arrival leaderboard with competition ranking
- Department attendance trends with moving averages
- MongoDB indexes for query performance and data integrity
- Query execution plans through the explain endpoint

## Prerequisites

Make sure Python 3.11 or newer and MongoDB 6.0 or newer are installed and running.

## Setup and Installation

Clone the repository and move into the project directory:

```bash
git clone <your-public-repository-url>
cd <repository-folder>
```

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

Configure the MongoDB connection using environment variables:

```bash
export MONGO_URI="mongodb://localhost:27017"
export MONGO_DB="attendance_db"
```

Make sure MongoDB is running before starting the application.

## Run the Application

Start the FastAPI server:

```bash
uvicorn app.main:app --port 8000
```

Alternatively, if needed to use the Python interpreter from the active virtual environment:

```bash
python -m uvicorn app.main:app --port 8000
```

The API will be available at:

- API base URL: `http://localhost:8000`
- Interactive Swagger documentation: `http://localhost:8000/docs`
- OpenAPI schema: `http://localhost:8000/openapi.json`

## Sample Data

To load the supplied sample dataset into MongoDB, run:

```bash
python sample_seed.py
```

Use the database configured through `MONGO_DB` and make sure the sample data is loaded before running tests that depend on it.

## Testing

Use Swagger UI at `/docs` to test the API endpoints and inspect request validation and response codes.

The API also provides an explain endpoint for inspecting MongoDB query execution plans and index usage.

## Project Structure

```text
.
├── app/
│   ├── __init__.py
│   └── main.py
├── sample_data/
├── sample_seed.py
├── requirements.txt
├── openapi.yaml
├── DATA_MODEL.md
├── REVIEW.md
├── DECISIONS.md
└── README.md
```

## Configuration and Security

- Configure the MongoDB URI and database name through environment variables.
- Do not commit `.env` files, passwords, or other secrets.
- Do not include the local `.venv` directory or Python cache files in the submitted repository.

