# AdventureWorks Sales — Power BI Dashboard
<img width="1370" height="728" alt="Screenshot 2025-09-05 104042" src="https://github.com/user-attachments/assets/d33995fb-ef63-4cfb-804c-3e15d358411c" />
A clean, ready‑t<img width="1370" height="732" alt="Screenshot 2025-09-05 104108" src="https://github.com/user-attachments/assets/078e21bd-1fc5-4e2c-85bd-5c1c26344aad" />
<img width="1375" height="717" alt="Screenshot 2025-09-05 104131" src="https://github.com/user-attachments/assets/4b370624-bf18-4f8d-9d61-5ea0afaa941b" />

o‑refresh Power BI dashboard for analyzing **AdventureWorks** sales performance. This README explains the files, how to open/refresh the report, common DAX measures, and tips for customization.

---

## 📦 Project Contents

```
.
├─ advanture.pbix                 # Power BI Desktop report (note the filename)
├─ AdventureWorks Sales.xlsx      # Excel data source
└─ README.md                      # This file
```

> If your `.pbix` expects a different folder location for the Excel file, update the data source path using **Transform Data → Data source settings** (steps below).

---

## 🚀 Quick Start

1. Install **Power BI Desktop** (latest).
2. Download/clon­­e this folder so that `advanture.pbix` and `AdventureWorks Sales.xlsx` are in the **same directory**.
3. Open `advanture.pbix` in Power BI Desktop.
4. If prompted to apply changes, click **Apply**.
5. Refresh data: **Home → Refresh**.

> **No gateway** is needed for local Excel refreshes in Desktop. If publishing to the Service, see the **Publishing & Scheduled Refresh** section.

---

## 🔁 Update / Fix Data Source Path

If Power BI can’t find the Excel file:

1. **Home → Transform data → Data source settings**.  
2. Select the **Excel file** and click **Change Source**.  
3. Browse to `AdventureWorks Sales.xlsx` in this project folder.  
4. **Close** → **Apply**.  
5. **Home → Refresh**.

---

## 📊 Report Pages (suggested layout)

- **Executive Summary** — Top KPIs, YoY growth, trend line for Total Sales, map of Sales by Region.
- **Sales Overview** — Sales by Date/Product/Channel with slicers (Year, Region, Category).
- **Product Performance** — Top/Bottom products by Sales, Margin, Units; decomposition or waterfall for drivers.
- **Customer Segments** — Sales by Customer, Segment, and RFM-style indicators.
- **Geography** — Sales by Country/State/City with map visual; tooltip details.
- **Time Intelligence** — Month‑over‑Month and Year‑over‑Year analytics; seasonality patterns.

> If your `.pbix` already contains different pages, keep those and use this as a guide for enhancements.

---

## 🧮 Core KPIs (examples)

- **Total Sales (₹)**  
- **Total Orders**  
- **Total Quantity**  
- **Average Order Value (₹)**  
- **Gross Margin (₹ / %)**  
- **YoY Growth (%)**  
- **Customer Count / New vs. Returning**  

---

## 📐 Example DAX Measures

> Update table/column names to match your model.

```DAX
-- Base Measures
Total Sales := SUMX ( 'Sales', 'Sales'[Quantity] * 'Sales'[Unit Price] )

Total Cost := SUMX ( 'Sales', 'Sales'[Quantity] * 'Sales'[Unit Cost] )

Gross Margin ₹ := [Total Sales] - [Total Cost]

Gross Margin % := DIVIDE ( [Gross Margin ₹], [Total Sales] )

Total Orders := DISTINCTCOUNT ( 'Sales'[OrderID] )

Total Qty := SUM ( 'Sales'[Quantity] )

Average Order Value ₹ := DIVIDE ( [Total Sales], [Total Orders] )

-- Time Intelligence (requires a marked Date table with continuous dates)
Sales PY := CALCULATE ( [Total Sales], DATEADD ( 'Date'[Date], -1, YEAR ) )

YoY Sales Growth % := DIVIDE ( [Total Sales] - [Sales PY], [Sales PY] )

MoM Sales Growth % := 
VAR Sales_MoM = CALCULATE ( [Total Sales], DATEADD ( 'Date'[Date], -1, MONTH ) )
RETURN DIVIDE ( [Total Sales] - Sales_MoM, Sales_MoM )
```

---

## 🗺️ Suggested Data Model

- **Fact**: `Sales` (OrderID, Date, ProductKey, CustomerKey, TerritoryKey, Quantity, Unit Price, Unit Cost, Discount, etc.)
- **Dimensions**:  
  - `Date` (a continuous calendar; mark as **Date table**)  
  - `Product` (Category, Subcategory, Product Name, SKU)  
  - `Customer` (Customer Name, Segment, Geography keys)  
  - `Territory/Geography` (Country, State, City)  

**Relationships**: One‑to‑many from each dimension to `Sales` via keys. Use **single direction** filters from dimensions → fact by default.

---

## 🎛️ Slicers to Include

- Year / Quarter / Month
- Region / Country
- Category / Subcategory
- Channel
- Customer Segment

---

## 🧰 Publishing & Scheduled Refresh (optional)

1. **Home → Publish** to your workspace in Power BI Service.  
2. In the Service, upload the **Excel** to the same workspace **or** store it in OneDrive/SharePoint and repoint the dataset to that file.  
3. Configure **Scheduled Refresh** (dataset settings) and map credentials for the Excel connector.  
4. If using an on‑prem file path, configure a **Gateway** and ensure the path is reachable by the gateway.

---

## ⚡ Performance Tips

- Prefer **import mode** for Excel sources; reduce columns and rows you don’t need.
- Use a dedicated **Date** table; avoid building time-intel on the fly.
- Replace high‑cardinality text columns in visuals with aggregated measures where possible.
- Turn off **auto date/time** (File → Options → Data Load) if you have a proper Date table.
- Use **Field Parameters** to swap dimensions in a single page instead of duplicating visuals.
- Limit heavy table visuals; prefer charts and KPI cards.

---

## 🧪 QA Checklist Before Sharing

- [ ] Data source path resolves without error  
- [ ] All visuals show data and tooltips are correct  
- [ ] Slicers and cross‑filtering behave as intended  
- [ ] Date table is marked and measures validate (YoY/MoM)  
- [ ] Currency and number formatting (₹, decimals)  
- [ ] Page titles, bookmarks, and navigation buttons work  
- [ ] Performance is acceptable on a full refresh  

---

## 🛠️ Troubleshooting

**Problem:** _“We couldn’t find the file” on refresh._  
**Fix:** Use **Data source settings → Change Source** to point to the correct `AdventureWorks Sales.xlsx` path; then **Refresh**.

**Problem:** Time‑intelligence measures return blank.  
**Fix:** Ensure a continuous **Date** table; mark it as Date; connect to the fact via a single active relationship.

**Problem:** Wrong currency symbol/format.  
**Fix:** Format your measures (Model view) and set the **₹** symbol and decimals as required.

---

## 📄 License

This project is provided as‑is for educational/analytical use. Add your organization’s license if redistributing.

---

## 🙋 Support / Contact

- Owner: Kaunsalye Pratik 
- Email: pratikkaunsalye@gmail.com

