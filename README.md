# Wind farm layout optimization problem (WFLOP) Python toolbox
## Reference
[1] Xinglong Ju, and Feng Liu. "Wind farm layout optimization using self-informed genetic algorithm with information guided exploitation." *Applied Energy*.<br/>
[2] Xinglong Ju, Victoria C. P. Chen, Jay M. Rosenberger, and Feng Liu. "Knot Optimization for Multivariate Adaptive Regression Splines." In *IISE Annual Conference*. Proceedings, Institute of Industrial and Systems Engineers (IISE), 2019.

<p align="center"> 
    <img width="400" src="/IMAGES/wfi.png" alt="Wind farm land cells illustration"/><br/>
    Wind farm land cells illustration.
</p>

## WFLOP Python toolbox guide
### Import necessary libraries
```python
import numpy as np
import pandas as pd
import MARS  # MARS (Multivariate Adaptive Regression Splines) regression class
import WindFarmGeneticToolbox  # wind farm layout optimization using genetic algorithms classes
from datetime import datetime
import os
```

### Wind farm settings and algorithm settings
```python
# parameters for the genetic algorithm
elite_rate = 0.2
cross_rate = 0.6
random_rate = 0.5
mutate_rate = 0.1

# wind farm size, cells
rows = 21
cols = 21
cell_width = 77.0 * 2 # unit : m

#
N = 60  # number of wind turbines
pop_size = 100  # population size, number of inidividuals in a population
iteration = 1  # number of genetic algorithm iterations
```
