# 08. Carrier

### Solution: 

```
# Create a StringIndexer
carr_indexer = StringIndexer(inputCol='carrier', outputCol='carrier_index')

# Create a OneHotEncoder
carr_encoder = OneHotEncoder(inputCol="carrier_index", outputCol="carrier_fact")
```

# 09. Destination

### Solution:
```
# Create a StringIndexer
dest_indexer = StringIndexer(inputCol='dest', outputCol='dest_index')

# Create a OneHotEncoder
dest_encoder = OneHotEncoder(inputCol="dest_index", outputCol='dest_fact')
```
