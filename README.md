# Budget Buddy — Backend API Postman Collection

A comprehensive Postman collection for testing the **Budget Buddy** personal finance application backend API. This collection covers user authentication, income and expense management, category handling, filtering, dashboard data, and Excel export capabilities.

---

## Description

**Budget Buddy** is a personal finance application that helps users track incomes and expenses, manage categories, and visualize their financial data through a dashboard. This repository contains a [Postman](https://www.postman.com/) collection that provides ready-to-use API requests for every endpoint exposed by the Budget Buddy backend, making it easy to explore, test, and integrate with the API.

---

## Features

- 🔐 **Authentication** — User registration, login, and account activation via Bearer token
- 💰 **Income Management** — Create, fetch, filter, and delete income records
- 💸 **Expense Management** — Create, fetch, filter, and delete expense records
- 🗂️ **Category Management** — Create, update, and retrieve categories by type
- 🔍 **Filtering** — Cross-resource filtering of incomes and expenses with custom criteria
- 📊 **Dashboard** — Aggregated financial summary data
- 📥 **Excel Export** — Download or email income/expense data as Excel files
- 🌍 **Multiple Environments** — Separate environment variables for development and production

---

## Requirements

- [Postman](https://www.postman.com/downloads/) (v10 or later recommended)
- A running instance of the Budget Buddy backend API
  - Development: configure `budget-buddy-backend-development` environment variable
  - Production: configure `budget-buddy-backend-production` environment variable

---

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/SalemNabeelSalem/budget-buddy-application-postman.git
cd budget-buddy-application-postman
```

### 2. Import the Postman Collection

1. Open **Postman**.
2. Click **Import** (top-left).
3. Select **Folder** and choose the `postman/collections/budget-buddy-application-backend` directory, **or** drag and drop the folder into the Import dialog.
4. Click **Import**.

### 3. Import the Environment

1. In Postman, go to **Environments** (left sidebar).
2. Click **Import**.
3. Select `postman/environments/budget-buddy-application.environment.yaml`.
4. Click **Import**, then activate the environment by selecting it from the environment dropdown (top-right).

### 4. Set Up Environment Variables

Fill in the required environment variables after importing (see [Environment Variables](#environment-variables) below).

---

## Usage

1. **Activate the environment** — Select `budget-buddy-application` from the Postman environment dropdown.
2. **Register or log in** — Run the `register` or `login` request in the **profile** folder.
3. **Set the Bearer token** — Copy the `token` from the login response and set it as the collection-level Bearer token:
   - Click the collection name → **Authorization** tab → paste the token.
4. **Run requests** — Navigate to any folder and send requests. All authenticated endpoints inherit the Bearer token automatically.

### Example Requests

**Register a new user (POST)**
```
POST {{budget-buddy-backend-development}}/api/{{version}}/profile/register

Body (JSON):
{
  "fullName": "John Doe",
  "email": "john@example.com",
  "password": "yourpassword"
}
```

**Login (POST)**
```
POST {{budget-buddy-backend-development}}/api/{{version}}/profile/login

Body (JSON):
{
  "email": "john@example.com",
  "password": "yourpassword"
}

Response:
{
  "token": "<bearer_token>"
}
```

**Fetch all incomes (GET)**
```
GET {{budget-buddy-backend-development}}/api/{{version}}/incomes

Headers:
  Authorization: Bearer <token>
```

**Create a new expense (POST)**
```
POST {{budget-buddy-backend-development}}/api/{{version}}/expenses

Headers:
  Authorization: Bearer <token>

Body (JSON):
{
  "name": "Transportation",
  "icon": "🚍",
  "date": "2026-03-01",
  "categoryId": 6,
  "amount": 200
}
```

---

## Environment Variables

| Variable | Description | Example |
|---|---|---|
| `budget-buddy-backend-development` | Base URL for the development environment | `http://localhost:8080` |
| `budget-buddy-backend-production` | Base URL for the production environment | `https://api.yourapp.com` |
| `version` | API version string used in URL paths | `v1.0` |

---

## Authentication

The Budget Buddy API uses **Bearer Token (JWT)** authentication.

1. Send a `POST` request to the **login** endpoint with valid credentials.
2. Copy the `token` value from the response body.
3. Set it as the Bearer token at the **collection level** in Postman:
   - Open the collection → **Authorization** tab → Type: `Bearer Token` → paste the token.
4. All requests in the collection will automatically inherit this token.

> ⚠️ Tokens have an expiration time. Re-authenticate and update the token if you receive a `401 Unauthorized` response.

---

## Folder Structure

The collection is organized into the following folders, each grouping endpoints by domain:

```
budget-buddy-application-backend/
├── profile/              # Authentication & account management
│   ├── register          # POST — Register a new user
│   ├── login             # POST — Authenticate and obtain a Bearer token
│   ├── activate          # POST — Activate a user account
│   └── me                # GET  — Retrieve the authenticated user's profile
│
├── category/             # Transaction category management
│   ├── create            # POST — Create a new category
│   ├── fetch all by type # GET  — List categories filtered by type
│   └── update            # PUT  — Update an existing category
│
├── incomes/              # Income record lifecycle
│   ├── create new income           # POST — Add a new income record
│   ├── fetch all incomes           # GET  — List all income records
│   ├── fetch top incomes           # GET  — Retrieve top income entries
│   ├── filter by date range        # GET  — Filter incomes by date range
│   ├── fetch total amount of all   # GET  — Get the total amount of all incomes
│   ├── update an exists income     # PUT  — Update an existing income record
│   └── delete an exists income     # DELETE — Remove an income record
│
├── expenses/             # Expense record lifecycle
│   ├── create new expense          # POST — Add a new expense record
│   ├── fetch all expenses          # GET  — List all expense records
│   ├── fetch top 5                 # GET  — Retrieve top 5 expense entries
│   ├── fetch all by date range     # GET  — Filter expenses by date range
│   ├── fetch total amount of all   # GET  — Get the total amount of all expenses
│   └── delete                      # DELETE — Remove an expense record
│
├── filters/              # Cross-resource filtering
│   ├── filter incomes    # GET  — Filter incomes with custom criteria
│   └── filter expenses   # GET  — Filter expenses with custom criteria
│
├── dashboard/            # Aggregated summary
│   └── get data          # GET  — Retrieve dashboard summary data
│
├── download-excel/       # Excel file download
│   ├── download expenses as excel  # GET  — Download expenses as an Excel file
│   └── download incomes as excel   # GET  — Download incomes as an Excel file
│
└── email-excel/          # Excel file delivery via email
    ├── email expenses as excel     # POST — Email expenses Excel to the user
    └── email incomes as excel      # POST — Email incomes Excel to the user
```

<!-- Example:
![Login Request](screenshots/login-request.png)
![Fetch Incomes Response](screenshots/fetch-incomes-response.png)
-->

---

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature/your-feature-name`
3. Make your changes (add requests, update descriptions, fix variables, etc.).
4. Commit your changes: `git commit -m "feat: add your feature description"`
5. Push to your fork: `git push origin feature/your-feature-name`
6. Open a Pull Request against the `main` branch.

Please ensure all new requests include clear descriptions and example request/response bodies where applicable.

---

## License

This project is licensed under the [MIT License](LICENSE).

```
MIT License

Copyright (c) 2026 Salem Nabeel Salem

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
