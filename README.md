Search Terms Performance Analysis (SQL + Tableau)

Overview

This project analyzes 90 days of Google Ads Search Term data to uncover keyword efficiency, wasted spend, and structural issues in search targeting.
The analysis combines SQL-based data modeling with Tableau visualizations and provides a full keyword classification framework to guide paid search optimization.

The final output identifies:
	•	High-performing terms to scale
	•	Inefficient terms to negate
	•	Campaign-level waste
	•	Relevance problems tied to impressions and CTR

⸻

Objectives
	•	Classify search terms into four performance groups: HERO, NEUTRAL, WASTE, LOW_CTR
	•	Identify opportunities for new keywords and negative keywords
	•	Compare cost, conversions, and CPA by campaign type
	•	Surface hidden inefficiencies using scatter plots and category-level summaries
	•	Build a scalable SQL framework for ongoing search term evaluation

⸻

Dataset

Source: Google Ads Search Terms Report
File used: WPWW search terms analysis.xlsx

Key fields:
	•	Search_term
	•	Match_type
	•	Added_Excluded
	•	Campaign
	•	Ad_group
	•	Keyword
	•	Clicks
	•	Impressions
	•	Cost
	•	Conversions

These were cleaned, normalized, and loaded into PostgreSQL for analysis.

⸻

Methodology

1. Data Cleaning & Normalization

The raw export was imported into PostgreSQL and normalized:
	•	Standardized field formatting
	•	Cleaned search term strings
	•	Cast all numeric fields (Clicks, Impressions, Cost, Conversions)
	•	Removed symbols (e.g., “%”)
	•	Ensured consistent lowercase formatting

Goal: produce a consistent dataset for aggregation and modeling.

⸻

2. Base Metric Calculation

Created a SQL layer that computes all core performance metrics:
	•	CTR = Clicks / Impressions
	•	Conv Rate = Conversions / Clicks
	•	Cost per Conversion (CPA) = Cost / Conversions
	•	Aggregated search term performance at the (term + campaign + match_type) level

These form the analytical backbone of the project.

⸻

3. Statistical Thresholds

Generated dynamic, data-driven thresholds in SQL:
	•	Median CPA (used to identify cost-efficient terms)
	•	Top 30% Conv Rate (used to identify high-quality intent terms)

These thresholds allow classification to adapt to the dataset, not hard-coded values.

⸻

4. Search Term Classification Framework

Each search term is labeled based on performance:
	•	HERO
	•	≥1 conversion
	•	Conv rate in top 30%
	•	CPA below median
	•	WASTE
	•	High clicks
	•	0 conversions
	•	LOW_CTR
	•	CTR < 2%
	•	NEUTRAL
	•	Everything that performs in the “average” range

This creates a clear behavioral taxonomy of search terms.

⸻

5. Keyword Opportunity Flags

Two SQL-driven flags were added:
	•	should_add_keyword = YES
For converting HERO terms that are not mapped to any existing keyword (common in broad match or P-Max).
	•	should_negative = YES
For WASTE terms generating clicks and cost with no conversions.

This makes optimization actionable.

⸻

6. Campaign-Level Aggregations

SQL grouped spend, conversions, and CPA by campaign type and term category:
	•	Spend per category
	•	Conversions per category
	•	Average CPA
	•	Contribution of each category to campaign performance

This enabled cross-campaign efficiency comparisons.

⸻

7. Master Table Export

All metrics, flags, and classifications were merged into a final Tableau-ready master table, used for dashboarding and visualization.

⸻

Visualizations (Tableau)

1. Scatter plot - Cost vs Conversion rate

The scatter plot shows each search term positioned by its total cost (horizontal axis) and its conversion rate (vertical axis), making it easy to see which terms are high spend, low performance vs. low spend, high performance. Each dot represents a single search term

2. Scatter plot - Search Term Relevance: Impressions vs CTR

The scatter plot shows how many impressions each search term receives (X-axis) compared to how often users click on it (CTR on the Y-axis), revealing which terms attract engagement and which ones generate visibility with almost no clicks.

3. Bar chart - Spend vs Conversions by Search Term Category

The bars show total spend for each term category (HERO, LOW_CTR, NEUTRAL, WASTE), while the circles on top show how many conversions each category produced

4. Bar chart - Campaign Performance: Spend vs CPA (by Term Category) 

The chart compares how much each campaign spends (bars) versus how expensive its conversions are (CPA squares) across different term categories, making it easy to see which channels are efficient and which ones drive wasted spend.

⸻

Key Findings
	•	16 HERO terms deliver ~34% of total conversions at ~$34 CPA — highly efficient.
	•	99 WASTE terms consume ~21% of spend and generate 0 conversions — clear targets for negatives.
	•	Majority of impressions fall into low-CTR terms, indicating poor search relevance or overly broad matching.
	•	Core performance comes from a small set of long-tail, high-intent queries.
	•	Non-brand search shows significantly higher CPA than other channels.
	•	Spend allocation is misaligned with results: high spend in NEUTRAL/WASTE, low spend in HERO.

⸻

Recommendations
	•	Scale HERO terms through dedicated keywords and higher budget share.
	•	Add WASTE terms to negative keyword lists.
	•	Tighten match types to reduce broad, non-relevant impressions.
	•	Reassess non-brand search structure and reduce budget leakage.
	•	Prioritize campaigns that show strong intent capture and efficiency.

⸻

Technologies Used
	•	SQL (PostgreSQL)
	•	Tableau
	•	Excel
	•	Google Ads Search Term Report

⸻

Project Structure

/sql
   01_base_metrics.sql
   02_thresholds.sql
   03_classification.sql
   04_keyword_flags.sql
   05_campaign_summary.sql

/tableau
   spend_vs_conversions.twbx
   cost_vs_conv_rate.twbx
   spend_vs_cpa_by_campaign.twbx
   impressions_vs_ctr.twbx

/data
   WPWW search terms analysis.xlsx

README.md
