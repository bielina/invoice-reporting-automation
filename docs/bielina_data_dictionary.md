# bielina / Project 01 — Data Dictionary

**Dataset:** 12,000 synthetic invoices for a fictional Polish company. **Period:** 2025. **Currency:** PLN. No actual PKP or supplier data.

## Columns

| Field | Type | Meaning / validation |
|---|---|---|
| `Invoice_ID` | Text | Invoice identifier; expected unique; Duplicate IDs intentionally included |
| `Invoice_Date` | ISO date text | Invoice issue date, YYYY-MM-DD; All valid dates |
| `Due_Date` | ISO date text | Payment deadline, YYYY-MM-DD; Some dates before invoice date |
| `Supplier` | Text | Fictional vendor; Some blanks |
| `Department` | Category | Internal cost-owning department; Seven categories |
| `Region` | Category | Polish voivodeship (ASCII spelling); Some blanks and invalid names |
| `Cost_Center` | Text | Fictional internal cost center; Some blanks |
| `Net_Amount_PLN` | Decimal | Net invoice amount in PLN; Some negative or zero values |
| `VAT_Rate` | Decimal | VAT fraction: 0, 0.08, 0.23; Rates simplified for synthetic scenario |
| `VAT_Amount_PLN` | Decimal | Expected round(Net_Amount_PLN * VAT_Rate, 2); Some mismatches |
| `Gross_Amount_PLN` | Decimal | Expected round(Net_Amount_PLN + VAT_Amount_PLN, 2); Some mismatches |
| `Payment_Status` | Category | Paid / Pending / Overdue / Cancelled; Some invalid Unknown values |

## Deliberate error scenarios

| Scenario | Records |
|---|---:|
| `duplicate_invoice_id` | 24 |
| `missing_supplier` | 35 |
| `missing_region` | 30 |
| `invalid_region` | 20 |
| `negative_net` | 20 |
| `zero_net` | 12 |
| `vat_mismatch` | 30 |
| `gross_mismatch` | 30 |
| `due_before_invoice` | 25 |
| `invalid_status` | 20 |
| `missing_cost_center` | 25 |

**Total:** 271 deliberately modified records (one injected scenario per selected row). The original ID column in the ground-truth file records identifiers before modification; duplicate-ID cases should be detected by checking the actual dataset. Some error types may trigger additional secondary validation failures.

## Business rules

- Invoice IDs should be unique and nonempty.
- Supplier, Region and Cost_Center should not be blank.
- Region must match one of the 16 supported voivodeships.
- Net amount should be greater than zero.
- VAT amount should equal `ROUND(Net_Amount_PLN * VAT_Rate, 2)` (tolerance 0.01 PLN).
- Gross amount should equal `ROUND(Net_Amount_PLN + VAT_Amount_PLN, 2)` (tolerance 0.01 PLN).
- Due date must not precede invoice date.
- Payment status must be Paid, Pending, Overdue or Cancelled.

## Reproducibility

- Deterministic random seed: `20261118`.
- Dates are ISO strings (`YYYY-MM-DD`) to avoid Excel locale confusion.
- CSV is UTF-8 with BOM for Excel compatibility; XLSX stores matching values.
- Suppliers, invoice IDs and cost centers are fictional.
- VAT is simplified for a portfolio exercise and should not be used for tax compliance.
