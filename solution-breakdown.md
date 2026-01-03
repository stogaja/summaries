# Solution Breakdown: MoPhones Product Data Analysis

This document outlines the step-by-step thought process, technical considerations, and logical flow used to develop the MoPhones analytics solution.

---

## Phase 1: Problem Discovery & Data Mapping

### 1.1 Understanding the Objective
The primary goal was to transition from "raw data" to "actionable product insights." I identified three core areas of interest:
*   **Portfolio Dynamics**: How is the lending business actually growing?
*   **Risk Drivers**: Who are the high-risk customers (by age and income)?
*   **Customer Experience (NPS)**: Does debt distress ruin the customer relationship?

### 1.2 Data Source Auditing
I audited the three primary data sources to understand their relationships:
*   **Credit Data**: 5 point-in-time quarterly snapshots. This required a "long-format" merge to track trends over time.
*   **Demographic Data**: Multi-sheet Excel files containing DOB and Income. I noted that `Loan ID` was the primary key, but required whitespace trimming for a clean join.
*   **NPS Data**: Survey scores that needed to be linked to the latest snapshot state to find correlations.

---

## Phase 2: Data Preparation & Feature Engineering

### 2.1 Handling "Point-in-Time" Challenges
Lending data is tricky because a customer's state (arrears, status) changes daily. 
*   **Consideration**: I decided to treat the aggregate of the 5 snapshots as a time-series to visualize growth, rather than just looking at the latest month.

### 2.2 Reconstructing Customer Profiles
I engineered two critical business features requested by the case study:
*   **Dynamic Age Grouping**: 
    *   *Constraint*: Age isn't static. 
    *   *Solution*: Calculated age by subtracting DOB from the *Snapshot Date* (not the current date) to ensure historical accuracy.
*   **Normalized Income Brackets**: 
    *   *Constraint*: Raw income came from three different sources (Banks, Paybills, etc.). 
    *   *Solution*: Aggregated all sources and divided by the loan `Duration` to get a "Monthly Available Flow," then mapped these to 8 standardized brackets to identify "Affordability Tiers."

---

## Phase 3: Analytical Deep Dive

### 3.1 Question 1: Portfolio Performance
I aggregated metrics (Total Paid, Arrears, DPD) by snapshot date.
*   **Insight**: I observed that while the portfolio count grew 173%, Arrears grew 225%. This was a "red flag" indicating that the aggressive expansion was possibly reaching lower-quality customer segments.

### 3.2 Question 2: Risk Indicators
I ran a cross-segmentation analysis on the latest snapshot (Dec 2025).
*   **Insight**: Found that the "Unknown" age/income groups had the highest arrears. 
*   **Technical Choice**: Instead of ignoring missing data, I categorized it as "Unknown" to quantify the *cost* of poor data collection (which turned out to be the highest-risk segment).

### 3.3 Question 3: NPS Correlation
I performed an inner join between the credit state and NPS scores.
*   **Insight**: There was a clear ~11% drop in satisfaction for customers with any arrears. 
*   **Product Consideration**: I noted a "Status Paradox" where customers in "Write-Off" status sometimes have higher NPS than those in "Active Arrears"—likely because the stress of collection activity had ceased.

---

## Phase 4: Engineering for Production (dbt)

### 4.1 The Layered Architecture
To make the analysis repeatable, I built a dbt pipeline using a "Medallion-style" architecture:
*   **Staging (Bronze)**: Atomic cleaning (casting types, renaming `Loan Id `).
*   **Marts (Gold)**: 
    *   `dim_customers`: Unified customer truth (joined demographics and income).
    *   `fct_credit_performance`: The primary engine for time-series reporting.

### 4.2 Encoding Business Logic (Macros)
*   **Why?**: I didn't want to rewrite the `CASE WHEN` logic for age groups in multiple scripts. 
*   **Solution**: Created `calculate_age_group.sql` and `calculate_income_group.sql` as reusable Jinja macros to ensure "Single Source of Truth."

### 4.3 Data Quality Framework
I implemented automated tests to ensure the "Production-Ready" requirement was met:
*   **Uniqueness**: Ensured no duplicate Loan IDs in the dimension table.
*   **Accepted Values**: Validated that NPS scores fall strictly between 0 and 10.

---

## Phase 5: Synthesis & Recommendations

### 5.1 Documenting Assumptions
I explicitly documented assumptions (e.g., assuming "Duration" is in months) to ensure stakeholders understand the "Confidence Intervals" of the data.

### 5.2 Strategic Roadmap
I moved beyond just "showing data" to "prescribing action":
*   **Data Governance**: Mandatory DOB collection.
*   **Credit Policy**: Lower interest rates for the 46+ age segment (proven lower risk).
*   **CX Strategy**: A "graduated collections" approach to protect NPS.

---

## Summary of Technical Decisions
1.  **Python for Speed**: Used Python for the initial "heavy lift" and complex date math.
2.  **dbt for Process**: Used dbt to prove how this would scale in a collaborative Data Team environment.
3.  **Markdown for Communication**: Chose a clear, structured document format over a messy slide deck to prioritize technical depth.
