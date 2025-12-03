# Xero, Notion, Google Sheets & GPT Integration Project

This project showcases an end-to-end automation framework that integrates **Xero**, **Notion**, **Google Sheets**, and **GPT (via Notion's AI Actions)** to streamline financial tracking and reporting. The system automatically syncs income and expenses from Xero into Notion, updates calculated profit figures, and enables natural language querying of expense data using custom GPTs.

Images:
![Expense-v2](./Expense_v2.png)
![For-Updating-Expenses-in-Master-Table-v12.png](./For-Updating-Expenses-in-Master-Table-v12.png)
![For-Updating-income-in-Master-Table-v2.png](./For-Updating-income-in-Master-Table-v2.png)
![GS-Income-Insertion-v4.png](./GS-Income-Insertion-v4.png)
![Income-v14.png](./Income-v14.png)
![Zap-Expense-Insertion-v4.png](./Zap-Expense-Insertion-v4.png)

---

## 🔧 Project Scope

- Automatically track **payments received and made** via Xero and log them in Notion.
- Maintain centralized **Income**, **Expense**, and **Profit** tables in Notion.
- Sync entries from Notion into **Google Sheets** for reporting.
- Build custom **GPT Assistants** for querying Notion data intelligently (property-wise filters, summaries, etc.).

---

## 🗃️ Notion Structure

The following Notion databases were created:

1. **Property Directory**
2. **Income Table**
3. **Expense Table**
4. **Master Table**
5. **Master Table Gallery** (view-only)

### 🧾 Income Table Fields
| Field | Type |
|-------|------|
| Property Code | Default |
| Property Name | Text |
| Tenant | Text |
| Date | Date |
| Transaction Type | Text |
| Income | Number |
| This Month Income | Formula: `if(formatDate(Date, "YYYY-MM") == formatDate(now(), "YYYY-MM"), Income, 0)` |

### 💸 Expense Table Fields
| Field | Type |
|-------|------|
| Property Code | Default |
| Property Name | Text |
| Date | Date |
| Transaction | Text |
| Expense | Number |
| This Month Expense | Formula: `if(formatDate(Date, "YYYY-MM") == formatDate(now(), "YYYY-MM"), Expense, 0)` |

### 📊 Master Table Fields
| Field | Type |
|-------|------|
| Code | Default |
| Property Code | Relation (to Property Directory, hidden) |
| Property Name | Rollup |
| This Month Income | Number (synced via Google Sheets) |
| This Month Expense | Number (synced via Google Sheets) |
| Profit | Formula: `This Month Income - This Month Expense` |

### 🖼️ Master Table Gallery View
A visual layout of key fields:
- Code
- Property Name
- Income
- Expense
- Profit

---

## 📊 Google Sheets Setup

### Tabs Created
1. **Income Log**
2. **Expense Log**
3. **Sheet4** – Monthly Aggregates
4. **Sheet2** – Expense Summary

### Example Formulas

- `Month-Year` column (auto-format):  
  ```excel
  =ARRAYFORMULA(IF(C2:C="", "", TEXT(C2:C, "yyyy-mm")))
Total by Month and Property (SUMIFS):

=SUMIFS('Expense Log'!E:E, 'Expense Log'!A:A, A2, 'Expense Log'!G:G, B2)

🔁 Zapier Integration (6 Zaps)
1. New Payment Receivable → Notion (Income Table)
Trigger: Xero → New Reconciled Payment (Receivable)

Actions:

Notion: Find/Create Property in Property Directory

Notion: Create Income Table entry

2. New Payment Payable → Notion (Expense Table)
Trigger: Xero → New Reconciled Payment (Payable)

Actions:

Notion: Find/Create Property in Property Directory

Notion: Create Expense Table entry

3. New Income Row in Notion → Insert into Google Sheet
Trigger: Notion → New Database Item (Income Table)

Actions:

Formatter by Zapier → Format Date

Google Sheets → Insert Row in Income Log

4. New Expense Row in Notion → Insert into Google Sheet
Same steps as Income flow above.

5. Google Sheet Row (Income) Updated → Update Notion Master Table
Trigger: Google Sheets → New/Updated Row in Sheet4

Actions:

Notion: Find Master Table Item by Code

Notion: Update "This Month Income"

6. Google Sheet Row (Expense) Updated → Update Notion Master Table
Same as above, but updates "This Month Expense"

🤖 GPT Integration with Notion (AI Actions)
Step-by-Step Setup
Go to Notion Integrations

Create a new internal integration → grant access to Expense Table

Copy the API key and use it in Notion GPT Actions

Sample GPT Setup
Field	Value
Name	Notion Expense Table Assistant
Description	Fetch, query, track, and summarize property-related expenses
Authentication	API Key
Table ID	2142-------------

Assistant Capabilities
Filter by property code, transaction, or date

Summarize total expense per property

Sort by transaction amount, date, or name

Enforce result limits via pagination

Avoid assumptions on missing filters or overloading queries

🚀 Technologies Used
Xero API

Notion API

Zapier Automation

Google Sheets (Formulas & API)

OpenAI GPT (via Notion AI Assistant)

📈 Outcomes & Use Cases
Real-time synchronization between accounting and property management platforms.

Simplified monthly reporting using automated aggregations.

GPT-powered financial queries for property managers or investors.

Easy plug-and-play solution for other clients with similar needs.

📬 Want a Similar Setup?
This project was designed to demonstrate expert-level automation across multiple platforms. If you’d like to implement something similar for your business—reach out!
