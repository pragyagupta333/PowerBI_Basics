# DAX With Implementation
# FILTER
Returns a table that represents a subset of another table or expression. 
#### Syntax 
```
FILTER(<table>, <filter>)
```
-> table : The table to be filtered.
#### Example : Go to CALCULATE section


# CALCULATE
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

# CALCULATETABLE
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


# SUM
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


# SUMX
Returns the sum of an expression evaluated for each row in a table. With this function you can operate on multiple columns in table row wise.

#### Syntax 
```
SUMX(<table>, <expression>)
```
#### Example :
``` 
TotalSales_SUMX = SUMX(Data, Data[Price Per Unit] * Data[Quantity])
```

# Difference SUM and SUMX
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

# ALL 
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


# ALLSELECTED
Returns all the values from the specified column or table, considering only the filters applied by the user and ignoring any context filters from visuals

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

# ALLEXCEPT
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


# Compare ALL, ALLSELECTED, ALLEXCEPT
- "ALL" removes all filters, disregarding user-applied and context filters.
- "ALLSELECTED" preserves user-applied filters but removes context filters.
- "ALLEXCEPT" removes filters from a specific column or table but preserves filters from selected columns or tables.

# ADDCOLUMNS
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

# SUMMARIZE 
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
