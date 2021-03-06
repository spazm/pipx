Thanks for your interest in contributing to pipx!

## Running pipx From Source Tree
To run the pipx executable from your source tree during development, run pipx from the src directory:

```
python src/pipx --version
```

## Running Tests

### Setup
pipx uses an automation tool called [nox](https://pypi.org/project/nox/) for development, continuous integration testing, and various tasks.

`nox` defines tasks or "sessions" in `noxfile.py` which can be run with `nox -s SESSION_NAME`. Session names can be listed with `nox -l`.

Install nox for pipx development:
```
python -m pip install --user nox
```

Tests are defined as `nox` sessions. You can see all nox sessions with
```
nox -l
```

At the time of this writing, the output looks like this
```
>> nox -l
Sessions defined in /home/csmith/git/pipx/noxfile.py:

* tests-3.6
* tests-3.7
* tests-3.8
- cover -> Coverage analysis
* lint
* docs
- develop-3.6
- develop-3.7
- develop-3.8
- build
- publish
- watch_docs
- publish_docs
```

### Unit Tests
To run unit tests in Python3.7, you can run
```
nox -s tests-3.7
```

!!! tip
    You can run a specific unit test by passing arguments to pytest, the test runner pipx uses:

    ```
    nox -s tests-3.8 -- -k EXPRESSION
    ```

    `EXPRESSION` can be a test name, such as

    ```
    nox -s tests-3.8 -- -k test_uninstall
    ```

    Coverage errors can usually be ignored when only running a subset of tests.

### Lint Tests

```
nox -s lint
```

## Testing pipx on Continuous Integration builds
When you push a new git branch, tests will automatically be run against your code as defined in `.github/workflows/on-push.yml`.

## Building Documentation

`pipx` autogenerates API documentation, and also uses templates.

When updating pipx docs, make sure you are either modifying a file in the `templates` directory, or the `docs` directory. If in the `docs` directory, make sure the file was not autogenerated from the `templates` directory. Autogenerated files have a note at the top of the file.

You can generate the documentation with
```
nox -s docs
```

This will capture CLI documentation for any pipx argument modifications, as well as generate templates to the docs directory.

To preview changes, including live reloading, open another terminal and run
```
nox -s watch_docs
```

### Publishing Doc Changes to GitHub pages
```
nox -s publish_docs
```

## Releasing New `pipx` Versions
### Pre-release

* Update pipx's version in `src/pipx/version.py` to the new version
* Make sure the changelog is updated
    * Put the version of the new release as the header for the latest block of changes (instead of "dev").
    * Make sure all notable changes are listed.
* Regenerate documentation.

### Release
To publish to PyPI: From a clone of the main pipxproject pipx repo (not a fork), run
```bash
nox -s publish
```

Additionally, create a new release in GitHub.

### Post-release
* Update pipx's version in `src/pipx/version.py` by adding a suffix `"dev0"` for unreleased development.
* Update the changelog to start a new section at the top entitled `dev`
