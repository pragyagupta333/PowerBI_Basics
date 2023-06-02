
# DAX 
DAX is a library of functions and operators that can be combined to build formulas and
expressions used By Microsoft BI Tools

## Where DAX Can be used
-    Power BI
-    Power Pivot for Excel
-   SSAS Tabular Model
-  Azure Analysis Services

## Purpose Of DAX
By using Data Analysis Expressions (DAX),

You can add three types of calculations to your data model

**1. Calculated columns :**
- Calculated columns are computed based on data that has already been **loaded** into your data model. 
- When you write a calculated column formula, it is automatically applied to the whole table and evaluated individually **for each row**. 
- The values in calculated columns are evaluated when you first define them and when you refresh your dataset. 
- Once evaluated, the column is **stored along with table** i.e **consumes memory**.
- Involves **simple** calculations


Example :  
- Name column : Firstname + lastname combine together
- Amount Column  :  Quantity * UnitPrice
- Profit column : Selling value - purchasing value

**2. Measures** :

- Measured columns are used as an **aggregate calculation** such as sum, average, minimum, maximum, total, counts, or more.
- Measured columns are computed at query time(**real-time**) and **not stored in memory**
- A measure is stored in the model as source code, but it is computed only when it is used in the report.
- Involves Complex calculations
Example :
-  Total sales column : Sum(sales)
- Average Salary column : Average(salary)
- Total Profit : Sum(selling value - purchasing value)
**3. Calculated tables** : 

- A calculated table can be defined as a Virtual Table that is created by using a physical Table through DAX (Data Analytic Expression). 

- Example :  Creating aggregated tables from a given table
- Date tables
- Role-playing dimensions
- What-if analysis

## Naming Fundamental 

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/c58ca69d-50a6-42ea-bfe0-85babf653ed2)

## Data Types For DAX
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/d1e21b7c-80a1-4140-969f-a12c1a2d39e7)

## Acknowledgements

 - [Dax By Analytics With Nags](https://www.youtube.com/watch?v=yTOSOgUGKe4&t=7909s)
 - [Measures by EnjoySharePoint](https://www.enjoysharepoint.com/create-a-measure-in-power-bi/)
 - [How to write a Good readme](https://bulldogjob.com/news/449-how-to-write-a-good-readme-for-your-github-project)
