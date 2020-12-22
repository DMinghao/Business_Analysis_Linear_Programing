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
`linprog` is a linear programming function in the `optimize` package of `SciPy` library. We will use the question 1(c) as an tutorial example. The example objective function is min[-4x-5y]


```python
obj = [-4, -5]
```
Defining the objective function's coefficients, and in this case, there are two independent variables (x, y) in this objective function. 
```python
lhs_ineq = [[4,  5], 
            [6,  3], 
            [4, 10], 
            [1, 0], 
            [0, 1]] 
```
Defining the left hand side of the constraints
```python
rhs_ineq = [101,            
            120,
            160, 
            40, 
            30] 
```
Defining the right hand side of the constraints. The scipy set the inequality as <= by default. If you want to plug in an >= inequality relationship, simply change the signs to negative.
```python
bnd = [(0, float("inf")),
       (0, float("inf"))]
```
Defining the boundary of the two decision veriabales. If you have multiple desicion veriables, simply add more boundries in the bnd=[ ]
```python
opt = linprog(c=obj3, A_ub=lhs_ineq, b_ub=rhs_ineq,
                bounds=bnd,
               method="simplex")
```
Defining the opt in order to give the scipy a sense of what we problem are solving at all. Print "opt" to see the optimal solution for the question.
```python
     con: array([], dtype=float64)
     fun: -101.0
 message: 'Optimization terminated successfully.'
     nit: 5
   slack: array([ 0. , 21.6,  0. , 29.5, 18.2])
  status: 0
 success: True
       x: array([10.5, 11.8])
```
Check the results. If the success is True, then the scipy find an optimal solution for you. If that is false, there are multiple reasons. The first, you might enter the wrong constraints. Second, there might no optimal solution in the fesiable region of the constraints you set.

### [PuLP](https://coin-or.github.io/pulp/)

#### HowTo


#### Example 
```python  
from pulp import LpMaximize, LpProblem, LpStatus, lpSum, LpVariable
```
import necessary functions from pulp library, where `LpMaximize` is the maximizing objective model for lp problem, and 

```python 

```