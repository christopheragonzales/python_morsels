# Introduction

What if we wanted to pass information to our Python program to change the way
that it runs? One of the most common ways to do this is with command-line
arguments.

## Passing command-line arguments

When you run a program from your [system command-line](
    https://www.pythonmorsels.com/terms/#CLI
), you can pass in [arguments](
    https://www.pythonmorsels.com/terms/#command-line-argument
) to the program.

Here we have a program called greet.py:

```python
print("Hello World!")
```

Here, we're calling this program:

```bash
$ python3 greet.py
Hello World!
```

## Function arguments vs command-line arguments

The word "argument" is a loaded term.

In Python, we have [function arguments](
    https://www.pythonmorsels.com/positional-vs-keyword-arguments/
) which are **inputs to a function**. But
*command-line arguments* are **inputs to an entire program**.

## Python accepts whatever command-line arguments we give it

What do you think will happen if we passed an argument to our greet.py program, as we run it through Python?

```bash
$ python3 greet.py Trey
```

Our greet.py program doesn't use command-line arguments at all. It's just a single line of code:

```python
print("Hello world")
```

So what's your guess?

- Will python print Hello world?
- Will it print Hello Trey?
- Will we get an error?
- Something else?

When we run this greet.py program with the argument Trey we see Hello world 
printed out:

```bash
$ python3 greet.py Trey
Hello world
```

That's a little bit odd.

Python is just **ignoring whatever argument we passed** to this program. Python
**stores all the arguments** that we give it, but it doesn't *do* anything with
the arguments: it's up to us (the implementer of this program) to do something
with them.

## Where are command-line arguments stored?

Python stores command-line arguments in the sys module within a list called
argv:

```python
import sys

print(sys.argv)
print("Hello world")
```

The sys.argv list has all of the arguments passed to our Python program:

```bash
$ python3 greet.py Trey
['greet.py', 'Trey']
Hello world
```

Our sys.argv list here has two things in it, our program name and the argument
we've passed in.

```bash
$ python3 greet.py Trey
['greet.py', 'Trey']
```

The first thing in sys.argv is always going to be our program name.

## Accessing command-line arguments

If we wanted to grab the argument after our program name we could read the second item (index 1) from sys.argv:

```python
import sys

print("Hello", sys.argv[1])
```

We're assuming we get an argument passed in and we're printing that argument
out:

```bash
$ python3 greet.py Trey
Hello Trey
$ python3 greet.py Carol
Hello Carol
```

If I pass more than one argument our program is currently just ignoring
everything after that first argument:

```bash
$ python3 greet.py Trey Hunner
Hello Trey
```

What if we pass too few arguments?

```bash
$ python3 greet.py
Traceback (most recent call last):
  File "/home/trey/greet.py", line 3, in <module>
    print("Hello", sys.argv[1])
IndexError: list index out of range
```

If we pass in two few arguments we'll get an IndexError (because sys.argv
doesn't have an index 1 in this case).

Processing sys.argv *manually* is something that you should only do for **very
simple Python programs**: programs that don't need much complex command-line 
argument processing and ideally only do this for programs that are just *for
your use*.

## Python provides all command-line arguments as strings

Let's take a look at another example. Here we have a program called add.py:

```python
import sys

program, x, y = sys.argv

print(x + y)
```

This program takes sys.argv and [unpacks](
    https://www.pythonmorsels.com/tuple-unpacking/
) it into three variables: the program name, and whatever two arguments are
given after that:

```python
program, x, y = sys.argv
```

Then it adds those arguments together and prints them out:

```python
print(x + y)
```

What should we see when we run add.py with 2 and 3?

```bash
$ python3 add.py 2 3
```

Take a guess.

- 6?
- 5?
- 100?

Here's what we see when we run add.py with 2 and 3:

```bash
$ python3 add.py 2 3
23
```

Weird, right? Clearly 2 plus 3 is not 23, or at least it shouldn't be 23, and
yet that's what we see.

If we print out sys.argv:

```python
import sys

program, x, y = sys.argv

print(sys.argv)
```

We'll see that it's a list of strings:

```bash
$ python3 add.py 2 3
['add.py', '2', '3']
```

**Python stores all command-line arguments as strings** because all
command-line arguments are given to Python as text by the [operating system](
    https://en.wikipedia.org/wiki/Operating_system
). Python doesn't do any processing with them; it's *up to us* to make meaning
of these arguments.

So if we want 2 and 3 to be numbers, we'll have to convert them to numbers ourselves:

```python
import sys

program, x, y = sys.argv

print(float(x) + float(y))
```

Now we finally see the result we're expecting:

```bash
$ python3 add.py 2 3
5.0
```

## Command-line arguments are stored in the sys.argv list

If you need to make a very simple command-line interface (one that's just for
you) and it doesn't need to be friendly, you can **read sys.argv to manually
process** the arguments coming into your program. But if you need something
more complex, you should probably use a proper command-line argument processing
tool (like Python's [argparse](https://www.pythonmorsels.com/parsing-command-line-arguments-python/) module).
