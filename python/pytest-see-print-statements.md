# See print statements in pytest

By default, pytest captures all output (like print statements, logging messages, etc.) during test execution. It only shows this captured output if a test fails. This is done to keep test results clean and focused on what passed or failed.
The `-s` flag tells pytest "don't capture any output" - it's actually a shorthand for `--capture=no`. When you run `pytest -s`, you'll see all output in real-time as your tests run, just like you would if you were running the code directly in Python.

```bash
pytest -s
```
or
```bash
pytest --capture=no
```
