# CareBridge — Full-stack DBMS coursework prototype

**Stack:** HTML/CSS/JavaScript frontend · Node.js + Express backend · PostgreSQL database.

This is a local demo using fictional seed data. It is not production-ready software and must not be used for real patient information or payments.

## Prerequisites
- Node.js 18+ (20+ recommended)
- Docker Desktop with Docker Compose

## Run on Windows PowerShell
1. Extract `CareBridge_Prototype.zip` and open PowerShell in the extracted folder.
2. Start PostgreSQL: `docker compose up -d`
3. Install backend packages: `npm install`
4. Start the server: `npm start`
5. Open `http://localhost:3000`
6. Optional health check: `http://localhost:3000/api/health`

The first database start executes `db/schema.sql` and `db/seed.sql` automatically. To reset all demo data, run `docker compose down -v` followed by `docker compose up -d`.

## Demo workflow
1. Choose **Patient portal** and request an appointment. Demo patient number: `CB-P-0001`.
2. Sign out and choose **Reception desk**. Confirm or reject the appointment; create an admission and allocate a bed.
3. Choose **Billing workspace**. Create an invoice linked to the appointment or admission ID, then record a payment. Each successful payment gets a receipt.
4. Return to the patient portal to see that patient's appointments, invoices and receipts.

The role picker is intentionally a **demo-login shortcut** for classroom exploration, not real authentication. It grants the selected demo role. Do not deploy this prototype publicly. Before any real deployment, replace demo login with proper authentication and account provisioning; configure secure cookies, CSRF protection, rate limiting, HTTPS, secret management, backups and a security review.

## Database design features
- Normalized tables for users, patients, doctors, departments, appointments, admissions, rooms, beds, allocations, invoices, invoice lines, payments, receipts and audit logs.
- Primary keys, foreign keys, `UNIQUE`, `CHECK` constraints and indexes.
- PostgreSQL exclusion constraints prevent overlapping active appointments for a doctor and overlapping active allocations for a bed (`btree_gist` extension).
- Transactions for admission + bed allocation and payment + receipt creation.
- Backend role checks and patient-specific filtering for their own records.
- Partial invoice payments with balance validation while locking the invoice row.

## Known prototype limitations
- Appointments are fixed at 30 minutes. Doctor availability is stored but not yet used to generate slots.
- Demo payments are records only; no payment gateway is connected.
- Some features from the PRD, such as staff account provisioning, full audit-log UI, discharge UI, room management UI and detailed schedule management, are not yet implemented.
- The session store is Express's in-memory development store. Use a persistent session store for any deployment.
