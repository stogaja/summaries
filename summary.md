# Project Presentation Summary: MoPhones Credit Analytics

This summary provides direct talking points and explanations for presenting the MoPhones Credit Analytics project.

## 1. Project Overview
*   **The Problem**: MoPhones needed to understand their portfolio performance, credit risk drivers, and how repayment behavior affects customer satisfaction.
*   **The Data**: I analyzed over 70,000 records across five quarterly snapshots, combining credit data, customer demographics, and NPS survey results.
*   **Technical Stack**: Python (Pandas) for exploratory analysis, dbt for production-ready data modeling, and markdown for comprehensive reporting.

## 2. Methodology & Implementation
*   **Unified Data Pipeline**: Merged disparate data sources (CSV snapshots and multi-sheet Excel workbooks) into a structured analytical dataset.
*   **Feature Engineering**:
    *   **Age Segmentation**: Categorized customers into ranges (18-25, 26-35, etc.) by calculating age dynamically against each snapshot date.
    *   **Disposable Income Groups**: Aggregated income from multiple channels (Banks, Paybills, Direct Receipts) and averaged it over loan duration to create 8 standard income brackets.
*   **dbt Architecture**: 
    *   **Staging Layer**: Cleaned and standardized column names and data types.
    *   **Marts Layer**: Built dimension and fact tables (dim_customers, fct_credit_performance) to support time-series analysis and segmentation.
    *   **Macros & Tests**: Used reusable macros for business logic and implemented data quality tests for uniqueness and referential integrity.

## 3. Key Findings (The "So What?")
*   **Portfolio vs. Risk Growth**: The portfolio grew by 173% in 2025, but arrears grew by 225%. This signaled that risk was outpacing growth, requiring a more granular credit policy.
*   **The Age Factor**: I identified a clear inverse relationship between age and risk. Customers aged 46+ show roughly 40% lower arrears than the 18-35 segment.
*   **Income Resilience**: Customers in the lowest income bracket (<5,000) have 2.6x higher arrears than the mid-tier groups, while the 5,000–9,999 group was surprisingly the most resilient.
*   **The Experience Gap**: Customer satisfaction (NPS) is 11% lower for customers in arrears. This proves that credit outcomes aren't just a financial metric—they are a leading indicator of brand loyalty.

## 4. Strategic Recommendations
*   **Data Quality First**: We need mandatory DOB and verified income data at onboarding to reduce the "Unknown" segments, which currently show the highest risk.
*   **Segmented Credit Strategy**: Suggested implementing age-based pricing and graduated credit limits for high-risk income groups.
*   **Proactive Collections**: Recommended moving from quarterly to daily snapshots for early-warning indicators (contacting customers at 15 days past due vs. waiting for 30).
*   **Product Fit**: Identified that early returns (3-day) correlate with the lowest NPS scores, suggesting a need for better onboarding education or product-market fit checks.

## 5. Technical Highlights (For Q&A)
*   **Repeatability**: The dbt project ensures that this analysis can be refreshed automatically as new data arrives.
*   **Scalability**: The data model separates "Staging" (source cleaning) from "Marts" (business logic), making it easy to add new data sources like payment transaction logs.
*   **Data Integrity**: Built tests to catch "First Payment Defaults" and validate NPS score ranges (0-10).
