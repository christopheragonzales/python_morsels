# Defining a Main Function in Python

Let's talk about **creating a `main` function in Python**.

Many programming languages have the notion of a main function (or a main
method), which acts as the entry point for a program. Python **does not have
main functions**.

## Python runs all code in Python scripts

We have a Python script called greet.py:

```python
import random

salutations = ["Hello", "Hey", "Hi", "Hiya", "Howdy"]


def greet():
    """Print a salutation."""
    print(random.choice(salutations))


greet()
```

When we run greet.py from our system command-line, Python **runs all of the
code** in this file:

```console
$ python3 greet.py
Hi
```

But what if we import this Python file?

## Python runs all code in Python modules

If we import our greet module, Python also runs all of the code in our file:

```console
>>> import greet
Hey
```

We're seeing "Hey" printed out at import time because Python ran our greet
function (which we called at the bottom of our module):

```console
greet()
```

It's very weird to see something printed out when we just import a module. But
that's just what Python does: Python runs all code in a module on import.

This greet.py file wasn't actually meant to be used as a module. This file
wasn't meant to be imported; it was meant to be **run as a script**.

## Using the same Python file as both a module and a script

What if we wanted to make one .py file that could both be **imported as a
module** and could be **used as a Python script** (by being run from the
command-line)?

This version of our greet.py file can be run as a program to print out a random
greeting:

```console
$ python3 greet.py
Howdy
$ python3 greet.py
Hiya
```

But if we import it as a module, it doesn't do anything (except give us access
to functions and whatever else is in that module):

```console
import greet
greet.greet()
Hiya
```

How is this possible in Python?

## Python's if `__name__ == "__main__"` check

It turns out there's a way to ask, **are we being run from the command-line**.
Or put another way, **is our current Python module the entry point** to our
Pythonprocess? This question can be asked using the statement
`__name__ == "__main__"`.

This version of greet.py works as both a command-line script and an import-able
module:

```python
import random

salutations = ["Hello", "Hey", "Hi", "Hiya", "Howdy"]


def greet():
    """Print a salutation."""
    print(random.choice(salutations))


if __name__ == "__main__":
    greet()
```

Every module has a `__name__` variable, and by default the `__name__` variable
in each module is a **string representing the name of that module**:

```console
>>> import greet
>>> greet.__name__
'greet'
```

That's the case if we're importing a module.

But what if we run that our greet.py file from the command-line?

In that case, `__name__` is not going to be the name of that module. Instead,
it's going to be `"__main__"`. That's why running greet.py also runs the greet
function:

```console
$ python3 greet.py
Hiya
```

This block of code is really asking the question are we the entry point to our
Python program (are we being run from the command-line rather than being
imported as a module)?

```python
if __name__ == "__main__":
    greet()
```

If we are being run from the command-line then we run our greet function.

## You can write a main function, but you don't need to

You may sometimes see Python programs that have a main function:

```python
import random

salutations = ["Hello", "Hey", "Hi", "Hiya", "Howdy"]


def main():
    """Print a salutation."""
    print(random.choice(salutations))


if __name__ == "__main__":
    main()
```

Python doesn't know about main functions, but there's nothing stopping us from
making a function called main that we only call if we're being run from the
command-line.

## Remember that if `__name__` == `"__main__"` incantation

If you need to make a single Python file that can both be used as a module (
being imported) and can be run as a Python script to do something, you can
check the variable `__name__` in your module to see whether it's equal to the
string `"__main__"`.
