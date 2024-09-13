# Commitlint hook for pre-commit

## Configuration

* Create your `commitlint` config file in the repo's root, as explained in Commitlint [docs](https://commitlint.js.org/reference/configuration.html#configuration).
* Add the following to your `.pre-commit-config.yaml`:
    ```
    - repo: https://github.com/alessandrojcm/commitlint-pre-commit-hook
      rev: <latest tag>
      hooks:
          - id: commitlint
            stages: [commit-msg]
    ```
* Add your [shared configurations](https://commitlint.js.org/reference/configuration.html#shareable-configuration) as a
  dependency using the `additional_dependencies` parameter of the hooks, here we use the `@commitlint/config-angular`
  as an example:
    ```
    - repo: https://github.com/alessandrojcm/commitlint-pre-commit-hook
      rev: <latest tag>
      hooks:
          - id: commitlint
            stages: [commit-msg]
            additional_dependencies: ['@commitlint/config-angular']
    ```
- Install the [`commit-msg`](https://pre-commit.com/#commit-msg) hook in your project repo:
    ```shell
    pre-commit install --hook-type commit-msg
    ```
  
Note that you **need** to specify a shared configuration in order for `commitlint` to work, if `commitlint` is new to you
just use their default configuration (`@commitlint/config-conventional`). For more information, refer to their [docs](https://commitlint.js.org/guides/getting-started.html).

### Usage in Travis CI

- Add the following to your `.pre-commit-config.yaml`:
    ```
    - repo: https://github.com/alessandrojcm/commitlint-pre-commit-hook
      rev: <latest tag>
      hooks:
          - id: commitlint-travis
            stages: [manual]
    ```
- Add the following to your `.travis.yml`:
    ```
    script:
      - pip install pre-commit
      - pre-commit run --hook-stage manual commitlint-travis
    ```
