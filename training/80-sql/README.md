# r_sql
Daniel Chen  
  



# Connecting to a SQL database


```r
library(sdalr)
library(DBI)
library(RPostgreSQL)

con <- con_db(dbname = 'arlington', user = 'chend')
```




```r
con
```

```
## <PostgreSQLConnection>
```

# Getting (Querrying) data from database in R


```r
df <- dbGetQuery(con, "")
head(df)
```

```
## NULL
```

You can also use an `sql` chunk
```
```{sql connection=con}
SELECT * FROM fire.medic_unit_movement_summary_2013;
```
```


```sql
SELECT * FROM fire.medic_unit_movement_summary_2013;
```


<div class="knitsql-table">


Table: Displaying records 1 - 10

    id  incidentdate       incidentnumber   doy   number   streetprefix   street           streettype   city        apparatusid   use   unit    unit_from   arrivedfacilitydatetime   shift   month     dow         hod   dispatchdatetime   arrivaldatetime    enroutescenedatetime   area   district   alarmdatetime      cleardatetime      resp_time   out_time   time_on_call   new_cp_time   call_processing_time 
------  -----------------  ---------------  ----  -------  -------------  ---------------  -----------  ----------  ------------  ----  ------  ----------  ------------------------  ------  --------  ----------  ----  -----------------  -----------------  ---------------------  -----  ---------  -----------------  -----------------  ----------  ---------  -------------  ------------  ---------------------
 31962  10/10/2012 0:09    2840001          106   1111                    ARMY NAVY        DR           Arlington   M105B               M105B   Ours        10/10/2012 0:42           C       October   Wednesday   0     10/10/2012 0:10    10/10/2012 0:14    10/10/2012 0:12        Arl    7510       10/10/2012 0:09    10/10/2012 1:07    0:04:39     0:01:56    0:55:33        0:00:22       0:00:57              
 55026  10/10/2012 0:22    2840002          331   1900                    COLUMBIA PK                   Arlington   M101                M101    Ours                                  C       October   Wednesday   0     10/10/2012 0:22    10/10/2012 0:42    10/10/2012 0:24        Arl    7110       10/10/2012 0:22    10/10/2012 0:51                0:01:50    0:26:40        0:00:15       0:00:33              
 47369  10/10/2012 0:39    2840003          281   4709                    7TH              ST           Arlington   M109                M109    Ours        10/10/2012 1:13           C       October   Wednesday   0     10/10/2012 0:40    10/10/2012 0:47    10/10/2012 0:42        Arl    7117       10/10/2012 0:39    10/10/2012 2:28    0:07:13     0:02:15    1:45:31        0:00:34       0:01:22              
 26206  10/10/2012 10:28   2840024          75    1211     S              EADS             ST           Arlington   M105                M105    Ours        10/10/2012 10:57          A       October   Wednesday   10    10/10/2012 10:28   10/10/2012 10:32   10/10/2012 10:29       Arl    7504       10/10/2012 10:27   10/10/2012 11:52   0:04:00     0:00:55    1:23:22        0:00:26       0:01:17              
  7664  10/10/2012 10:29   2840025          356   3452     S              WAKEFIELD        ST           Arlington   M109                M109    Ours                                  A       October   Wednesday   10    10/10/2012 10:29                      10/10/2012 10:30       Arl    7701       10/10/2012 10:28   10/10/2012 10:36               0:00:55    0:05:24        0:00:22       0:01:44              
 35065  10/10/2012 1:04    2840004          238   2467     S              LOWELL           ST           Arlington   M101                M101    Ours        10/10/2012 1:31           C       October   Wednesday   1     10/10/2012 1:04    10/10/2012 1:09    10/10/2012 1:06        Arl    7905       10/10/2012 1:03    10/10/2012 2:12    0:05:02     0:01:29    1:05:49        0:00:19       0:01:03              
 54039  10/10/2012 10:59   2840027          126   1728                    KIRKWOOD         RD           Arlington   M104                M104    Ours        10/10/2012 11:31          A       October   Wednesday   11    10/10/2012 11:00   10/10/2012 11:12   10/10/2012 11:03       Arl    7409       10/10/2012 10:59   10/10/2012 12:46               0:03:51    1:43:01        0:00:30       0:00:30              
 15862  10/10/2012 1:11    2840006                4300     N              CARLIN SPRINGS   RD           Arlington   M102                M102    Ours                                  C       October   Wednesday   1     10/10/2012 1:11    10/10/2012 1:14    10/10/2012 1:12        Arl    7203       10/10/2012 1:10    10/10/2012 1:20    0:02:38     0:01:14    0:07:50        0:00:15       0:01:15              
 48867  10/10/2012 11:20   2840029          190   3650     S              GLEBE            RD           Arlington   M301                M301    Outside     10/10/2012 11:53          A       October   Wednesday   11    10/10/2012 11:21   10/10/2012 11:29   10/10/2012 11:24       Arl    7506       10/10/2012 11:19   10/10/2012 12:42   0:07:42     0:03:08    1:17:56        0:00:49       0:01:53              
 22090  10/10/2012 11:23   2840030          211   1320     N              VEITCH           ST           Arlington   M110                M110    Ours        10/10/2012 11:50          A       October   Wednesday   11    10/10/2012 11:25   10/10/2012 11:28   10/10/2012 11:26       Arl    7003       10/10/2012 11:23   10/10/2012 12:23   0:03:31     0:01:13    0:56:32        0:01:19       0:01:40              

</div>

# Creating Tables

Everyone should have a database named their pid.


```r
con <- con_db(dbname = 'chend', user = 'chend')
```




```r
# delete a table (you have permissino to do)
dbSendQuery(con, "DROP TABLE customers;")

# create a new table
dbSendQuery(con, "CREATE TABLE customers (customer_no INTEGER, first_name TEXT, last_name TEXT);")

# add row information into a table
dbSendQuery(con, "INSERT INTO customers (customer_no, first_name, last_name) VALUES (1, 'MC', 'Hammer')")
dbSendQuery(con, "INSERT INTO customers (customer_no, first_name, last_name) VALUES (2, 'Hammer', 'Time')")
```



```r
dbGetQuery(con, "SELECT * from customers;")
```

```
##   customer_no first_name last_name
## 1           1         MC    Hammer
## 2           2     Hammer      Time
```


```r
# save results to an R data frame

df <- dbGetQuery(con, "SELECT * from customers;")
df
```

```
##   customer_no first_name last_name
## 1           1         MC    Hammer
## 2           2     Hammer      Time
```

# Get data in and out of chunks

When working with chunks that use different language engines, you will need a way to pass
R values into the engine, and get values back into the R environment

## Get R data into chucnks

To get an `R` variable into a chunk, you prepend the R variable by a `?`, e.g., `?x` if you want to use the variable `x` in the chunk


```r
x <- 'Time'
```

```
```{sql connection=con}
SELECT * FROM ?x;
```
```


```sql
SELECT * FROM customers WHERE last_name = ?x;
```


<div class="knitsql-table">


Table: 1 records

customer_no   first_name   last_name 
------------  -----------  ----------
2             Hammer       Time      

</div>

## Get chucnk values into R

You can get values and results back into the R enviornment by using the `output.var` chunk option

```
```{sql connection=con, output.var=df_sql}
SELECT * FROM ?x;
```
```


```sql
SELECT * FROM customers WHERE last_name = 'Hammer';
```

In R:


```r
df_sql
```

```
##   customer_no first_name last_name
## 1           1         MC    Hammer
```

