# 1. Data cleaning review

There are many benefits for using Spark for data cleaning 

    - Spark offers high peformance
    - Spark allows orderly data flows.
    - Spark can use strictly defined schemas while ingesting data. 
        
~~Spark can only handle thousands of records.~~

# 2. Defining a schema

```
# Import the pyspark.sql.types library
from pyspark.sql.types import *

# Define a new schema using the StructType method
people_schema = StructType([
  # Define a StructField for each field
  StructField('name', StringType(), False),
  StructField('age', IntegerType(), False), 
  StructField('city', StringType(), False)
])
```

# 3. Immutability and lazy processing

Spark use immutable data frames to eefficiently handle data thoughout the cluster.