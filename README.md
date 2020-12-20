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
If encounter error: `Solver not found` or `NoneType Error`
Install pulp solver 
- Mac: 
    ```bash
    brew install glpk
    ```
- Conda environment: 
    ```bash
    conda install -c conda-forge glpk
    ```

<br/>

## Code Walkthrough

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
import necessary functions from pulp library, where `LpMaximize` is the maximizing objective model for lp problem, and 

```python 

```