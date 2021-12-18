# Handling large files

Every data scientics needs to go through a point where his files overgrow his compute resources. Suddenly your CSV doesn't fit into operational memory (RAM) of your lab machine. You could try to buy some more memory, but you can only get so much before you run out of money or memory slots. What then?

## Eating CSV slice by slice like pizza

Brief example on how to process your CSV slice by slice using pandas [read_csv](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html):

```
import pandas as pd
filename = 'pizza.csv'
# How many lines to process at once = chunksize
chunksize = 10 ** 8


def process(chunk):
    """This method processes only a chunk (slice) of a CSV
    // Here goes your code


for chunk in pd.read_csv(filename, chunksize=chunksize):
    process(chunk)
```

This fits simple processing. If you used to calculate average and your CSV grew too big, you would use the example and addapt the process method to calculate a variation of [Moving average](https://en.wikipedia.org/wiki/Moving_average). Moving average is a good example of an algorithm that does not need all the data to be present in memory at the same time. You can probably find similar one for your use case.
