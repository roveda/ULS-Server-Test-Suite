# Test Procedure Description

This how-to guide describes the tests and results necessary to assure the correct operation and performance of the ULS-server. 
It covers most features of the common usage. It delivers the main test execution instructions and the expected results 
to verify the proper operation of the ULS-server. Some additional documents, like reports and limit definitions, 
which are referenced in this document are kept as separate documents.


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

## About the Test Suite

This test suite is used to check the proper functionality of a complete working ULS environment. 

The test suite contains the test plan, the test case specifications, their execution instructions and scripts. 
All used data is explained, as well as the expected results whether through the browser-based interactive analysis or through reports.

The test suite was developed and is constantly evolving with each ULS version to check the functionality of the ULS-server. 
The tests are grouped into functional sections.

- Administrative actions define the general requirements for all tests.
- Well-defined values are sent to the ULS-server and checked for correctness by the browser-based interactive analysis.
- Boundary tests are made to assure the covered value ranges.
- Thresholds are defined and their violations are checked for the most common scenarios, including notifications containing message bodies with replaced statically and dynamically calculated embedded placeholders.
- Aggregation of values over hour, day and week.
- Reports are used to check some of the expected results and to check the report functionality itself.
- Logging of user actions concerning administrative changes are verified.
- Tests of the effectiveness of security based separations on users and groups.
- Check of generated notifications based on threshold violations by using the ULS ticket tracking (UTT) and e-mail transmission.

## Test Plan

The ULS stage environment is used to perform the first test. These test scenarios are independent of any other data, it is therefore safe to run it in the production environment as well.

The ULS-server runs in a production-like mode, doing all the processing, aggregation and checks in the same manner as the production, unless otherwise specified for particular tests.

The approach is:
- use the administration section to define new groups, users and notification destinations
- run scripts on operating system level to generate values of all supported value types and send them to the ULS-server.
- use different users to perform the interactive analysis on those values
- create several reports to show the correct results
- define various possible notification scenarios and verify the correctly generated notifications
- define aggregation of values and verify the results
- define various possible notification scenarios on aggregated values and verify the correctly generated notifications

The schedule is:
- some notification scenarios must be defined BEFORE the first test script is run
- all test scripts are executed periodically by cron, until the end of all tests
- some test scripts only produce a limited number of values, they finish when all values have been generated and transferred
- the results and test examinations must be done within 14 days
- the results and test examinations on aggregated values must be done within 120 days


## Elements of this Test Procedure Document

Elements of this test procedure document:

| Element            | Description  |
|----|-----|
| test module        | Main subject of tests (mostly in one top level chapter), it defines the test module abbreviation for the numbering of test cases, e.g. NTFY for notifications. |
| test case          | A test scenario for a related set of tests with limited or enclosed scope, including its full description. |
| test case abstract | Short list of keywords that describe the test case (optional). |
| test               | One specific test within a test case. It is identified by an abbreviation of the test module and a unique number. |
| test result        | TEST: <module abbreviation> <id>: <test case abstract>:  { UNTESTED TODO PASSED FAILED }  |

Description of test results:

| Test Result | Description | 
|--|--|
| UNTESTED | 	This test is currently not tested at all. It will be implemented in a future version of the test procedure. |
| TODO 		| 	This test has still to be tested.	|
| PASSED 	|	This test has been tested and the expected result(s) were observed. |
| FAILED 	|	This test has been tested, defects have been found. The findings are described in the following paragraph. |


## General Structure of Test Sequences

This is of informational use for testers and has no general relevance for ULS.

General structure of test sections:

- Specifications
  -  define specifications
  -  specifications successfully defined? applied correctly? properly propagated?
  -  correct operation?
- Change
  - change specifications
  - applied correctly? properly propagated?
  - correct operation?
- Deactivation
  - deactivate specifications
  - applied correctly? properly propagated?
  - correct operation?
- Removal
  - remove specifications
  - properly removed?
  - all dependent specifications, too?

## What This Guide Is Not
  
This guide is not an ULS documentation, that explains how ULS works. Although, it will describe and refer to some specific operational features in some sections, because of necessary tests.

## Table of Contents
  
- [Preparations](preparations.md)
- [Changelog of ULS-server](changelog.md)
