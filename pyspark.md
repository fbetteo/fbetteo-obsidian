## PySpark

##### Data type
When going from pyspark dataframe to pandas df with .toPandas() don't forget to declare the type of numeric columns if they are not properly signaled. **Massive difference in times.**

```python
df_spark\
	.withColumn('numeric_col', df['numeric_col'].cast(DoubleType())\
	.toPandas()
```

#### Division
 When dividing decimal columns (at least in a SQL query) remember to round or change the type to avoid memory overflow. Might be a better way but this worked for me.

```sql
SELECT
	other_col,
	round(decimal_col1,2) / round(decimal_col2, 2) division
FROM db.table

```

#### Create list for where clause
SQL requires a list of the form :
``` sql
select * from db.table
where col1 in ('XX', 'YY')
```

To generate that list in python from a dataframe we can do:
```python
list_of_prods = ', '.join([f"'{x}'" for x in list(df.col)])
query = f"select * from db.table where col1 in ({list_of_prods})
```
We generate the string as it's needed and include it in a sql query.
If we are in spark we can later:
```python
df_sql = spark.sql(query).toPandas()
```

## Optimization

Temp view are recalculated each time they are used. We can cache it or create a temporary table if it's required multiple times

Hive partitions define how partitioned the original table is. Smaller subtables are generated (by state for ex)

Filtrar un dataset para join por una partition es critico.

Change spart.sql.shuffle.partitions? suffle stage input / 200 aprox ponele
check shuffle spill. If you have in the median, probably need to increase partitions


maxpartitionbytes? some reasons to modify the default. bringin it down.
If we are not using all the partitiions availbe, we can decrease to increase parallelims.

dataframe.rdd.partitions.size

optimize()

tambein es problematico si tenes spill muy alto en el max y median 0. tenes algo muy desbalanceado. tambien se ve en el shuffle read

skewjoin? Maybe for products, some must sell in much more stores than others?

pandasUdf

filter data on read
