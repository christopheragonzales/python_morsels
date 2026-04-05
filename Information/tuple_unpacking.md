# Tuple Unpacking in Python

## An Alternative to Hard-Coded Indexes

We have a three-item tuple, called p:

```python 
>>> p = (2,1,3)
```

We can access each of the things in the tuple by indexing it:

```python
>>> print(p[0], p[1], p[2])
2 1 3
```

But we could also give names to things in this tuple:

```python
>>> x, y, z = p
```

This is called **tuple unpacking**. We have taken a three-item tuple and
*unpacked* that tuple into three variables (x, y, z):

```python
>>> print(x, y, z)
2 1 3
```

We can think of creating a tuple as *packing* values together and unpacking a
tuple as undoing that work. We're unpacking each of the values in our tuple
above into separate variable names.

If we try to unpack a three-item tuple into just two variables, we'll get an
error.

```python
x, y = p
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 2)
```

**Tuple unpacking describes the shape of the tuple** you're unpacking. So,
*the number of variables* you're unpacking into must be the same as the number
of items in the tuple-to-be-unpacked.

Tuple unpacking is really handy for **avoiding hard-coded indexes** and instead,
giving **descriptive names** to each of the things inside a tuple.

## Unpacking without an Equals Sign

Tuple unpacking is most often used, not with an equals sign, but instead in a
for loop.

If we call the items method on a dictionary, we'll get an iterable of two-item tuples:

```python
>>> things = {"ducks": 2, "lamps": 3, "chairs": 0}
>>> things.items()
dict_items([('ducks', 2), ('lamps', 3), ('chairs', 0)])
```

These tuples represent the key-value pairs in our dictionary.

```python
>>> for item in things.items():
    print(item)

('ducks', 2)
('lamps', 3)
('chairs', 0)
```

We already know that we can *unpack* each of the items in each of these
key-value tuples into two variables (say thing and count):

```python
>>> for item in things.items():
    thing, count = item
    print(thing, count)

ducks 2
lamps 3
chairs 0
```

But we actually don't even need an equals sign to do tuple unpacking.

Very iteration of a for loop does an **implicit assignment.** The thing
between the *for* and the *in* in a for loop is very similar to the thing on
the left-hand of an equal sign in an assignment statement.

We can actually do that tuple unpacking right in the for line:

```python
>>> for thing, count in things.items():
    print(thing, count)

ducks 2
lamps 3
chairs 0
```

We didn't even need that item variable at all.

We can do the tuple unpacking right inside the for loop itself because
**anything you can put on the left-side of the equal sign**, you can
**put in between the for and the in** in a for loop. This is
**the most common place** you'll see tuple unpacking used: on the first line of
a for loop.

## Use Tuple Unpacking as a Descriptive Alternative to Indexing

**Tuple unpacking** is also called **multiple assignment** and it's sometimes
called **iterable unpacking** because you can actually use it with
*any iterable* in Python, not just with tuples.

You'll most often see tuple unpacking used when looping over an iterable of
two-item tuples (or maybe an iterable of three-item tuples), but you can
actually use tuple unpacking anywhere in your code where you'd like to
**give descriptive names to the the things within a tuple**.
