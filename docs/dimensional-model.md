# Dimensional Model

## 1. Overview

The warehouse follows a Kimball-style dimensional modelling approach.

The model separates business processes into individual fact tables while using shared, conformed dimensions to provide consistent analytical context across the organisation.

The design is intended to support financial reporting, operational analysis and Power BI analytics.

---

## 2. Business Processes

The initial dimensional model covers four primary business processes:

1. General Ledger
2. Budgeting
3. Procurement
4. Sales / Revenue

Each business process is represented by a separate fact table because each process has a different grain and represents a different type of business event.

---

## 3. Fact Tables

### 3.1 FactGL

**Business Process:** General Ledger

**Grain:** One row represents one posted general ledger transaction line.

**Primary measures:**

* Debit Amount
* Credit Amount
* Transaction Amount

**Foreign keys:**

* DateKey
* AccountKey
* DepartmentKey
* VendorKey
* CustomerKey
* CompanyKey

**Additional attributes:**

* Transaction ID
* Description

---

### 3.2 FactBudget

**Business Process:** Budgeting

**Grain:** One row represents one budget allocation for a specific account, department and reporting period.

**Primary measure:**

* Budget Amount

**Foreign keys:**

* DateKey
* AccountKey
* DepartmentKey
* CompanyKey

---

### 3.3 FactPurchase

**Business Process:** Procurement / Purchasing

**Grain:** One row represents one purchase transaction line.

A purchase invoice containing multiple products or services is represented by multiple fact rows.

**Primary measures:**

* Purchase Amount
* Quantity

**Foreign keys:**

* DateKey
* VendorKey
* ProductKey
* DepartmentKey
* AccountKey
* CompanyKey

---

### 3.4 FactSales

**Business Process:** Sales / Revenue

**Grain:** One row represents one sales transaction line.

A sales invoice containing multiple products or services is represented by separate fact rows for each transaction line.

**Primary measures:**

* Sales Amount
* Quantity

**Foreign keys:**

* DateKey
* CustomerKey
* ProductKey
* DepartmentKey
* AccountKey
* CompanyKey

### Accounting Assumption

For this project, each sales transaction line is assumed to be associated with a revenue account.

This allows sales to be analysed by the corresponding revenue account.

---

# 4. Dimension Tables

## 4.1 DimDate

Provides the time context for all business processes.

### Key attributes

* DateKey
* Date
* Day
* Month
* Month Name
* Quarter
* Year

DimDate is a conformed dimension shared across multiple fact tables.

---

## 4.2 DimAccount

Provides financial account context.

### Key attributes

* AccountKey
* AccountCode
* AccountName
* AccountType
* AccountCategory

Examples of account types include:

* Revenue
* Expense
* Asset
* Liability
* Equity

DimAccount is shared across General Ledger, Budget, Sales and Procurement where applicable.

---

## 4.3 DimCustomer

Provides customer context for customer-related transactions.

### Key attributes

* CustomerKey
* CustomerID
* CustomerName
* CustomerType
* City
* County
* Country
* Industry
* RegistrationDate

CustomerKey will be the warehouse surrogate key, while CustomerID will be retained as the source/business key.

The dimension will support historical tracking using Slowly Changing Dimension techniques where required.

---

## 4.4 DimProduct

Provides product or service context.

### Potential attributes

* ProductKey
* ProductID
* ProductName
* ProductCategory
* ProductType

The dimension supports analysis of sales and procurement at product level.

---

## 4.5 DimVendor

Provides supplier context for procurement and vendor-related financial transactions.

### Potential attributes

* VendorKey
* VendorID
* VendorName
* VendorType
* City
* County
* Country

---

## 4.6 DimDepartment

Provides organisational context.

### Key attributes

* DepartmentKey
* DepartmentCode
* DepartmentName

Department will be implemented as a reusable conformed dimension across relevant fact tables.

Selected organisational attributes may use SCD Type 2 to preserve historical changes.

---

## 4.7 DimCompany

Provides legal entity or company context where multiple entities are represented.

### Key attributes

* CompanyKey
* CompanyCode
* CompanyName

This allows financial information to be analysed by company where applicable.

---

# 5. Star Schema Relationships

The model follows a star schema design in which fact tables sit at the centre and connect directly to descriptive dimensions.

Conceptually:

```text
                         DimDate
                            |
                            |
                     +-------------+
                     |   FactGL    |
                     +-------------+
                      /    |    \
                     /     |     \
              DimAccount   |   DimDepartment
                           |
                      DimCustomer
                      DimVendor
                      DimCompany
```

The other business processes follow the same dimensional modelling pattern.

```text
                         DimDate
                            |
                            |
                     +-------------+
                     | FactSales   |
                     +-------------+
                    /      |       \
                   /       |        \
           DimCustomer DimProduct DimDepartment
                         |
                    DimAccount
                         |
                     DimCompany
```

```text
                         DimDate
                            |
                            |
                     +-------------+
                     | FactPurchase|
                     +-------------+
                    /      |       \
                   /       |        \
              DimVendor DimProduct DimDepartment
                         |
                    DimAccount
                         |
                     DimCompany
```

```text
                         DimDate
                            |
                            |
                     +-------------+
                     | FactBudget  |
                     +-------------+
                       /    |     \
                      /     |      \
              DimAccount DimDepartment DimCompany
```

---

# 6. Conformed Dimensions

Several dimensions are designed to be shared across multiple fact tables.

### DimDate

Shared across:

* FactGL
* FactBudget
* FactPurchase
* FactSales

### DimAccount

Shared across:

* FactGL
* FactBudget
* FactPurchase
* FactSales

where an accounting account is available for the business process.

### DimDepartment

Shared across relevant financial and operational processes.

### DimCompany

Shared across business processes where multiple legal entities are represented.

Using conformed dimensions ensures that the same business definitions are used when comparing different processes.

---

# 7. Fact vs Dimension Design Principles

The model follows these principles:

### Fact tables

Fact tables represent measurable business events at a defined grain.

They contain:

* Foreign keys to dimensions
* Transaction-level identifiers where appropriate
* Numeric measures
* Additional event-level attributes where required

### Dimension tables

Dimensions provide descriptive context for analysing business events.

They contain attributes such as:

* Names
* Categories
* Types
* Locations
* Organisational classifications

---

# 8. Aggregation Principle

The fact tables retain detailed transaction-level information rather than storing pre-aggregated totals.

For example:

```text
SalesAmount
    ↓
SUM(SalesAmount)
    ↓
Total Revenue
```

This preserves flexibility for analysis at different levels of detail.

Aggregations will primarily be performed in the analytical/semantic layer.

---

# 9. Historical Data Strategy

Selected dimensions will use Slowly Changing Dimension Type 2.

For example, if a customer changes location:

```text
CustomerKey | CustomerID | CustomerName | City
------------|------------|--------------|---------
101         | C001       | ABC Ltd      | Nairobi
205         | C001       | ABC Ltd      | Mombasa
```

The source/business key remains the same while a new surrogate key represents the new historical version.

This allows historical transactions to remain associated with the dimensional attributes that were valid at the time of the transaction.

---

# 10. Design Rationale

The dimensional model was selected because the primary consumption layer is analytical reporting through Power BI.

The star schema provides:

* Simple analytical queries
* Clear business definitions
* Reusable dimensions
* Consistent filtering across fact tables
* Efficient aggregation
* A structure that is intuitive for BI users

Separate fact tables are maintained for different business processes because General Ledger, Budgeting, Procurement and Sales have different grains and represent different business events.

---

# 11. Important Modelling Considerations

The final physical model will be refined during implementation based on the characteristics of the source data.

Specific decisions to validate include:

* Whether all GL transactions have customer or vendor references
* Whether departments apply to every sales and purchase transaction
* How sales transactions map to revenue accounts
* Whether budget data is available at monthly or annual grain
* Whether products represent physical goods, services or both
* Which dimensions require SCD Type 2
* How unknown or missing dimension members will be handled

These assumptions will be validated before the physical warehouse is implemented.
