# Business_Analysis

This repo is for the BA final project. In this project we use two python packages ([SciPy](https://www.scipy.org/), [PuLP](https://coin-or.github.io/pulp/)) to solve Linear Programing problems in ICE2 of the class. 
## Getting Started

### Install 
Require Python 3 or higher

- Install SciPy
    ``` bash
    pip install SciPy
    ```

- Install PuLP
    ```python
    pip install pulp
    ```

### Troubleshoot 
If encounter error: 
- `Solver not found`
- `NoneType Error` 

PuLP solver is missing -> Install PuLP solver 
- For Mac: 
    ```bash
    brew install glpk
    ```
- For Conda environment: 
    ```bash
    conda install -c conda-forge glpk
    ```

<br/>

## Implementation 

### [SciPy](https://www.scipy.org/)

#### HowTo
the `linprog` function accepts the following parameters: 

| | |
|-|-|
| | |
| | |

#### Example 

```Python
from scipy.optimize import linprog
```
`linprog` is a linear programming function in the `optimize` package of `SciPy` library. 

```python
obj = [-4, -5]
```
Defining the objective function's coefficients, and in this case, there are two independent variables (x1, x2) in this objective function. 



### [PuLP](https://coin-or.github.io/pulp/)

#### HowTo


#### Example 
```python  
from pulp import LpMaximize, LpProblem, LpStatus, lpSum, LpVariable
```
import necessary functions from pulp library, where `LpMaximize` is the maximizing objective model for lp problem.

The first step is to initialize an instance of LpProblem to represent model

```python 
# Create the model
model = LpProblem(name="dog_cat_food_profit", sense=LpMaximize)
```
X1 = bags of Dog food to produce    
X2 = bags of Cat food to produce

```python
# Initialize the decision variables
x = LpVariable(name="X1", lowBound=0) 
y = LpVariable(name="X2", lowBound=0)
```
Add the objective function to the model  MAX: 4 X1 + 5 X2
```python
obj_func = 4 * x + 5 * y
model += obj_func
```
Add the constraints to the model

4 X1 + 5 X2 ≤100 (meat)

6 X1 + 3 X2 ≤120 (corns)

4 X1 + 10 X2 ≤ 160 (filler)

X1 ≤ 40 (Dog food demand) and 
X2 ≤ 30 (Cat food demand)
```python
model += (4*x + 5*y <= 101, "meat_constraint") 
model += (4*x + 5*y <= 101, "meat_constraint") 
model += (4*x + 10*y <= 160, "filler_constraint")
model += (x <= 40, "dog_food_constraint")
model += (y <= 30, "cat_food_constraint")

model
```