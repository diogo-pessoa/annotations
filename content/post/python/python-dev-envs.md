---
title: "Python Dev - local Env tools"
date: 2024-07-06
description: "grouping tools from my local dev setup."
featured: true
draft: false
toc: true
categories:
  - coding
tags:
  - python
  - in-hindsight
series:
  - diary-of-a-flawed-pythonista
---

## Summary

Short note on simple tools that streamline my local dev-ide development.

---

### IDE - tools

* My IDE of choice [PyCharm](https://www.jetbrains.com/pycharm/)

#### IDE - Debugging

Even though Debugging is a topic of its own. I rely so much on PyCharm's Debugger, that's Grouping
both together.

* [PyCharm](https://www.jetbrains.com/pycharm/) for the win.
    * Haven't used much of else, probably because I didn't have too. The Debugger in PyCharm is
      fantastic.

### Testing

#### pytest

* [pytest](https://docs.pytest.org/en/6.2.x/)
* [unittest](https://docs.python.org/3/library/unittest.html).

* Running **unittests**

```
python3 -m unittest discover -s tests/ -p '*_test.py'

```

* worth mentioning `@unnitest` decorators are great. Sometimes, when not done with a test, just
  `.skip()` for a while.

This is not an excuse to leave tests unfinished. It's a means to an end. If you need to use it too
often. Refactoring :smile:


#### unittest + mock, patch

* [MagicMock](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.MagicMock)


* patch

```python
from unittest.mock import patch, MagicMock

@patch('os.makedirs')
    def test_create_local_dir_creates_directory_when_not_exists(self,
                                                                mock_makedirs):
                                                                
    # pylint: disable=protected-access
        assert data_loader._create_local_dir('test_dir')
        mock_makedirs.assert_called_once_with('test_dir')
```

#### unittest + coverage

- I usually use testing to validate my approach. Sometimes, extra tests for edge cases.
- I hardly go too deep down the TDD rabbit hole, and often ignore the coverage %.
  Honestly, If I'm spending to much time testing, it feels to me, there's too much logic in a single
  snippet.
  When I did run the coverage, I've used the [coverage](https://pypi.org/project/coverage/)

---

### Custom decorator and timeit

Writing your own decorator feels great. That feeling you have, when you made life that bit easier.
Anyway, Decorators are great, useful and fun.

Here's a fun scenario. While preparing for those dreading coding interviews, I used to wrap my
functions with the [timeit](https://docs.python.org/3/library/timeit.html) and a custom decorator
to "benchmark" my code. It was fun, by no means
the [timeit](https://docs.python.org/3/library/timeit.html)
decorator is a pragmatic benchmark. but it's entertaining, and although it won't serve as evidence
for highly efficient code. It definitely exposes bad logic :laughing:

#### decorator timeit - test sample

Add the decorator to the function. I've added the methodName print in the setUp method. Then if you
have some free time you can customise the tests to plot some extra info:

```python
# function
    @timeit_decorator
    def quick_sort_first_el(self, list_to_sort, left_index, right_index):
    
# test

def setUp(self):
        print(f"Running test: {self._testMethodName}")
        self.quick_sort = QuickSort()
```

#### decorator + timeit - test output

```bash

Running test: test__quick_sort_by_first_element
quick_sort_first_el took 0.000001 seconds to execute
quick_sort_first_el took 0.000001 seconds to execute
```

the wrapper
function: [timeit_decorator](https://github.com/diogo-pessoa/coding-exercises/blob/main/helpers/TimeItDecorator.py)

### Static Code and Dependency Analysis

Quite new to me. I'm starting to wet my toes with running security scanners locally. That came to
my attention after enabling [dependabot](https://github.com/dependabot) in my GitHub projects.
Seeing the amount of weekly
notifications, I realised the benefits of locally scanning. _Prevention, better than remediation._

Enough said, here's what I'm experimenting with:

#### safety

* [Safety](https://github.com/pyupio/safety/)

While great for personal projects, unless you go premium there's a 30 days old limitation when
scanning your project. Still worth it.
> Safety is using PyUp's free open-source vulnerability database. This data is 30 days old and
> limited.

* Running

```python
pip install safety

# pip freeze > requirements.txt in case you didn't generate the file yet'
safety check -r requirements.txt 

```

* Scanning

```
  Safety v3.2.4 is scanning for Vulnerabilities...
  Scanning dependencies in your files:

  -> requirements.txt

  Using open-source vulnerability database
  Found and scanned 98 packages
  Timestamp 2024-07-06 17:26:11
  1 vulnerability reported
  0 vulnerabilities ignored

```

#### Safety check - Report

```python
+===============================================================================================================+
 VULNERABILITIES REPORTED 
+===============================================================================================================+

-> Vulnerability found in jinja2 version 3.1.4
   Vulnerability ID: 70612
   Affected spec: >=0
   ADVISORY: In Jinja2, the from_string function is prone to Server Side Template Injection (SSTI)
   where it takes the "source" parameter as a template object, renders it, and then returns it. The...
   CVE-2019-8341
   For more information about this vulnerability, visit https://data.safetycli.com/v/70612/97c
   To ignore this vulnerability, use PyUp vulnerability id 70612 in safety’s ignore command-line argument or
   add the ignore to your safety policy file.

```

#### Safety check - Remediation? not for free.

```
+===============================================================================================================+
   REMEDIATIONS

  1 vulnerability was reported in 1 package. For detailed remediation & fix recommendations, upgrade to a 
  commercial license. 

+===============================================================================================================+

 Scan was completed. 1 vulnerability was reported. 

+===============================================================================================================+

```

#### Safety Conclusion

It's seem useful and the project pretty active, however a lot of features are behind a sign-up or
paid solution.

* `Check` is limited to last 30 days.
* `scan` is only usable if you sign up.

---

#### pip-audit

```
pip install pip-audit
pip-audit --requirement requirements.txt
No known vulnerabilities found 
```

#### pip-audit offers Remediation

Weirdly though, pip-audit didn't flag the vulnerability safety reported and sampled on the previous
topic.

* Remediation

```python

pip-audit --fix

```

---

#### pipdeptree (not a scanner)

- [pipdeptree](https://github.com/tox-dev/pipdeptree/tree/main)

* Run

```
pip install pipdeptree
pipdeptree
```

Not really a Security scanner, but it shows the dependency tree. I can see how it helps to correlate
errors. Additionally, helpful in finding which package is loading a flagged package. The unhappy
thoughts of
troubleshooting ['transitive dependencies'](https://en.wikipedia.org/wiki/Transitive_dependency)
comes to mind

#### Bandit - Security scanner

- [Bandit](https://bandit.readthedocs.io/en/latest/)

* Run

```python
pip install bandit

```

#### Bandit - output

```python

Code scanned:
        Total lines of code: 1236517
        Total lines skipped (#nosec): 4

Run metrics:
        Total issues (by severity):
                Undefined: 0
                Low: 9444
                Medium: 331
                High: 102
        Total issues (by confidence):
                Undefined: 0
                Low: 75
                Medium: 226
                High: 9576
Files skipped (0):
```

### Git commit-hooks

The icing on the cake, git-hooks. Most of this stuff can be configured to run on a pre-commit hook.

Pretty useful, however until you get it all working neatly, I'd wrap it all in a script and run
on-demand.

- [Git hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
- [pre-commit hooks](https://github.com/pre-commit/pre-commit-hooks)

## Code static checking - tools

### Radon - Code Metrics

- [Radon](https://radon.readthedocs.io/en/latest/)

Radon is very comprehensive, worth navigating the documentation. To use more completely.

- [Radon CLI options](https://radon.readthedocs.io/en/latest/commandline.html)

```bash
pip install radon
```

### Linting

* pylint  
  To run pylint, use the following command:

```python
pip install pylint
pylint $(git ls-files '*.py') 
```

I've read a little about [flake8](https://flake8.pycqa.org/en/latest/). Used a couple times. But,
fell back to the comforting familiarity pylint.

---

## Conclusion

I'm big in trying new tools, in scrutinizing features that will make my life easier.
I look for how the tool will improve my current setup. Having said that, once I've passed the point,
I'll happily miss some fancy new feature, the opposite of FOMO.

In conclusion, some tools listed are here, simply because the work for me, others made the cut as a
recent replacement.

In with the new, Static code analysis and dependency scanning is quite new to me. I'm still testing
it
out. Until then, not commit-hooks setup. The idea is to make the best of without slowing down
development. The eternal struggle between speed and quality.

## References

- [PyCharm](https://www.jetbrains.com/pycharm/)
- [timeit](https://docs.python.org/3/library/timeit.html)
- [pytest](https://docs.pytest.org/en/6.2.x/)
- [unittest](https://docs.python.org/3/library/unittest.html)
- [MagicMock](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.MagicMock)
- [coverage](https://pypi.org/project/coverage/)
- [pylint](https://pypi.org/project/pylint/)
- [flake8](https://flake8.pycqa.org/en/latest/)
- [dependabot](https://github.com/dependabot)
- [Safety](https://github.com/pyupio/safety/)
- [pip-audit](https://pypi.org/project/pip-audit/)
- [pipdeptree](https://github.com/tox-dev/pipdeptree/tree/main)
- [Bandit](https://bandit.readthedocs.io/en/latest/)
- [Radon](https://radon.readthedocs.io/en/latest/)
- [Git hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
- [pre-commit hooks](https://github.com/pre-commit/pre-commit-hooks)
