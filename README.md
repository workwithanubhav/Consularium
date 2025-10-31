# 🏗️ DOMS IIT — AeroSports Supply Chain Analytics

**Repository Purpose:**  
This repository contains the full analytical workflow, simulation logic, and framework documentation used in the *AeroSports Supply Chain Case* — developed as part of **Consularium: The McKinsey Consulting Event**.  
Every chart and value in the final consulting deck can be traced back to this notebook.

---

## 1️⃣ Problem Context

AeroSports manages a **three-region, multi-echelon supply chain**:

- **Regions:** Asia, Europe, North America  
- **Network:** 3 DCs feeding 15 downstream warehouses  
- **Portfolio:** ~30 SKUs across Apparel, Shoes, and Accessories  
- **Timeline:** FY 2023–24 (12 months)  

Despite steady sales, the company faces:

- Excess **working capital** in A-class SKUs  
- **Air freight overuse** raising cost and carbon  
- **Lead time variability** driving high safety stock  
- **Service–cost imbalance** between regions  

**Business Question:**  
> “Where is the money locked, what drives logistics cost, and how can AeroSports unlock capital without hurting service?”

---

## 2️⃣ Repository Structure

```text
├── data/
│   ├── SKU_Wise_Data.xlsx          # SKU-level demand and sales
│   ├── DC_Inbound.xlsx             # Supplier→DC lanes
│   ├── DC_Outbound.xlsx            # DC→Warehouse lanes
│   ├── Network_Map.xlsx            # Network topology reference
│   └── Case_Study.pdf              # Problem statement and assumptions
│
├── notebooks/
│   └── AeroSports.ipynb            # Core 15-step analytical model
│
├── output/
│   ├── chart_p1_*                  # Page 1 – Diagnosis visuals
│   ├── chart_p2_*                  # Page 2 – Inventory & WC visuals
│   ├── chart_p3_*                  # Page 3 – Logistics Efficiency
│   ├── chart_p4_*                  # Page 4 – Mode-Shift Simulation
│   └── chart_p5_*                  # Page 5 – Strategic Roadmap
│
└── README.md                       # This file
```

## 3️⃣ Analytical Roadmap — 15 Steps

### **Step 1. Setup & Libraries**
- Import libraries: `pandas`, `numpy`, `matplotlib`, `scipy.stats`
- Define helper functions:
  - `calc_wape()` → Weighted Absolute Percentage Error  
  - `calc_cov()` → Coefficient of Variation  
  - `calc_ss()` → Safety Stock  
  - `calc_csl()` → Cycle Service Level (Φ(z))

---

### **Step 2. Data Load & Clean**
- Read all datasets and merge into a unified `SKU–Region–Month` table  
- Standardize column names, parse date columns, and handle missing/null values  

---

### **Step 3. Data Validation**
- Cross-check monthly totals and remove duplicate records  
- Ensure reconciled sales and shipment volumes between inbound/outbound data  

---

### **Step 4. Forecast Accuracy (A1)**
- Compute **WAPE** and **Forecast Bias** for each Region×Month:

WAPE = Σ|F − A| / ΣA
Bias = Σ(F − A) / ΣA

- Output visualization: **Heatmap — WAPE by Region×Month**

---

### **Step 5. Demand Volatility (A2, A4)**
- For each SKU×Region:

σd = STDEV(Demand)
CoV = σd / μd

- Classify volatility into XYZ categories:
- **X ≤ 0.5:** Stable demand  
- **Y ≤ 1.0:** Moderate variability  
- **Z > 1.0:** Erratic demand  

---

### **Step 6. ABC–XYZ Segmentation (A3, A4)**
- Combine **ABC by revenue value** and **XYZ by volatility** to form a **9-cell matrix**
- Output chart: **Inventory Mix by ABC–XYZ (FY Avg €)**  

---

### **Step 7. Lead Time Modeling (A11)**
- Using `DC_Outbound.xlsx`, compute:

μLT = mean(lead_time)
σLT = stdev(lead_time)

- Visualize: **Network Lanes (DC → WH) — Lead Times by Lane**

---

### **Step 8. Safety Stock & Reorder Point (A5, A6)**
- Apply continuous-review (R,S) model formulas:

SS = z × σd × √LT
ROP = μd × LT + SS

- Links customer service level (Φ(z)) to demand variability and lead time  

---

### **Step 9. Working Capital in Inventory (A7)**
- Calculate Working Capital tied in inventory:

- Links customer service level (Φ(z)) to demand variability and lead time  

---

### **Step 9. Working Capital in Inventory (A7)**
- Calculate Working Capital tied in inventory:


WC = (SS + CycleStock) × UnitCost

- Separate **Safety Stock €** vs **Excess Stock €** components  

---

### **Step 10. Inventory KPIs (A8–A10)**
- Compute key performance metrics:

Turns = COGS / AvgInventory
DIO = 365 / Turns
GMROI = GrossMargin / AvgInventory

- Output tables and visualizations:
- **Inventory KPI Table**
- **Cash-to-Cash (C2C) Funnel**

---

### **Step 11. Inbound Cost Analysis (A13)**
- Aggregate inbound cost by **Region × Mode (Ocean, Air, Other)**
- Output chart: **Inbound Cost Breakdown — Ocean / Air / Other**
- Derived from `DC_Inbound.xlsx`

---

### **Step 12. Outbound Efficiency (A11, A13)**
- Compute:

Mean Lane Cost (€) = Σ Cost / # Lanes
Avg Lead Time (weeks) = mean(Transit Time)

- Output visualization: **Bubble Chart — Lead Time vs Mean Lane Cost**

---

### **Step 13. Mode-Shift Scenario (A15)**
- Scenario setup:
- Cap **Air Freight ≤ 20%**  
- Shift remaining to **Ocean / Multimodal**
- Compute cost impact:

ΔCost = (Cost_air − Cost_ocean) × ShiftedVolume


---

### **Step 14. Service-Level Simulation (A16)**
- Recalculate Safety Stock and CSL for new scenario parameters:

CSL = Φ(z)

- Output visualization: **Cost–Service Frontier — Baseline vs Scenario**

---

### **Step 15. Efficient Frontier Model (A17)**
- Compare **Baseline** vs **Scenario** total cost curves  
- Quantify logistics savings:
> ≈ €1.8–€1.9 M improvement from mode-shift & lead-time optimization  
- Output charts:
- **Savings Bridge**
- **Δ Cost by Region**
- **Top 10 Lane Savings**

## 4️⃣ Submission Acknowledgement

### 🏛️ **Consularium**  
*The McKinsey Consulting Event*

---

**Submission by:**  
**Team Solution Patrol**  
*Department of Management Studies, IIT*

---

> 🏆 **AeroSports Supply Chain Analytics — Diagnose • Optimize • Transform**


