# Caching a DataFrame

```
start_time = time.time()

# Add caching to the unique rows in departures_df
departures_df = departures_df.distinct().cache()

# Count the unique rows in departures_df, noting how long the operation takes
print("Counting %d rows took %f seconds" % (departures_df.count(), time.time() - start_time))

# Count the rows again, noting the variance in time of a cached DataFrame
start_time = time.time()
print("Counting %d rows again took %f seconds" % (departures_df.count(), time.time() - start_time))
```

Output: 
```
Counting 139358 rows took 1.083266 seconds
Counting 139358 rows again took 0.331406 seconds
```

# Removing a DataFrame from cache
```
# Determine if departures_df is in the cache
print("Is departures_df cached?: %s" % departures_df.is_cached)
print("Removing departures_df from cache")

# Remove departures_df from the cache
departures_df.unpersist()

# Check the cache status again
print("Is departures_df cached?: %s" % departures_df.is_cached)
```
Output:
```
Is departures_df cached?: True
Removing departures_df from cache
Is departures_df cached?: False
```

# Improve import performance

# File import performance

```
# Import the full and split files into DataFrames
full_df = spark.read.csv('departures_full.txt.gz')
split_df = spark.read.csv('departures_000.txt.gz')

# Print the count and run time for each DataFrame
start_time_a = time.time()
print("Total rows in full DataFrame:\t%d" % full_df.count())
print("Time to run: %f" % (time.time() - start_time_a))

start_time_b = time.time()
print("Total rows in split DataFrame:\t%d" % split_df.count())
print("Time to run: %f" % (time.time() - start_time_b))
```

Output:
```
Total rows in full DataFrame:	139359
    Time to run: 0.348870
    Total rows in split DataFrame:	10000
    Time to run: 0.212406
```

# Reading Spark configurations

```
# Name of the Spark application instance
app_name = spark.conf.get('spark.app.name')

# Driver TCP port
driver_tcp_port = spark.conf.get('spark.driver.port')

# Number of join partitions
num_partitions = spark.conf.get('spark.sql.shuffle.partitions')

# Show the results
print("Name: %s" % app_name)
print("Driver TCP port: %s" % driver_tcp_port)
print("Number of partitions: %s" % num_partitions)
```
Output:
```
Name: pyspark-shell
Driver TCP port: 41309
Number of partitions: 200
```

# Writing Spark configurations

```
# Store the number of partitions in variable
before = departures_df.rdd.getNumPartitions()

# Configure Spark to use 500 partitions
spark.conf.set('spark.sql.shuffle.partitions', 500)

# Recreate the DataFrame using the departures data file
departures_df = spark.read.csv('departures.txt.gz').distinct()

# Print the number of partitions for each instance
print("Partition count before change: %d" % before)
print("Partition count after change: %d" % departures_df.rdd.getNumPartitions())
```


# Performance improvements

## Normal Joins

```
# Join the flights_df and aiports_df DataFrames
normal_df = flights_df.join(airports_df, \
    flights_df["Destination Airport"] == airports_df["IATA"] )

# Show the query plan
normal_df.explain()

```

## Using broadcasting on Spark joins
```
# Import the broadcast method from pyspark.sql.functions
from pyspark.sql.functions import broadcast

# Join the flights_df and airports_df DataFrames using broadcasting
broadcast_df = flights_df.join(broadcast(airports_df), \
    flights_df["Destination Airport"] == airports_df["IATA"] )

# Show the query plan and compare against the original
broadcast_df.explain()
```
Output:
```
== Physical Plan ==
*(2) BroadcastHashJoin [Destination Airport#12], [IATA#29], Inner, BuildRight
:- *(2) Project [Date (MM/DD/YYYY)#10, Flight Number#11, Destination Airport#12, Actual elapsed time (Minutes)#13]
:  +- *(2) Filter isnotnull(Destination Airport#12)
:     +- *(2) FileScan csv [Date (MM/DD/YYYY)#10,Flight Number#11,Destination Airport#12,Actual elapsed time (Minutes)#13] Batched: false, Format: CSV, Location: InMemoryFileIndex[file:/tmp/tmp1bzkbdaj/AA_DFW_2018_Departures_Short.csv.gz], PartitionFilters: [], PushedFilters: [IsNotNull(Destination Airport)], ReadSchema: struct<Date (MM/DD/YYYY):string,Flight Number:string,Destination Airport:string,Actual elapsed ti...
+- BroadcastExchange HashedRelationBroadcastMode(List(input[1, string, true]))
   +- *(1) Project [AIRPORTNAME#28, IATA#29]
      +- *(1) Filter isnotnull(IATA#29)
         +- *(1) FileScan csv [AIRPORTNAME#28,IATA#29] Batched: false, Format: CSV, Location: InMemoryFileIndex[file:/tmp/tmp1bzkbdaj/airportnames.txt.gz], PartitionFilters: [], PushedFilters: [IsNotNull(IATA)], ReadSchema: struct<AIRPORTNAME:string,IATA:string>
```

## Comparing broadcast vs normal joins

```
start_time = time.time()
# Count the number of rows in the normal DataFrame
normal_count = normal_df.count()
normal_duration = time.time() - start_time

start_time = time.time()
# Count the number of rows in the broadcast DataFrame
broadcast_count = broadcast_df.count()
broadcast_duration = time.time() - start_time

# Print the counts and the duration of the tests
print("Normal count:\t\t%d\tduration: %f" % (normal_count, normal_duration))
print("Broadcast count:\t%d\tduration: %f" % (broadcast_count, broadcast_duration))
```

Output:
```
Normal count:		119910	duration: 2.948945
Broadcast count:	119910	duration: 0.776134
```