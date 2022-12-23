# pre-commit-demo
Demo repository showing the use of pre-commit, a flexible tool for specifying git hooks.

# tl;dr;
Just want to see how this works? Ok, the prerequisite is that you have `conda` installed.
```
git clone https://github.com/lincc-frameworks/pre-commit-demo.git
conda create --name pre-commit-demo python=3.10 --file requirements.txt
conda activate pre-commit-demo
pre-commit install
```

Open `.../pre-commit-demo/hello_world.py`. 

Add two blank lines at the end of the file.

```
git commit -m 'Feeling cute, added some blank lines.'
```

Witness the automatic removal of the blank lines so that the commit adheres to the linting standards.

# The Details

## Setup
Clone the repository and then create a sandbox environment like so:
```
conda create --name pre-commit-demo python=3.10 --file requirements.txt
conda activate pre-commit-demo
```
Notes: 
- Conda is not required, it's just convenient. Feel free to use venv+pip, or just your base environment if you're feeling frisky.
- The requirements.txt file contains only `pre-commit`

### One manual step for now
Run the following command in your local repository:
`pre-commit install` 

That will install some scripts in the `.../pre-commit-demo/.git/` directory. This step is necessary because git explicity excludes that folder from being committed to a reposity, but it also happens to be where git looks for commit hooks. :shrug: The scripts that pre-commit installs are what actually intercept the git command and run the various pre-commit checks. 

I believe this manual step can be automated using setuptools in a pyproject.toml file. However, I wanted simplicity _and_ understanding in this demo repository, so I didn't want to hide it with automation. Ideally, if we would want to enforce a certain set of requirements for all future developers of a code base that we own, we would want the commit hooks to be installed automatically when the developer `python setup.py --dev`'s the code.

## What is checked before a commit
Take a look at the .pre-commit-config.yaml file and you'll see all the pre-commit checks that will be run against the code before you commit it. 
Some will produce warning messages and prevent a commit. Others will automatically fix the problem and push head with the commit. 

### Examples to try locally
1. Add a file to the `tests/` directory such that the file name is not `*_test.py`. You'll get a error when you attempt a `git commit` of this new file. If it really needs to be committed this way, you can use the `--no-verify` flag in the commit.  

2. Add a couple of blank lines at the end of `hello_world.py` and commit that, you'll see that a pre-commit hook identified those blank lines as a code style error and automatically corrected it before finishing the commit. 

## Other neat things
There are lots of different pre-commit hooks are already available and are listed here: https://pre-commit.com/hooks.html

Some examples:
- black formatting
- flake8
- Jupyter notebook formatting
- toml file formatting check

Pre-commit also supports the creation of custom hooks, there's a lot to it, but might be valuable for very specific use cases: https://pre-commit.com/#new-hooks

The official pre-commit demo repo, https://github.com/pre-commit/demo-repo, has another example showing more pre-commit hooks.
