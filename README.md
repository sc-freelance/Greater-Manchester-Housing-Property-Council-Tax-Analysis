# Greater Manchester Housing, Property & Council Tax Analysis

An exploratory analysis of house prices and housing stock across Greater Manchester's 10 boroughs (Bolton, Bury, Manchester, Oldham, Rochdale, Salford, Stockport, Tameside, Trafford and Wigan), using official UK open data and Python (pandas, Matplotlib, scikit-learn).

## Data sources

| Dataset | What it contains | File in this repo |
|---|---|---|
| HM Land Registry, Price Paid Data (monthly update) | Individual property sales registered in the latest monthly release | `pp-monthly-update-new-version.csv` |
| Valuation Office Agency, Council Tax: Stock of Properties (CTSOP 1.0), 1993–2024 | Number of homes in each council tax band, per local authority, at each annual snapshot | `CTSOP1-0-1993-2024.zip.zip` |

## What the notebook does

1. Filters both datasets to the 10 Greater Manchester boroughs.
2. Cleans the price data: converts prices to numbers, keeps only standard sales (`ppd_category = A`), and removes changed or deleted records (`record_status = A` only).
3. Uses medians rather than averages, so a few very expensive sales don't distort the results.
4. Produces six charts, each followed by notes on **what it shows** and **what it doesn't show**.

## Key findings

* **House prices (sample of recent sales):** Trafford (£370,000) and Stockport (£325,000) have the highest median sale prices. Wigan (£202,000) and Rochdale (£210,000) have the lowest.
* **Property type:** detached homes have the highest median (about £392,500). Terraced homes and flats have the lowest (about £200,000 and £195,000).
* **Housing stock:** Manchester has the most homes (about 249,000) and Bury the fewest (about 85,000). Greater Manchester grew from about 1.07 million homes in 1993 to 1.29 million in 2024, a rise of about 21%.
* **Council tax bands:** 55% of Manchester's homes are in Band A, compared with 19% in Trafford and 24% in Stockport.

## Limitations

* The Price Paid file is a **monthly update**, not a full record of every sale, so borough medians are a snapshot rather than official statistics.
* The two datasets are analysed side by side, not merged.
* Council tax bands in England are based on 1991 valuations, so they show the relative mix of housing, not current values.

## Problems I ran into (and fixed)

* **A 75-million-home outlier.** The VOA zip includes a combined all-years file that my file pattern also picked up. Filtering out large totals hid the problem but also deleted the real 1993 snapshot. The fix was to match only the single-year file names, so all 32 snapshots are now included.
* **A misleading price trend.** A degree-10 polynomial fitted to monthly prices back to 1995 looked convincing, but most early months had only one or two sales in this file. The chart now only shows months with at least 30 sales, with the count next to each median.
* **The first sale was read as the header.** The Price Paid CSV has no header row, so the column names are now supplied when loading.

## Next steps

* Use a full year of Price Paid Data so borough medians cover all sales.
* Combine the datasets, for example each borough's median price against its share of Band A homes.
* Add ONS earnings data to calculate an affordability ratio (house price divided by earnings) for each borough.

## How to run

```bash
pip install pandas numpy matplotlib scikit-learn jupyter
jupyter notebook GM-Housing-Property-Council-Tax-Analysis.ipynb
```

Keep the data files in the same folder as the notebook. The notebook extracts the VOA zip into a `voa_data/` folder itself.
