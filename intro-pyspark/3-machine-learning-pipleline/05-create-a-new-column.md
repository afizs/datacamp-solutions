# Create a new column

### Solution: 
```
# Create the column plane_age
model_data = model_data.withColumn("plane_age", model_data.year - model_data.plane_year)
```
### Output excepted:
```
+-------+----+-----+---+--------+---------+--------+---------+-------+------+------+----+--------+--------+----+------+----------+--------------------+--------------+-----------+-------+-----+-----+---------+---------+
|tailnum|year|month|day|dep_time|dep_delay|arr_time|arr_delay|carrier|flight|origin|dest|air_time|distance|hour|minute|plane_year|                type|  manufacturer|      model|engines|seats|speed|   engine|plane_age|
+-------+----+-----+---+--------+---------+--------+---------+-------+------+------+----+--------+--------+----+------+----------+--------------------+--------------+-----------+-------+-----+-----+---------+---------+
| N846VA|2014|   12|  8|     658|       -7|     935|       -5|     VX|  1780|   SEA| LAX|     132|     954|   6|    58|      2011|Fixed wing multi ...|        AIRBUS|   A320-214|      2|  182|   NA|Turbo-fan|      3.0|
| N559AS|2014|    1| 22|    1040|        5|    1505|        5|     AS|   851|   SEA| HNL|     360|    2677|  10|    40|      2006|Fixed wing multi ...|        BOEING|    737-890|      2|  149|   NA|Turbo-fan|      8.0|
| N847VA|2014|    3|  9|    1443|       -2|    1652|        2|     VX|   755|   SEA| SFO|     111|     679|  14|    43|      2011|Fixed wing multi ...|        AIRBUS|   A320-214|      2|  182|   NA|Turbo-fan|      3.0|
| N360SW|2014|    4|  9|    1705|       45|    1839|       34|     WN|   344|   PDX| SJC|      83|     569|  17|     5|      1992|Fixed wing multi ...|        BOEING|    737-3H4|      2|  149|   NA|Turbo-fan|     22.0|
| N612AS|2014|    3|  9|     754|       -1|    1015|        1|     AS|   522|   SEA| BUR|     127|     937|   7|    54|      1999|Fixed wing multi ...|        BOEING|    737-790|      2|  151|   NA|Turbo-jet|     15.0|
| N646SW|2014|    1| 15|    1037|        7|    1352|        2|     WN|    48|   PDX| DEN|     121|     991|  10|    37|      1997|Fixed wing multi ...|        BOEING|    737-3H4|      2|  149|   NA|Turbo-fan|     17.0|
| N422WN|2014|    7|  2|     847|       42|    1041|       51|     WN|  1520|   PDX| OAK|      90|     543|   8|    47|      2002|Fixed wing multi ...|        BOEING|    737-7H4|      2|  140|   NA|Turbo-fan|     12.0|
| N361VA|2014|    5| 12|    1655|       -5|    1842|      -18|     VX|   755|   SEA| SFO|      98|     679|  16|    55|      2013|Fixed wing multi ...|        AIRBUS|   A320-214|      2|  182|   NA|Turbo-fan|      1.0|
| N309AS|2014|    4| 19|    1236|       -4|    1508|       -7|     AS|   490|   SEA| SAN|     135|    1050|  12|    36|      2001|Fixed wing multi ...|        BOEING|    737-990|      2|  149|   NA|Turbo-jet|     13.0|
| N564AS|2014|   11| 19|    1812|       -3|    2352|       -4|     AS|    26|   SEA| ORD|     198|    1721|  18|    12|      2006|Fixed wing multi ...|        BOEING|    737-890|      2|  149|   NA|Turbo-fan|      8.0|
| N323AS|2014|   11|  8|    1653|       -2|    1924|       -1|     AS|   448|   SEA| LAX|     130|     954|  16|    53|      2004|Fixed wing multi ...|        BOEING|    737-990|      2|  149|   NA|Turbo-jet|     10.0|
| N305AS|2014|    8|  3|    1120|        0|    1415|        2|     AS|   656|   SEA| PHX|     154|    1107|  11|    20|      2001|Fixed wing multi ...|        BOEING|    737-990|      2|  149|   NA|Turbo-jet|     13.0|
| N433AS|2014|   10| 30|     811|       21|    1038|       29|     AS|   608|   SEA| LAS|     127|     867|   8|    11|      2013|Fixed wing multi ...|        BOEING|  737-990ER|      2|  222|   NA|Turbo-fan|      1.0|
| N765AS|2014|   11| 12|    2346|       -4|     217|      -28|     AS|   121|   SEA| ANC|     183|    1448|  23|    46|      1992|Fixed wing multi ...|        BOEING|    737-4Q8|      2|  149|   NA|Turbo-fan|     22.0|
| N713AS|2014|   10| 31|    1314|       89|    1544|      111|     AS|   306|   SEA| SFO|     129|     679|  13|    14|      1999|Fixed wing multi ...|        BOEING|    737-490|      2|  149|   NA|Turbo-jet|     15.0|
| N27205|2014|    1| 29|    2009|        3|    2159|        9|     UA|  1458|   PDX| SFO|      90|     550|  20|     9|      2000|Fixed wing multi ...|        BOEING|    737-824|      2|  149|   NA|Turbo-fan|     14.0|
| N626AS|2014|   12| 17|    2015|       50|    2150|       41|     AS|   368|   SEA| SMF|      76|     605|  20|    15|      2001|Fixed wing multi ...|        BOEING|    737-790|      2|  151|   NA|Turbo-jet|     13.0|
| N8634A|2014|    8| 11|    1017|       -3|    1613|       -7|     WN|   827|   SEA| MDW|     216|    1733|  10|    17|      2014|Fixed wing multi ...|        BOEING|    737-8H4|      2|  140|   NA|Turbo-fan|      0.0|
| N597AS|2014|    1| 13|    2156|       -9|     607|      -15|     AS|    24|   SEA| BOS|     290|    2496|  21|    56|      2008|Fixed wing multi ...|        BOEING|    737-890|      2|  149|   NA|Turbo-fan|      6.0|
| N215AG|2014|    6|  5|    1733|      -12|    1945|      -10|     OO|  3488|   PDX| BUR|     111|     817|  17|    33|      2001|Fixed wing multi ...|BOMBARDIER INC|CL-600-2C10|      2|   80|   NA|Turbo-fan|     13.0|
+-------+----+-----+---+--------+---------+--------+---------+-------+------+------+----+--------+--------+----+------+----------+--------------------+--------------+-----------+-------+-----+-----+---------+---------+
only showing top 20 rows
```