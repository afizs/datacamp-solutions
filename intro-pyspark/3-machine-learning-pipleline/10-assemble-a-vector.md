# Assemble a vector

Combine all the columns containing our features into a single column. 

```
# Make a VectorAssembler
vec_assembler = VectorAssembler(inputCols=['month', 'air_time', 'carrier_fact', 'dest_fact', 'plane_age'], outputCol='features')
```