# Python's "If" Statement

Let's talk about if statements in Python.

## Conditional code with if statements

If you'd like to **run some code only if a certain condition is met**, you can
use an if statement.

Here we have a program called language.py:

```python
value = input("What programming language are you learning? ")

if value == "Python":
    print("Cool! This program was written in Python.")
```

In this program, we're prompting a user to enter a value, and then we're
printing out a response only if the value that they entered is Python:

```console
$ python language.py
What programming language are you learning? Python
Cool! This program was written in Python.
```

If they enter anything else, we don't do anything at all:

```console
$ python language.py
What programming language are you learning? JavaScript
```

We're using an if statement to do this. The if statement has a condition, and
**if that condition is true, the block of code just after that condition is
run**.

## Using else with if in Python

What if we wanted to print out a different response when our condition isn't
met? For that, we could use **an else statement**.

An else statement also comes after an if:

```python
value = input("What programming language are you learning? ")

if value == "Python":
    print("Cool! This program was written in Python.")
else:
    print(f"Neat. I haven't tried {value} yet.")
```

When the condition we're checking is true, we run the block of code right after
the if statement (the first print line).

```console
$ python language.py
What programming language are you learning? Python
Cool! This program was written in Python.
```

Otherwise, we run the block of code just after the else statement (the second print line).

```console
$ python language.py
What programming language are you learning? JavaScript
Neat. I haven't tried JavaScript yet.
```

Our program now always performs an action, but the action will be different
depending on the value the user provided:

## Checking multiple conditions with elif

What if we had a **variety of different conditions that we wanted to check**
one after the other, until one of them was true? We could use elif statements
for that.

This light.py program prints out a color of light based on a randomly generated
wavelength:

```python
from random import randint

wavelength = randint(390, 780)
print(f"{wavelength}nm wavelength")

if wavelength < 390:
    print("Not visible")
elif wavelength < 455:
    print("Violet")
elif wavelength < 492:
    print("Blue")
elif wavelength < 577:
    print("Green")
elif wavelength < 597:
    print("Yellow")
elif wavelength < 622:
    print("Orange")
elif wavelength < 780:
    print("Red")
else:
    print("Not visible")
```

This program works thanks to Python's if, elif, and else statements.

These conditions in that if-elif chain **cascade**. Meaning, Python looks
through the conditions one-by-one, **until it finds a condition that's true**,
it runs the block of code for that condition, and then it stops, jumping to the
end of the if statement chain.

Both else statements and elif statements are attached to the end of an if
statement. An if statement can have any number of elif statements after it
(including none at all) and it can optionally have a single else statement at
the end.

## Run Python code conditionally with if statements

If you'd like to **run some code only when a certain condition is met**, you
can use an if statement in Python.
