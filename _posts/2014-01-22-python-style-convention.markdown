---
layout: post
title: "Python style convention"
date: 2014-01-22 09:14:17 +0800
comments: true
categories: programming 
---

Most are from [PEP8](http://www.python.org/dev/peps/pep-0008/). I only list what I'd use.

- - -

# Indentation #

- Use 4 spaces per indentation level.

- Spaces are the preferred indentation method.


# Blank lines #

- Separate top-level function and class definitions with two blank lines.


# Imports #

- Imports should usually be on separate lines, e.g.:

Yes: 

    import os
    import sys

No: 

    import sys, os

- It's okay to say this though:

    from subprocess import Popen, PIPE

- Wildcard imports (```from <module> import *```) should be avoided


# White space in expression and statements #

Avoid extraneous whitespace in the following situations:

- Immediately inside parentheses, brackets or braces.

Yes: 

    spam(ham[1], {eggs: 2})

No:
  
    spam( ham[ 1 ], { eggs: 2 } )


- Immediately before a comma, semicolon, or colon:

Yes: 

    if x == 4: print x, y; x, y = y, x

No:  

    if x == 4 : print x , y ; x , y = y , x

- Immediately before the open parenthesis that starts the argument 

list of a function call:

Yes: 

    spam(1)

No:  

    spam (1)

- Immediately before the open parenthesis that starts an indexing or 

slicing:

Yes: 
    
    dict['key'] = list[index]
    
No:  
    
    dict ['key'] = list [index]

- More than one space around an assignment (or other) operator to align it with another.

Yes:

    x = 1
    y = 2
    long_variable = 3

No:

    x             = 1
    y             = 2
    long_variable = 3 


# Others #

- Always surround these binary operators with a single space on either side: assignment ```(=)```, augmented assignment ```(+=, -= etc.)```, comparisons ```(==, <, >, !=, <>, <=, >=, in, not in, is, is not)```, Booleans ```(and, or, not)```.

- Don't use spaces around the = sign when used to indicate a keyword argument or a default parameter value.

Yes:

    def complex(real, imag=0.0):
        return magic(r=real, i=imag)

No:

    def complex(real, imag = 0.0):
        return magic(r = real, i = imag) 


# Comments #

- Comments should be complete sentences. If a comment is a phrase or sentence, its first word should be capitalized, unless it is an identifier that begins with a lower case letter (never alter the case of identifiers!).

- You should use two spaces after a sentence-ending period.


# Naming #

## Package and Module names ##

- Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability.

- Python packages should also have short, all-lowercase names, although the use of underscores is discouraged.


## Class Names ##

- Class names should normally use the CapWords convention.


## Exception Names ##

- Because exceptions should be classes, the class naming convention applies here. However, you should use the suffix "Error" on your exception names (if the exception actually is an error).


## Function Names ##

- Function names should be lowercase, with words separated by underscores as necessary to improve readability.


## Function and Method Arguments ##

- Always use self for the first argument to instance methods.

- Always use cls for the first argument to class methods.

- If a function argument's name clashes with a reserved keyword, it is generally better to append a single trailing underscore.


## Method Names and Instance Variables ##

- Use the function naming rules: lowercase with words separated by underscores as necessary to improve readability.

- Use one leading underscore only for non-public methods and instance variables.


## Constants ##

- Constants are usually defined on a module level and written in all capital letters with underscores separating words. Examples include MAX_OVERFLOW and TOTAL.


# Designing Guidelines #

- Public attributes should have no leading underscores.

- If your public attribute name collides with a reserved keyword, append a single trailing underscore to your attribute name.

- For simple public data attributes, it is best to expose just the attribute name, without complicated accessor/mutator methods.

- Should you find that a simple data attribute needs to grow functional behavior. In that case, use properties.

- Avoid using properties for computationally expensive operations; the attribute notation makes the caller believe that access is (relatively) cheap.


# Programming Recommandations #

- Comparisons to singletons like None should always be done with is or is not, never the equality operators.

- When implementing ordering operations with rich comparisons, it is best to implement all six operations ```(__eq__, __ne__, __lt__, __le__, __gt__, __ge__)``` rather than relying on other code to only exercise a particular comparison.

- Always use a def statement instead of an assignment statement that binds a lambda expression directly to a name.

Yes:
    
    def f(x): return 2*x

No:
    
    f = lambda x: 2*x

- When a resource is local to a particular section of code, use a with statement to ensure it is cleaned up promptly and reliably after use. A try/finally statement is also acceptable.

- Context managers should be invoked through separate functions or methods whenever they do something other than acquire and release resources. For example:

Yes:

    with conn.begin_transaction():
        do_stuff_in_transaction(conn)

No:

    with conn:
        do_stuff_in_transaction(conn)

- Use string methods instead of the string module.

- Use ```''.startswith()``` and ```''.endswith()``` instead of string slicing to check for prefixes or suffixes.

```startswith()``` and ```endswith()``` are cleaner and less error prone. For example:

Yes: 

    if foo.startswith('bar'):

No:  
    
    if foo[:3] == 'bar’:

- Object type comparisons should always use ```isinstance()``` instead of comparing types directly.

Yes: 

    if isinstance(obj, int):

No:  

    if type(obj) is type(1):

When checking if an object is a string, keep in mind that it might be a unicode string too! In Python 2, str and unicode have a common base class, basestring, so you can do:

    if isinstance(obj, basestring):

Note that in Python 3, unicode and basestring no longer exist (there is only str) and a bytes object is no longer a kind of string (it is a sequence of integers instead)

- For sequences, (strings, lists, tuples), use the fact that empty sequences are false.

Yes: 
    
    if not seq:
    if seq:

No: 

    if len(seq)
    if not len(seq)

- Don't compare boolean values to True or False using ```==```.

Yes:   

    if greeting:

No:    
    
    if greeting == True:

Worse: 

    if greeting is True:


# Executing Modules as Scripts #

Source: [doc_module](http://docs.python.org/2/tutorial/modules.html)

When you run a Python module with

    python fibo.py <arguments>

the code in the module will be executed, just as if you imported it, but with the ```__name__``` set to ```"__main__"```. That means that by adding this code at the end of your module:

    if __name__ == "__main__":
        import sys
        fib(int(sys.argv[1]))

you can make the file usable as a script as well as an importable module, because the code that parses the command line only runs if the module is executed as the “main” file.


