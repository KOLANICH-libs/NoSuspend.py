NoSuspend.py [![Unlicensed work](https://raw.githubusercontent.com/unlicense/unlicense.org/master/static/favicon.png)](https://unlicense.org/)
============
~~[wheel (GitLab)](https://gitlab.com/KOLANICH-libs/NoSuspend.py/-/jobs/artifacts/master/raw/dist/NoSuspend-0.CI-py3-none-any.whl?job=build)~~
[wheel (GHA via `nightly.link`)](https://nightly.link/KOLANICH-libs/NoSuspend.py/workflows/CI/master/NoSuspend-0.CI-py3-none-any.whl)
~~![GitLab Build Status](https://gitlab.com/KOLANICH-libs/NoSuspend.py/badges/master/pipeline.svg)~~
~~![GitLab Coverage](https://gitlab.com/KOLANICH-libs/NoSuspend.py/badges/master/coverage.svg)~~
~~[![GitHub Actions](https://github.com/KOLANICH-libs/NoSuspend.py/workflows/CI/badge.svg)](https://github.com/KOLANICH-libs/NoSuspend.py/actions/)~~
[![Libraries.io Status](https://img.shields.io/librariesio/github/KOLANICH-libs/NoSuspend.py.svg)](https://libraries.io/github/KOLANICH-libs/NoSuspend.py)
[![Code style: antiflash](https://img.shields.io/badge/code%20style-antiflash-FFF.svg)](https://codeberg.org/KOLANICH-tools/antiflash.py)

This is a library to prevent the system from entering powersaving mode such as [ACPI S1-4](https://en.wikipedia.org/wiki/Advanced_Configuration_and_Power_Interface#Power_states).

Requirements
------------
* For Linux you need `python3-dbus` and some programms providing the used D-Bus interfaces.

Tutorial
--------
* A very basic cross-platform way.
 * Import the lib:
```python
from NoSuspend import *
```
 * Use the context manager:
```python
with NoSuspend():
	doLongWork()
```


* You can provide additional arguments depending on platform:

 * on Windows you can provide additional parameters, for example to keep the screen enabled
```
with NoSuspend(suspend=True, display=True, hidden=False, inherit=True):
	doLongWork()
```

 * on Linux (with desktop environment) you can provide your application name and the reason
```python
with NoSuspend(suspend=True, display=False, hidden=False, appName="MySuperApp", reason="doing long work..."):
	doLongWork()
```

* You can retrieve the state when used context manager:
```python
with NoSuspend() as state:
	print(state)
```
on Windows you can just retrieve it using
```python
print(NoSuspend.getCurrentState())
```


* The state is nested, the default state on Windows is ```EXECUTION_STATE.CONTINUOUS | EXECUTION_STATE.SYSTEM_REQUIRED``` ( coresponds to `suspend=True` ) as expected.
There are 2 inherit modes:
```python
print(NoSuspend.getCurrentState())
with NoSuspend() as state1:
	print(state1, NoSuspend.getCurrentState())
	with NoSuspend(display=True, inherit=False) as state2: # the default one, replaces the state
		print(state2, NoSuspend.getCurrentState())
	print(NoSuspend.getCurrentState())
```

```python
print(NoSuspend.getCurrentState())
with NoSuspend() as state1:
	print(state1, NoSuspend.getCurrentState())
	with NoSuspend(EXECUTION_STATE.DISPLAY_REQUIRED, inherit=True) as state2: # adds flags to the state
		print(state2, NoSuspend.getCurrentState())
	print(NoSuspend.getCurrentState())
```

Also a console interface is available

```bash
python3 -m NoSuspend echo a
NoSuspend echo a
caffeinate echo a
```
