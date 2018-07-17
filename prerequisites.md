* Working Python Environment Minimum need Python3
* Defining Functions, Passing arguments, Returning Values

   ### What are  position arguments, default arguments,keyword arguments, *args and **kwargs

    * Position argumens are simply arguments the values are passed by position
    * keyword argument are same arguments like above but values are passed with the name or keyword, so the position does not matter
    * default argument are those in which there is a default value for the argument which is taken when no value is passed
    * *args are seen as a tuple where we can pass any number of values. You would use *args when you're not sure how many arguments might be passed to your function, i.e. it allows you pass an arbitrary number of arguments to your function.
    * If both ** and * arguments are present then the format is 
    def function(arg1, arg2, *args, kwarg1, kwarg2, **kwarg)

    ```python
    >>> def print_everything(*args):
        for count, thing in enumerate(args):
    ...         print( '{0}. {1}'.format(count, thing))
    ...
    >>> print_everything('apple', 'banana', 'cabbage')
    0. apple
    1. banana
    2. cabbage
    ```

    * **kwargs are dictionary arguments where we can pass any number of named arguments which we haven't defined before

    ```python
    >>> def table_things(**kwargs):
    ...     for name, value in kwargs.items():
    ...         print( '{0} = {1}'.format(name, value))
    ...
    >>> table_things(apple = 'fruit', cabbage = 'vegetable')
    cabbage = vegetable
    apple = fruit
    ```

    * You can also use the `*` and `**` syntax when calling a function. For example
    
    ```python
    >>> def print_three_things(a, b, c):
    ...     print( 'a = {0}, b = {1}, c = {2}'.format(a,b,c))
    ...
    >>> mylist = ['aardvark', 'baboon', 'cat']
    >>> print_three_things(*mylist)
    a = aardvark, b = baboon, c = cat
    ```

* Be able to work with basic single file modules in python. Creating importing and executing

    For creating a module we create a simple script and save it as `.py`. When we want to make it import ready we have to wrap the main tasks in the `if __name__ == "__main__"` this will avoid running the entire program when we import it. There are two ways to execute the python, either the script directly or import it and access the functions. 

* Built-in types. int, float, str, list, dict and set
* Python single inheritance

    When you inherit from one class, using super()
* Instance attributes and Class attributes

    Class attributes are owned by class itself and instance attributes are owned by instances of the class. 

    ```python
    >>> class A:
    ...     a = "I am a class attribute!"
    ... 
    >>> x = A()
    >>> y = A()
    >>> x.a = "This creates a new instance attribute for x!"
    >>> y.a
    'I am a class attribute!'
    >>> A.a
    'I am a class attribute!'
    >>> A.a = "This is changing the class attribute 'a'!"
    >>> A.a
    "This is changing the class attribute 'a'!"
    >>> y.a
    "This is changing the class attribute 'a'!"
    >>> # but x.a is still the previously created instance variable:
    ... 
    >>> x.a
    'This creates a new instance attribute for x!'
    >>> 
    ```

* Raising and handling exception Raising Exceptions, Catching Exceptions, Finally, defining your own exception
* Iterables and iterators. For-loop, next function, 
* Classes with methods
* Reading and writing text files and binary files
* unittest and debugging
* special methods, double underscore methods aka dunder method `__<method name>__`
* 