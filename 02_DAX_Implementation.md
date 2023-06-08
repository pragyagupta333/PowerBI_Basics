# DAX With Implementation

Some Dax Functions are discussed below : 

## FILTER
Returns a table that represents a subset of another table or expression. 
#### Syntax 
```
FILTER(<table>, <filter>)
```
-> table : The table to be filtered.
#### Example : Go to CALCULATE section


## CALCULATE
Evaluates an expression wrt a condition in FILTER
#### Syntax 
```
CALCULATE( <expression>, <filter1>, <filter2>… )
-> Expression :  aggregation function like SUM, MIN,etc
```
#### Example : Measure for total sales for product category "Casual Wear"
``` 
TotalSales_CasualWear = 
CALCULATE(SUM(Data[Sales Amount]),filter(Data,Data[Product Category]="Casual Wear"))
```
#### Example : Measure for total sales for product category "Casual Wear" and product name as " Jeans Levis"
``` 
TotalSales_Category_JeansLevis = 
CALCULATE(SUM(Data[Sales Amount]),FILTER(Data, AND(Data[Product Category]="Casual Wear",Data[Product name]="Jeans Levis")))
```

## CALCULATETABLE
Allows you to create virtual tables that you can filter using multiple conditions and use that table to make further calculations.

#### Syntax 
```
CALCULATETABLE(<expression> , <filter1>  , <filter2> , …)
```

-> Expression : Table

#### Example : 
````
Sales_More_Than_1200_ct =
 SUMX(CALCULATETABLE(Data, Data[Sales Amount]>1200),Data[Sales Amount])

````
Created a  virtual table that you can filter using multiple conditions and use that table to make further calculations.

Perform sum of sales only for sales amount > 1200  

## Difference Between Calculate And CalculatedTable 

CALCULATETABLE is identical to CALCULATE, except for the result: it returns a table instead of a scalar value.

## SUM
Calculates the sum of all numbers in a column.

**Note**  : SUM support only single argument.

#### Syntax 
```
SUM(<Column>)
```
#### Example :
``` 
TotalSales = SUM(Data[Sales Amount])

Profit = SUM(Data[Sales Amount]) - SUM(Data[Cost Amount])
```


## SUMX
Returns the sum of an expression evaluated for each row in a table. With this function you can operate on multiple columns in table row wise.

#### Syntax 
```
SUMX(<table>, <expression>)
```
#### Example :
``` 
TotalSales_SUMX = SUMX(Data, Data[Price Per Unit] * Data[Quantity])
```

## Difference SUM and SUMX
SUM : takes only one column as its argument. Calculates sum value for entire column

#### Syntax 
```
SUM(<Column>)
```

SUMX :  can take single amd multiple columns. Calculates the expression for each row in the table and returns the sum of those values.
#### Syntax 
```
SUMX(<table>, <expression>)
```

[Refer](https://www.antmanbi.com/post/sum-vs-sumx-in-dax)

## ALL 
Removes all filters from column or table and returns all the values from that column or table.
#### Syntax 
```
ALL ({<table> or <column>, [<column>], [<column>] …})
```

#### Example
````
ALL_DAX = 
CALCULATE(Sum(Orders[Sales]), ALL(Orders[Product Sub-Category]))
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/23162fdd-dbbc-44e1-82c1-d773048b25c0)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/ad1262a8-7f0b-4463-beb9-d9a24eb66612)


## ALLSELECTED
Returns all the values from the specified column or table, considering only the filters applied by the user and ignoring any context filters from visuals.

#### Syntax 
```
ALLSELECTED ([<tableName> or <columnName>])
```

#### Example
````
ALLSELECTED_DAX =

CALCULATE(Sum(Orders[Sales]),ALLSELECTED(Orders[Product Sub-Category]))
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/ac1999ce-9726-41bc-93d4-0a683cd582ad)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/d5ac123b-c698-451a-82fa-233bddd17dc2)

## ALLEXCEPT
The "ALLEXCEPT" function removes filters from a specific column or table, similar to the "ALL" function. However, it preserves filters from selected columns while removing filters from other columns. 

#### Syntax 
```
ALLEXCEPT (<table>, <column>, [<column>] …)
```

#### Example
````
ALLEXCEPT_SALES =
CALCULATE (
    SUM ( Orders[Sales] ),
    ALLEXCEPT ( Orders, Orders[Product Category] )
)
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/4779079a-c7cf-4fc4-986a-d0e53cbcdd29)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/b8194090-6d8a-46a1-9946-eb546c89caa7)


## Compare ALL, ALLSELECTED, ALLEXCEPT
- "ALL" removes all filters, disregarding user-applied and context filters.
- "ALLSELECTED" preserves user-applied filters but removes context filters.
- "ALLEXCEPT" removes filters from a specific column or table but preserves filters from selected columns or tables.

## ADDCOLUMNS
 Adds calculated columns to the given table or table expression.
  
 It will return a table with all its original columns and the added ones.
#### Syntax 
```
ADDCOLUMNS(<table>, <name>, <expression>[ <name>, <expression>]…)

```

#### Example
````
NewTable_addcol = 
ADDCOLUMNS('Product Master',"Product Detail",'Product Master'[Category name] & "-" & 'Product Master'[Product Name])
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/609951d6-b54c-4a51-8eb5-16efb35da717)

## SUMMARIZE 
Returns a summary table for the requested totals over a set of groups.
#### Syntax 
```
SUMMARIZE (<oldtable>, <groupBy_columnName_1>, <groupBy_columnName_2> …,
   <name_column>, <expression> …)
```
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/ef96382d-48e7-4655-ae9d-04b23248bcbe)

#### Example
````
SummarizeTable = SUMMARIZE(Data,Data[Product Category], Data[Product Name],
"Total Profit", sum(Data[Profit]),
"Total Sales", SUM(Data[Sales Amount]),
"Total Cost",SUM(Data[Cost Amount])
)
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/dc715098-d6fb-40a1-b479-980fa7378270)

Note : "Data" is already created old table whose columns will be added into new summarizeTable



## Group By

Returns a table with a set of selected columns.

GROUP BY permits DAX CURRENTGROUP function to be used inside aggregation functions in the extension columns that it adds.

It attempts to reuse the data that has been grouped making it highly performant.

#### Syntax 
```
GROUPBY (<table>, [<groupBy_columnName1>], [<name>, <expression>]… )
```

#### Example
````
NewTable_GroupBy = 
GROUPBY(Data,Data[Product Category],"Total Profit",SUMX(CURRENTGROUP() ,Data[Profit]))
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/5ac8a3c0-5b24-4050-ba74-4505d02798db)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/9dfb0179-35af-4fc0-a7bf-12c5fc889e8e)

[Refer](https://powerbidocs.com/2020/09/05/dax-groupby-function/)

## Difference Between Group By And Summarize

DAX GROUPBY function is similar to DAX SUMMARIZE function. However, GROUPBY does not do an implicit CALCULATE for any extension columns that it adds.

[Refer](https://www.mavaanalytics.com/post/summarizecolumns)

## DATATABLE 
Create Static Dataset/ Table in Power BI, that cannot be refreshed but you can modify it.

#### Power BI Data Types as below:

- BOOLEAN (True/False)
- CURRENCY (Fixed Decimal Number)
- DATETIME (Date/Time)
- DOUBLE (Decimal Number)
- INTEGER (Whole Number)
- STRING (Text)

#### Syntax 
```
DATATABLE (column1, datatype1,
coulmn2, datatype2,
{
 {value1, value2},
 {value3, value4 }
}
)
```

#### Example
````
NewTable_Datatable = 
DATATABLE ("Name", STRING,
"Age", INTEGER ,
{
 {"Pragya", 23},
 {"Priya", 45 }
}
)
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/9916381e-45c3-462e-b1a3-e4468d29dc6e)

## Row
Row DAX function returns a table with a single row containing values that result from the expressions given to each column.
#### Syntax 
```
ROW(<name>, <expression>, <name>, <expression>.....)
```

#### Example
````
NewTable_Row = ROW("Total Sales", SUM(Data[Sales Amount]),

"Total Profit", SUM(Data[Profit]),

"Total CostAmount", SUM(Data[Cost Amount]))
````
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/879d60ff-9a5b-4788-8e9b-2cc393d67c25)

# CONCATENATE 
Join two text strings into one text string. 

# Syntax
```
CONCATENATE( <Text1>, <Text2> )
```

# Example 
```
CONCATENATE_Result = 

CONCATENATE(Orders[Product Category], Orders[Product Sub-Category])
```

# CONCATENATEX  
 Concatenates the result of an expression evaluated for each row in a table using the specified delimiter.

# Syntax
```
CONCATENATEX (
   <table>, <expression>, [<delimiter>], [<OrderBy_Expression1>],[<Order>] …
)
```

# Example 
```
CONCATENATEX (
            CALCULATETABLE ( VALUES ( 'Product'[Color] ) ),
            Product[Color],
            ", ",             -- Separator (optional)
            Product[Color],   -- Sorting expression (optional)
            ASC               -- Sorting direction (optional)
        )
```

## Difference Between CONCATENATE  And CONCATENATEX
The key difference between CONCATENATE and CONCATENATEX is that CONCATENATE is used to directly concatenate multiple text values, while CONCATENATEX is used to perform calculations on a table or an expression and concatenate the results. CONCATENATEX is particularly useful when you need to concatenate values based on specific conditions or calculations applied to a table.

## Count
Count function counts the number of cells in a column that contains **numbers,** **Dates,** **strings**, **NOT** **boolean**

The Count function **does not count empty** (or) blank values.

The Count function does not count boolean functions but If you include true/false with categorical data and numerical values it will count. The only boolean values does not count.

The Count function contains blank data it returns blank.
#### Syntax 
```
COUNT(<column>)
```

#### Example
````
count_Age = COUNT(Table[Age])
````
#### Demo 
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/d2c23484-e2d0-42d5-9aaa-8ae5349ce71f)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/9fc6cb77-fb92-41f5-8df2-aefc476d0d0a)

## CountA
The COUNTA function counts the number of cells in a column that are not empty for numbers, Dates, strings, **and** **boolean**

The CountA function counts the number of cells in a column.

The CountA function **does not count empty** (or) blank values.

The CountA function contains blank data it returns a blank.

#### Syntax 
```
COUNTA(<column>)
```

#### Example
````
countA_Age = COUNTA(NewTable_Datatable[Age])
````

#### Demo 
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/d2c23484-e2d0-42d5-9aaa-8ae5349ce71f)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/5d5b0932-dba7-42b8-8f26-4b3a75efef2b)

## CountX
CountX function counts the number of cells in a column that contains numbers, Dates, strings, but **not boolean values**

The CountX function **does not count empty** (or) blank values.

The CountX function does not count boolean values but
If you include true/false with categorical data and numerical values it will count. The only boolean values does not count.

#### Syntax 
```
 CountX (<table_name>,<expression>)
```

#### Example
````
countX_Age = COUNTX(NewTable_Datatable,NewTable_Datatable[Age])
````
#### Demo :
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/d2c23484-e2d0-42d5-9aaa-8ae5349ce71f)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/360e7d99-a5db-49e6-8e1d-6593779f6f91)

## CountAX
CountAX function counts numbers, strings,dates,**boolean values**.

CountAX function contain blank data it return blank.

The CountAX function **does not count empty** (or) blank values.

The CountAX function does not count boolean values but If you include true/false with categorical data and numerical values it will count. The only boolean values does not count
#### Syntax 
```
COUNTAX(<table_name>,<expression>)
```

#### Example
````
countAX_Age = COUNTAX(NewTable_Datatable,NewTable_Datatable[Age])
````
#### Demo :
![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/d2c23484-e2d0-42d5-9aaa-8ae5349ce71f)

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/f36ae3d9-b5e7-4321-a73d-0f1386067d14)


## Compare Count, CountX, CountA, CountAX.

![image](https://github.com/pragyagupta333/PowerBI_Basics/assets/125549428/e402d7e5-ef42-43e6-815e-e319218f4c64)

## Yet to document : 
- min,mina,minx,etc
- relational Dax
- time intelligence
- Reference For these : [Here](https://powerbidocs.com/power-bi-dax-functions/)
