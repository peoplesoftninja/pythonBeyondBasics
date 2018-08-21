# Python Quick Start for Linux Admin

# Why Python?

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

# ipython

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

# Managing file system with Module

* import os is used to provie a common interface to both Linux and Windows

![](./img/osCommands.png)

## OS functions

![](./img/osCommands2.png)

![](./img/shutilcommands.png)

## Interacting with Linux System

## Slicing

* Extracts part of a sequence to create a new sequence
* Does not modify the original sequence

![](./img/slicing.png)

![](./img/slicingCommands.png)

## Command Line Arguments

![](./img/commandLine.png)

optparse

argparse

```python
import argparse

# https://docs.python.org/3/library/argparse.html#module-argparse

parser = argparse.ArgumentParser(description="Process Some integers")
parser.add_argument('integer',
                    metavar='N',  # Optional DISPLAY name , seen in help
                    type=int,
                    nargs='+',  # 1 to n arguments can be passed to integer since it doesn't have -- or - you can give arguments directly
                    help="an integer for an accumulator")
parser.add_argument("--sum",
                    metavar="Sum/max",
                    dest='accumulate',
                    action='store_const',
                    const=sum,
                    default=max,
                    help="sum the integers(default find max)")
args = parser.parse_args()
print(args.accumulate(args.integer))
```

## Accessing the Environment

```python
>>> import os
>>> os.environ
```

## Files Streams and Filters

![](./img/fileLikeObjs.png)
![](./img/openFile.png)

```python
import sys

print("This is standard output written on stdout")

print("this is written to stderr", file=sys.stderr)

f = open("out1", "w")
print("This is written out1", file=f)
f.close()

with open("out2", "w") as f:
    print("this is written to out2", file=f)

with open("out3", "w") as f:
    sys.stdout = f
    print("This is written in out3")
```

Simple Python program to reverse line by either giving n text files or giving input from command prompt. uses sys.argv and python slicing


```python
import sys

version = 2

if version == 1:  # Boring Loop
    def revline(line):
        outline = ''
        index = len(line)
        while index:
            index -= 1
            outline += line(index)
        print(outline, end='')

if version == 2:  # recursive loop
    def revline(line):
        if line:
            revline(line[1:])
            print(line[0], end='')

if version == 3:  # Pythonic slicing
    def revline(line):
        print(line[::-1], end='')


def process_code(f):
    for line in f:
        revline(line)


if (len(sys.argv)) == 1:
    process_code(sys.stdin)
else:
    for path in sys.argv[1:]:
        try:
            with open(path, "r") as file:
                process_code(file)
        except Exception as e:
            print("Got an exception:{}".format(e), file=sys.stderr)
```

## Signals

Term = Terminate
core = create cores

![](./img/signalsPython.png)

```python
# time out using sigalarm

import sys
from signal import *


def timeout_handler(signum, frame):  # standard signal method, signumber and frame is signal frame(?)
    raise IOError("User not responding")


def get_name():
    signal(SIGALRM, timeout_handler)  # signal method name
    alarm(5)
    n = sys.stdin.readline()
    alarm(0)
    return n


signal(SIGINT, SIG_IGN)  # CNTRL+Z IGNORED
print("enter your name: ", end='')
sys.stdout.flush()
try:
    name = get_name()
except IOError:
    print("You did not reply, I will call you sleepy")
    name = 'Sleepy'

print("hello {}".format(name))
```

# Combining Python with other tools

## String vs Byte Objects

* Python String is a sequence of unicode characters
* Textual data in Linux is a sequence of bytes

* Everything stored in computer is in the form of bytes
* There are about 256 symbols which have a direct SINGLE byte for them, what about other languages?
* Use two bytes, but that was not enough
* Assign charcters to code points(integers) 1.1M code points, 110k assigned

https://stackoverflow.com/questions/10060411/byte-string-vs-unicode-string-python

```python
>>> a = "foo"
>>> type(a)
<class 'str'>
>>> a = b'foo'
>>> type(a)
<class 'bytes'>
>>> a = r'foo'
>>> type(a)
<class 'str'>
>>> a = b'foo'
>>> type(a)
<class 'bytes'>
>>> a.decode()
'foo'
>>> type(a)
<class 'bytes'>
>>> a = a.decode()
>>> type(a)
<class 'str'>
>>> a = 'foo'
>>> type(a)
<class 'str'>
>>> a = a.encode()
>>> type(a)
<class 'bytes'>
```

## Subprocess

```python
#Demonstrating subprocess printing files > 10k bytes

from subprocess import Popen, PIPE

lister = Popen(["ls", "-l"], stdout=PIPE)

for bytes in lister.stdout:
    line = bytes.decode()
    if line.startswith("total"):
        continue
    spline = line.split()
    if int(spline[4]) > 1000:
        print(spline[8])
```
## Mail

![](./img/pythonSMTPMail.png)

## Sample Program to check disk space in AIX /u01 folder

```py
#! /scs/bin/python3
import subprocess
import argparse
import time

parser = argparse.ArgumentParser()
parser.add_argument('--threshold', '-t', type=int, default=50)
parser.add_argument('--repeat', '-r', action="store_true", default=False)
args = parser.parse_args()

threshold = 50  # default threshold percentage
partition = "/u01"  # default partition

df = subprocess.Popen(["df", "-g"], stdout=subprocess.PIPE)


for line in df.stdout:
    # split into space seperated fields
    splitline = line.split()
    u = splitline[6]
    if u.decode() == partition:
        while True:
            if int(splitline[3][:-1]) > args.threshold:
                print("Warning!, size of /u01 greater than {}%".format(args.threshold))
            if args.repeat:
                time.sleep(5)
            else:
                exit(0)
```

## Open a Tar archive

```py

>>> import tarfile
>>> import os
>>> t = tarfile.open("test.tar","w")
>>> for file in ["out1","out2","out3"]:
...     t.add(file)
...
>>> t.close()
>>> exit()
>>> t = tarfile.open("test.tar")
>>> t.getnames()
['out1', 'out2', 'out3']
>>> t.extractall()

```

```py
# Simple tar program
import sys
import tarfile

if len(sys.argv) > 2:
    list = ["."] # if not files are passed it will archive the directory
else:
    list = sys.arv[1:]

with tarfile.open("temptar.tar", "w") as t:
    for file in list:
        t.add(file)

```

# Manipulating Strings in Python

* Python string is IMMUTABLE sequence of unicode charcters

## format

```py
# Example of specifying field width precision

import math

x = 1
for i in range(10):
    x = x * 2
    y = math.sqrt(x)
    print("{0:4}{1:10}{2:10.4f}".format(i, x, y))
```

## String Test

![](./img/stringTextEx.png)


## Date time

```py
# What day you were born on?

from datetime import datetime

line = input("Enter your DOB in DD/MM/YY format: ")
birthday = datetime.strptime(line, "%d/%m/%Y")
print("You were born on {0:%A}".format(birthday))

```

![](./img/dateTimePercent.png)

## Regular Expression

* Used for Input Validation as above, searching and Text substitution

```py
>>> import re
>>> d3 = re.compile(r"[0-9]{3,}") # compiling
>>> re.search(d3, "abc1234xyz")
<_sre.SRE_Match object; span=(3, 7), match='1234'>
>>> re.search(d3, "abc12xyz")
```

Substituting US to UK dates

```py
>>> import re
>>> convertDate = re.compile(r"(\d\d)-(\d\d)-(\d{4})")
>>> us_dates = "11-24-1997, 03-01-2018, 06-28-2010"
>>> uk_dates = re.sub(convertDate, r"\2-\1-\3", us_dates)
>>> uk_dates
'24-11-1997, 01-03-2018, 28-06-2010'
>>> uk_dates = re.sub(convertDate, r"\2-\1-\3", us_dates, count=2)
>>> uk_dates
'24-11-1997, 01-03-2018, 06-28-2010'
```

# Processing Files

## Logging

* Long running background services require logging to file, syslog or systemd journal
  
![](./img/LogArchitecture.png)

![](./img/LogAttributes.png)

![](./img/Loggerheriarchy.png)

![](./img/LogHandlers.png)

![](./img/LogSeverity.png)

```py
>>> import logging
WARNING:root:Somethin's wrong?
>>> logging.info("some info") # By default logging level is Warning. To change it exit out of repl and type belo
```

```py
>>> import logging
>>> logging.basicConfig(level = logging.DEBUG, format = "%(levelname)s, %(asctime)s, %(message)s")
>>> logging.warning("this is warning")
WARNING, 2018-08-20 07:30:38,893, this is warning
>>> logging.info("works?")
INFO, 2018-08-20 07:31:07,840, works?
```

## Logging DMO using systemd, standard for Linux

To read from Journal use `Journalctcl | tail`

```py
# Logging Demo

import logging
from systemd.journal import JournalHandler  # Preferred Logging Method - 1 import

# Logging to a file

logging.basicConfig(filename="LogDmo.log", level=logging.DEBUG,
                    format="%(levelname)s %(asctime)s %(message)s")

logging.info("Log Started")

# Logging to systemd journal - 7 steps process
# This can be done with 3 steps. 3, 6, 7. Declare Handler, add handler to log, and write log

jlogger = logging.getLogger("journal-logger")  # 2 log name
jhandler = JournalHandler()  # 3 create handler
jformatter = logging.Formatter(fmt="%(levelname)s %(message)s")  # 4 create formatter
jhandler.setFormatter(jformatter)  # 5 update handler with formatter

jlogger.addHandler(jhandler)  # 6 update logger with handler
jlogger.warning("This is warning sent to the Journal")  # 7 write to log


# To stop updating log LogDmo.log as the above will be written there as well

jlogger.propagate = False
jlogger.warning("warning ONLY the Journal")

# Printing the stack trace with exception handler


def bad_idea():
    try:
        1 / 0
    except:
        logging.error("Failed to divide", exc_info=True)


bad_idea()
```

## Processing an Apache Log

```py
# use log and counting x

import collections

hist = collections.defaultdict(int)  # This is same as dict but will call a int value 0 whenever the key is not available

with open("xyz.log") as file:
    for line in file:
        url = line.split()[6].split('?')[0]
        hist[url] += 1  # regular dict fails at his step if new url is not assigned to 1 already

for url in sorted(hist, key=hist.get, reverse=True):
    if hist[url] < 50:  # ignoring small entries
        break
    print("{0:-40s}: {1:6d}".format(url, hist[url]))
```

# Automatic Updates to /etc/fstab 

```py
# Program to replace device names in /etc/fstab

import re
from subprocess import Popen, PIPE

regex = re.compile(r"(/dev/sd[ab][1-9])(.*)")

outfile = open("fstab.out",mode="w")

for line in open("fstab.in"):
    match = re.search(regex, line)
    if match:
        print("Need to replace {}".format(match.group(1)))
        lsblk = Popen(["lsblk","-n","--output","UIUD",match.group(1)],stdout=PIPE)
        uiud = lsblk.stdout.readline().decode()
        replacement = "UIUD=" + uuid[:-1] + re.sub(regex, r"\2",line)
        print("replacement line is {}".format(replacement))
        print(replacement, end='', file=outfile)
    else: # copy the line through unchanged
        print(line, end='', file=outfile)

outfile.close()
```