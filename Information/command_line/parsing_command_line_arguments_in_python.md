# Parsing Command Line Arguments in Python

Let's talk about parsing command line arguments in Python.

## Parsing command-line arguments

We have a program here called add.py that uses Python's argparse module to
parse two arguments, x and y:

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('x', type=float)
parser.add_argument('y', type=float)
args = parser.parse_args()

print(args.x + args.y)
```

When called, it converts the arguments to floating point numbers and then adds
them up:

```console
$ python3 add.py 3 4
7.0
```

Now, we have a lot of code here just to parse two command-line arguments. Why
did we use argparse for this?

## Why not use sys.argv to get command-line arguments?

Instead of all that code, we could just read sys.argv (which is a list of all
command-line arguments as strings) and then convert our two expected arguments
to floating point numbers:

```python
import sys

[program, x, y] = sys.argv

print(float(x) + float(y))
```

As you can see, this does work:

```console
$ python3 add_old.py 3 4
7.0
```

But the downside of doing things this way is, if we make a mistake when calling
this command-line program, **we won't see very helpful error messages**.

For example, if we pass too few arguments Python doesn't really tell us much
about what's going on:

```console
$ python3 add_old.py 3
Traceback (most recent call last):
  File "/home/trey/_/add_old.py", line 3, in <module>
    [program, x, y] = sys.argv
ValueError: not enough values to unpack (expected 3, got 2)
```

Similarly, if we pass in something that isn't a number, we get only a slightly
helpful error message:

```console
$ python3 add_old.py 3 a
Traceback (most recent call last):
  File "/home/trey/_/add_old.py", line 5, in <module>
    print(float(x) + float(y))
ValueError: could not convert string to float: 'a'
```

And if we ask for help, we won't get very helpful information at all:

```console
$ python3 add_old.py --help
Traceback (most recent call last):
  File "/home/trey/_/add_old.py", line 3, in <module>
    [program, x, y] = sys.argv
ValueError: not enough values to unpack (expected 3, got 2)
```

In all of these cases we're seeing an unhelpful traceback printed out instead
of a helpful error message. Python's argparse module **will print out helpful
error message in all of these cases instead**.

## Comparing argparse to manually reading sys.argv

Here's our command-line program from before that uses argparse:

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('x', type=float)
parser.add_argument('y', type=float)
args = parser.parse_args()

print(args.x + args.y)
```

When we **ask this program for help** (by passing --help argument) it shows us
usage information:

```console
$ python3 add.py --help
usage: add.py [-h] x y

positional arguments:
  x
  y

optional arguments:
  -h, --help  show this help message
              and exit
```

If we parse in too few arguments, it tells us there's another argument that's
required:

```console
$ python3 add.py 3
usage: add.py [-h] x y
add.py: error: the following arguments are required: y
```

Similarly, if we pass in the wrong value for an argument (a non-number), it
tells us that our argument can't be converted to a floating point number:

```console
$ python3 add.py 3 a
usage: add.py [-h] x y
add.py: error: argument y: invalid float value: 'a'
```

The argparse module is really handy for **making command-line interfaces that
are friendly**.

But argparse is **not just for simple command-line interfaces** like this one,
we can also accept **optional arguments** using argparse.

## Accepting optional command-line arguments

This version of our add.py program accepts an optional --expected or -e argument:

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('x', type=float)
parser.add_argument('y', type=float)
parser.add_argument('--expected', '-e', type=float)
args = parser.parse_args()

total = args.x + args.y
print(total)
if args.expected is not None and args.expected != total:
    print(f"Expected {args.expected} but got {total}")
```

This line of code adds an option, that is an optional argument, to our
command-line parser:

```python
parser.add_argument('--expected', '-e', type=float)
```

When running this program, this optional argument can be specified with either `--expected=SOMETHING` or `-e SOMETHING`.

If this expected argument isn't provided then it will default to the value
`None`.

When this `expected` argument is not `None` we check to see it was equal to
`total` (which is the total sum of our `x` and `y` arguments):

```python
if args.expected is not None and args.expected != total:
    print(f"Expected {args.expected} but got {total}")
```

For example if we pass in 3 and 4 and an expected value of 7 we won't see
anything printed out (since 3 + 4 is 7):

```console
$ python3 add.py 3 4 -e 7
7.0
```

We could have also specified that expected argument with --expected= instead
of -e:

```console
$ python3 add.py 3 4 --expected=7
7.0
```

This argument might be a little bit silly, but we could use it for confirming
issues with floating point addition.

For instance, we're adding 0.1 and 0.02 to check whether it's exactly equal to
0.12 (it's not):

```console
$ python3 add.py 0.1 0.02 --expected=0.12
0.12000000000000001
Expected 0.12 but got 0.12000000000000001
```

This addition gave us a number very close to 0.12 (but not exactly 0.12) so we
see an error message printed out.

## Optional command-line arguments without a value

We can also make optional command-line arguments that **don't accept a value**.

In this version of add.py we're accepting an optional `--verbose` argument:

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('x', type=float)
parser.add_argument('y', type=float)
parser.add_argument('--verbose', action='store_true')
args = parser.parse_args()

if args.verbose:
    print(f"Adding {args.x} and {args.y}")
print(args.x + args.y)
```

The mere presence of a `--verbose` argument will store the value `True`:

```python
parser.add_argument('--verbose', action='store_true')
```

If that argument is not given, the verbose ``attribute`` will be something
**falsey** (see truthiness) and if it is given, we'll get `True`.

```python
if args.verbose:
    print(f"Adding {args.x} and {args.y}")
```

So when the argument `--verbose` is given, our program prints out the numbers
we're adding together:

```console
$ python3 add.py --verbose 3 4
Adding 3.0 and 4.0
7.0
```

When `--verbose` is not given our program does exactly what it did before:

```console
$ python3 add.py 3 4
7.0
```

## Adding documentation for command-line interface using argparse

The argparse module also allows you to **add documentation for your
command-line interfaces**.

Here's one more version of our add.py program with lots of help text added:

```python
import argparse

parser = argparse.ArgumentParser("Add two numbers together")
parser.add_argument('x', type=float, help="First number to add")
parser.add_argument('y', type=float, help="Second number to add")
parser.add_argument('--verbose', action='store_true', help="Show more context")
args = parser.parse_args()

if args.verbose:
    print(f"Adding {args.x} and {args.y}")
print(args.x + args.y)
```

If the end user asks for help when running this program they'll see some very
helpful usage information:

```console
$ python3 add.py --help
usage: Add two numbers together [-h] [--verbose] x y

positional arguments:
  x           First number to add
  y           Second number to add

optional arguments:
  -h, --help  show this help message and exit
  --verbose   Show more context
```

This usage information let's end users know that this program:

1. Will "add two numbers together"
2. Accepts two positional arguments, the "first number to add" and the "second
3. number to add"
4. Accepts a --verbose argument to "show more context"

## Use argparse for making friendly command-line interfaces

If you're trying to make a friendly command-line interface in Python you can
use the argparse (included in Python's standard library).

The argparse module makes pretty standard command-line interfaces which feel
typical or normal on Linux, Mac, and Windows.
