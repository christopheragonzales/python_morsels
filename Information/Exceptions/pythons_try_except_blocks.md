# Python's try-except Block

Let's talk about **exception handling in Python**.

## A program that counts TODOs in a file

Here we have a program called find_todos.py:

```python
from argparse import ArgumentParser
from pathlib import Path


def count_in_file(path, string):
    count = 0
    with open(path) as text_file:
        for line in text_file:
            if string in line:
                count += 1
    return count


def main():
    parser = ArgumentParser()
    parser.add_argument("paths", type=Path, nargs="+")
    args = parser.parse_args()
    for path in args.paths:
        todos = count_in_file(path, "TODO")
        if todos:
            print(f"{path}: {todos} TODOs found")


if __name__ == "__main__":
    main()
```

When this program is running from the command-line, it will accept any number
of arguments we can give to it, and it'll assume that they're all file paths:

```python
def main():
    parser = ArgumentParser()
    parser.add_argument("paths", type=Path, nargs="+")
    args = parser.parse_args()
```

It'll loop over those given file paths, counting the number of lines that
contain the string TODO:

```python
    for path in args.paths:
        todos = count_in_file(path, "TODO")
        if todos:
            print(f"{path}: {todos} TODOs found")
```

If TODO was found in this file, it'll print out the number of TODOs that were
found, and it'll continue onward looping over the other file paths that were
given.

If we run this program with a couple filenames, it will print out the number of
TODOs that were found in these files (if one or more was found):

```console
~/my_project $ python3 find_todos.py setup.py README.md
README.md: 1 TODOs found
In README.md, one TODO that was found.
```

## Python prints a traceback for unhandled exceptions

If we run this program with something that isn't a valid filename (a directory
for example) we'll get an error and a traceback will be printed out:

```console
~/my_project $ python3 find_todos.py setup.py src README.md
Traceback (most recent call last):
  File "/home/trey/my_project/find_todos.py", line 25, in <module>
    main()
  File "/home/trey/my_project/find_todos.py", line 19, in main
    todos = count_in_file(path, "TODO")
  File "/home/trey/my_project/find_todos.py", line 7, in count_in_file
    with open(path) as text_file:
IsADirectoryError: [Errno 21] Is a directory: 'src'
```

A traceback was printed out because an exception was raised in our code, and
**that exception went unhandled**.

When a directory is given, it would be nice to print out an error message and
then continue onward, printing out the number of TODOs for all the other given
files.

## Reading our traceback to figure out where the exception occurred

To handle this specific exception, we could start on **line 7**, because that's
where the exception was actually raised in our code:

```console
  File "/home/trey/my_project/find_todos.py", line 7, in count_in_file
    with open(path) as text_file:
IsADirectoryError: [Errno 21] Is a directory: 'src'
```

The exception occurred when we called the built-in open function in our
count_in_file function:

```python
def count_in_file(path, string):
    count = 0
    with open(path) as text_file:
        for line in text_file:
            if string in line:
                count += 1
    return count
```

But this count_in_file function isn't actually a great place to handle this
exception, **because we're returning from this function**. We probably
shouldn't print an error and return within the same function: that's not the
job of this function.

## Handling an exception with a try-except block

We should probably go one more level up in our call stack (remember that's
what a traceback represents):

```console
  File "/home/trey/my_project/find_todos.py", line 19, in main
    todos = count_in_file(path, "TODO")
  File "/home/trey/my_project/find_todos.py", line 7, in count_in_file
    with open(path) as text_file:
IsADirectoryError: [Errno 21] Is a directory: 'src'
```

**Line 19** is the next level up in our code, where that count_in_file function
was actually called:

```python
def main():
    parser = ArgumentParser()
    parser.add_argument("paths", type=Path, nargs="+")
    args = parser.parse_args()
    for path in args.paths:
        todos = count_in_file(path, "TODO")
        if todos:
            print(f"{path}: {todos} TODOs found")
```

So when our count_in_file function is called, **we'll catch this exception** by
using a try-except block:

```python
    for path in args.paths:
        try:
            todos = count_in_file(path, "TODO")
        except IsADirectoryError:
            print(f"Error: {path} is a directory")
            continue
        if todos:
            print(f"{path}: {todos} TODOs found")
```

First we try to run that one line of code (where we call the count_in_file
function). If an `IsADirectoryError` occurs we print out an error (stating that
the given path is a directory) and then we continue to the next iteration of
our for loop (skipping over the current path, and handling the next path):

We're catching an IsADirectoryError because that's the type of exception that
was raised, as shown in the last line in our traceback:

```console
IsADirectoryError: [Errno 21] Is a directory: 'src'
```

Now if we run this code again, it works:

```console
~/my_project $ python3 find_todos.py setup.py src README.md
Error: src is a directory
README.md: 1 TODOs found
```

It says Error: src is a directory, and then **it continues onward**, finding
that there are TODOs in README.md.

## We use try-except in Python instead of try-catch

To catch an exception in Python, you'll first need to **identify the line that
you need to handle that exception on** (the line that raised the exception).

Then you'll want to put that line in a try block, using an except block to note
**the *specific* type of exception that was raised**, and then noting the block
of code that should be run to handle that exception.
