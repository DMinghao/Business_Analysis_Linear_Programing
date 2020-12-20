# Business_Analysis

This repo is for the BA final project. In this project we use two python packeges to solve Linear Programing problems in ICE2 of the class. 
## Getting Started

### Install 
Require Python 3 or higher

- Install SciPy
    ``` bash
    pip install SciPy
    ```

- Install Pulp 
    ```python
    pip install pulp
    ```

### Iroubleshoot 
If encounter error: 'Solver not found' or 'NoneType Error'
Istall pulp solver 
- Mac: 
    ```bash
    brew install glpk
    ```
- Conda environment: 
    ```bash
    conda install -c conda-forge glpk
    ```

## Code Walkthrough

### SciPy 

```Python
from scipy.optimize import linprog
```
linprog is a linear programming function in the optimize package of SciPy library. 


### Pulp 