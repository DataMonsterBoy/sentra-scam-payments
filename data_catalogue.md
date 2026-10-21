# Sentra Bank Ghana — Data Catalogue

This catalogue describes the payments and scam-report data extracted for your project. The data covers **outgoing payments from 1 January 2025 to 30 June 2026**, and scam reports received up to the date of the extract.

All amounts are in **Ghana cedis (GHS)**. All times are **Accra time** (GMT).

---

## The files at a glance

| File | One row is… | Rows (approx.) | What it's for |
|---|---|---|---|
| `customers.csv` | a customer | 10,000 | Who our customers are |
| `accounts.csv` | a bank account | 15,000 | The accounts customers hold with us |
| `payees.csv` | a saved payee on an account | 220,000 | Who customers send money to |
| `payments.csv` | an outgoing payment | 1,170,000 | Every payment customers made or attempted |
| `sessions.csv` | a login to app or internet banking | 900,000 | How and when customers logged in |
| `fraud_reports.csv` | a scam report | 730 | Scams reported to the bank |
| `report_payments.csv` | a link between a report and a payment | 1,050 | Which payments a report is about |

## How the files connect

```
customers ──< accounts ──< payees
                 │            │
                 └──< payments >┘
                        │   │
           sessions >───┘   └───< report_payments >─── fraud_reports >── customers
```

| To get from… | …to | Join on |
|---|---|---|
| customers | accounts | `customer_id` |
| accounts | payments | `account_id` |
| accounts | payees | `account_id` |
| payments | payees | `payee_id` |
| payments | sessions | `session_id` |
| payments | fraud_reports | `payment_id` → `report_payments` → `report_id` |
| fraud_reports | customers | `customer_id` |

A customer can have several accounts. An account can have many payees and many payments. A scam report can relate to several payments.

---

## `customers.csv`

One row per customer of Sentra Bank Ghana.

| Column | What it means | Example |
|---|---|---|
| `customer_id` | Unique customer identifier | `C000123` |
| `full_name` | Customer's name as held by the bank | `Akosua Mensah` |
| `gender` | `M` or `F` | `F` |
| `date_of_birth` | Date of birth | `1984-05-13` |
| `region` | Region of Ghana where the customer lives (one of 16) | `Greater Accra` |
| `customer_since` | Date the customer joined the bank | `2021-09-04` |
| `segment` | Customer segment used by the bank: `mass`, `mass_affluent`, `affluent`, `small_business`, `internal` | `mass` |
| `employment_type` | `salaried`, `self_employed`, `student`, `retired`, `unemployed`, `staff` | `salaried` |

## `accounts.csv`

One row per account. Every customer has a current account; some also have savings or business accounts.

| Column | What it means | Example |
|---|---|---|
| `account_id` | Unique account identifier | `A0001056` |
| `customer_id` | Customer who owns the account | `C000123` |
| `account_type` | `current`, `savings` or `business_current` | `current` |
| `opened_at` | Date the account was opened | `2021-09-04` |
| `status` | `active` or `dormant` | `active` |

## `payees.csv`

One row per payee saved on an account. A payee is someone a customer sends money to: a person, a business, or their own mobile money wallet. If two customers pay the same person, each customer has their own payee row.

| Column | What it means | Example |
|---|---|---|
| `payee_id` | Unique payee identifier | `PY0028328` |
| `account_id` | Account the payee is saved on | `A0001056` |
| `payee_name` | Name the **customer typed** when saving the payee. This may be a nickname. | `Auntie Ama` |
| `payee_rail` | Where the money goes: `bank` (a bank account) or `mobile_money` (a MoMo wallet) | `mobile_money` |
| `payee_network` | Which bank or MoMo network receives the money. MoMo: `MTN MoMo`, `Telecel Cash`, `AT Money`. Banks: `Sentra Bank Ghana` or another bank's code (`GH-BK-01` to `GH-BK-22`). | `MTN MoMo` |
| `payee_identifier` | Masked account number or phone number, as shown on screen | `055****954` |
| `payee_account_ref` | A hashed reference for the receiving account. **The same receiving account has the same reference everywhere**, even when different customers pay it. | `ea5b836f7b66` |
| `created_at` | When the payee was saved | `2025-02-05 18:43:13` |

## `payments.csv`

One row per outgoing payment, including payments that were cancelled or failed.

| Column | What it means | Example |
|---|---|---|
| `payment_id` | Unique payment identifier | `P00192180` |
| `account_id` | Account the money was sent from | `A0001056` |
| `payee_id` | Payee the money was sent to | `PY0028328` |
| `created_at` | When the payment was made | `2026-05-20 15:38:49` |
| `amount_ghs` | Payment amount in cedis | `510.00` |
| `channel` | How the customer made the payment: `app` (mobile app), `ussd` (dial code, e.g. on a basic phone), `internet` (web banking), `branch` (in person) | `app` |
| `reference` | Free-text reference the customer typed. Often blank. | `school fees` |
| `name_check_result` | Before a payment, the bank checks the payee name the customer typed against the name registered on the receiving account. `match`, `partial` (some of the name matches), `no_match`, or `unregistered` (the receiving wallet has no registered name). | `match` |
| `warning_shown` | `True` if the customer was shown a scam warning screen before confirming | `False` |
| `stated_purpose` | The purpose the customer selected from a drop-down when asked: `goods_and_services`, `family_and_friends`, `investment`, `bills_and_rent`, `other`. Blank when the customer wasn't asked. | `family_and_friends` |
| `status` | `completed`, `cancelled` (customer stopped the payment) or `failed` | `completed` |
| `review_outcome` | Result of the fraud operations team's review, which happens a few days after the payment: `cleared`, `customer_contacted` or `confirmed_scam`. Blank if the payment wasn't reviewed. | `cleared` |
| `session_id` | The login session the payment was made in, if one was recorded | `S00147548` |

## `sessions.csv`

One row per login to the mobile app or internet banking.

| Column | What it means | Example |
|---|---|---|
| `session_id` | Unique session identifier | `S00147548` |
| `customer_id` | Customer who logged in | `C000123` |
| `started_at` | When the session started | `2026-05-20 15:31:02` |
| `device_id` | Identifier for the device used | `D0021741` |
| `device_type` | `android`, `iphone` or `web` (a browser) | `android` |
| `is_new_device` | `True` if this is the first time the customer used this device | `False` |
| `duration_s` | Length of the session in seconds | `480` |

## `fraud_reports.csv`

One row per scam report, from the start of the period up to the date of the extract. Most come from customers; some are raised by the bank's own fraud team.

| Column | What it means | Example |
|---|---|---|
| `report_id` | Unique report identifier | `R000020` |
| `customer_id` | Customer the report is about | `C001777` |
| `reported_at` | When the report was received | `2025-04-14 18:19:23` |
| `reported_via` | How it was reported: `call_centre`, `branch`, `app` or `internal_review` (raised by our own fraud team) | `call_centre` |
| `reported_by` | Who made the report: `customer`, `family_member`, `fraud_team` or `other` | `customer` |
| `reported_category` | Scam category recorded by the staff member taking the report. Free text, so it isn't always consistent, and it's sometimes blank. | `Purchase scam` |
| `claimed_amount_ghs` | Amount the customer says they lost | `290.00` |
| `notes` | Staff member's summary of what the customer said | `Customer paid GHS 290 for a TV advertised on Instagram…` |
| `case_status` | Outcome of the bank's investigation: `open`, `closed_confirmed` (confirmed as a scam), `closed_rejected` (not a scam) or `closed_insufficient_info` | `closed_confirmed` |
| `amount_recovered_ghs` | Money the bank recovered for the customer | `279.46` |
| `closed_at` | When the investigation was closed. Blank if still open. | `2025-05-08 10:25:39` |

## `report_payments.csv`

Links scam reports to the payments they're about. One report can be linked to several payments.

| Column | What it means | Example |
|---|---|---|
| `report_id` | The report | `R000178` |
| `payment_id` | A payment the report relates to | `P00047029` |

---

## Glossary

| Term | Meaning |
|---|---|
| **APP fraud** | Authorised push payment fraud: the customer is tricked into sending the money themselves. |
| **MoMo** | Mobile money: a wallet linked to a phone number. |
| **USSD** | Banking by dialling a short code (e.g. `*xxx#`) on any phone, including basic phones without internet. |
| **Payee** | Someone a customer has saved so they can send them money. |
| **Susu** | An informal savings scheme. Members pay regular contributions to a collector or group. |
