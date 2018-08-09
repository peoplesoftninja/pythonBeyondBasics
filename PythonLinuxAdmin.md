# Python Quick Start for Linux Admin

## Why Python?

* Easy to learn and write
* REPL, can be used interactively
* Powerful built-in data types
    * Strings, integers, floats
    * List, tuple,set, dictionary
* Object-Oriented
    * Many pre-defined classes
* Batteries Included
    * Huge library of moduels
    * Many more in your distro's repo
    * Huge number at Python package index pypi.python.org

![](./img/ListOfModules1.png)

![](./img/ListofModules2.png)

## ipython

* ipython3 is REPL+Shell
* sudo apt-get install ipython3
```python
>>>automagic # to turn off, after this u need to give % befor every command
>>>%automagic to turn on
>>>alias findmysyslinks ls -l /etc/ | grep '^l'
>>>run example.py
>>>command(?) will give help
>>>import xyz # to get all modules of xyx just give xyz. and tab
```

## Managing file system with Module