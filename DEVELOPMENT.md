## Start Developing

<details><summary><h3>with the Command Line</h3></summary>

1. Make sure to have [Git](https://git-scm.com/),
   [pip](https://pip.pypa.io/en/stable/installation/),
   [Python3.9+](https://www.python.org/downloads/), and
   [PDM](https://pdm.fming.dev/latest/) installed.

   Optionally, make sure to have [GNU Make](https://www.gnu.org/software/make/)
   available on your system if you aren't using a UNIX-based system for a better
   developer experience. If you don't want to use `make` then please refer to
   the [Makefile](./Makefile) for specific commands on a given make target.


2. Clone the source code from your fork of BentoML's GitHub repository:

   ```bash
   git clone git@github.com:username/BentoML.git && cd BentoML
   ```

3. Add the BentoML upstream remote to your local BentoML clone:

   ```bash
   git remote add upstream git@github.com:bentoml/BentoML.git
   ```

4. Configure git to pull from the upstream remote:

   ```bash
   git switch main # ensure you're on the main branch
   git fetch upstream --tags
   git branch --set-upstream-to=upstream/main
   ```

5. Install BentoML in editable and all development dependencies:

   ```bash
   pdm install -G all
   pre-commit install
   ```

   This installs BentoML with editable mode via `pdm` and development
   dependencies in a isolated environment. If you wish not to setup within an
   isolated environment, pass `--no-isolation` to pdm

   > **Note**: Make sure to prepend `pdm run` to all commands within this guide
   > if you are using isolated environment via `pdm`.

6. Test the BentoML installation either with `bash`:

   ```bash
   bentoml --version
   ```

   or in a Python session:

   ```python
   import bentoml
   print(bentoml.__version__)
   ```

</details>

<details><summary><h3>with VS Code</h3></summary>

1. Confirm that you have the following installed:

   - [Python3.9+](https://www.python.org/downloads/)
   - VS Code with the [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) and [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) extensions

2. Fork the BentoML project on [GitHub](https://github.com/bentoml/BentoML).

3. Clone the GitHub repository:

   1. Open the command palette with Ctrl+Shift+P and type in 'clone'.
   2. Select 'Git: Clone(Recursive)'.
   3. Clone BentoML.

4. Add an BentoML upstream remote:

   1. Open the command palette and enter 'add remote'.
   2. Select 'Git: Add Remote'.
   3. Press enter to select 'Add remote' from GitHub.
   4. Name your remote 'upstream'.

5. Pull from the BentoML upstream remote to your main branch:

   1. Open the command palette and enter 'checkout'.
   2. Select 'Git: Checkout to...'
   3. Choose 'main' to switch to the main branch.
   4. Open the command palette again and enter 'pull from'.
   5. Click on 'Git: Pull from...'
   6. Select 'upstream'.

6. Open a new terminal by clicking the Terminal dropdown at the top of the window, followed by the 'New Terminal' option. Next, add a virtual environment with this command:
   ```bash
   python -m venv .venv
   ```
7. Click yes if a popup suggests switching to the virtual environment. 

8. Update your PowerShell execution policies. Win+x followed by the 'a' key opens the admin Windows PowerShell. Enter the following command to allow the virtual environment activation script to run:
   ```
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```
   </details>

## Making Changes

<details><summary><h3>using the Command Line</h3></summary>

1. Make sure you're on the main branch.

   ```bash
   git switch main
   ```

2. Use the git pull command to retrieve content from the BentoML Github repository.

   ```bash
   git pull
   ```

3. Create a new branch and switch to it.

   ```bash
   git switch -c my-new-branch-name
   ```

4. Make your changes!

5. Use the git add command to save the state of files you have changed.

   ```bash
   git add <names of the files you have changed>
   ```

6. Commit your changes.

   ```bash
   git commit
   ```

7. Push all changes to your fork on GitHub.
   ```bash
   git push
   ```
   </details>

<details><summary><h3>using VS Code</h3></summary>

1. Switch to the main branch:

   1. Open the command palette with Ctrl+Shift+P.
   2. Search for 'Git: Checkout to...'
   3. Select 'main'.

2. Pull from the upstream remote:

   1. Open the command palette.
   2. Enter and select 'Git: Pull...'
   3. Select 'upstream'.

3. Create and change to a new branch:

   1. Type in 'Git: Create Branch...' in the command palette.
   2. Enter a branch name.

4. Make your changes!

5. Stage all your changes:

   1. Enter and select 'Git: Stage All Changes...' in the command palette.

6. Commit your changes:

   1. Open the command palette and enter 'Git: Commit'.

7. Push your changes:
   1. Enter and select 'Git: Push...' in the command palette.

</details>

## Run BentoML with verbose/debug logging

To view internal debug loggings for development, set the `BENTOML_DEBUG` environment variable to `TRUE`:

```bash
export BENTOML_DEBUG=TRUE
```

And/or use the `--verbose` option when running `bentoml` CLI command, e.g.:

```bash
bentoml get IrisClassifier --verbose
```


Run linter/format script:

```bash
pre-commit run --all-files
```

Run type checker:

```bash
make type
```

## Editing proto files

The proto files for the BentoML gRPC service are located under [`bentoml/grpc`](./bentoml/grpc/).
The generated python files are not checked in the git repository, and are instead generated via this [`script`](./scripts/generate_grpc_stubs.sh).
If you edit the proto files, make sure to run `./scripts/generate_grpc_stubs.sh` to
regenerate the proto stubs.

## Deploy with your changes

Test out your changes in an actual BentoML model deployment, you can create a new Bento with your custom BentoML source repo:

1. Install custom BentoML in editable mode. e.g.:
   - git clone your bentoml fork
   - `pip install -e PATH_TO_THE_FORK`
2. Set env var `export BENTOML_BUNDLE_LOCAL_BUILD=True`
3. Build a new Bento with `bentoml build` in your project directory
4. The new Bento will include a wheel file built from the BentoML source, and
   `bentoml containerize` will install it to override the default BentoML installation in base image


## Testing

Make sure to install all dev dependencies:

```bash
pdm install
```

BentoML tests come with a Pytest plugin. Export `PYTEST_PLUGINS`:

```bash
export PYTEST_PLUGINS=bentoml.testing.pytest.plugin
```

To run all tests with PDM, do the following:

```bash
pdm run nox
```

## Benchmark

BentoML has moved its benchmark to [`bentoml/benchmark`](https://github.com/bentoml/benchmark).

## Creating Pull Requests on GitHub

Push changes to your fork and follow [this
article](https://help.github.com/en/articles/creating-a-pull-request)
on how to create a pull request on github. Name your pull request
with one of the following prefixes, e.g. "feat: add support for
PyTorch". This is based on the [Conventional Commits
specification](https://www.conventionalcommits.org/en/v1.0.0/#summary)

- feat: (new feature for the user, not a new feature for build script)
- fix: (bug fix for the user, not a fix to a build script)
- docs: (changes to the documentation)
- style: (formatting, missing semicolons, etc; no production code change)
- refactor: (refactoring production code, eg. renaming a variable)
- perf: (code changes that improve performance)
- test: (adding missing tests, refactoring tests; no production code change)
- chore: (updating grunt tasks etc; no production code change)
- build: (changes that affect the build system or external dependencies)
- ci: (changes to configuration files and scripts)
- revert: (reverts a previous commit)

Once your pull request is created, an automated test run will be triggered on
your branch and the BentoML authors will be notified to review your code
changes. Once tests are passed and a reviewer has signed off, we will merge
your pull request.

## Documentations

Refer to [BentoML Documentation Guide](./docs/README.md) for how to build and write
docs.
