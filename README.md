This repository contains the **modular Python pipeline** used to generate the Sahamit Daily Stock Report.

The scripts handle data extraction, transformation, stock calculations, purchase order aggregation, and report generation.

This repository focuses on the **data processing logic behind the report**.

---

## Data Sources

### 1) Weekly CJ Stock Report
- File: `Sahamit Report ..-..-2025.xlsx`
- Sheet: `Sahamit Report`
- Content: weekly product master and store-level stock information
- Used fields:
  - CJ_Item
  - Division
  - Product Name
  - Category / Subcategory
  - Brand
  - Store Stock
  - Assortment metrics

### 2) Daily DC Stock Report
- File: `DC_End..-..-2025.xlsx`
- Sheet: `Sheet1`
- Content: remaining stock in each distribution center
- Used fields:
  - Material → mapped to `CJ_Item`
  - Plant (DC1, DC2, DC4)
  - Stock Qty
  - Stock Value

### 3) Sell-out Past 30 Days
- File: `sellout_past30D.xlsx`
- Content: daily sales transactions for the last 30 days
- Used fields:
  - Calendar Day
  - CJ_Item
  - Total_SellOut_Qty
  - Total_Sellout_Amt

### 4) Access Database Extract
- File: `data_from_access.xlsx`
- Sheets:
  - `Master_Product`
  - `Pending_All_Div`
- Content:
  - product master data
  - pending purchase orders across divisions
- Used fields:
  - CJ_Item
  - SHM_Item
  - Supplier Name
  - Order Qty
  - Unit
  - Delivery Date
  - DC_Name

### 5) Master Lead Time
- File: `Master_LeadTime.xlsx`
- Sheet: `All_Product`
- Content: supply chain ownership and lead time information
- Used fields:
  - CJ_Item
  - SHM_Item
  - OwnerSCM
  - Base Lead Time (Days)

---

## What the scripts do

1. **Load input datasets**
   - Read Excel files containing stock, sales, and purchase order data.

2. **Process CJ Stock data**
   - Clean product information.
   - Filter excluded divisions.

3. **Process DC stock**
   - Map plant codes to DC names.
   - Aggregate remaining stock by distribution center.

4. **Process sell-out data**
   - Clean date fields.
   - Aggregate sales for the last 30 and 7 days.

5. **Process pending purchase orders**
   - Convert order quantities to consistent units.
   - Aggregate PO quantities by item and DC.
   - Extract earliest delivery dates.

6. **Process master lead time**
   - Attach supply chain ownership.
   - Add product lead time.

7. **Merge all datasets**
   - Combine stock, sales, and PO information into a unified dataset.

8. **Calculate supply chain metrics**
   - Remaining stock across DCs.
   - Sales averages (90D / 30D / 7D).
   - Sales distribution ratios by DC.

9. **Calculate DOH (Days on Hand)**
   - Current DOH per DC.
   - DOH across all DCs.
   - DOH including incoming purchase orders.

10. **Calculate stock cover dates**
   - Estimate the date when stock will run out.
   - Incorporate PO delivery dates.

11. **Generate final report dataset**
   - Clean columns.
   - Format date fields.
   - Remove empty rows.

12. **Export report**
   - Save final results as an Excel file.

---

## Output

The pipeline generates a final Excel report:

`Sahamit_Daily_Stock_Report_<date>.xlsx`
