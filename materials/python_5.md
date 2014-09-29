##Pandas

We've talked a whole bunch about looping and opening files and reading them. So far we've worked with plain-text files. But some files have unusual dialects. One common example of such a file is an Excel file.

You should have downloaded a file called 'locations.xlsx'. This is the same file as we used earlier, but written in Excel format. To use it, we need to parse that file. The library 'pandas' can do this.

We will first import pandas.

```python
>>> import pandas
```

Many users have written their own libraries for doing fun things. These users may choose to distribute these functions for general use by the community. Pandas (http://pandas.pydata.org/) is such a library.

When we use a library that we downloaded from the internet, it is not part of Python's base functionality. Therefore, we tell Python that we'd like to use this extra library with the import command.

Now we're going to take our spreadsheet and use Pandas to import it.

```python
>>> xl = pandas.ExcelFile('locations.xlsx')
>>> xl
<pandas.io.parsers.ExcelFile object at 0x102a39610>
```
This should seem familiar - much like with our initial exercises in opening files, we are referencing the file and not the data within it. So let's liberate that data!

Excel documents are made up of sheets, which are linked documents containing different subsets of data. We're keeping it simple - all our data is in one sheet.

```python
>>> xl.sheet_names
[u'Sheet1']
>>> df = xl.parse('Sheet1')
>>> df.head()
         Site  Observations  Species  Expenditure
0  Lake Creek             4       12          180
1  Los Alamos             3        8          340
2    Big Bend             4       16          280
3    McDonald             5       20          280
4  Balmorrhea             3        3          174
>>> 
```

Hey look - our data! But ... it's inserted an index column, rather than indexing by our field sites. How can we fix that?

```python
>>> df = xl.parse('Sheet1', index_col=0)
```

Perfect.

In Pandas, we can do indexing of our data by location or by values. For example, by location:

```python

>>> df.ix[0]
Site            Lake Creek
Observations             4
Species                 12
Expenditure            180

```

Above, I have indexed by the location. A single digit in the square brackets tells Pandas we'd like to see all the values for the zeroth row in our data. The ix command allows us to index by a mixture of names and numbers. For example, it we wanted the first column from row "Lake_creek", we can do this:

```python
>>> df.ix['Lake_Creek', 1]
```

If I wanted to see a specific observation in the zeroeth row, say, the third one, I could index in a pure numerical sense, also like so:

```python
>>> df.ix[0,3]
180.0
```

Or, entirely label-based.

```python
>>> df.ix['Lake_Creek', 'Observations']
```

Alternatively, I could choose to view the third column for all rows:

```python

>>> df.ix[:,2]
Site
Lake_Creek    180
Los_Alamos    NaN
Big_Bend      280
McDonald      280
Balmorrhea    174
Name: Expenditure, dtype: float64
```

Try slicing up your data in different ways. Does everything give you the expected output? What does the dtype keyword mean?

|Name of type | Function | Base Python Equivalent |
|------------|---------|-------------------------|
|Object | The most general dtype. Will be assigned to your column if column has mixed types (numbers and strings). | String | 
|int64 | Numerical characters. 64 refers to memory allocated to hold this character. | Int. |
| float64 | Numerical characters with decimals. If a column contains numbers and NaNs, pandas will default to float64, in case your missing value has a decimal | float |

You might have noticed a couple of non-numeric symbols. How can we fix this?

```python
>>> df_fixed = df.convert_objects(convert_numeric=True)
```

What is a NaN? A NaN is "not a number". This is a non-numerical value meant to placehold missing data. Pandas has some nice functions for fixing NaNs, and so if you have non-numeric characters in your data, often changing them to NaNs streamlines the process of error-checking.

```python

>>> df_z = df_fixed.fillna(0)
```

In the above code, we have replaced NaNs with zeros. Now, we can move on and do some math with these columns/

We can break our data down for manipulation. We can use this to build more complex functions:

```python

>>> a = df_z.ix[:,'Observations']
>>> a
Site
Lake Creek    4
Los Alamos    8
Big Bend      0
McDonald      5
Balmorrhea    3
Name: Observations, dtype: float64
```

a behaves like a list, so we can do iterative functions on it:

```python
>>> b = 0
>>> for x in a:
...     b = b + int(x)
... 
>>> b
20
```

Because Excel is kind of terrible (and writing it out requires another library), you can output your data to a csv like so:

```python
>>> outfile = open('output.csv', 'w')
>>> df_z.to_csv(outfile)
>>> outfile.close()
```


###Challenge Problem:

+ Take various slices of the data. Try three different combinations of names and numerical slices. Do they look like you think they ought to?
+ Try some different mathematical calculations with these slices.
+ Look at the dtypes of these slices. Are they as you expected?


