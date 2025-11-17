# CypherSol - CA Offline Suite

A comprehensive desktop application designed for Chartered Accountants (CAs) to extract, analyze, and process bank statements with advanced AI-powered categorization and reporting capabilities.

## Overview

CypherSol enables CAs to:
- Extract transactions from bank statement PDFs (including password-protected files)
- Automatically categorize transactions using machine learning (spaCy NLP, Transformers)
- Generate detailed financial reports in Excel format
- Import transaction data into Tally accounting software (Tally ERP & Tally Prime)
- Track commission opportunities and perform comprehensive financial analysis
- Visualize financial data with interactive dashboards and charts

## Key Features

### 1. **Bank Statement PDF Processing**
- Upload and parse bank statements from multiple banks and formats
- Password-protected PDF support
- Multi-page statement handling with automatic page trimming
- Intelligent extraction of account numbers and IFSC codes using Named Entity Recognition

### 2. **AI-Powered Transaction Categorization**
- Automatic extraction and categorization of transactions
- 60+ predefined transaction categories (Cash Deposits, Salary, Loans, Investments, etc.)
- Machine learning models using spaCy NLP & Transformer architecture
- Manual category rectification interface

### 3. **Financial Analytics & Dashboards**
- **Main Dashboard**: Summary statistics (reports processed, statements, transactions, time saved)
- **Individual Statement Dashboard**: Detailed transaction analysis
- **Case Dashboard**: Multi-statement consolidated view
- Real-time metrics by time period (today, week, month, year, all-time)
- End-of-Day (EOD) balance tracking

### 4. **Advanced Financial Reports**
- Summary Sheet: Income, expenses, contra entries
- Transaction Sheet: Detailed transaction listings
- Debtor/Creditor Lists: Outstanding receivables and payables
- Investment Tracking: Long-term investment analysis
- EMI/Loan Management: Installment tracking
- UPI Transaction Segregation
- Cash Deposit/Withdrawal Analysis
- Suspense Account Tracking

### 5. **Excel Report Generation**
- Multi-sheet formatted workbooks with color-coded tabs
- Automatic column width adjustment and styling
- Embedded charts and visualizations
- Filter-enabled data exploration

### 6. **Tally Integration**
- Import transactions into Tally Prime and Tally ERP
- Automated voucher creation (Payment, Receipt, Purchase, Sale, Ledger)
- XML generation for Tally compatibility
- Manual and automatic import workflows

### 7. **License Management**
- Secure license key validation with online verification
- OS-level secure credential storage
- License expiration countdown and offline functionality

### 8. **Auto-Update System**
- GitHub-based release management
- Automatic update checking and installation
- Database backup before updates

## Architecture

### Three-Tier Architecture

```
┌─────────────────────────────────────────────────────────┐
│         ELECTRON DESKTOP APPLICATION LAYER             │
│  (Main Process + Preload + IPC Communication)          │
└────────────────┬────────────────────────────────────────┘
                 │
        ┌────────┴──────────────────┐
        │                           │
┌───────▼──────────┐      ┌────────▼──────────┐
│  FRONTEND LAYER  │      │  BACKEND LAYER    │
│  (React App)     │      │  (FastAPI Server) │
│  Port: 3000      │      │  Port: 7500       │
└────────┬─────────┘      └────────┬──────────┘
         │                        │
         └────────────┬───────────┘
                      │
              ┌───────▼────────┐
              │ LOCAL DATABASE │
              │   (SQLite3)    │
              └────────────────┘
```

## Technology Stack

### Frontend
- **Framework**: React 18/19 with Electron 33.3
- **Styling**: Tailwind CSS 3.4 with Radix UI components
- **Routing**: React Router v6 (HashRouter)
- **Charts**: Recharts 2.15 (bar, line, pie charts)
- **State Management**: React Context API (Auth, Reports, Loading, Breadcrumbs)
- **Data Tables**: React Data Table Component
- **HTTP Client**: Axios

### Backend
- **Framework**: FastAPI 0.115.6 with Uvicorn server
- **PDF Processing**: pdfplumber, PyPDF2, PyMuPDF
- **ML/NLP**:
  - spaCy 3.8 (Named Entity Recognition)
  - Transformers 4.47 (Deep learning models)
  - PyTorch 2.5.1 with TorchVision
  - Hugging Face Hub (pretrained models)
- **Data Processing**: Pandas 2.2, NumPy 2.2
- **Excel Generation**: openpyxl, XlsxWriter

### Database & ORM
- **Database**: SQLite3 (file-based relational database)
- **ORM**: Drizzle ORM 0.38.3 with migration support
- **Tables**: Users, Cases, Statements, Transactions, Categories, Summaries, Vouchers, etc.

### Security & Session Management
- **Authentication**: bcrypt password hashing
- **Credential Storage**: keytar (OS-level secure storage)
- **Session**: electron-store for persistence

## Project Structure

```
ca-offline-suite/
├── backend/                          # FastAPI Backend
│   ├── main.py                       # FastAPI application entry point
│   ├── requirements.txt              # Python dependencies
│   ├── tax_professional/             # CA-specific business logic
│   │   └── banks/
│   │       └── CA_Statement_Analyzer.py  # Core extraction & analysis
│   ├── common_functions.py           # Shared business functions
│   ├── pdf_to_name.py                # NER for name extraction
│   └── Final_Category.xlsx           # Category reference data
│
├── frontend/                         # Electron + React App
│   ├── main.js                       # Electron main process
│   ├── preload.js                    # Electron preload script (IPC bridge)
│   ├── SessionManager.js             # User session & license countdown
│   ├── LicenseManager.js             # License validation & storage
│   ├── drizzle.config.js             # Database configuration
│   │
│   ├── db/                           # Database layer
│   │   ├── db.js                     # Drizzle ORM instance
│   │   └── schema/                   # Database schema definitions
│   │
│   ├── ipc/                          # Electron IPC handlers
│   │   ├── authHandlers.js           # Login/signup/license validation
│   │   ├── mainDashboard.js          # Dashboard data aggregation
│   │   ├── reportHandlers.js         # Report generation logic
│   │   ├── tallyHandlers.js          # Tally import/export
│   │   └── excelDownloadHandler.js   # Excel report generation
│   │
│   ├── drizzle/                      # Database migrations
│   │   └── [0000-0012]_*.sql         # SQL migration files
│   │
│   └── react-app/                    # React frontend application
│       ├── src/
│       │   ├── App.js                # Root component with routing
│       │   ├── Pages/                # Main dashboard pages
│       │   ├── components/           # React components
│       │   ├── contexts/             # Context providers
│       │   └── hooks/                # Custom React hooks
│       └── build/                    # Production build output
│
├── docs/
│   └── api-design.md                 # API documentation
│
└── README.md                         # This file
```

## Prerequisites

- **Node.js**: v16+ (for Electron and React)
- **Python**: 3.8+ (for FastAPI backend)
- **pip**: Python package manager
- **npm**: Node package manager

## Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/Shama-Cyphersol/ca-offline-suite.git
cd ca-offline-suite
```

### 2. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Create Python virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
source venv/Scripts/activate
# On macOS/Linux:
source venv/bin/activate

# Install Python dependencies
pip install -r requirements.txt

# Download spaCy language model
python -m spacy download en_core_web_sm

# Return to root directory
cd ..
```

### 3. Frontend Setup

```bash
# Navigate to frontend directory
cd frontend

# Install Electron and frontend dependencies
npm install

# Navigate to React app and install dependencies
cd react-app
npm install

# Return to frontend directory
cd ..
```

### 4. Database Setup

```bash
# Run database migrations (from frontend directory)
npx drizzle-kit migrate
```

## Running the Application

### Development Mode

From the `frontend` directory:

```bash
npm start
```

This will automatically start all three components:
- **FastAPI Backend**: http://localhost:7500
- **React Frontend**: http://localhost:3000
- **Electron Desktop App**: Launches automatically

### Production Build

```bash
# From frontend directory
npm run build

# This will:
# 1. Build the React app (react-scripts build)
# 2. Package the Electron app with electron-builder
# 3. Create platform-specific installers in /dist
```

## Deployment Targets

The application supports the following platforms:

- **Windows**: NSIS installer (.exe) - 64-bit
- **macOS**: DMG package + ZIP archive
- **Linux**: AppImage format

## Development Guide

### Project Workflow

1. **Frontend Development**: Edit files in `frontend/react-app/src/`
2. **Backend Development**: Edit files in `backend/`
3. **Database Changes**: Update schema in `frontend/db/schema/`, then run migrations
4. **IPC Handlers**: Add new handlers in `frontend/ipc/`

### Key Commands

```bash
# Start development environment
npm start

# Build React app only
npm run build:react

# Build Electron app only
npm run build:electron

# Run database migrations
npx drizzle-kit migrate

# Generate new migration
npx drizzle-kit generate:sqlite

# Deploy with auto-update
npm run deploy
```

### Environment Variables

- **Development**: Database at `/frontend/db.sqlite3`
- **Production**: Database at `~/.userData/db.sqlite3`

### Adding New Features

1. **Frontend**: Create components in `react-app/src/components/`
2. **Backend**: Add endpoints in `backend/main.py` or create new modules
3. **IPC**: Add handlers in `frontend/ipc/` and register in `main.js`
4. **Database**: Update schema in `frontend/db/schema/` and generate migration

## Database Schema

The application uses 10+ tables:

- **users**: User authentication and profile
- **cases**: CA case management
- **statements**: Bank statement records
- **transactions**: Individual transaction records
- **categories**: Transaction category definitions
- **summaries**: Financial summary data
- **eod_balance**: End-of-day balance tracking
- **trade_vouchers**: Tally trade vouchers
- **finance_vouchers**: Tally finance vouchers
- **opportunity_to_earn**: Commission tracking

## License Management

The application requires a valid license key for activation:

1. Users register with email, password, and license key
2. License is validated against the server (online)
3. License information is securely stored in OS keychain
4. Session countdown tracks license expiration
5. Automatic logout on license expiration

### License Validation Endpoints

- **Production**: https://cyphersol.co.in
- **UAT/Development**: https://cyphersol-uat.duckdns.org

## Auto-Update Configuration

Updates are managed through GitHub releases:

- **Repository**: Shama-Cyphersol/ca-offline-suite
- **Provider**: GitHub
- **Auto-install**: Yes (on app quit)
- **Database backup**: Automatic before updates

## API Documentation

For detailed API documentation, see [docs/api-design.md](docs/api-design.md).

## Contributing

When contributing to this repository:

1. Create a feature branch with prefix `claude/` and session ID
2. Make your changes and commit with clear messages
3. Push to the feature branch
4. Create a pull request for review

## Support

For issues, feature requests, or questions:
- Create an issue in the GitHub repository
- Contact the development team

## Credits

Developed by **CypherSol** for Chartered Accountants to streamline financial analysis and reporting.

---

**Version**: 1.0.0
**Last Updated**: 2025-11-17
