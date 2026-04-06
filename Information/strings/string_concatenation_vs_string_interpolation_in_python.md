# String Concatenation vs String Interpolation in Python

Let's talk about how to build-up bigger strings out of smaller strings in Python. The two methods for doing this are string concatenation and string interpolation.

## String Concatenation

We've got a string that represents a name Trey, and we're going to make a new
string `"My name is Trey"`. We're doing this by using the + symbol between the
string, My name is, and the string Trey:

```console
>>> name = "Trey"
>>> message = "My name is " + name
>>> message
'My name is Trey'
```

This is called **string concatenation**. The word concatenation is basically
just a fancy word for gluing things together. So when we're concatenating two
strings, we're **gluing them together** by using the + sign.

Now, string concatenation is great, but it can get a little bit unwieldy at
times.

Here's a much longer example of string concatenation:

```console
>>> "My name is " + name + " which has " + len(name) + " characters."
```

It's easy to make typos, while you're using string concatenation because you've
got quotes and pluses all interspersed between each other. We didn't end up
making a typo above, but we do have a bug in our code.

```console
"My name is " + name + " which has " + len(name) + " characters."
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```

Calling len(name) returns an integer. While we can use + to add an integer to
an integer and we can use + to concatenate a string with a string, we cannot
use + between a string and an integer. See the article [TypeError: can only
concatenate str (not "int") to str](
    https://www.pythonmorsels.com/can-only-concatenate-str/
) for more details.

So, we need to convert len(name) to a string in order to concatenate all these
strings together:

```console
>>> "My name is " + name + " which has " + str(len(name)) + " characters."
'My name is Trey which has 4 characters.'
```

The code that we ended up with is pretty readable Python code, but it could be
more readable.

## String Interpolation

The other way to build up a bigger string out of smaller strings is **string
interpolation**. In Python, this is often called string formatting. To do string formatting, we can use an [**f-string**](
    https://docs.python.org/3/whatsnew/3.6.html#whatsnew36-pep498
).

An f-string isn't the only tool for this (the [string format method is the
other](https://docs.python.org/3.8/library/stdtypes.html#str.format)), but it's
the most common one you'll see:

```console
>>> f"My name is {name} which has {len(name)} characters."
'My name is Trey which has 4 characters.'
```

An f-string is a string with an "f" in front of it. Without that "f" in front
of it, it's just a regular string:

```console
>>> "My name is {name} which has {len(name)} characters."
'My name is {name} which has {len(name)} characters.'
```

Simply by putting an f just before the string, regardless of whether we're
using double quotes " or single quotes ', Python will look for curly braces
inside the string we've written and it will execute whatever is inside of those
curly braces ({...}), taking whatever object comes back from that code
execution, and then convert it to a string if needed (as with len(name)). Next,
it will stick those little strings inside of our bigger string, essentially
injecting them into our bigger string:

```console
>>> f"My name is {name} which has {len(name)} characters."
'My name is Trey which has 4 characters.'
```

## Summary

You can think of string concatenation as *gluing* strings together. And, you
can think of **string interpolation** with strings as **injecting strings inside
of other strings**.

The two ways to build up a bigger string out of smaller strings are **string
concatenation** (using the + symbol) and **string interpolation (a.k.a. string
formatting)** which uses **f-strings**.
