# Commitlint hook for pre-commit

This is a simple library that provides a "hook" for the
[pre-commit](https://pre-commit.com/) tool to provide commit message
linting using [commitlint](https://commitlint.js.org/#/).

## Configuration

### 1) Prepare for commitlint

Create a `.commitlintrc.json` configuration file in the root of your
repository. A good starting point is this example file:

```json
{
  "extends": ["@commitlint/config-conventional"],
  "rules": {
    "header-max-length": [2, "always", 50],
    "body-max-line-length": [1, "always", 72],
    "scope-empty": [1, "never"],
    "subject-case": [1, "always", "sentence-case"]
  }
}
```

### 2) Add commitlint to pre-commit

- Add the following to your `.pre-commit-config.yaml`, under the `repos`
  entry:

  ```yaml
  - repo: https://github.com/biarri/commitlint-pre-commit-hook
    rev: v5.0.0
    hooks:
      - id: commitlint
        stages: [commit-msg]
        additional_dependencies: ["commitlint/config-conventional"]
  ```

- You'll want to tell pre-commit to continue to just run `pre-commit`
  hooks as the default stage for regular hooks, by setting the
  `default_stages` key in the root of the `.pre-commit-config.yaml` file
  too:

  ```yaml
  default_stages: [commit]
  ```

- Install the
  [`commit-msg`](https://pre-commit.com/#pre-commit-for-commit-messages)
  hook in your project repo: 
  ```shell
  pre-commit install --hook-type commit-msg
  ```

## Glossary

The terms used for commit linting are quite overloaded and can be
confusing at first.

| Term                                                                       | Definition                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[commitlint](https://commitlint.js.org)**                                | A tool that can check if a commit conforms to the **Conventional Commits** specification. Installed by **pre-commit (Software)**.                                                                                                                                   |
| **commitlint-pre-commit-hook** (this repo)                                 | A small plugin that allows **pre-commit (Software)** to download and run **commitlint** on your commit messages.                                                                                                                                                    |
| **[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)** | A _specification_ for how to write commits. No code, tools, or anything else. Conceptually similar to Semantic Versioning for example.                                                                                                                              |
| **Git Hook**                                                               | A script that `git` itself will run when particular events occur, such as just before commits, pushes, etc. These are the scripts in your project's `.git/hooks/` directory. The **pre-commit (Software)** handles managing these, don't worry about touching them. |
| **Git Hook: `pre-commit`**                                                 | Runs just before a commit is made, usually to check if linting, tests, and type-checking are ok. May optionally abort the commit if a problem is found. Most git hooks are pre-commit hooks.                                                                        |
| **Git Hook: `commit-msg`**                                                 | Git hook just for validating the commit message. May optionally abort a commit if a problem is found. The **commitlint** tool is setup to use this hook to validate commit messages.                                                                                |
| **[pre-commit (Software)](https://pre-commit.com/)**                       | A tool you install thats manages **Git Hooks** according to a `.pre-commit-config.yaml` file. Runs tools before commits such as prettier, black, and **commitlint**.                                                                                                |
| **pre-commit (Software) Hook**                                             | A definition in the `.pre-commit-config.yaml` file, defining a particular action to occur during a **Git Hook's** execution.                                                                                                                                        |


## Full Examples

You can copy these examples verbatim if you're getting started or have
gotten stuck.

<details>
  <summary>Full <code>.pre-commit-config.yaml</code> Example</summary>
  
  ```yaml
  # Pre-commit defaults to running all hooks for all installed stages.
  # This causes the linters to get confused trying to lint the
  # .git/COMMIT_EDITMSG file during commits, as we install a commit-msg
  # hook for the commitlint tool. This makes most hooks only run for
  # staged files in the pre-commit phase.
  default_stages: [commit]

  repos:
    - repo: https://github.com/pre-commit/pre-commit-hooks
      rev: v3.3.0
      hooks:
        - id: end-of-file-fixer
        - id: trailing-whitespace
    - repo: https://github.com/psf/black
      rev: stable
      hooks:
        - id: black
    - repo: https://github.com/prettier/prettier
      rev: 2.1.2
      hooks:
        - id: prettier
    - repo: https://github.com/biarri/commitlint-pre-commit-hook
      rev: v5.0.0
      hooks:
        - id: commitlint
          stages: [commit-msg]
          additional_dependencies: ["@commitlint/config-conventional"]
  ```

</details>

