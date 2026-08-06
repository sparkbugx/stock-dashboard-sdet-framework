# Stock Dashboard SDET Framework

A Software Development Engineer in Test (SDET) repository designed to validate end-to-end (E2E) workflows for an Angular Stock Dashboard frontend and verify backend integrations using the **Alpha Vantage Intraday API**.

> **Status:** The Angular application is scaffolded and the project structure is in place. The Playwright E2E and `.http` API test layers described below are **planned** and will be built out progressively.

---

## 🏗️ Architecture Overview

```
stock-dashboard-sdet-framework/
├── src/                      # Angular frontend application
│   └── app/                  # Core components, routes & unit tests
├── e2e/                      # Playwright E2E Test Automation  [planned]
│   ├── fixtures/             # Custom test fixtures & mock data
│   ├── page-objects/         # Page Object Model (POM) architecture
│   ├── tests/                # Automated UI test specs
│   └── playwright.config.ts  # Playwright configuration file
├── api-tests/                # API Automation Test Suites (.http)  [planned]
│   ├── environments.json     # Environment variables (Base URL, API Key)
│   └── intraday-suite.http   # Automated API assertions for Alpha Vantage
├── package.json              # Dependencies and test execution scripts
└── README.md                 # Project documentation
```

---

## 🚀 Key Features & Tech Stack

| Layer | Technology |
| --- | --- |
| Frontend | Angular (Stock Dashboard Application) |
| External API | Alpha Vantage (`TIME_SERIES_INTRADAY`) |
| Unit Testing | Angular unit tests (Vitest via `ng test`) |
| E2E UI Testing | Playwright (TypeScript) — *planned* |
| API Testing | `.http` REST Client files with automated test suites — *planned* |
| Automation Pattern | Page Object Model (POM) for clean, maintainable UI automation — *planned* |

---

## 🛠️ Prerequisites

Ensure you have the following installed on your machine before setup:

- **Node.js** (v18.x or higher)
- **npm** (v9.x or higher)
- **VS Code** (recommended) with the following extensions:
  - [Playwright Test for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright)
  - [httpyac](https://marketplace.visualstudio.com/items?itemName=anweber.vscode-httpyac) (recommended for `.http` suites with automated assertions) or [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
- **Alpha Vantage API Key:** Get a free API key at [Alpha Vantage](https://www.alphavantage.co/support/#api-key).

---

## ⚙️ Installation & Setup

1. **Clone the repository:**
```bash
git clone https://github.com/sparkbugx/stock-dashboard-sdet-framework.git
```

2. **Install dependencies:**
```bash
npm install
```

3. **Install Playwright browsers** *(once the E2E layer is added):*
```bash
npx playwright install
```
> On Linux, add `--with-deps` to also install OS-level browser dependencies.

4. **Environment Configuration:**
   Update the variables in `api-tests/environments.json` (or create a `.env` file in the root directory):
```env
ALPHA_VANTAGE_API_KEY=your_api_key_here
BASE_URL=http://localhost:4200
```

5. **Run the application:**
```bash
npm start
```
The dashboard is served at `http://localhost:4200`.

---

## 🧪 Executing Tests

### 1. Unit Tests (Current)

Angular unit tests run through Vitest and cover component behavior.

| Script | Description |
| --- | --- |
| `npm test` | Runs all Angular unit tests |

### 2. API Testing Suite (`.http` Automation) — *planned*

API tests are defined using standard `.http` syntax. They execute raw HTTP requests and run assertions against responses (status codes, headers, and JSON body structures). Open `api-tests/intraday-suite.http` in VS Code and click **Send Request** or **Run Suite** above any request block.

Example `.http` test snippet:

```http
@baseUrl = https://www.alphavantage.co
@apiKey = {{ALPHA_VANTAGE_API_KEY}}

### Test Intraday Stock Data Endpoint Status & Schema
# @name getIntradayIBM
GET {{baseUrl}}/query?function=TIME_SERIES_INTRADAY&symbol=IBM&interval=5min&apikey={{apiKey}} HTTP/1.1
Accept: application/json

?? status == 200
?? header content-type matches application/json
?? body Meta Data['2. Symbol'] == IBM
?? body Meta Data['4. Interval'] == 5min
```

### 3. Playwright E2E Testing Suite — *planned*

Playwright handles browser automation and UI validation for the Angular frontend.

| Script | Description |
| --- | --- |
| `npm run test:e2e` | Runs all Playwright E2E tests headlessly |
| `npm run test:e2e:ui` | Opens Playwright's interactive UI Mode |
| `npm run test:e2e:headed` | Runs tests in visible browser windows |
| `npm run test:report` | Serves the HTML test execution report |

---

## 📊 Test Coverage & Strategy

```
+-------------------------------------------------------------------+
|                        Quality Assurance Layer                     |
+-------------------------------------------------------------------+
|  [Playwright UI E2E]   ---> Validates Angular Components,         |
|                             Charts, Filters, and User Flow        |
+-------------------------------------------------------------------+
|  [Playwright API Mock] ---> Intercepts Alpha Vantage Requests     |
|                             for Deterministic UI Testing          |
+-------------------------------------------------------------------+
|  [.http Test Suite]   ---> Direct Contract Validation against     |
|                             Live Alpha Vantage Intraday Endpoint  |
+-------------------------------------------------------------------+
```

1. **Contract & API Validation:** `.http` files quickly ensure Alpha Vantage's `TIME_SERIES_INTRADAY` responses conform to expected schemas without loading heavy browser instances.
2. **Deterministic UI Testing:** Playwright intercepts backend network calls to inject mock JSON responses, preventing API rate-limit errors (5 requests/min on the free tier) during UI automation.
3. **End-to-End User Journeys:** Live integration tests cover searching for stock tickers, rendering charts, and handling error states (e.g., API throttling or invalid symbols).

---

## 🗺️ Roadmap

- [x] Angular frontend scaffold & unit test setup
- [ ] Playwright E2E suite with Page Object Model (POM)
- [ ] Playwright config with network mocking for deterministic UI tests
- [ ] `.http` API test suite for the Alpha Vantage `TIME_SERIES_INTRADAY` endpoint
- [ ] `test:e2e*` npm scripts and CI-ready report generation
