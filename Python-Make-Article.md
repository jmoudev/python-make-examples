Have you ever found yourself running the same commands over and over again to run, test, and install dependencies on your Python project.

If you haven't heard of **make** then the answer to that question is probably YES!!!

The following guide should provide some make basics and useful resources for running make on your Python project.

All examples in the guide are run on macOS using a Bash terminal with Python 3.13.

# What Is GNU Make?

[GNU Make](https://www.gnu.org/software/make/) is a widely used build automation tool, which enables users to define sets of commands (known as rules), to make files (known as targets).

## Installing GNU Make

Installing make for Linux and Mac should be straightforward via the standard package manager for your Linux OS, or via brew on macOS.

For guidance for Windows see article: [How to install and use "make" in Windows?](https://stackoverflow.com/a/32127632).

## Basics

A project using make usually has a file named Makefile at the project root.

Let's define a basic rule for target foo:

```Makefile
# foobar/Makefile
foo:
	@echo Creating file foo
	@echo foo file contents > foo
```

Running make for target foo from the Bash terminal:

```bash
$ make foo
Creating file foo
```

Executing `make foo` from the project root runs the rule, creating the target file foo.

Let's create another rule for bar with a pre-requisite foo:

```Makefile
# foobar/Makefile
.PHONY: bar
bar: foo
	@echo bar
```

```bash
$ make bar
bar
```

The line `bar: foo` states that the file foo is a pre-requisite for bar, and therefore will be created before bar.

However, notice `Creating file foo` is not printed when running `make bar`. To demonstrate what's going on here we can run make target foo again:

```bash
$ make foo
make: 'foo' is up to date.
```

`foo` is not created as the file was already created via `make foo`.

You may notice .PHONY declaration above bar. This declares the file as a phony target, which is not typically the name of a file, but a rule to be executed when you run make.

Let's amend target foo and declare it as a phony target:

```Makefile
# foobar/Makefile
.PHONY: foo
foo:
	@echo foo
```

```bash
$ make bar
foo
bar
```

Now when we run `make bar`, foo is printed to the terminal.

# Python Applications

To demonstrate some common applications let's think about some common user stories which could be defined in a Makefile:
- As a user I want to run my project
- As a developer I want to set up the project for development
- As a developer I want to run the tests locally
- As a developer I want CI to test across a range of Python versions
- As a developer I want CI to build the documentation
- As a developer I want CI to perform lint and formatting checks

## User

Provided a basic script `main.py` which should print "Hello, world!", we can easily run the script via make:

```Python
# hello-world/main.py
def main():
    return "Hello, world!"

if __name__ == "__main__":
    print(main())
```

```Makefile
# hello-world/Makefile
.PHONY: run
run:
	python main.py
```

```bash
$ make run
Hello, world!
```

As well as for Python scripts, the `run` target could be useful for any Python application which takes basic args such as a Flask server.

In the example where we have a CLI application where there's a specific command to run the application, it may be more useful to include a `help` target to print the CLI helper.

## Developer

When developing on a shared / open source project there are often coding standards to follow, and tests to be run before committing code.

### Developer Setup

Pre-requisites:
- A project using [git](https://git-scm.com)
- [Pre-commit configuration](https://pre-commit.com/#2-add-a-pre-commit-configuration)
- A minimal setup.py or pyproject.toml file, see [example pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#a-full-example)
- Compatible build-backend for editable builds: see [How to install a package using pip in editable mode with pyproject.toml?](https://stackoverflow.com/a/69711730) for pyproject.toml compatible versions

This project makes use of pre-commit to enforce coding standards upon commit ([pre-commit hooks](https://pre-commit.com)), as well as [pytest](https://docs.pytest.org/en/stable/) testing framework.

The target make dev, installs the required Python packages for development, and installs the pre-commit hooks:

```Makefile
# hello-world/Makefile
.PHONY: dev
dev:
	python -m pip install -e . pre-commit pytest
	pre-commit install --install-hooks
```

```bash
$ make dev
python -m pip --require-virtualenv install -e . pre-commit pytest
...
Successfully installed main-0.0.0
pre-commit install
pre-commit installed at .git/hooks/pre-commit
```

The above example also utilizes [editable mode](https://pip.pypa.io/en/stable/topics/local-project-installs/#editable-installs) which enable us to make changes and test these changes without re-installing the application / package(s) to our local venv.

### Testing Locally

Pre-requisites:
- Run `make dev` to install `pytest`
- Some unit tests in the `tests/` directory at the project root

```Makefile
# hello-world/Makefile
.PHONY: test
test:
	pytest tests/
```

```bash
$ make test
pytest tests/
...
test_main.py .
```

## CI

For larger shared / open source projects you may encounter a continuous integration (CI) pipeline, which can run a number of checks on code before it is merged into the main branch.

There are many pre-requisites for setting up CI on a repository which will not be covered in this article.

### Setup

```bash
$ make install-ci
python -m pip install mypy ruff tox
...
```

### Lint and Type Checking

Some examples of rules for lint and type checking:

```bash
$ make lint
ruff check . --fix
All checks passed!
```

```bash
$ make type
mypy main.py
Success: no issues found in 1 source file
```

### Testing Across Multiple Python Versions

Pre-requisites:
- [tox config](https://tox.wiki/en/stable/config.html)
- Available Python versions as specified

We may want to run the tests in ci across a range of Python versions with tox:

```bash
$ make test-all
tox run-parallel
```

## Further Suggestions

Further suggestions for improving your Makefile:
- Add an environment variable (i.e. PYTEST_ARGS) to pytest command to allow passing in the specific test file / test
- Measure tests coverage (pytest-cov)
- Build documentation (sphinx)
- Publish the project to PyPI (twine)

## Conclusion

By adding a Makefile to your Python project, you can streamline so many aspects of development.

Basic tasks like running the project or tests, linting, and packaging your project and more can be standardized with a simple Makefile. This saves time and ensures consistency across your development workflow, as well as reducing the barrier to entry for developers to contribute to your project.

As you grow and share your Python projects, **make** can be invaluable to maintain consistent and efficient development process.

The examples in this guide and more are available in the following GitHub repository: [https://github.com/jmoudev/python-make-examples](https://github.com/jmoudev/python-make-examples)

Thanks for reading!
