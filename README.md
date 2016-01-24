# Tutorial 8 - Python for C# and Java Developers

The purpose of this tutorial is to explain the fundamentals of Python from the perspective of someone who is familiar with C# and/or Java.

_** Wait! I hear you shout, C# and Java are not the same. Well, compared with Python they are in the same group **_

## Language features

* There are a **lot** of implicit conventions, these are documented in "PEP" documents on the Python language site.
* PEP8 is the style guide. You should read this, but there is a tool called `flake8` which can check this for you.
* Python 3 and 2 are quite different, mainly because in Python 2, strings are ASCII and in 3, they are binary for unicode support. This causes some confusion, if you are starting fresh, start with 3.5 unless you have a good reason not to. Python 3 does not have backward compatibility to 2.

## None

There is no null in Python, Python uses `None` as the replacement.

## Code blocks

Code blocks are indicated by the `:` then a nested (4 spaces) block of text. For example in the if statement

```python

if something is something_else:
  do_stuff()
```

A carriage return indicates an execution statement, if you want line continuation, you use the `\` character. You do **not** need to do this within Parenthesis.

```python
something ( a
            b)
if something is \
  something_else:
  do_Stuff()

```

## Callables

In Python, everything is an object, this includes a member function of a class, or a method in the general namespace.

Let me show you an example (by the way `pass` just means `break`)

```python

def my_method(arg_1, arg_2):
  pass

what_method = my_method

print(what_method) # Shows the memory address of the callable method

what_method("fa", "la") # Executes my_method
```

You can define methods anywhere (including inside another method, like a TFunc in C#)

```python

def my_function(some):
  i = 1
  def another_function(arg1):
    return arg1 * 20
  return another_function

print(my_function(blah)(10)) # Prints "200"
```

## Parenthesis

Parenthesis are used for 2 things, calling methods (as we saw above) and Tuples. Do not use Parenthesis in evaluation statements.

```python
return (something is something_else) # Don't do this!!
```

### Tuples

This is a really nice feature of the language, you can use tuples for lots of methods that accept a single statement. For example, the `is_instance` checks if `a` is an instance of class `b`. But you can do this to check if it is an instance of 3 classes in one call.

```python
is_instance(a, (class_a, class_b, class_c))
```

You can also use it in the value replacement statements

```python
print("Error in %s line %i" % (message, line))
```
__


## Switch statements

There is no switch statement. **Get over it**. The designers of the language say switch statements lead to violation of the single
responsibility principle. I tend to agree now that I don't have access to it, I don't write code like this:

```csharp

public void MyMethodThatDoesTooMuch(AnEnumeratedClass of){
  switch (of){
    case AnEnumeratedClass.FirstThing:
      run_fun_stuf();
    default:
      do_something_completely_different();
  }
}

```

## Evaluation

There are 2 ways to check equality (and quite unique)

* Using `is` which checks the reference value, does not perform value equality. `is not` is the opposite statement.
* Using `==` which checks for value equality

```python
return something is not something_else
```

```python
return something != something else
```

## Type checking

Type checking can be done using `is_instance`, general design recommendation is that you don't do this too often. If you have to write endless `is_instance` calls in your code, then your design is bad.

## Classes

Some unique properties of Python Classes

* You have multiple inheritance!
* The multiple inheritance algorithm can call
* Constructors are called __init__
* Private methods are indicated by the `_` as the first character of the method name. These tend to go at the end of the class, not the beginning!
* Call the constructor of the base classes using the `base` reserved callable word.
* The first parameter of a class member function is `self`, if absent this is a static member.
* There are no overloads, you can use keyword arguments (`kwargs`) instead or just name your methods with more description.

```python
class MyClass (BaseClass1, BaseClass2):
  def __init__(self, arg1):
    base(MyClass, self, arg1)

  def _in_private(self, ooh):
    pass
```

An `instance` is an instantiated variable of a class, (an object).

There are 2 types of fields in the Python classes,

* Instance level
* Class level

This is where you might trip up:

```python
class Dog(object):
  has_fleas = False

  def __init__(self):
    pass

dingo = Dog()
rover = Dog()
rover.has_fleas = True
print(dingo.has_fleas) # Prints False
print(Dog.has_fleas) # Prints False
Dog.has_fleas = True
print(dingo.has_fleas) # Prints True!
```

### Inheritance and Duck Typing

**IF it looks like a duck and quacks like a duck..**

This example illustrates how the leniancy in typing means that your can substitute inheritance with implicit convention.

```python
class Duck:
    def quack(self):
        print("Quaaaaaack!")
    def feathers(self):
        print("The duck has white and gray feathers.")

class Person:
    def quack(self):
        print("The person imitates a duck.")
    def feathers(self):
        print("The person takes a feather from the ground and shows it.")
    def name(self):
        print("John Smith")

def in_the_forest(duck):
    duck.quack()
    duck.feathers()

def game():
    donald = Duck()
    john = Person()
    in_the_forest(donald)
    in_the_forest(john)

game()
```

## Interfaces (or lack of) and abstract methods

Python uses methods named under convention (called protocols) in place of interfaces. These are for core protocols, like disposable objects.

You can also use decorators to indicate abstract member or interface.

## Decorators

A function that takes a function as a parameter and executes it (whilst doing something else) is called a decorator.

You can call them explicitly or with the `@` syntax, e.g.

```python
def p_decorate(func):
   def func_wrapper(name):
       return "<p>{0}</p>".format(func(name))
   return func_wrapper

@p_decorate
def get_text(name):
   return "lorem ipsum, {0} dolor sit amet".format(name)

print get_text("John")
```

The `functools` package contains some useful decorators. https://docs.python.org/2/library/functools.html

## Arguments and keyword Arguments

Read this http://pythontips.com/2013/08/04/args-and-kwargs-in-python-explained/

## Python Protocols

Class behaviours are implemented by using protocols, these are documented in PEPs. An example of a slice list protocol implementation:

```python
class MyClass:
    def __getslice__(self, i, j):
        return self[max(0, i):max(0, j):]
    def __setslice__(self, i, j, seq):
        self[max(0, i):max(0, j):] = seq
    def __delslice__(self, i, j):
        del self[max(0, i):max(0, j):]
```

## Disposable

Since there is no interface for IDisposable, another protocol is used (called the context manager), it expects __enter__ and __exit__ members. PEP343 documents this https://www.python.org/dev/peps/pep-0343/
