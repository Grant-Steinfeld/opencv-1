[![Grant-Steinfeld](https://circleci.com/gh/Grant-Steinfeld/opencv-1.svg?style=svg)](https://grantsteinfeld.com/)
# OpenCV 
## python experiments



## Install the pre-requisites

1. Python version 3
1. Pipenv - Python virtual environment

## Installation steps = windows 10

### 1. Python3

Make sure you have Python installed and it's availible from your command line. You can check if it's installed and determine it's version by running:

```sh
python --version
```

You shoud get some output like `3.8` If you don't have this version of Python, please install the latest `3.x` version.



To [install Python3 on Windows](https://phoenixnap.com/kb/how-to-install-python-3-windows)

To install Python on any other platform take a look at the [Installing Python](https://docs.python-guide.org/starting/installation/) section of **_The Hitchhikers Guide to Python_** or refer to [python.org](http://python.org)


### 2. Pipenv - Python virtual environment

To check you have pipenv installed run the following:

```sh
pipenv --version
```

You should see something like `version 2018.11.26` if not please setup the latest version of pipenv as follows.




To install pipenv on anyplatform with `pip`

```sh

python3 -m pip install pipenv

```

For more detailed instruction [see here](https://pipenv-fork.readthedocs.io/en/latest/install.html#installing-pipenv)


It is a best practice to use use Python virtual environments to isolate project-specific dependencies and create reproducible environments.


### Intializing a `pipenv` Python Virtual Environment

How does one setup a Python Virtual Environment using `pipenv`?



#### You may be asking yourself where your new virtual environment is stored?

Ordinarilly, by default, the `pipenv` virutal enviroments is written to a global (your user's home ) dirctory. The issue here is if you move your project directory this will corrupt the virutal environment.

So never fear!



#### PowerShell on Windows 10
```powershell
$env:PIPENV_VENV_IN_PROJECT=1
```


### Creating a new Pipenv Python3 Virtual Environment

At your command line `cd` to the `root directory` of your application

```sh
#install 
pipenv --three

# or use current version (3)
pipenv install

```

You should now confirm the new pipenv python 3 virtual env in this project

```sh
$ dir

.venv
doc
...
```




The `pipenv` virtual environment should be running confirm by:

```sh
pipenv check
```

Output should confirm all is good!

![check with pipenv](./doc/images/pipenv-check.png)

To exit the `Pipenv` Python Virtual environment simply type `exit`

To return to the pipenv venv shell type:
```sh
pipenv shell
```


### Setting up tooling for Testing

#### Setting up the pytest unit-test framework

> pytest is a no-boilerplate alternative to Python’s standard unittest module

```sh
pipenv install --dev pytest
```

`pytest` is used to write tests first and begin our journey towards Test Driven Development, been a fully-featured and extensible test tool, it boasts a simple syntax. Creating a test suite is as easy as writing a module with a couple of functions:

```python
#contents of tests/unit/test_sample.py
def plusOne(x):
    return x + 1

def test_simple():
    assert plusOne(7) == 8
```

the test is run by running the pytest command.

```sh
pytest tests/unit/test_sample.py
```

![pytest running sample test in pipenv](./doc/images/pytest-running-test-in-pipenv.png)

### Code stylers and formatters

`Flake8` is a command-line utility for enforcing style consistency across Python projects.

<details><summary><strong>learn more about flake8</strong></summary>

> [Flake8](https://flake8.pycqa.org/en/latest/index.html), by default it includes lint checks provided by the PyFlakes project, PEP-0008 inspired style checks provided by the PyCodeStyle project, and McCabe complexity checking provided by the McCabe project. It will also run third-party extensions if they are found and installed.

</details>

`Black` is a Python formatting tool.

<details><summary><strong>learn more about Black</strong></summary>
> By using Black, you agree to cede control over minutiae of hand-formatting. In return, Black gives you speed, determinism, and freedom from pycodestyle nagging about formatting. You will save time and mental energy for more important matters.

> Black makes code review faster by producing the smallest diffs possible. Blackened code looks the same regardless of the project you’re reading. Formatting becomes transparent after a while and you can focus on the content instead.

[Read the Black documentation](https://black.readthedocs.io/en/stable/) for more information

</details>

To install these:

```sh
pipenv install --dev flake8 black==19.10b0
```



Add new python packages:

```sh
pipenv install --dev pre-commit
pipenv install --dev flake8-bugbear
```

## CI/CD
### create requirements.txt

```sh
 pipenv lock -r > requirements.txt
 ```


## Logging

Logs provide visibility into the behavior of a running app. Logs are the stream of aggregated, time-ordered events collected from the output streams of all running processes and backing services.

<details><summary><strong>Learn more about logging</strong></summary>

> A [twelve-factor app[(https://12factor.net/logs)] never concerns itself with routing or storage of its output stream. It should not attempt to write to or manage logfiles. Instead, each running process writes its event stream, unbuffered, to `stdout`. During local development, the developer will view this stream in the foreground of their terminal to observe the app’s behavior.

> In staging or production deploys, each process’ stream will be captured by the execution environment, collated together with all other streams from the app, and routed to one or more final destinations for viewing and long-term archival. These archival destinations are not visible to or configurable by the app, and instead are completely managed by the execution environment.

### Motivation to instrument logging in your code

> Diagnostic logging records events related to the application’s operation. If a user calls in to report an error, for example, the logs can be searched for context.

> Audit logging records events for business analysis. A user’s transactions can be extracted and combined with other user details for reports or to optimize a business goal.

[The Hitchhiker's Guide to Python: Logging](https://docs.python-guide.org/writing/logging/)

</details>

## Start Test Driven Development

### Red-Green-Refactoring.

There are 5 basic steps as illustrated in Figure 1. below.

![5 steps Red-Green-Refactor cycle of Test Driven Devlopment](./doc/images/tdd-red-green-refactoring-v8.jpg)

**_Figure 1. The 5 stages in the Red-Green-Refactor software development cycle_**

Requests originate from the BDD / Agile Design phase/thinking sessions.

Common requests are new feature stories or issue/bug fixes.

These are the 5 steps:

1. Pick a request from your project management system [5]

   1. Action it! by Read, understand the request

1. Write a test to reflect the requirement
   1. run test it must fail!! `(Red)`
1. Write the code
   1. run test - code until test passes `(Green)`
1. refine, cleanup code `(Refactor)`
   1. run test - if fails continue to refactor till it passes
1. rinse, lather, repeat.

### Sometimes tests expect exctpions so how to make them not fail.

How to properly assert that an exception gets raised in pytest?

According to this [Stackoverflow post](https://stackoverflow.com/questions/23337471/how-to-properly-assert-that-an-exception-gets-raised-in-pytest) Pytest has 2 ways to accomadate this:

1. Using `pytest.raises` is likely to be better for cases where you are testing exceptions your own code is deliberately raising

1. using `@pytest.mark.xfail` with a check function is probably better for something like documenting unfixed bugs (where the test describes what "should" happen) or bugs in dependencies.

## Foot notes

[5] Project management tool include:

- Jira
- Pivotal Tracker
- ZenHug
- GitHub issues
- other

# Resources

[Python Testing with pytest: Simple, Rapid, Effective, and Scalable.](https://pragprog.com/book/bopytest/python-testing-with-pytest) Okken, Brian. Pragmatic Bookshelf.

# License

This code is licensed under the Apache License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1](https://developercertificate.org/) and the [Apache License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache License FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
