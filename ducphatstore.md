# DucPhat Store

Full-stack installment sales management system for an electronics and furniture retail business. Handles the complete lifecycle from inventory procurement and pricing through installment contract creation, payment collection, delivery tracking, and financial reporting. Processes 30,000+ invoices with 2M+ USD equivalent in cumulative revenue.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Python, Flask 1.1 |
| Database | SQLite (via sqlite3 module) |
| Templating | Jinja2, Flask-Bootstrap |
| Task Queue | Celery + Redis (async jobs) |
| File Handling | Flask-Uploads, Cloudinary |
| Data Export | XlsxWriter, xlrd |
| E-commerce | Haravan API integration |
| Social | Facebook API integration |
| Deployment | Docker, Gunicorn |
| Media | Cloudinary CDN, QR Code generation |

## Architecture Overview

```
flaskr/
  __init__.py           # Flask app factory, blueprint registration (24 modules)
  db.py                 # SQLite connection management
  schema.sql            # 14 table definitions (full schema)
  config.py             # Application configuration
  auth.py               # Authentication and role-based permissions

  # Core Business Modules
  invoice.py            # Installment contract CRUD (main business logic)
  invoice_model.py      # Invoice data access layer
  invoice_thanhly.py    # Contract settlement / early payoff
  invoice_giaonhan.py   # Delivery and receipt tracking
  invoice_log.py        # Audit trail for invoice changes
  inventory.py          # Product inventory management
  inventory_model.py    # Inventory data access layer
  inventory_report.py   # Inventory reporting
  client.py             # Customer management (with blacklist)
  staff.py              # Employee management

  # Financial Modules
  expense.py            # Expense tracking
  lsdongtien.py         # Payment history and cash flow
  nhapthemtienmat.py    # Cash deposit recording
  nocong.py             # Supplier debt management
  vayvonnganhang.py     # Bank loan tracking
  statistic.py          # Financial statistics and summaries
  report.py             # Comprehensive reporting
  banggia.py            # Price list management

  # Operations
  chamcong.py           # Employee attendance tracking
  requestprocess.py     # Approval workflow for business requests
  filter.py             # Invoice filtering and search
  filter_inventory.py   # Inventory filtering
  dpfile.py             # File upload management
  blog.py               # Internal blog / announcements
  huongdan.py           # User guide module

  # Integrations
  haravan.py            # Haravan e-commerce platform sync
  facebookapi.py        # Facebook lead management
  bot.py                # Notification bot
  api.py                # REST API endpoints

  templates/            # 26 template directories (Jinja2 HTML)
  static/               # CSS, JS, images
```

## Database Schema (14 Tables)

- **user** -- Staff accounts with store assignment and role-based permissions
- **client** -- Customer profiles with ID, address, blacklist status
- **inventory** -- Product stock with category, brand, cost, sale price, store location
- **invoice** -- Installment contracts linking client, seller, items, payment schedule
- **items** -- Line items per invoice with base cost, sale cost, profit
- **own** -- Payment records (deposits, installment payments)
- **expense** -- Business expense tracking by category and store
- **workday** -- Employee attendance records
- **nocong** -- Supplier debt and procurement tracking
- **nhaptienmat** -- Cash deposit and transfer records
- **requestprocess** -- Business approval workflows
- **invoicelog** -- Audit log for invoice modifications
- **facebooksdt** -- Facebook lead capture and follow-up
- **invoice_giaonhanhd** -- Delivery and receipt confirmation

## Key Features

- Multi-store installment contract management with configurable payment schedules
- Role-based access control (admin, store manager, salesperson)
- Real-time inventory tracking across multiple store locations
- Comprehensive payment history and cash flow reporting
- Employee attendance and payroll management
- Supplier debt tracking and bank loan management
- Facebook lead capture and CRM pipeline
- Haravan e-commerce platform integration
- QR code generation for invoices
- Excel export for all reports
- Audit trail for all invoice modifications

## How It Works

1. **Inventory Management**: Products are added with cost price, sale price, category, and assigned to a store location. Stock levels update automatically when invoices are created.
2. **Invoice Creation**: A salesperson selects a customer (or creates one), picks inventory items, sets the installment plan (number of payments, amount per payment, deposit), and generates the contract.
3. **Payment Collection**: Each payment is recorded against the invoice. The system tracks remaining balance, payment history, and flags overdue accounts.
4. **Delivery Tracking**: The delivery module manages handoff from warehouse to delivery staff to customer, with scan-based confirmation at each step.
5. **Reporting**: Financial statistics aggregate revenue, expenses, profit margins, and cash flow by store, date range, and staff member. All reports exportable to Excel.
