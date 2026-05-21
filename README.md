# NovaBank — Modern Digital Banking Platform

## Project Overview

**Name:** NovaBank
**Type:** Modern Digital Banking Platform
**Goal:** Simulate a real-world banking infrastructure with secure transactions, account management, audit logging, and administrative operations.

This project demonstrates:

* enterprise backend architecture
* financial transaction handling
* security best practices
* scalable system design
* DevOps workflows

---

# Tech Stack

## Frontend

* Next.js
* TypeScript
* Tailwind CSS

## Backend

* NestJS
* TypeScript

## Database

* PostgreSQL

## ORM

* Prisma

## Cache / Queue

* Redis

## Infrastructure

* Docker
* Docker Compose

## Authentication

* JWT
* Refresh Tokens
* Argon2 Password Hashing
* OTP / 2FA

---

# System Architecture

```text
                    ┌──────────────────┐
                    │   Client App     │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │    API Gateway   │
                    └────────┬─────────┘
                             │
        ┌────────────────────┼────────────────────┐
        ▼                    ▼                    ▼
 ┌────────────┐      ┌──────────────┐      ┌──────────────┐
 │ AuthSvc    │      │ AccountSvc  │      │ Transaction  │
 └────────────┘      └──────────────┘      └──────────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Ledger Service   │
                    └──────────────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ PostgreSQL DB    │
                    └──────────────────┘
```

---

# Main Applications

## 1. Client Banking App

### Features

* Register/Login
* View Balance
* Transfer Money
* Transaction History
* Manage Profile
* Notifications
* Multi-currency Wallets
* OTP Verification

---

## 2. Admin Dashboard

### Features

* User Management
* KYC Validation
* Account Freeze/Unfreeze
* Fraud Monitoring
* Transaction Search
* Audit Logs
* Analytics Dashboard
* Manual Transaction Review

---

## 3. API Server

Central backend service responsible for:

* authentication
* business logic
* transaction processing
* account management
* notifications

---

# Core Modules

## Authentication Module

### Responsibilities

* Login/Register
* JWT generation
* Refresh token rotation
* Password hashing
* OTP verification
* Session handling

### Endpoints

```http
POST /auth/register
POST /auth/login
POST /auth/refresh
POST /auth/logout
POST /auth/verify-otp
```

---

# Account Module

### Responsibilities

* Create accounts
* Manage balances
* Generate IBAN/account number
* Multi-currency support

### Endpoints

```http
GET /accounts/me
GET /accounts/:id
POST /accounts/create
PATCH /accounts/freeze
```

---

# Transaction Module

### Responsibilities

* Internal transfers
* Transaction validation
* Balance checks
* Transaction history
* Pending/settled states

### Endpoints

```http
POST /transactions/transfer
GET /transactions/history
GET /transactions/:id
```

---

# Ledger System

## Double-Entry Accounting

Every transaction creates:

* one debit entry
* one credit entry

Example:

```text
User A sends $100 to User B

Ledger:
--------------------------------
Debit  | User A | -100
Credit | User B | +100
--------------------------------
```

This ensures:

* accounting consistency
* rollback safety
* financial traceability

---

# Database Design

## User Table

```sql
id
email
password_hash
role
is_verified
created_at
```

---

## Account Table

```sql
id
user_id
iban
currency
balance
status
created_at
```

---

## Transaction Table

```sql
id
sender_account_id
receiver_account_id
amount
currency
status
reference
created_at
```

---

## Ledger Entry Table

```sql
id
transaction_id
account_id
type
amount
created_at
```

---

# Security Features

## Authentication

* JWT access tokens
* Refresh token rotation
* Argon2 hashing

---

## API Security

* Rate limiting
* Helmet
* CORS protection
* CSRF protection
* Request validation

---

## Banking Security

* Transfer limits
* Suspicious activity detection
* Audit logging
* Transaction rollback
* Idempotency keys

---

# Fraud Detection Rules

Example rules:

* multiple failed logins
* unusual transfer volume
* rapid withdrawals
* transfers to flagged accounts

Flagged transactions become:

```text
PENDING_REVIEW
```

---

# Audit Logging

Every critical action should be logged:

```text
USER_LOGIN
PASSWORD_CHANGE
TRANSFER_CREATED
ACCOUNT_FROZEN
ADMIN_ACTION
```

---

# Redis Usage

## Use Cases

* OTP storage
* session cache
* rate limiting
* queue processing
* notification jobs

---

# Notifications System

## Channels

* Email
* SMS (mock)
* In-app notifications

Examples:

* Transfer completed
* Password changed
* Suspicious login detected

---

# DevOps Setup

## Docker Services

```yaml
services:
  api:
  postgres:
  redis:
  client:
  admin:
```

---

# Environment Variables

```env
DATABASE_URL=
JWT_SECRET=
REDIS_URL=
SMTP_HOST=
SMTP_USER=
SMTP_PASS=
```

---

# Suggested Folder Structure

```text
apps/
 ├── client-app/
 ├── admin-panel/
 └── api/

packages/
 ├── shared-types/
 ├── ui/
 └── utils/

infrastructure/
 ├── docker/
 └── nginx/
```

---

# Advanced Features

## Level 1

* Cards system
* Virtual cards
* QR payments

---

## Level 2

* Scheduled transfers
* Savings accounts
* Loans simulation

---

## Level 3

* Cryptocurrency wallets
* Real-time exchange rates
* AI fraud detection

---

# CI/CD Pipeline

Use:

* GitHub Actions

Pipeline:

```text
Lint
→ Test
→ Build
→ Dockerize
→ Deploy
```

---

# Testing

## Unit Tests

* services
* guards
* validators

## Integration Tests

* transfer flows
* auth flow
* transaction consistency

---

# API Documentation

Use:

* Swagger/OpenAPI

Available at:

```text
/ api/docs
```

---

# Performance Goals

* Response time < 200ms
* Atomic transactions
* Horizontal scalability
* Queue-based background jobs

---

# Future Improvements

* Kubernetes deployment
* Microservices architecture
* Kafka event streaming
* Multi-region support
* Real payment gateway integration

---

# Real Banking Workflows

## User Registration Flow

```text
1. User registers
2. Email verification sent
3. User verifies email
4. KYC pending
5. Admin reviews KYC
6. Account activated
```

---

## Login Flow

```text
1. User enters credentials
2. Password verified
3. Optional OTP verification
4. JWT access token issued
5. Refresh token stored securely
6. Session audit log created
```

---

## Money Transfer Flow

```text
1. Validate sender account
2. Validate receiver account
3. Check account status
4. Check balance
5. Create pending transaction
6. Open database transaction
7. Create ledger debit entry
8. Create ledger credit entry
9. Update balances atomically
10. Commit database transaction
11. Mark transaction completed
12. Send notifications
13. Create audit logs
```

---

# Admin System

## Admin Roles

```text
SUPER_ADMIN
ADMIN
SUPPORT_AGENT
FRAUD_ANALYST
AUDITOR
CUSTOMER
```

---

## Admin Features

### User Management

* Suspend users
* Freeze accounts
* Reset MFA
* Review KYC
* Change account status

### Fraud Monitoring

* Inspect suspicious transfers
* Review flagged activity
* Lock accounts temporarily
* Review failed login attempts

### Financial Monitoring

* Search transactions
* Review ledger entries
* Export reports
* View transfer analytics

### Audit System

* View all sensitive actions
* Track admin operations
* Monitor security events

---

# Transaction States

```text
PENDING
PROCESSING
COMPLETED
FAILED
REVERSED
PENDING_REVIEW
```

---

# Account States

```text
ACTIVE
FROZEN
SUSPENDED
CLOSED
PENDING_KYC
```

---

# API Modules

## Auth Module

Responsible for:

* registration
* login
* JWT
* refresh tokens
* MFA
* password reset

---

## Account Module

Responsible for:

* balances
* account creation
* account states
* IBAN generation

---

## Transaction Module

Responsible for:

* transfers
* validation
* settlements
* transaction history

---

## Ledger Module

Responsible for:

* immutable accounting records
* debit/credit entries
* reconciliation

---

## Fraud Module

Responsible for:

* anomaly detection
* suspicious activity rules
* risk scoring

---

## Notification Module

Responsible for:

* emails
* SMS mockups
* security alerts
* transaction alerts

---

## Audit Module

Responsible for:

* tracking actions
* security logs
* compliance records

---

# Recommended Repository Structure

```text
novabank/
├── apps/
│   ├── api/
│   ├── client-app/
│   └── admin-panel/
│
├── packages/
│   ├── shared-types/
│   ├── ui/
│   └── utils/
│
├── infrastructure/
│   ├── docker/
│   ├── nginx/
│   └── scripts/
│
├── docs/
├── prisma/
├── tests/
└── .github/
```

---

# Recommended Development Order

## Phase 1

* Setup monorepo
* Setup Docker
* Setup PostgreSQL
* Setup Redis
* Setup NestJS
* Setup Prisma

---

## Phase 2

* Authentication system
* JWT
* Refresh tokens
* RBAC
* Admin authentication

---

## Phase 3

* Account creation
* Balance management
* Ledger system
* Transaction engine

---

## Phase 4

* Admin dashboard
* Fraud monitoring
* Audit logs
* Notifications

---

## Phase 5

* Docker deployment
* CI/CD
* Rate limiting
* Security hardening
* Integration testing

---

# GitHub README Summary

```md
NovaBank is a modern digital banking platform built with NestJS, PostgreSQL, Redis, and Next.js.

Features:
- Secure authentication
- Double-entry accounting
- Transaction processing
- Admin dashboard
- Fraud detection
- Audit logs
- Dockerized infrastructure
```
