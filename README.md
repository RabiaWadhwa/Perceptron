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


# How to use package
```python

from perceptron_package.perceptron_class import Perceptron 
import pandas as pd

def prepare_data(df):
  """ Used to separate dependent and independent features       

  Args:
      df (pd.dataframe): pandas dataframe

  Returns:
      tuple: returns tuple of dependent & independent variables
  """
  X = df.drop("y",axis=1)
  y = df["y"]
  return X,y



def main(data ,eta,epochs):
       df = pd.DataFrame(data)
       df  # Shape = (4,3)

       X,y = prepare_data(df) 

       model = Perceptron(eta=eta, epochs=epochs)  # Creating object of class Perceptron
       model.fit(X, y) # Weights in last epoch are considered as final weights for prediction

       _ = model.total_loss() # last Epoch's Sum of Errors , '_' indicates dummy variable

       
if __name__ == '__main__': # define entry point of program execution
       AND = {"x1":[0,0,1,1],
              "x2":[0,1,0,1],
              "y" :[0,0,0,1]
              }

       ETA = 0.3 # between 0 and 1
       EPOCHS = 10
       main(AND,ETA,EPOCHS)
      
```