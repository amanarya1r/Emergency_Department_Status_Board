# Emergency Department Status Board
Emergency Department Status Board in short EDSB → EB is a excel dashboard for patients referred to hospital emergency department.

*The purpose EDSB:* is to create a Hospital emergency room analysis dashboard in excel to imporve efficiency and provide useful insights. The dashboard will help stakeholders to monitor, analyze and make better decisions for managing patients and improving services.


## How Status Board Works:
![Status_Board_Working](/img/working.gif)


## Project Steps
* Business Requirement Gathering 
* Understanding of Data 
* Data Connection (Import Data Using Power Query)
* Data Cleaning and Data Quality Check Using Power Query
* Creating Calendar Table using Power Query 
* Data Modeling Power Pivot 
* Adding Required Ciolumns(DAX Calculation in Power Pivot) ← DAX means data analysis expression
* Creating Pivots and DAshboard Lay outing
* Charts Development and Formatting 
* Dashboard / Report Devlopment
* Insight Generation


## Requirement of EDSB:
* **Number of Patients:** Count of total number of patients visiting the ER each day. Showing a daily trend with an area or column chart to spot patterns like busy days or seasonal trends.
* **Average Wait Time:** Finding the average time patients wait to see a medical professional. Using an area or column chart to track daily changes and highlighting days with longer wait times that might need improvements.
* **Patient Satisfaction Score:** Checking the average daily satisfaction score of patients to assess service quality


## Charts to Create:
* **Patient Admission Status:** Showing how many patients were admitted vs. not admitted.
* **Patient Age Distribution:** Grouping patients by age(difference of 10 years).
* **Timeliness:** Measuring the percentage of patients seen withing 30 minutes.
* **Gender Analysis:** Displaying the number of patients by gender.
* **Department Referrals:** Checking which departments patients are referred to the most.

  > **Here is the data set:** [EDSB_data.csv](data_set/EDSB_data.csv)


## Formula Used:
1. **For sorting dates:** ```= List.Dates(#date(2023,01,01),731,#duration(1,0,0,0))```
	> * ```#date(2023,01,01)``` → is a start date
	> * ```731``` → its for two years [365+366=731](since there is a leap year)
	> * ```#duration(1,0,0,0)``` → it's for interval of 1 day
2. **For inserting age groups:** ```=IF([Patient Age]>70,"70-79")```
   > * ```IF``` → starts a conditional statement.
   > * ```[Patient Age]>70``` → checks if the patient's age is greater than 70.
   > * `"70-79"` → if true (age > 70), it leaves the cell blank, if false it leaves the cell blank.
3. **For inserting response time:** → it a measure of time of medical staff attending the patient. <br>`=IF([Patient Waittime]>30,"Delay","Ontime")`
   > * `IF` → starts the conditional logic.
   > * `[Patient Waittime]>30` → checks if the patient's wait time exceeds 30 minutes.
   > * `Delay` → if true (wait time > 30), the cell will display "Delay".
   > * `Ontime` → if false (wait time <= 30), the cell will display "Ontime".
4. **`J33: =GETPIVOTDATA("[Measures].[Average of Patient Waittime]", $G$4)`**  
   >* Pulls the **"Average of Patient Waittime"** value from the PivotTable located at `$G$4`.  
   *(This is a cube function used in Power Pivot or Data Model-based PivotTables.)*
5. **`K33: =TEXT(J33, "0.00") & " mins"`**  
   >* Formats the value in `J33` to **2 decimal places** and adds the **label " mins"**.
6. **`J37: =GETPIVOTDATA("[Measures].[Average of Patient Satisfaction Score]", $A$17)`**  
   >*  Fetches the **Average Satisfaction Score** from the PivotTable located at `$A$17`.

7. **`K37: =TEXT(J37, "0.00")`**  
    >*  Converts the numeric score from `J37` to a **text** string with **2 decimal places**.
8. **For adding data into charts:** `='Pivot Report'!A5`
   > * `Pivot Report'!A5` → references cell A5 from the worksheet named "Pivot Report".
   > * The formula fetches data from that specific cell in  another sheet.
   > * Same process done for other charts.


## Data Relationship:
![connection.png](/img/connection.png)
> * Since the Date inside the Calendar_Table is unique that's why indicated as '1'.
> * But there multiple common Date inside the EDSB_data that's why indicated as '*'.
> * Such a relationship between tables which is defined by a pipe in the following image is called cardinality. *Three types of cardinality:*
> 	1. One to Many
>   2. Many to One
>   3. Many to Many
> * This relation can be seen via arrow in the pipe ![arrow.png](/img/arrow.png) currently it's a One to Many relationship
---

