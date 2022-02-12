```
# Import pyspark.sql.functions as F
import pyspark.sql.functions as F

# Group by month and dest
by_month_dest = flights.groupBy('month', 'dest')

# Average departure delay by month and destination
by_month_dest.avg('dep_delay').show()

# Standard deviation of departure delay
by_month_dest.agg(F.stddev('dep_delay')).show()
```

## OutPut:

```
+-----+----+--------------------+
|month|dest|      avg(dep_delay)|
+-----+----+--------------------+
|   11| TUS| -2.3333333333333335|
|   11| ANC|   7.529411764705882|
|    1| BUR|               -1.45|
|    1| PDX| -5.6923076923076925|
|    6| SBA|                -2.5|
|    5| LAX|-0.15789473684210525|
|   10| DTW|                 2.6|
|    6| SIT|                -1.0|
|   10| DFW|  18.176470588235293|
|    3| FAI|                -2.2|
|   10| SEA|                -0.8|
|    2| TUS| -0.6666666666666666|
|   12| OGG|  25.181818181818183|
|    9| DFW|   4.066666666666666|
|    5| EWR|               14.25|
|    3| RDM|                -6.2|
|    8| DCA|                 2.6|
|    7| ATL|   4.675675675675675|
|    4| JFK| 0.07142857142857142|
|   10| SNA| -1.1333333333333333|
+-----+----+--------------------+
    only showing top 20 rows
only showing top 20 rows

+-----+----+----------------------+
|month|dest|stddev_samp(dep_delay)|
+-----+----+----------------------+
|   11| TUS|    3.0550504633038935|
|   11| ANC|    18.604716401245316|
|    1| BUR|     15.22627576540667|
|    1| PDX|     5.677214918493858|
|    6| SBA|     2.380476142847617|
|    5| LAX|     13.36268698685904|
|   10| DTW|     5.639148871948674|
|    6| SIT|                  null|
|   10| DFW|     45.53019017606675|
|    3| FAI|    3.1144823004794873|
|   10| SEA|     18.70523227029577|
|    2| TUS|    14.468356276140469|
|   12| OGG|     82.64480404939947|
|    9| DFW|    21.728629347782924|
|    5| EWR|     42.41595968929191|
|    3| RDM|      2.16794833886788|
|    8| DCA|     9.946523680831074|
|    7| ATL|    22.767001039582183|
|    4| JFK|     8.156774303176903|
|   10| SNA|    13.726234873756304|
+-----+----+----------------------+
only showing top 20 rows
```