# When / Otherwise

```
# Add a column to voter_df for a voter based on their position
voter_df = voter_df.withColumn('random_val',
                               when(voter_df.TITLE == 'Councilmember', F.rand())
                               .when(voter_df.TITLE == 'Mayor', 2)
                               .otherwise(0))

# Show some of the DataFrame rows
voter_df.show()

# Use the .filter() clause with random_val
voter_df.filter(voter_df.random_val==0).show()
```


# User defiend functions

```
def getFirstAndMiddle(names):
  # Return a space separated string of names
  return ' '.join(names)

# Define the method as a UDF
udfFirstAndMiddle = F.udf(getFirstAndMiddle, StringType())

# Create a new column using your UDF
voter_df = voter_df.withColumn('first_and_middle_name', udfFirstAndMiddle(voter_df.splits))

# Show the DataFrame
voter_df.show()

```

### Partitioning and lazy processing
```
# Select all the unique council voters
voter_df = df.select(df["VOTER NAME"]).distinct()

# Count the rows in voter_df
print("\nThere are %d rows in the voter_df DataFrame.\n" % voter_df.count())

# Add a ROW_ID
voter_df = voter_df.withColumn('ROW_ID', F.monotonically_increasing_id())

# Show the rows with 10 highest IDs in the set
voter_df.orderBy(voter_df.ROW_ID.desc()).show(10)
```

### IDs with different partitions:

```
# Print the number of partitions in each DataFrame
print("\nThere are %d partitions in the voter_df DataFrame.\n" % voter_df.rdd.getNumPartitions())
print("\nThere are %d partitions in the voter_df_single DataFrame.\n" % voter_df_single.rdd.getNumPartitions())

# Add a ROW_ID field to each DataFrame
voter_df = voter_df.withColumn('ROW_ID', F.monotonically_increasing_id())
voter_df_single = voter_df_single.withColumn('ROW_ID', F.monotonically_increasing_id())

# Show the top 10 IDs in each DataFrame 
voter_df.orderBy(voter_df.ROW_ID.desc()).show(10)
voter_df_single.orderBy(voter_df_single.ROW_ID.desc()).show(10)
```


### More ID tricks

```
# Determine the highest ROW_ID and save it in previous_max_ID
previous_max_ID = voter_df_march.select('ROW_ID').rdd.max()[0]

# Add a ROW_ID column to voter_df_april starting at the desired value
voter_df_april = voter_df_april.withColumn('ROW_ID', previous_max_ID + F.monotonically_increasing_id())

# Show the ROW_ID from both DataFrames and compare
voter_df_april.select('ROW_ID').show()
voter_df_march.select('ROW_ID').show()
```
Output: 
```
+-------------+
|       ROW_ID|
+-------------+
|1717986918400|
|1735166787584|
|1743756722176|
|1752346656768|
|1760936591360|
|1812476198912|
|1821066133504|
|1941325217792|
|1949915152384|
|2070174236672|
|2104533975040|
|2310692405248|
|2345052143616|
|2379411881984|
|2516850835456|
|2559800508416|
|2654289788928|
|2671469658112|
|2714419331072|
|2757369004032|
+-------------+
only showing top 20 rows

+-------------+
|       ROW_ID|
+-------------+
|   8589934592|
|  25769803776|
|  34359738368|
|  42949672960|
|  51539607552|
| 103079215104|
| 111669149696|
| 231928233984|
| 240518168576|
| 360777252864|
| 395136991232|
| 601295421440|
| 635655159808|
| 670014898176|
| 807453851648|
| 850403524608|
| 944892805120|
| 962072674304|
|1005022347264|
|1047972020224|
+-------------+
only showing top 20 rows
```