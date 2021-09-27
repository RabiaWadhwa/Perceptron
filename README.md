# Perceptron

# References
* [Python Package Creation Docs](https://packaging.python.org/tutorials/packaging-projects/)
* [Github Actions - Publishing to package registries](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python#publishing-to-package-registries)

CI/CD.yml code
```python

# name of the workflow
name: Upload Python Package

#when to execute - when you push changes on "main" branch
on:
  push:
    branches:
    - main
    # - dev  => add for more branches if needed

# what to execute - lists jobs
jobs:
  deploy: # first job ,you can list more jobs as well
    runs-on: ubuntu-latest # CREATE UBUNTU ENVIRONMENT
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python  #sets up python
        uses: actions/setup-python@v2
        with:
          python-version: '3.7'  # python version needed
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install build
      - name: Build package
        run: python -m build
      - name: Publish package
        uses: pypa/gh-action-pypi-publish@27b31702a0e7fc50959f5ad993c78deac1bdfc29
        with:
          user: __token__
          password: ${{ secrets.PYPI_API_TOKEN }}

        
```