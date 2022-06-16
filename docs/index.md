# ULS Server Test Suite

This is a collection of documents and scripts to test the ULS Server functionality.
See [Universal Logging System - Overview & Features](https://www.universal-logging-system.org/dokuwiki/doku.php?id=uls:overview)
for more information.

This document describes the tests and results necessary to assure the correct operation and performance of the ULS Server in version 1.9.6. 
It covers most features of the common usage.

## Conventions
Terminal in- and output is marked by `mono spaced font`.
Long commands and output lines are wrapped without any notice. 
Output can be cutted or cropped for better readability. The ellipsis (`...`) is then used to indicate the intentional omission of text parts. 

Command lines that start with a dollar sign as prompt like:
```
$ ... 
```
indicate that this command is executed by a user with standard operating system privileges (in contrast to root). 
You have probably to be a specific user to run the command successfully, that depends on the context.

Command lines that start with a hash sign as prompt like:
```
# ...
```
indicate that the following command is executed by a user with root privileges.

Commands in command lines may contain parameters. 
Square brackets (`[ ]`) are used to indicate optional parameters, 
a vertical bar (`|`) indicates a choice of parameters, 
the ellipsis (`...`) indicates that the parameter is repeatable and 
curly braces (=curly brackets) (`{ }`) indicate that at least one of these embraced parameters must be included.

Commands in command lines may contain placeholders. 
These must be replaced by real values. They are indicated by angle brackets (`< >`).

Emphasized text expressions are in _italics_ or **bold**.

## What You Need







