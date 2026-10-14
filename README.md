# Employee Attendance & Analytics API

A REST API built with **Python, FastAPI, and MongoDB** for managing employee attendance, processing attendance corrections, and generating attendance analytics.

This project was developed as part of the HROne Software Engineer Trainee technical assignment.

## Tech Stack

- **Language:** Python 3.11+
- **API Framework:** FastAPI
- **Database:** MongoDB 6.0+
- **Database Driver:** PyMongo
- **Data Validation:** Pydantic

## Features

- Create and list employees with filtering and pagination
- Record employee punch-in and punch-out
- Retrieve attendance records with filters and pagination
- Correct attendance records through manual regularization with an audit trail
- Generate employee-level monthly attendance analytics
- Calculate department-wise attendance summaries
- Rank employees by late-arrival minutes, including tied ranks
- Generate department attendance trends with moving averages
- Create MongoDB indexes for data integrity and query performance
- Inspect MongoDB query execution plans through the explain endpoint

## Prerequisites

Before running the application, ensure you have:

- Python 3.11 or newer
- MongoDB 6.0 or newer
- Git

MongoDB can run locally or through a MongoDB Atlas connection string.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/SonuGupta07/HRone-Assignment.git
cd HRone-Assignment
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure the database

Make sure MongoDB is running, then configure the connection using environment variables:

```bash
export MONGO_URI="mongodb://localhost:27017"
export MONGO_DB="attendance_db"
```

For MongoDB Atlas, replace `MONGO_URI` with your connection string. Keep credentials private and never commit database passwords or other secrets.

### 5. Start the application

Run the API using the required application entry point:

```bash
uvicorn app.main:app --port 8000
```

Alternatively:

```bash
python -m uvicorn app.main:app --port 8000
```

The server starts on port `8000`.

## API Documentation

Once the application is running, access:

| Resource | URL |
|---|---|
| API base URL | `http://localhost:8000` |
| Health check | `http://localhost:8000/health` |
| Interactive Swagger UI | `http://localhost:8000/docs` |
| Generated OpenAPI schema | `http://localhost:8000/openapi.json` |

Swagger UI can be used to send requests, inspect request parameters, and review API responses.

## Main API Capabilities

The API covers the following areas:

- **Employees:** Employee creation and filtered, paginated listing
- **Attendance:** Punch-in, punch-out, attendance listing, and manual corrections
- **Employee analytics:** Monthly attendance and working-day calculations
- **Department analytics:** Department summaries and date-wise attendance trends
- **Late-arrival analytics:** Leaderboard with competition ranking
- **Query diagnostics:** MongoDB execution plans and index usage

Refer to the assignment's `openapi.yaml` for the complete endpoint contract and business rules.

## Database and Performance

The application uses MongoDB indexes to enforce uniqueness and support common query patterns. Analytics endpoints use database aggregation where appropriate, and the explain endpoint helps inspect query execution plans.

The implementation was tested against a performance dataset containing 100,000 attendance records.

## Testing

Start the application and use Swagger UI at `/docs` to exercise the endpoints.

For meaningful analytics results, use a MongoDB database containing the expected employee and attendance records. The original candidate kit provides sample-data and seed files, but these supporting files are not included in this submission repository.

## Project Structure

```text
HRone-Assignment/
├── app/
│   └── main.py
├── .gitignore
├── requirements.txt
├── README.md
├── REVIEW.md
└── DECISIONS.md
```

## Configuration and Security

- Configure the MongoDB connection through environment variables.
- Do not commit `.env` files, passwords, API keys, or other secrets.
- Do not commit the local `.venv/` directory or Python cache files.
- Keep database credentials private when using MongoDB Atlas.

## Author

**Sonu Gupta**

GitHub: [SonuGupta07](https://github.com/SonuGupta07)

Repository: [HRone-Assignment](https://github.com/SonuGupta07/HRone-Assignment)
