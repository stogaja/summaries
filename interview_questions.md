# Potential Evaluation Questions: MoPhones Case Study

This document contains potential questions that may be asked during an interview or evaluation of this submission, categorized by topic.

---

## 1. Analytical Thinking & Insights

*   **Q: You noted that arrears grew faster (225%) than the portfolio count (173%). What would be your first step to investigate why this is happening?**
    *   *Approach*: Discuss performing "Vintage Analysis" to see if newer cohorts are performing worse than older ones, or checking if the increase is driven by a specific geographic region or product type.
*   **Q: Why do you think the 46-55 age group has significantly lower arrears than the 18-25 group?**
    *   *Approach*: Mention factors like financial stability, historical credit experience, and lower mobility/higher stability in income sources compared to younger customers.
*   **Q: Your analysis shows that customers in "Write-Off" status sometimes have higher NPS than those in "Active Arrears." How do you interpret this?**
    *   *Approach*: This is likely due to the cessation of collection pressure. Once a debt is written off, the friction between the company and the customer decreases, which can paradoxically improve reported satisfaction.

---

## 2. Technical Implementation (dbt & SQL)

*   **Q: Why did you choose a layered architecture (Staging vs. Marts) for the dbt project?**
    *   *Approach*: Explain the benefits of "Separation of Concerns." Staging handles raw data cleaning (renaming, casting) while Marts handle business logic. This makes the project easier to maintain and audit.
*   **Q: How do your macros for age and income grouping improve the scalability of the project?**
    *   *Approach*: Explain that by centralizing the `CASE WHEN` logic in a macro, you ensure consistency across all models. If the business decides to change the age brackets, you only have to update it in one place.
*   **Q: What was the reasoning behind the specific data quality tests you implemented in `schema.yml`?**
    *   *Approach*: Focus on "Uniqueness" and "Not Null" for primary keys (Loan ID) to prevent fan-outs during joins, and "Accepted Values" for NPS to ensure data integrity during input.

---

## 3. Data Engineering & Python

*   **Q: The MoPhones data was "point-in-time." How did you handle the risk of "Logical Data Corruption" when merging multiple snapshots?**
    *   *Approach*: Discuss the importance of ensuring that a customer’s attributes (like age) were calculated relative to the snapshot date, not the execution date, to maintain the historical context.
*   **Q: Why did you use Python for the initial analysis instead of doing everything in SQL?**
    *   *Approach*: Mention Python's superior handling of complex date math across multiple non-relational sources (like Excel multi-sheets) and its ability to generate rapid exploratory visualizations.

---

## 4. Product & Strategy

*   **Q: If MoPhones had to cut their marketing budget and focus only on one segment, which one would you recommend based on your findings?**
    *   *Approach*: Highly recommend the 46+ age segment with mid-tier income (5,000-50,000), as they show the best balance of growth and low delinquency.
*   **Q: How would you balance the need for high collections with the goal of maintaining a high NPS?**
    *   *Approach*: Propose a "Graduated Collection Strategy"—using soft reminders and educational content for early-stage delinquency (1-15 days) and saving more intensive collection efforts for later stages.

---

## 5. Data Quality, Assumptions & Limitations

*   **Q: You categorized a large portion of data as "Unknown" for age and income. How does this affect the reliability of your recommendations?**
    *   *Approach*: Acknowledge the limitation but point out that the "Unknown" segment had the highest arrears, making "Improved Data Collection" a high-ROI recommendation in itself.
*   **Q: What additional data points would have made your analysis significantly more accurate?**
    *   *Approach*: Mention "Payment Transaction Logs" (to see frequency vs. amount), "Marketing Channel" (to see if specific channels bring in high-risk customers), and "Support Ticket Logs."
