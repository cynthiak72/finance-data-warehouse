# Business Requirements

## 1. Business Context

Amani Financial Services is a fictional financial services
organisation that requires a central analytical platform to support
financial reporting and management decision-making.

The organisation currently generates financial information from
multiple operational processes, including general ledger transactions,
sales, purchasing and budgeting.

Management requires a consistent and historical view of financial
performance across these business areas.

---

## 2. Business Objective

The objective is to design and implement a financial data warehouse
that provides reliable, historical and analysis-ready financial
information.

The warehouse will support management reporting and analysis of:

- Revenue
- Expenses
- Budget
- Actual financial performance
- Budget variance
- Customers
- Vendors
- Accounts
- Departments
- Financial performance over time

---

## 3. Key Business Questions

### Revenue

Management needs to answer:

- What is total revenue?
- How has revenue changed over time?
- Which departments generate the most revenue?
- Which customers generate the most revenue?
- Which accounts contribute most to revenue?

### Expenses

Management needs to answer:

- What are total expenses?
- Which departments incur the highest expenses?
- Which vendors receive the highest payments?
- How have expenses changed over time?

### Budget

Management needs to answer:

- What was the approved budget?
- What was actual expenditure?
- What is the budget variance?
- Which departments are over or under budget?

### Financial Performance

Management needs to answer:

- What is total revenue?
- What are total expenses?
- What is the net surplus or deficit?
- How does financial performance change over time?

---

## 4. Reporting Requirements

The analytical solution should support reporting by:

- Date
- Month
- Quarter
- Year
- Account
- Department
- Customer
- Vendor

Users should be able to filter and analyse financial information
across these dimensions.

---

## 5. Historical Reporting Requirements

The warehouse must preserve historical information for selected
business attributes.

For example, if a department changes its name or organisational
structure, historical transactions should remain associated with
the dimensional attributes that were valid at the time of the
transaction.

The project will therefore demonstrate Slowly Changing Dimension
techniques, including Type 1 and Type 2.

---

## 6. Data Integration Requirements

The warehouse will integrate information from multiple simulated
source systems.

The project will use synthetic data to represent:

- General ledger transactions
- Sales transactions
- Purchase transactions
- Budget records
- Customer information
- Vendor information
- Account information
- Department information

No confidential organisational data will be used.

---

## 7. Expected Outputs

The project will produce:

1. A dimensional data warehouse
2. A documented star schema
3. ETL/ELT processes
4. Slowly Changing Dimension implementation
5. Data quality and validation procedures
6. A Power BI semantic model
7. An executive financial dashboard
8. Technical and architectural documentation

---

## 8. Project Scope

### In Scope

- Financial transaction analysis
- Revenue analysis
- Expense analysis
- Budget vs actual analysis
- Customer analysis
- Vendor analysis
- Department analysis
- Historical dimensional data
- Dimensional data warehouse design
- Power BI reporting

### Out of Scope

- Actual banking transactions
- Real customer personal information
- Production financial systems
- Regulatory reporting
- Live organisational data
