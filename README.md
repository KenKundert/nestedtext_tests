Official NestedText Test Suite
==============================

Version: 2.0.0

Test cases for NestedText are written in JSON, for the purpose of allowing 
NestedText implementations in any language to use the same tests.

Understanding the tests
-----------------------
Each test case consists of a directory with some number of the following files:

- `load_in.nt`: A NestedText file that is meant to be loaded.  The presence 
  of this file indicates that this case is meant to test the `load()` function, 
  and that one of (but not both of) `load_out.json` or `load_err.json` must 
  also be present.

- `load_out.json`: A JSON file encoding the data structure that should be 
  loaded from `load_in.nt`.  The presence of this file indicates that the data 
  structure should be loaded without errors.

- `load_err.json`: A JSON file describing the parameters of the error that 
  should be triggered when attempting to load `load_in.nt`.  Specifically, 
  these parameters include:

  - `lineno`: The line number (counting from 1) where the error occurs.
  - `colno`: The column number (counting from 0) where the error occurs.
  - `message`: The error message that should be produced.

- `dump_in.json`: A JSON data structure that is meant to be dumped to 
  NestedText.  The presence of this file indicates that this case is meant to 
  test the `dump()` function, and that one of (but not both of) `dump_out.nt` 
  or `dump_err.json` must also be present.  This file may have a different file 
  type (e.g. `*.py`) for tests involving data structures that cannot be 
  represented in JSON (e.g. dictionary keys with newlines).

- `dump_out.nt`: A NestedText file that should be dumped from the data 
  structure in `dump_in.json`.  The presence of this file indicates that the 
  data structure should be dumped without error.

- `dump_err.json`: A JSON file describing the parameters of the error that 
  should be triggered when attempting to dump the data structure in 
  `dump_in.json`.  Specifically, these parameters include:

  - `culprit`: The specific object responsible for the error.
  - `message`: The error message that should be produced.

It's very possible for a single test case to test both the `load()` and 
`dump()` functions.  For such tests, it's common to symlink `load_in.nt` to 
`dump_out.nt` and/or `load_out.json` to `dump_in.json`.

Using the tests
---------------

### Installation
This repository is meant to be included as a submodule of any repository that 
implements the NestedText standard.  This approach makes it easy to share tests 
between implementations (e.g. corner cases found in one implementation could be 
added to this repository and made available to every implementation) and easy 
to control which version of the tests are being used (e.g. ).

It would be good to familiarize yourself with the `git submodule` command 
before attempting to incorporate these tests into a project, but the basic 
command to do so is shown below.  Note that it's conventional to name the 
submodule `official_tests`:

    $ git init «new-project»
    $ cd «new-project»
    $ git submodule add git@github.com:KenKundert/nestedtext_tests.git tests/official_tests

To incorporate new tests from the upstream repository:

    $ cd tests/official_tests
    $ git pull

### APIs
The `api/` directory contains code that parses the test cases into native data 
structures for different languages.  If you're writing an implementation for a 
language that isn't represented in the `api/` directory yet, it would be much 
appreciated if you could submit an API (via pull request) and make it easier 
for others to create implementations.

### Helper scripts
In addition to the tests themselves, this repository contains a number of 
helper scripts to make it easier to work with the tests.  These scripts 
include:

- `show_test_cases.py`: Print the actual test inputs and outputs.  This can be 
  a good way to make sure that a certain feature is being adequately exercised.

- `make_test_case.py`: Create a new test case.  You will be automatically 
  prompted to fill out the relevant files.

- `renumber_test_cases.py`: Renumber the test cases.  This provides an easy way 
  to keep the numbering logical after adding or removing tests.

For more detailed usage information, run any of these scripts with the `-h` 
flag.
