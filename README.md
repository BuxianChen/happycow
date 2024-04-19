# happycow

This is a demo project for poetry, upload to PyPI, github action, etc. following:

[https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/)

## Prepare code

```bash
conda create --name happycow python=3.10
conda activate happycow
# conda install pipx
# pipx install poetry
poetry new happycow
cd happycow
git init
# .gitignore copy from https://github.com/github/gitignore/blob/main/Python.gitignore
git add .gitignore
git commit -m "add gitignore"
# modify code
git add .
git commit -m "add code"
poetry install
poetry run pytest
```

Now we can use `poetry build` and `poetry publish` to publish package, but we will do this by Github Action.

## Add GitHub action

1. Fill form [https://pypi.org/manage/account/publishing/](https://pypi.org/manage/account/publishing/) with

    ```
    PyPI Project Name: happycow
    Owner: BuxianChen
    Repository name: happycow
    Workflow name: publish-to-pypi.yml
    Environment name: pypi
    ```

2. Generate an API token [https://pypi.org/manage/account/](https://pypi.org/manage/account/)

3. Create a empty repository on github named `happycow`.

4. Add a Environment secrets in `pypi` environment [https://github.com/BuxianChen/happycow/settings/environments](https://github.com/BuxianChen/happycow/settings/environments) with

    ```
    PYPI_API_TOKEN: <API token from PyPI above>
    ```

5. Add workflow to codebase and push tags (trigger github action)

    ```bash
    # add .github/workflows/publish-to-pypi.yml
    git add .github/
    git commit -m "add github workflow"
    git tag v0.1.0
    git remote add origin git@github.com:BuxianChen/happycow.git
    git push origin master:master
    git push --tags
    ```

6. Verify

    Verify pacakges are locate on PyPI and GitHub Releases

    - PyPI: [https://pypi.org/project/happycow/0.1.0/](https://pypi.org/project/happycow/0.1.0/)
    - GitHub Releases: [https://github.com/BuxianChen/happycow/releases/tag/v0.1.0](https://github.com/BuxianChen/happycow/releases/tag/v0.1.0)

    We can use pip to download the package and verify:

    ```bash
    conda create --name verify python=3.10
    conda activate verify
    pip install happycow
    happycow
    ```