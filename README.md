# Commitlint hook for pre-commit

This is a simple library that provides a "hook" for the
[pre-commit](https://pre-commit.com/) tool to provide commit message
linting using [commitlint](https://commitlint.js.org/#/).

## Configuration

* Create your `commitlint` config file in the repo's root, as explained in Commitlint [docs](https://commitlint.js.org/#/reference-configuration).
* Add the following to your `.pre-commit-config.yaml`:
    ```
    - repo: https://github.com/alessandrojcm/commitlint-pre-commit-hook
      rev: <latest tag>
      hooks:
          - id: commitlint
            stages: [commit-msg]
    ```
* If you are using one of Commitlint's [shared configurations](https://commitlint.js.org/#/reference-configuration?id=shareable-configuration),
  add it to the `additional_dependencies` parameter of the hooks, i.e:
    ```
    - repo: https://github.com/alessandrojcm/commitlint-pre-commit-hook
      rev: <latest tag>
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

