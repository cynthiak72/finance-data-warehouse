# Fact Table Grain Definitions

## 1. Purpose

Grain defines exactly what one row in a fact table represents.

The grain is established before the physical fact table is designed to ensure that measurements are stored consistently and to prevent double counting during analytical queries.

For this project, the grain of each business process has been explicitly defined before implementation.

---

## 2. General Ledger

### Business Process

General Ledger transactions.

### Grain

**One row represents one posted general ledger transaction line.**

Each transaction line is associated with a specific date, account and organisational context.

### Example

A GL transaction line may represent:

* Date: 15 January 2026
* Account: Office Rent
* Department: Finance
* Transaction Amount: KES 100,000

### Measures

* Debit Amount
* Credit Amount
* Transaction Amount

### Dimensions

Potential dimensions include:

* Date
* Account
* Department
* Vendor
* Customer
* Company

---

## 3. Budget

### Business Process

Budget allocation.

### Grain

**One row represents one budget allocation for a specific account, department and reporting period.**

The reporting period will support monthly analysis while allowing aggregation to quarter and year.

### Example

A budget record may represent:

* Year: 2026
* Month: January
* Account: Office Rent
* Department: Finance
* Budget Amount: KES 100,000

### Measure

* Budget Amount

### Dimensions

Potential dimensions include:

* Date
* Account
* Department
* Company

---

## 4. Procurement

### Business Process

Procurement / Purchasing.

### Grain

**One row represents one purchase transaction line.**

A purchase invoice containing multiple products or services will therefore generate multiple fact rows.

### Example

An invoice containing:

* 5 laptops
* 5 monitors
* 10 keyboards

will generate three purchase fact rows when each item represents a separate transaction line.

### Measures

* Purchase Amount
* Quantity

### Dimensions

Potential dimensions include:

* Date
* Vendor
* Product
* Department
* Account
* Company

---

## 5. Sales

### Business Process

Sales / Revenue generation.

### Grain

**One row represents one sales transaction line.**

An invoice containing multiple products or services will generate separate fact rows for each transaction line.

### Example

An invoice containing:

* Product A: 5 units
* Product B: 2 units
* Product A: 10 units

will be represented by three sales fact rows.

This preserves transaction-level detail and allows analysis of product quantities and revenue.

### Measures

* Sales Amount
* Quantity

### Dimensions

Potential dimensions include:

* Date
* Customer
* Product
* Department
* Company

---

## 6. Grain Design Principle

The fact tables will retain transaction-level detail wherever the business process provides transaction-level information.

Aggregations such as total revenue, total expenditure and total quantity will be calculated during analytical queries rather than stored as pre-aggregated transaction records.

This approach preserves analytical flexibility and reduces the risk of double counting.
