# QuickBooks Online MCP Server

Model Context Protocol (MCP) server for the [QuickBooks Online Accounting API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account). Enables Claude and other MCP-compatible clients to manage QBO customers, invoices, expenses, payments, and reports.

## Quick Start

### Prerequisites

- Node.js >= 20
- QuickBooks Online OAuth2 app credentials (requires an Intuit developer account)

### Install and Build

```bash
npm install
npm run build
```

### Run (stdio mode)

```bash
QBO_ACCESS_TOKEN=your-access-token QBO_REALM_ID=your-realm-id npm start
```

### Run (HTTP mode)

```bash
MCP_TRANSPORT=http QBO_ACCESS_TOKEN=your-access-token QBO_REALM_ID=your-realm-id npm start
```

The server listens on `http://0.0.0.0:8080/mcp` by default.

### Docker

```bash
docker build -t qbo-mcp .
docker run -p 8080:8080 \
  -e MCP_TRANSPORT=http \
  -e QBO_ACCESS_TOKEN=your-access-token \
  -e QBO_REALM_ID=your-realm-id \
  qbo-mcp
```

## Environment Variables

| Variable | Required | Default | Description |
|---|---|---|---|
| `QBO_ACCESS_TOKEN` | Yes (env mode) | — | QuickBooks Online OAuth2 access token |
| `QBO_REALM_ID` | Yes (env mode) | — | QuickBooks Online company (realm) ID |
| `MCP_TRANSPORT` | No | `stdio` | Transport type: `stdio` or `http` |
| `MCP_HTTP_PORT` | No | `8080` | HTTP server port |
| `MCP_HTTP_HOST` | No | `0.0.0.0` | HTTP server bind address |
| `AUTH_MODE` | No | `env` | Auth mode: `env` or `gateway` |

## Gateway Mode

When `AUTH_MODE=gateway`, credentials are passed per-request via HTTP headers instead of environment variables:

- `X-Qbo-Access-Token` — OAuth2 access token
- `X-Qbo-Realm-Id` — QuickBooks Online company (realm) ID

This allows a gateway/proxy to manage multi-tenant credentials.

## Available Tools

Tools are organized into domains. Use `qbo_navigate` to select a domain, then use the domain-specific tools.

### Navigation

- `qbo_navigate` — Select a domain (customers, invoices, expenses, payments, reports)
- `qbo_back` — Return to domain selection

### Customers

- `qbo_customers_list` — List customers
- `qbo_customers_get` — Get customer by ID
- `qbo_customers_create` — Create a new customer
- `qbo_customers_search` — Search customers by name or other criteria

### Invoices

- `qbo_invoices_list` — List invoices
- `qbo_invoices_get` — Get invoice by ID
- `qbo_invoices_create` — Create a new invoice
- `qbo_invoices_send` — Send an invoice via email

### Expenses

- `qbo_expenses_list_purchases` — List purchase transactions
- `qbo_expenses_list_bills` — List bills
- `qbo_expenses_get_purchase` — Get purchase by ID
- `qbo_expenses_get_bill` — Get bill by ID

### Payments

- `qbo_payments_list` — List payments
- `qbo_payments_get` — Get payment by ID
- `qbo_payments_create` — Create a new payment

### Reports

- `qbo_reports_profit_and_loss` — Generate Profit and Loss report
- `qbo_reports_balance_sheet` — Generate Balance Sheet report
- `qbo_reports_aged_receivables` — Generate Aged Receivables report
- `qbo_reports_aged_payables` — Generate Aged Payables report
- `qbo_reports_customer_sales` — Generate Customer Sales report

## License

Apache-2.0
