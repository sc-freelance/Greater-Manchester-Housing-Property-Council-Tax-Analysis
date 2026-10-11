# Greater Manchester Property & Housing Analytics Suite

An end-to-end data engineering and visualization pipeline built to analyze multi-decade housing stock dynamics and market transactions across Greater Manchester, utilizing official UK government open datasets from the **Valuation Office Agency (VOA)** and **HM Land Registry**.

---

## 📋 Project Overview

This project processes historical multi-year records and cross-sectional snapshots, transforming complex government data into an insightful, publication-ready analytical suite across Greater Manchester’s 10 local authorities (Manchester, Bolton, Bury, Wigan, Tameside, Stockport, Trafford, Salford, Oldham, and Rochdale).

---

## 🗄️ Integrated Data Sources

* **1. GOV.UK VOA CTSOP 1.0 Dataset (`CTSOP1-0-1993-2024.zip`):** Multi-year historical housing stock snapshots (1993–2024) categorized by Council Tax valuation bands across local authority districts.
* **2. HM Land Registry Datasets:** Property transaction records and house price index data providing deep insights into market liquidity, average transaction values, and price growth trajectories across the region.

---

## 📊 Visualization Suite & Analytical Findings

### 1. Chart 1: Borough Housing Stock Hierarchy Snapshot
* **Analytical Insight:** This cross-sectional horizontal bar chart establishes the baseline structural size disparity across Greater Manchester's 10 local authorities. **Manchester** commands the largest share of the regional housing inventory with approximately 249,300 residential properties, followed closely by Wigan (152,670) and Salford (134,060). Conversely, **Bury** sits at the lower end of the regional footprint with roughly 85,100 properties, underscoring distinct urban density concentrations.
* **Technical Implementation:** Built using an object-oriented Matplotlib approach paired with pandas, utilizing `.set_index()` to map borough names directly as categorical row labels in a horizontal bar layout (`kind='barh'`) with explicit coordinate text annotations.
* **Refinement & Challenges:** Initial raw data frames presented unformatted alphabetical lists, making rapid regional comparison difficult. This was successfully resolved by introducing `.sort_values('total_stock', ascending=True)` to transform the visualization into a structured descending hierarchy.

### 2. Chart 2: Multi-Decade Time-Series Growth & Polynomial Regression
* **Analytical Insight:** Captures the continuous, multi-decade expansion of Greater Manchester’s residential housing inventory over a 30-year horizon (1993–2024). The data reveals a steady upward climb from roughly 1.07 million homes in the early 1990s to over 1.28 million by 2024, highlighting non-linear construction acceleration patterns.
* **Technical Implementation:** Automates multi-file ingestion via `glob` and dynamic year extraction via string splitting (`filename.split('_')`). Integrates `scikit-learn` (`PolynomialFeatures(degree=2)` and `LinearRegression`) with high-density linear space generation (`np.linspace`) to map a smooth predictive curve over scatter data points.
* **Outlier Management & Error Resolution:** Early development runs encountered a severe historical anomaly: unmasked 1993 snapshots contained national/regional summary rows that bypassed filtering, producing a distorted U-shaped curve skewed by an erroneous 75-million-property outlier. This was systematically corrected by engineering a robust **defensive guardrail filter** (`yearly_growth['all_properties'] < 2500000`) and dynamic column-resolution logic.

### 3. Chart 3: Council Tax Band Composition (Stacked Bar Chart)
* **Analytical Insight:** Breaks down the latest 2024 baseline inventory across Greater Manchester by valuation tiers (Council Tax Bands A through I). It exposes socio-economic housing disparities, revealing dense concentrations of lower valuation bands (Bands A and B) in urban centers like Manchester and Wigan compared to suburban profiles.
* **Technical Implementation:** Targets the most recent annual CSV file via dynamic pattern matching, extracts all valuation band attributes, and renders a professional stacked horizontal bar chart (`stacked=True`) utilizing a distinct categorical color map (`colormap='tab20_r'`) and an anchored external legend.
* **Handling Data Schema Discrepancies:** Shifting government naming conventions across decades initially triggered `KeyError` exceptions. This was resolved using intelligent fallback generators (`next()` keyword lookups), data-type coercion (`pd.to_numeric`), and clean dataframe projection to eliminate pandas performance fragmentation warnings.

---

## 🛠️ Technical Architecture & Data Engineering

* **Automated Multi-File Ingestion:** Leverages Python's `glob` module and `zipfile` extraction to parse dozens of annual CSV snapshots without manual intervention.
* **Dynamic Schema Resolution:** Employs intelligent fallback generators to dynamically adapt to changing column names and header schemas across historical files.
* **Defensive Outlier Guardrails:** Automatically detects and filters out erroneous national or regional aggregate summary rows to preserve true regional scaling.
* **Machine Learning Integration:** Combines `scikit-learn` polynomial regression with `np.linspace` for smooth growth trend modeling.
* **Object-Oriented Matplotlib:** Utilizes clean figure-axes instantiation (`plt.subplots()`, `ax`), explicit value annotations, custom color maps, and `tight_layout()` formatting.

---

## ⚙️ Prerequisites & Installation

Ensure you have Python 3.8+ installed along with the required data science libraries:

```bash
pip install pandas numpy matplotlib scikit-learn
