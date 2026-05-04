# Facturo — Billing System

> A full-stack billing system built with Laravel, Next.js, and an external Auth as a Service. Create and send invoices, accept online payments via Stripe, and automate client reminders.

---

## Repositories

| Repo | Description |
|------|-------------|
| [billing-system-api](https://github.com/your-org/billing-system-api) | Laravel REST API |
| [billing-system-ui](https://github.com/your-org/billing-system-ui) | Next.js frontend |
| [auth-service-project](https://github.com/patrick-rakotoharilalao/auth-service-project) | External AaaS (Auth as a Service) |

---

## Features

- **Client management** — create, edit, and track your clients
- **Invoice lifecycle** — Draft → Sent → Paid → Overdue
- **PDF generation** — modern invoice PDFs with mPDF
- **Stripe payments** — hosted checkout with webhook integration
- **Automated emails** — invoice delivery, reminders, and payment confirmations
- **Scheduler** — automatic overdue detection and reminder emails
- **Dashboard** — revenue charts, invoice stats, top clients

---

## System Architecture

```
┌─────────────────────────────────────────────────────┐
│                    Browser                          │
│                  (Next.js UI)                       │
└──────────┬──────────────────────┬───────────────────┘
           │                      │
           │ Login/Register       │ API calls
           ▼                      ▼
┌──────────────────┐   ┌──────────────────────────────┐
│   AaaS Node.js   │   │      Laravel REST API        │
│  (Auth Service)  │◄──│   VerifyJwtToken Middleware  │
│                  │   │                              │
│  - Register      │   │  - Clients CRUD              │
│  - Login         │   │  - Invoices CRUD             │
│  - Verify JWT    │   │  - PDF Generation            │
│  - Refresh Token │   │  - Stripe Integration        │
└──────────────────┘   │  - Email Queue               │
                       │  - Scheduler                 │
                       └──────────────┬───────────────┘
                                      │
                              ┌───────▼───────┐
                              │     MySQL     │
                              └───────────────┘
```

---

## Getting Started

### 1. Start the AaaS

Follow the setup instructions in the [AaaS repository](https://github.com/patrick-rakotoharilalao/auth-service-project).

```bash
# The AaaS should run on port 3001
```

### 2. Start the Laravel API

```bash
cd billing-system-api
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve           # port 8000
php artisan queue:work      # in a separate terminal
```

### 3. Start the Next.js UI

```bash
cd billing-system-ui
npm install
cp .env.example .env.local
npm run dev                 # port 3000
```

### 4. (Optional) Start Stripe webhook forwarding

```bash
stripe listen --forward-to localhost:8000/api/v1/webhooks/stripe
```

---

## Environment Overview

| Service | Default Port |
|---------|-------------|
| Next.js UI | `3000` |
| Laravel API | `8000` |
| AaaS | `3001` |
| MySQL | `3306` |

---

## Invoice Workflow

```
1. Create invoice (Draft)
      ↓
2. Send invoice → Stripe payment link created → Email sent to client
      ↓
3a. Client pays via Stripe → Webhook → Invoice marked as Paid → Confirmation email
3b. Due date passes without payment → Scheduler → Invoice marked as Overdue → Reminder email
```

---

## Built With

| Layer | Technology |
|-------|------------|
| Frontend | Next.js 15, TypeScript, Tailwind CSS, shadcn/ui |
| Backend | PHP 8.5, Laravel 12 |
| Auth | Node.js, TypeScript (AaaS) |
| Database | MySQL |
| Payments | Stripe |
| PDF | mPDF |
| Email | Laravel Mail + Mailtrap (dev) |
| Charts | Recharts |

---

## Author

**Patrick Rakotoharilalao** — Full Stack Developer
