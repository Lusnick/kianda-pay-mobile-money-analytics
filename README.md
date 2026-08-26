# Kianda Pay — Mobile Money Analytics

Analysis of a simulated mobile money operator in Angola (18,000 transactions), focused on identifying and quantifying the root cause of transaction failures — and turning that into an actionable business recommendation.

*Note: this uses a simulated dataset built to reflect real patterns in mobile money operations, not production data from an actual company.*

## The Business Problem

Mobile money agents need enough **float** (cash + electronic balance) on hand to complete cash-in and cash-out transactions. When an agent runs out of float, the transaction fails, the customer doesn't get their money, and trust in the platform erodes. This is one of the most common operational challenges in mobile money networks across emerging markets.

## Key Findings

- **Overall failure rate: 10.1%** but cash-out transactions specifically fail at **17.5%**, since they're the operation most dependent on agent liquidity.
- **58% of cash-out failures** are caused by insufficient agent float not fraud, not customer error, not network issues.
- Failure rate **triples** with poor network signal (20.8% vs 6.9% with good signal).
- Float shortfall is concentrated in specific provinces (Huambo, Huíla, Uíge) and a small set of high-risk agents not evenly distributed.
- Fraud flag rate is 2.4%, and unverified-KYC customers show a fraud alert rate 3x higher than verified customers.

## Recommendation

Prioritize agent float replenishment in the highest-risk provinces and the top 10 highest-risk agents identified in the dashboard, rather than distributing working capital evenly, this targets the largest, most fixable driver of transaction failure.

## Tech Stack

- **Power BI**  data cleaning (Power Query), DAX measures, and interactive dashboard
- **Excel** source dataset

## Repository Structure


## Repository Structure

- [`data/`](data/) — raw and cleaned dataset
- [`powerbi/`](powerbi/) — Power BI (.pbix) dashboard file
- [`screenshots/`](screenshots/) — dashboard views (since .pbix can't be previewed on GitHub)


## Dashboard Preview

[Dashboard overview](screenshots/dashboard_overview.png)

Network Health & Float Risk Dashboard 

<img width="1433" height="783" alt="Screenshot 2026-08-26 145738" src="https://github.com/user-attachments/assets/37277797-d743-4d28-b70b-45428e1c68cb" />

![Dashboard overview](screenshots/dashboard_overview.png)
*Network Health & Float Risk Dashboard — key KPIs and province-level risk breakdown*

![Agent risk table](screenshots/agent_risk_table.png)
*Highest-risk agents by float shortfall rate*



## Cleaning the Data

Before any analysis, I had to get the raw data into shape, 18,414 rows down to a clean 18,000:

- Removed 54 blank rows and 360 exact duplicates that would have skewed every metric downstream
- Standardized province names and category labels, which were inconsistently capitalized/abbreviated across the file
- Parsed the date/time field properly, since it came in as inconsistent text
- Cleaned up amount and float balance fields, stripped currency labels, fixed formatting
- Found 113 rows with negative transaction amounts, which shouldn't happen. Rather than just deleting them or quietly flipping the sign, I converted them to positive values and added an `Amount_Was_Negative` flag, so anyone auditing the data later can still see exactly which rows were corrected and why
- Fixed a handful of invalid entries in customer tenure
- Added a few derived fields (year, month, cash-out flag, failure flag, etc.) to make the later analysis easier

Full log of every step is in `data/Kianda_Pay_CLEANED.xlsx`, on the Cleaning_Log tab.

## Building the Dashboard

With the data clean, I built DAX measures for failure rate, cash-out failure rate, float shortfall share, fraud flag rate, approval rate, and average agent float balance.

I also built a Top N ranking of the highest-risk agents by float shortfall rate, filtered to agents with a minimum transaction volume, so agents with only a handful of transactions couldn't distort the ranking with unreliable percentages.

Everything was designed around one central question: *why do transactions fail, and where should the company intervene first?*

## Contact

Isabel Lusakueno — [LinkedIn] — isabelajuntos@gmail.com
