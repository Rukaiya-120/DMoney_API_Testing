# DMoney API Testing — Assignment B18

> Automated API test suite for the DMoney financial transaction system, covering user creation, admin activation, deposits, money transfers, and cashout flows.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Test Flow](#test-flow)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Environment Setup](#environment-setup)
- [How to Run](#how-to-run)
- [Test Cases](#test-cases)
- [API Documentation](#api-documentation)
- [Newman Report](#newman-report)
- [.gitignore Configuration](#gitignore-configuration)

---

## Project Overview

This project contains a complete Postman collection with **30 test cases** (positive & negative) that test the full DMoney API transaction lifecycle including user registration, admin activation, system deposit, agent deposit, send money, and cashout operations.

---

## Test Flow

The end-to-end test scenario follows this sequence:

```
1. Create 2 Customers + 1 Agent (via API)
        ↓
2. Admin activates all 3 users
        ↓
3. System deposits 5000 BDT → Agent account
        ↓
4. Agent deposits 2000 BDT → Customer 1
        ↓
5. Customer 1 sends 1000 BDT → Customer 2
        ↓
6. Customer 2 cashout 500 BDT ← Agent
```

---

## Technologies Used

| Tool | Purpose |
|------|---------|
| **Postman** | API test collection & scripting |
| **Newman** | CLI runner for Postman collections |
| **newman-reporter-htmlextra** | HTML report generation |
| **Node.js** | Runtime for Newman |
| **dotenv / .env** | Environment variable management |

---

## Project Structure

```
DMoney_API_Testing_B18/
├── DMoney_Collection.postman_collection.json
├── DMoney_Environment.postman_environment.json
├── .env                          # ← listed in .gitignore
├── .gitignore
├── package.json
├── Reports/
│   ├── newman-report.html
│   └── newman-report-screenshot.png
├── node_modules/                 # ← listed in .gitignore
└── README.md
```

---

## Environment Setup

### Prerequisites

- [Node.js](https://nodejs.org/) v14+
- npm

### Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/DMoney_API_Testing_B18.git
cd DMoney_API_Testing_B18

# Install dependencies
npm install

# Install Newman globally (optional)
npm install -g newman
npm install -g newman-reporter-htmlextra
```

### Environment Variables (`.env`)

Create a `.env` file in the project root:

```env
BASE_URL=http://dmoney.roadtocareer.net
ADMIN_EMAIL=admin@roadtocareer.net
ADMIN_PASSWORD=4321
SYSTEM_EMAIL=system@roadtocareer.net
SYSTEM_PASSWORD=4321
```

> ⚠️ **Never commit the `.env` file to GitHub.** It is included in `.gitignore`.

---

## How to Run

### Run with Newman (CLI)

```bash
newman run DMoney_Collection.postman_collection.json \
  --environment DMoney_Environment.postman_environment.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export Reports/newman-report.html
```

### Run with npm script

```bash
npm test
```

---

## Test Cases

📊 **[View Full Test Cases (Excel)](https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID)**  
*(Replace with your actual Google Sheets or GitHub link after uploading `DMoney_TestCases_B18.xlsx`)*

### Test Case Summary

| Module | Positive | Negative | Total |
|--------|----------|----------|-------|
| User Registration | 3 | 3 | 6 |
| Admin Activate | 4 | 2 | 6 |
| System Deposit | 2 | 3 | 5 |
| Agent Deposit | 2 | 3 | 5 |
| Send Money | 2 | 3 | 5 |
| Cashout | 2 | 3 | 5 |
| **Total** | **15** | **17** | **30** |

### Sample Test Cases

#### ✅ Positive Test Cases

| TC ID | Title | Method | Expected Status |
|-------|-------|--------|----------------|
| TC_REG_01 | Create Customer 1 with valid data | POST | 201 |
| TC_REG_02 | Create Customer 2 with valid data | POST | 201 |
| TC_REG_03 | Create Agent with valid data | POST | 201 |
| TC_ACT_02 | Activate Customer 1 using admin token | PATCH | 200 |
| TC_DEP_02 | System deposits 5000 BDT to agent | POST | 200 |
| TC_ADEP_02 | Agent deposits 2000 BDT to Customer 1 | POST | 200 |
| TC_SEND_02 | Customer 1 sends 1000 BDT to Customer 2 | POST | 200 |
| TC_CASH_02 | Customer 2 cashes out 500 BDT from agent | POST | 200 |

#### ❌ Negative Test Cases

| TC ID | Title | Method | Expected Status |
|-------|-------|--------|----------------|
| TC_REG_04 | Create user with duplicate email | POST | 400 |
| TC_REG_05 | Create user with missing required field | POST | 400 |
| TC_ACT_05 | Activate non-existent user ID | PATCH | 404 |
| TC_ACT_06 | Activate without admin token | PATCH | 401 |
| TC_DEP_03 | Deposit below minimum amount | POST | 400 |
| TC_SEND_04 | Customer sends money to themselves | POST | 400 |
| TC_CASH_03 | Cashout more than available balance | POST | 400 |

---

## API Documentation

📖 **[View Postman API Documentation](https://documenter.getpostman.com/view/48040404/2sBXqQGdXD)**

### API Endpoints Summary

| # | Endpoint | Method | Description | Auth Required |
|---|----------|--------|-------------|---------------|
| 1 | `/user/create` | POST | Register a new user (customer/agent) | No |
| 2 | `/user/login` | POST | Login and get JWT token | No |
| 3 | `/user/activate/:id` | PATCH | Admin activates a user account | Admin Token |
| 4 | `/transaction/deposit` | POST | Deposit money to an account | User Token |
| 5 | `/transaction/sendmoney` | POST | Send money between customers | User Token |
| 6 | `/transaction/cashout` | POST | Customer cashes out via agent | User Token |

---

## Newman Report

### Report Screenshot

> 📸 Upload your Newman HTML report screenshot here after running:
> ```bash
> newman run ... --reporter-htmlextra-export Reports/newman-report.html
> ```
> Then take a screenshot and save it as `Reports/newman-report-screenshot.png`.

![Newman Report](Reports/newman-report-screenshot.png)

### How to Generate the Report

```bash
newman run DMoney_Collection.postman_collection.json \
  -e DMoney_Environment.postman_environment.json \
  -r htmlextra \
  --reporter-htmlextra-export ./Reports/newman-report.html \
  --reporter-htmlextra-title "DMoney API Test Report" \
  --reporter-htmlextra-browserTitle "DMoney Report"
```

---

## .gitignore Configuration

The `.gitignore` file includes:

```gitignore
# Dependencies
node_modules/

# Environment variables (sensitive data)
.env

# Newman HTML Reports (optional - include if you want reports ignored)
# Reports/

# OS files
.DS_Store
Thumbs.db
```

> ✅ `node_modules/`, `Reports/` folder (optional), and `.env` are all properly excluded from version control.

---

## Author
Rukaiya Haque 
