#Grab Bag

##Sets 

Sets distill down to the unique values in a group of objects

```python
>>> name = ['A', 'p','r','i','l','w','r','i','g','h','t']
>>> set(name)
... {'A', 'g', 'h', 'i', 'l', 'p', 'r', 't', 'w'}
```

As you can see: this is unordered and case sensitive. But these are very useful for crunching down large amounts of data.


##List comprehensions

List comprehensions compact the creation and population of a list into one step.

With a regular list:

```python
>>> common_letter = []
>>> last_name = ['L','i','e','b','s','k','i','n','d']
>>> for letter in name:
>>>     if letter in last_name:
>>>         common_letter.append(letter)
```

With a list comprehension:

```python
>>> common = [letter for letter in last_name if letter in name]
```

These give us equivalent answers:

```python
>>> common_letter == common
... True
```


