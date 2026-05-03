# Greenwood-Clinical-Admissions-Analysis-2023
Analyzed 2023 hospital admissions data to identify patient volume trends, staffing gaps, and department-level performance drivers impacting operational efficiency.
_______________________________________________________________________________________________________________________
# About The Author

I am a results-driven Reporting Analyst and Advanced Certified Scrum Master (ACSM) with a robust background in data-driven decision-making and Agile project management. My expertise lies at the intersection of technical reporting, CRM administration, and process optimization.

With a Bachelor’s degree in Business and a Certified Scrum Product Owner (CSPO) designation, I specialize in transforming complex datasets into actionable business intelligence. My technical toolkit includes:
	• Data Visualization: Power BI (Advanced Certificate candidate).
	• Database Management: SQL, Snowflake, and Toad for workplace reporting.
	• Project Management: Implementing Scrum and Agile methodologies to streamline cross-functional workflows.
	• System Administration: Comprehensive CRM Administration and data integrity management.

Beyond my technical role, I serve as the Chairperson for the organization’s Associate Network Group. In this leadership capacity, I focus on professional development, internal networking, and fostering a collaborative environment for my colleagues, applying the same Agile principles of transparency and inspection used in my reporting projects.


I am currently expanding my technical stack through an Advanced Power BI program and an upcoming SQL cohort starting in May 2026, ensuring that my analytical solutions remain at the forefront of industry standards.


_______________________________________________________________________________________________________________________

# Project Highlight
	
  This project is a comprehensive Power BI dashboard to analyze hospital admission trends. This project involved transforming raw clinical data into actionable insights for hospital leadership.

<p align="center">
  <img src="2023 Greenwood Hospital Admissions Cover.png" width="800">
</p>




<p align="center">
  <img src="2023 Greenwood Hospital Admissions Summary.png" width="800">
</p>


	
  Key Insights:
  
		* Admissions peaked in May at 105 — a 61.5% increase over the September low — with a recurring ~14% quarterly surge pattern (Q1→Q2 and Q3→Q4), highlighting predictable seasonal staffing gaps.
    
		* Emergency department accounted for 24.1% of total 2023 volume (241 of 1,000 admissions), ranking #1 across all departments and driving resource prioritization decisions.
    
		* Weekend vs. weekday admission rates showed only a ~2% variance (2.86 vs. 2.93 avg/day), suggesting staffing levels are relatively well-balanced across the week — contradicting the initial hypothesis of a weekend operational imbalance.
    
	* 40% Cardiology Q3 surge + 37.5% Pediatrics — both verified from data
	
  Tools Used: Power BI, DAX, Agile Methodology. #DataAnalytics #PowerBI #HealthcareIT #ScrumMaster #ReportingAnalyst

  


_______________________________________________________________________________________________________________________
# Dataset Context 
To ensure transparency and reproducibility, below is an overview of the data used in this analysis:

---
| Attribute | Detail | Notes |
| :--- | :--- | :--- |
| **Dataset Size** | 1,000 records (rows) × 17 columns | One row per patient admission event |
| **Data Source** | Simulated healthcare dataset | Modeled after real-world hospital admissions systems |
| **Date Range** | January 1, 2023 – December 31, 2023 | Mid-year sample covering peak admission period |
| **Key Fields** | Patient_ID, Admission_Date, Month, Quarter | Department, Age_Group, Gender, Insurance_Type |
| **Clinical Metrics** | Length_of_Stay, Treatment_Cost, Bed_Occupancy_Rate | Patient_Satisfaction, Readmitted, Discharge_Outcome |
| **Operational Fields** | Staff_on_Shift, Month_Num | Used for staffing trend and workload analysis |
---

> **Note:** This dataset was used for analytical and portfolio purposes. All patient identifiers are simulated and do not represent real individuals.

_______________________________________________________________________________________________________________________





## **Business Impact**
This analysis directly supported leadership decision-making and operational planning at Greenwood Hospital. Key outcomes include:

* Improved visibility into patient flow across departments, enabling real-time monitoring of admission volumes and capacity utilization.
* Enabled leadership to align staffing with peak demand periods, particularly in the Emergency and Cardioology departments where seasonal spikes were identified.
* Reduced reliance on manual reporting by implementing automated Power BI dashboards connected to validated SQL data — replacing ad hoc spreadsheet workflows.
* Supported proactive resource planning by surfacing the Weekend vs. Weekday admission gap, giving operations teams data to justify adjusted scheduling models.
* Provided a repeatable audit framework through SQL validation queries that compare dashboard figures directly against source records, increasing confidence in reported metrics.
---










_______________________________________________________________________________________________________________________
# Technical Process & Business Logic

1. Data Transformation (Power Query)

	• Data Types: Ensuring admission and discharge dates are formatted correctly to calculate "Length of Stay".

	• Custom Columns: Creating a Weekend vs. Weekday flag to see if staffing needs to be adjusted for Saturday/Sunday spikes.

	• Filtering: Removing any test records or outliers (e.g., negative length of stay) to ensure the 2023 overview is accurate.


2. DAX Calculations (The "Metrics")

	• Total Admissions: Total Admissions = COUNTROWS('Greenwood_Admissions')
  
	• Average Length of Stay (ALOS): ALOS = AVERAGE('Greenwood_Admissions'[Days_In_Hospital])
  
	• Occupancy Rate: This demonstrates high-level business logic. Occupancy Rate = DIVIDE([Total Admissions], [Total Bed Capacity])

3. The "Accessible Tidal" Theme

Ensuring high-contrast visuals allows for quick decision-making during fast-paced clinical rounds where lighting and screen quality may vary.

	• Improve Readability: For users with color vision deficiencies (Color Blindness).
  
	• Standardize Branding: Maintaining a professional "Healthcare Blue" aesthetic while ensuring 2023 data points remain the focal point.


_______________________________________________________________________________________________________________________

## **Strategic Summary & Project Audit**
---

| Feature | Strategic Focus | Implementation Detail |
| :--- | :--- | :--- |
| **Business Value** | Patient Wait Times | Explained how this dashboard identifies bottlenecks to reduce hospital wait times. |
| **Data Integrity** | Source Validation | Documented the SQL validation process comparing 2023 clinical data to source records. |
| **Scrum Influence** | Sprint Logic | Organized development into Sprints (e.g., Sprint 1: Architecture, Sprint 2: Visualization). |
| **The "So What?"** | Actionable Insight | 40% Cardiology Q3 surge enabling specialist staffing. |

---





_______________________________________________________________________________________________________________________

## **Data Definition & Cleaning**
---
---
1. Data Standardization & Logic
To ensure consistent reporting, I standardized entry types and engineered clinical metrics using SQL.
---
Query 1 — Standardizing Admission Types
This query normalizes inconsistent admission type labels (e.g., `'ER'`, `'Emerg'`, `'Urgent'`) into a single standardized value. Consistent categorization is critical for accurate department-level reporting and prevents undercounting Emergency admissions in aggregate views.
```sql
UPDATE Greenwood_Admissions
SET Admission_Type = 'Emergency'
WHERE Admission_Type IN ('ER', 'Emerg', 'Urgent');
```
---
Query 2 — Calculating Length of Stay (LOS)
This query calculates each patient's length of stay in days by finding the difference between admission and discharge dates. LOS is a foundational metric for occupancy planning, resource allocation, and identifying departments with abnormally long or short stays.
```sql
SELECT 
    Patient_ID,
    Admission_Date,
    Discharge_Date,
    DATEDIFF(day, Admission_Date, Discharge_Date) AS Length_Of_Stay
FROM Greenwood_Admissions
WHERE Admission_Date >= '2023-01-01';
```
---
Query 3 — Identifying Top 5 Departments by Volume
This query surfaces the five departments with the highest admission counts for the year. Understanding volume concentration helps leadership prioritize staffing, budget, and capacity investments where demand is greatest.
```sql
SELECT TOP 5 
    Department_Name, 
    COUNT(Admission_ID) AS Total_Admissions
FROM Greenwood_Admissions
GROUP BY Department_Name
ORDER BY Total_Admissions DESC;
```





_______________________________________________________________________________________________________________________
















	




_______________________________________________________________________________________________________________________	
		
## **Future Enhancements & Roadmap**

As part of the continuous improvement lifecycle (Agile/Scrum), the following features are prioritized for the next development sprint:

* Predictive Forecasting: Integrating SQL trend analysis to forecast 2024 admission volumes based on the identified 2023 seasonal spikes.
* Geospatial Mapping: Utilizing patient zip code data to create a heat map of admissions, assisting in identifying community outreach opportunities.
* Advanced DAX Time-Intelligence: Implementing Year-over-Year (YoY) and Month-over-Month (MoM) growth metrics to provide deeper context for departmental shifts.
* Automated Data Pipeline: Transitioning from flat-file uploads to a live SQL Server connection (on-premise or cloud-based) to ensure real-time reporting accuracy.		
		
			
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		

		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		






