## PySpark
* When going from pyspark dataframe to pandas df with .toPandas() don't forget to declare the type of numeric columns if they are not properly signaled. **Massive difference in times.**

```python
df_spark\
	.withColumn('numeric_col', df['numeric_col'].cast(DoubleType())\
	.toPandas()
```

* When dividing decimal columns (at least in a SQL query) remember to round or change the type to avoid memory overflow. Might be a better way but this worked for me.

```sql
SELECT
	other_col,
	round(decimal_col1,2) / round(decimal_col2, 2) division
FROM db.table

```
