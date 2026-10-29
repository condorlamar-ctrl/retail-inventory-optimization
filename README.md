Inventory Shrinkage & Stock Efficiency Analysis

**Analyzing 936 store-product-month records to identify shrinkage priorities and investigate stock efficiency.**

Store 6 recorded the most shrink units (**804**), while Store 5 had the highest shrink cost (**6,998.60**) and weighted shrink rate (**1.97%**). This project uses inventory checks, weighted-rate comparisons, and store and product rankings to identify practical investigation priorities.

**[View the Jupyter Notebook](inventory_shrinkage_analysis%20%281%29.ipynb)** | **[Explore the main dataset](stock_months_validated.csv)** | **[View store rankings](store_shrinkage_rankings.csv)** | **[Compare product rankings](product_rankings_complete.csv)**

## Business Problem

Inventory shrinkage reduces the stock available for sale, while slow-moving inventory can tie up working capital. For a retailer, the key business question is:

> Which stores and products should the business prioritize for shrinkage investigation, and where do stock-efficiency signals suggest further review?

The analysis uses `stock_months_validated.csv` as the main dataset, supported by store rankings, product rankings, coded-month summaries, and inefficiency-overlap flags. The main file contains 936 records covering **6 stores, 13 products, and 12 coded months**. Each row represents one store-product-month combination.

## Results at a Glance

| Metric | Result |
| --- | ---: |
| Store-product-month records analyzed | 936 |
| Stores represented | 6 |
| Products represented | 13 |
| Coded months represented | 12 |
| Units sold | 99,840 |
| Recorded shrink units | 3,248 |
| Weighted shrink rate | 1.37% |
| Recorded shrink cost | 26,560.10 |

The weighted shrink rate is **total shrink units divided by total available units**: 3,248 / 236,993. The denominator sums availability across store-product-month records; it does not represent 236,993 distinct physical units. Cost values have **no specified currency denomination**.

### Shrinkage by Store

| Store ID | Shrink units | Weighted shrink rate | Shrink cost |
| --- | ---: | ---: | ---: |
| **5** | **772** | **1.97%** | **6,998.60** |
| **6** | **804** | **1.92%** | **5,653.40** |
| 4 | 573 | 1.43% | 4,453.40 |
| 3 | 501 | 1.27% | 3,984.00 |
| 2 | 368 | 0.94% | 3,219.30 |
| 1 | 230 | 0.62% | 2,251.40 |

Stores are ordered by total shrink cost. Stores 5 and 6 together account for **48.5% of recorded shrink units**. Store 6 ranks first by unit volume, but Store 5 ranks first by both cost and weighted rate.

### Products with the Highest Shrink Cost

| Product ID | Shrink units | Weighted shrink rate | Shrink cost | Share of total shrink cost |
| --- | ---: | ---: | ---: | ---: |
| **1** | **387** | **2.26%** | **10,836.00** | **40.8%** |
| 3 | 599 | 3.06% | 5,391.00 | 20.3% |
| 13 | 239 | 1.48% | 3,346.00 | 12.6% |

These are the three highest-cost products among all 13 products. Product 3 ranks first by both shrink units and weighted rate, while Product 1 ranks first by cost. The dataset identifies products by code; product names and categories are not supplied.

**Business takeaway:** Shrink volume, exposure-adjusted rate, and cost identify different priorities. Stores 5 and 6 warrant investigation, while Product 1 deserves particular attention because it represents **40.8% of recorded shrink cost**. These rankings describe the supplied data and do not identify the causes of shrinkage.

## Approach & Tools

**Analysis tools:** Python, Pandas, NumPy, Matplotlib, and Seaborn for validation, descriptive analysis, and visualization.

1. **Validate the main data:** Check dimensions, missing values, duplicate keys, stock-flow equations, and cost calculations.
2. **Compare shrinkage measures:** Calculate count-based weighted rates and distinguish them from averages of recorded rate fields.
3. **Assess store, product, and coded-month priorities:** Reconcile the supporting summaries to the main dataset and compare volume, rate, and cost rankings.
4. **Review stock-efficiency overlap:** Identify entities with both below-median mean sell-through and above-median mean closing stock within their comparison level.

The supplied overlap flags identify **Stores 3 and 4**, **Products 7 and 9**, and **coded months 1, 2, 4, 5, and 7** as meeting both criteria. These are relative screening signals, separate from the highest-shrinkage rankings. They do not establish overstocking or operational failure.

Coded month 4 has the highest recorded shrink volume (**393 units**) and weighted rate (**1.85%**). Coded month 11 has the lowest weighted rate (**0.97%**). Calendar dates and year mapping are not supplied, so these codes should not be assigned calendar names or interpreted as evidence of seasonality.

## Data Validation

| Validation check | Result | What it establishes |
| --- | ---: | --- |
| Missing values in the main dataset | 0 | All 22 supplied fields are populated |
| Duplicate store-product-month combinations | 0 | Each combination appears once |
| Record-level stock-flow reconciliation | 936 of 936 pass | Available units equal opening plus received units; closing units equal available minus sold and shrink units |
| Cost and value calculations | 936 of 936 pass | Sales, cost of sales, shrink cost, and opening/closing values agree with the corresponding quantities and unit values |
| Supporting-summary reconciliation | All 4 files match | Directly comparable totals, means, rates, rankings, and overlap flags agree with the main data within numerical precision |
| Adjacent coded-period stock continuity | 856 of 858 pairs differ | Opening stock usually differs from the previous coded period's closing stock for the same store-product pair |

The dataset is complete and internally consistent within each record, but **continuous inventory balances are not established**. When records are ordered by `month_id`, 856 of 858 adjacent opening/previous-closing comparisons differ. Treat the records as period-level observations until the meaning and sequence of month codes, stock adjustments, and balance continuity are documented.

The count-based weighted shrink rate (**1.37%**) is different from the mean recorded `shrink_rate` (**2.36%**). They are not interchangeable. Recorded shrink units match rounded `unsold_units × shrink_rate` in all 936 rows, so independent inventory counts and documentation are needed to establish how those quantities were measured or derived.

## Recommended Operational Investigations

| Business priority | Proposed action | Outcome to measure |
| --- | --- | --- |
| Investigate Stores 5 and 6 | Review stock counts, receiving records, write-offs, and product mix; compare against lower-rate stores | Verified unexplained unit differences and shrink cost relative to available stock |
| Focus on high-cost products | Investigate Product 1 for cost exposure and Product 3 for shrink volume and rate | Independently verified shrink units, costs, and documented reasons |
| Review stock-efficiency signals | Examine replenishment, demand, and stock age for Stores 3 and 4 and Products 7 and 9 | Sell-through, aged inventory, service levels, and stock availability |
| Improve inventory reconciliation | Confirm month-code definitions and reconcile opening stock to prior closing stock with documented adjustments | Unexplained balance differences and completeness of adjustment records |

These are proposed investigations. The project does not report implemented interventions, reduced shrinkage, or realized financial savings.

## Limitations & Next Steps

- **Descriptive evidence:** Rankings and median-based flags identify review priorities. The supplied files contain no hypothesis tests, confidence intervals, or predictive-model evaluation, so statistical significance and predictive accuracy are not established.
- **Inventory continuity:** Coded-period opening and prior closing balances usually differ. Reconcile the sequence and any adjustments before using these records as a continuous inventory ledger or forecasting inventory needs.
- **Metric definitions:** The weighted rate uses available units; recorded rate fields and rounded shrink quantities follow different calculations. Preserve their definitions when comparing results.
- **Missing operational context:** Shrink reasons, independent stock counts, inventory age, demand, stockouts, and adjustment records are unavailable. High closing stock alone does not prove inefficiency.
- **Coded identifiers:** Store and product names, calendar dates, year mapping, and data provenance are not documented. Findings should not be generalized beyond the supplied records.
- **Financial interpretation:** Currency is unspecified. `shrink_cost` equals shrink units multiplied by unit cost; it is not a verified cash loss, lost-sales estimate, or net-profit impact.
- **Reproducibility:** The notebook rebuilds stock/value checks, aggregate summaries, rankings, and screening flags from the five CSVs, and verifies agreement with the supplied summaries. Original extraction or data-generation code is not supplied.

## Run the Notebook

Keep `inventory_shrinkage_analysis (1).ipynb` beside the five CSVs listed below.

**Google Colab:** Open the notebook and select **Runtime → Run all**. If any CSVs are missing, the loading cell opens an upload dialog; select all five supplied files together. Existing files are loaded directly. If necessary, run `%pip install pandas numpy matplotlib seaborn jinja2` in a separate cell, restart the runtime, and rerun all cells.

**Local Jupyter:** From the folder containing the notebook and CSVs, install the dependencies and start Jupyter:

```bash
python -m pip install notebook pandas numpy matplotlib seaborn jinja2
python -m notebook
```

Open the notebook and select **Restart Kernel and Run All**. Paths are relative to the working directory; change `DATA_DIR` only if the files are in a subfolder. The notebook has been executed end to end with only the five required CSVs, and its package versions are recorded in the setup output. Figures are embedded; analysis does not overwrite input data.

## Project Files

| File | Contents |
| --- | --- |
| [Analysis notebook](inventory_shrinkage_analysis%20%281%29.ipynb) | Executed data validation, store/product/month comparisons, summary verification, charts, and business recommendations |
| [Main inventory dataset](stock_months_validated.csv) | 936 store-product-month records and 22 fields covering inventory, sales, shrinkage, costs, and values |
| [Store shrinkage rankings](store_shrinkage_rankings.csv) | Shrink volume, weighted rate, cost, and stock-efficiency measures for 6 stores |
| [Product shrinkage rankings](product_rankings_complete.csv) | Shrink volume, weighted rate, and cost rankings for 13 products |
| [Coded-month shrinkage summary](monthly_shrinkage_summary.csv) | Shrinkage, sales-volume, and inventory summaries across 12 coded months |
| [Stock-efficiency overlap summary](inefficiency_overlap_summary.csv) | Median-based sell-through and closing-stock flags for stores, products, and coded months
