# Business_Analysis 

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FDMinghao%2FBusiness_Analysis_Linear_Programing&count_bg=%23F81C1C&title_bg=%231E2330&icon=skyliner.svg&icon_color=%23F81C1C&title=Repo+View+Count&edge_flat=false)](https://hits.seeyoufarm.com)

In this project we use two python packages ([SciPy](https://www.scipy.org/), [PuLP](https://coin-or.github.io/pulp/)) to solve Linear Programing problems. 
</br>

**Table of Contents**
- [Getting Started](#getting-started)
  - [Install](#install)
  - [Troubleshoot](#troubleshoot)
- [Implementation](#implementation)
  - [SciPy](#scipy)
      - [HowTo](#howto)
      - [Example](#example)
  - [PuLP](#pulp)
      - [HowTo](#howto-1)
      - [Example](#example-1)
- [About](#about)
  - [Contributing](#contributing)
  - [References](#references)
  - [License](#license)

This repo consist of the original [ICE2 MSword document](ICE%202%20documents/BA%20ICE%202%20.docx) and the provided correct answer [MSexcel document](ICE%202%20documents/BA%20ICE%202%20Solutions(1).xlsx). For some problems in the ICE2, both implementation methods are used, and some others are only implemented with one. Please check out the [Contributing](CONTRIBUTING.md) document for contributing to this repo (implementing more methods, etc.). 
<!-- <small>There are some errors in the excel document causing inconsistent results</small> -->

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

> SciPy (pronounced “Sigh Pie”) is open-source software for mathematics, science, and engineering.

#### HowTo

Use `linprog` to minimize a linear objective function subject to linear
equality and inequality constraints. For maximization, invert objective function's coefficients. 
```python 
res = scipy.optimize.linprog(c, A_ub=None, b_ub=None, A_eq=None, b_eq=None, bounds=None, method='revised simplex', callback=None, options=None, x0=None)
```

> **_NOTE:_**  Below is a lite version of `linprog` function's [official documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html#scipy.optimize.linprog)

##### Parameters

| Param Name | Data Type | Required | Explanation |
| :--: | :--: | :--: | :-- |
| **c** | 1-D array | ✔ |  The coefficients of the linear objective function to be minimized. |
| **A_ub** | 2-D array | ❌ | The inequality constraint matrix. Each row of ``A_ub`` specifies the coefficients of a linear inequality constraint on ``x``. |
| **b_ub** | 1-D array | ❌ | The inequality constraint vector. Each element represents an upper bound on the corresponding value of ``A_ub @ x``|
| **A_eq** | 2-D array | ❌ | The equality constraint matrix. Each row of ``A_eq`` specifies the coefficients of a linear equality constraint on ``x``. |
| **b_eq** | 1-D array | ❌ | The equality constraint vector. Each element of ``A_eq @ x`` must equal the corresponding element of ``b_eq``. |
| **bounds** | Tuple array | ❌ | <ul><li> A sequence of ``(min, max)`` pairs for each element in ``x``, defining the minimum and maximum values of that decision variable. Use ``None`` to indicate that there is no bound. By default, bounds are ``(0, None)`` (all decision variables are non-negative). </li><li> If a single tuple ``(min, max)`` is provided, then ``min`` and ``max`` will serve as bounds for all decision variables. </li></ul>|
| **method** | Enumerated | ❌ | {`interior-point`, `revised simplex`, `simplex`}
| **callback** | Callable | ❌ | If a callback function is provided, it will be called at least once per iteration of the algorithm. The callback function must accept a single [`scipy.optimize.OptimizeResult`](#optimizeresult) object. |
| **options** | dict | ❌ | <ul><li> **maxiter** : *int* -> Maximum number of iterations to perform. </li><li> **disp** : *bool* -> Set to ``True`` to print convergence messages. </li><li> **autoscale** : *bool* -> Set to ``True`` to automatically perform equilibration. Consider using this option if the numerical values in theconstraints are separated by several orders of magnitude. </li><li> **presolve** : *bool* -> Set to ``False`` to disable automatic presolve. </li><li> **rr** : *bool* -> Set to ``False`` to disable automatic redundancy removal. </li> </ul> | 
| **x0** | 1-D array | ❌ |<ul><li> Guess values of the decision variables, which will be refined by the optimization algorithm. </li><li> This argument is currently used only by the `revised simplex` method, and can only be used if `x0` represents a basic feasible solution. </li></ul>|

##### Returns

The `linprog` function returns a <a id="optimizeresult">`scipy.optimize.OptimizeResult`</a> class, consisting of the fields:

<!-- <center> -->
| Atribute | Data Type | Explianation |
|:-:|:-:|:-|
| **x** | 1-D array| The current solution vector. |
| **fun** | float | The current value of the objective function ``c @ x``. |
| **success** | bool | ``True`` when the algorithm has completed successfully. |
| **slack** | 1-D array | The (nominally positive) values of the slack, ``b_ub - A_ub @ x``. |
| **con** | 1-D array | The (nominally zero) residuals of the equality constraints, ``b_eq - A_eq @ x``. |
| **phase** | int | The phase of the algorithm being executed. |
| **status** | int | <ul><li> ``0`` -> Optimization proceeding nominally. </li><li> ``1`` -> Iteration limit reached. </li><li>``2`` -> Problem appears to be infeasible. </li><li>``3`` -> Problem appears to be unbounded. </li><li>``4`` -> Numerical difficulties encountered. </li></ul>|
| **nit** | int | The current iteration number. |
| **message** | str | A string descriptor of the algorithm status. |
<!-- </center> -->

</br>

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

> PuLP is an LP modeler written in Python. PuLP can generate MPS or LP files and use external libraries (GLPK, CPLEX, etc.) to solve linear problems. 
#### HowTo

Use `LpProblem` to model a maximize or minimize linear problem subject to linear
equality and inequality constraints. 

> **_NOTE:_**  Below is a lite version of `PuLP` libary's [official documentation](https://coin-or.github.io/pulp/index.html)

##### Constants
- `LpSenses`
    |Key|Str Value| Int Value |
    |:-|:-:|:-:|
    |`LpMaximize`|“Maximize”|-1|
    |`LpMinimize`|“Minimize”|1|
- `LpStatus`
    |Key|Str Value| Int Value |
    |:-|:-:|:-:|
    |`LpStatusOptimal`|“Optimal”|1|
    |`LpStatusNotSolved`|“Not Solved”|0|
    |`LpStatusInfeasible`|“Infeasible”|-1|
    |`LpStatusUnbounded`|“Unbounded”|-2|
    |`LpStatusUndefined`|“Undefined”|-3|
- `LpConstraintSenses`
    |Key|Str Value| Int Value |
    |:-|:-:|:-:|
    |`LpConstraintEQ`|“==”|0|
    |`LpConstraintLE`|“<=”|-1|
    |`LpConstraintGE`|“>=”|1|

##### Classes  

- `LpProblem` 
  - Parameters 
    - **name** -> name of the problem used in the output .lp file 
    - **sense** -> of the LP problem objective. Either `LpMinimize` (default) or `LpMaximize`.
  - Returns -> An LP Problem model/object
- `LpVariable`
  - Parameters 
    - **name** -> The name of the variable used in the output .lp file
    - **lowBound** -> The lower bound on this variable’s range. Default is negative infinity
    - **upBound** -> The upper bound on this variable’s range. Default is positive infinity
    - **cat** -> The category this variable is in, `LpInteger`, `LpBinary` or `LpContinuous`(default)
    - **e** -> Used for column based modelling: relates to the variable’s existence in the objective function and constraints
  - Returns -> An LP Variable object
- `LpAffineExpression` 
  - A linear combination of `LpVariable`
  - Parameters
    - **e** 
      - `None` -> i.e. None => `constant`
      - `(LpVariable,coefficient) list` -> e.g. "[(x1,10),(x2,-15)]" => 10\*x1 + -15\*x2 + `constant`
    - **constant** -> 0 (default) 
    - **name** -> The name of expression used in the output .lp file
  - Returns -> An LP Expression object
- `LpConstraint`
  - Parameters 
    - **e** -> an instance of `LpAffineExpression`
    - **sense** -> `LpConstraintEQ`, `LpConstraintGE`, or `LpConstraintLE` (0, 1, or -1)
    - **name** -> The name of constraint used in the output .lp file
    - **rhs** -> numerical value of constraint target
  - Returns -> An Lp Constraint object
- `lpSum`
  - Takes in a list of `LpAffineExpression` or `LpVariable` and combined everything to one `LpAffineExpression`

##### Procedure

1. Initiate a `LpProblem` with one of the `LpSenses` const
2. Define `LpVariable`(s) 
3. Initiate a `LpAffineExpression` with `LpVariable`(s) and corresponding coefficients as the objective function 
4. `LpProblem` += objective function
5. Define `LpConstraint`(s) with `LpAffineExpression` and `LpConstraintSenses` as problem constraints 
6. foreach problem constraints -> `LpProblem` += constraint
7. print(`LpProblem`)

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
```
Below is the result output
```python
dog_cat_food_profit:
MAXIMIZE
4*X1 + 5*X2 + 0
SUBJECT TO
meat_constraint: 4 X1 + 5 X2 <= 101

corns_constraint: 6 X1 + 3 X2 <= 120

filler_constraint: 4 X1 + 10 X2 <= 160

dog_food_constraint: X1 <= 40

cat_food_constraint: X2 <= 30

VARIABLES
X1 Continuous
X2 Continuous
```

## About 
This project was initiated by [@DMinghao](https://github.com/DMinghao) [@HLiu-970725](https://github.com/HLiu-970725) [@LYLwithDCT](https://github.com/LYLwithDCT) [@ReynaYC](https://github.com/ReynaYC) 

### Contributing
Please check out the [CONTRIBUTING](CONTRIBUTING.md)  document

### License
By contributing, you agree that your contributions will be licensed under its [MIT License](LICENSE).

### References 
- https://www.scipy.org/ 
- https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html#scipy.optimize.linprog 
- https://github.com/coin-or/pulp 
- https://coin-or.github.io/pulp/
- http://www.gnu.org/software/glpk/glpk.html 
