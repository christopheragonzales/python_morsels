# Deep Tuple Unpacking

## Indexing nested tuples

We have a for loop which, as we loop over it, will give us tuples which contain a number, a color and an animal:

```python
>>> colors = ['purple', 'blue', 'coral', 'white', 'green', 'cyan']
>>> animals = ['duck', 'penguin', 'lynx', 'monkey', 'snake', 'turtle']
>>> for t in enumerate(zip(colors, animals), start=1):
    print(t)

(1, ('purple', 'duck'))
(2, ('blue', 'penguin'))
(3, ('coral', 'lynx'))
(4, ('white', 'monkey'))
(5, ('green', 'snake'))
(6, ('cyan', 'turtle'))
```

There are three items in these tuples but they aren't three-item tuple. These are actually two-item tuples:

```python
>>> len(t)
2
```

The first item is a number:

```python
>>> t[0]
6
```

And the second item is another tuple:

```python
>>> t[1]
('cyan', 'turtle')
```

So if we were wanted to separately print each item in this tuple, we might do something like this:

```python
>>> for t in enumerate(zip(colors, animals), start=1):
    print(t[0], f"Look! A {t[1][0]} {t[1][1]}")

1 Look! A purple duck
2 Look! A blue penguin
3 Look! A coral lynx
4 Look! A white monkey
5 Look! A green snake
6 Look! A cyan turtle
```

We're indexing this tuple to grab the first thing (index 0), which is a number.

And then we're indexing it to get the second thing (index 1) which is another tuple. We're then indexing that inner tuple even further to get the color, and the animal within it:

```python
>>> t[1][0], t[1][1]
('cyan', 'turtle')
```

Usually with tuples it's best to avoid hard-coded indices and instead use tuple unpacking in Python (aka **multiple assignment**) to give names to each item in the tuple.

## A failed attempt at tuple unpacking

To unpack the 3 items in our tuples, we could try to use tuple unpacking like this:

```python
>>> for n, color, animal in enumerate(zip(colors, animals), start=1):
    print(n, f"Look! A {color} {animal}!")

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: not enough values to unpack (expected 3, got 2)
```

But that gives us an error.

This didn't work because as we loop over enumerate, we're getting two-item tuples, not three-item tuples:

```python
>>> t
(6, ('cyan', 'turtle'))
```

We're trying to unpack two-item tuples into three variables, which doesn't make sense to Python.

Tuple unpacking describes the size of the tuple we're working with. We're working with two-item tuples but our tuple unpacking is expecting a three-item tuple.

## Deep tuple unpacking

What we need to do is unpack another level deeper: we need to unpack that inner tuple too.

We could get clever and unpack into two variables:

```python
>>> for n, z in enumerate(zip(colors, animals), start=1):
```

And then further unpack this second variable, z, (which points to a tuple) into two more variables:

```python
    color, animal = z
    print(n, f"Look! A {color} {animal}!")
```

And that works:

```python
>>> for n, z in enumerate(zip(colors, animals), start=1):
    color, animal = z
    print(n, f"Look! A {color} {animal}!")

1 Look! A purple duck!
2 Look! A blue penguin!
3 Look! A coral lynx!
4 Look! A white monkey!
5 Look! A green snake!
6 Look! A cyan turtle!
```

But it turns out we can actually merge the first two lines together into one tuple unpacking. Using parentheses ((...)), we can basically embed our second tuple unpacking into our first tuple unpacking:

```python
>>> for n, (color, animal) in enumerate(zip(colors, animals), start=1):
    print(n, f"Look! A {color} {animal}!")

1 Look! A purple duck!
2 Look! A blue penguin!
3 Look! A coral lynx!
4 Look! A white monkey!
5 Look! A green snake!
6 Look! A cyan turtle!
>>> t
(6, ('cyan', 'turtle'))
```

So this is a way of telling Python, this is a two-item tuple that we're working with and the second thing in this tuple is another two-item tuple and we'd like to unpack these tuples-within-tuples two levels deep.

Deep tuple unpacking allows us to name each of the three things we're working with, even though we're working with it a deeply nested tuple.

## The outer parentheses are optional

It might be a little clearer what's going on if we put even more parentheses around this deep unpacking:

```python
for (n, (color, animal)) in enumerate(zip(colors, animals), start=1):
    print(n, f"Look! A {color} {animal}!")

1 Look! A purple duck!
2 Look! A blue penguin!
3 Look! A coral lynx!
4 Look! A white monkey!
5 Look! A green snake!
6 Look! A cyan turtle!
```

The outer parentheses in a tuple unpacking are always optional. We've added them in above to make our tuple unpacking look even more like the tuples we're actually unpacking.

That unpacking looks more like a tuple-of-tuples this way, just like the tuples we're working with:

```python
t
(6, ('cyan', 'turtle'))
```

I don't usually put outer parentheses around my tuple unpackings unless they're particularly deep or complex, but if you find it more readable you're welcome to do so.

## Summary

So in Python, if you're working with tuples-of-tuples, you can unpack those nested tuples multiple levels deep in order to give names to each of the items you're working with by using deep tuple unpacking.

You just have to make sure you place the parentheses in the right place to describe the shape of the nested tuples that you're working (ensuring you mirror the parentheses you see in your tuples-of-tuples).
