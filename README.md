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

### Create a WindFarmGenetic object
```python
# all data will be save in data folder
data_folder = "data"
if not os.path.exists(data_folder):
    os.makedirs(data_folder)

# create an object of WindFarmGenetic
wfg = WindFarmGeneticToolbox.WindFarmGenetic(rows=rows, cols=cols, N=N, pop_size=pop_size,
                                             iteration=iteration, cell_width=cell_width, elite_rate=elite_rate,
                                             cross_rate=cross_rate, random_rate=random_rate, mutate_rate=mutate_rate)
# set wind distribution
# wind distribution is discrete (number of wind speeds) by (number of wind directions)
# wfg.init_4_direction_1_speed_12()
wfg.init_1_direction_1_N_speed_12()
```

### Generate initial populations
```python
init_pops_data_folder = "data/init_pops"
if not os.path.exists(init_pops_data_folder):
    os.makedirs(init_pops_data_folder)
# n_init_pops : number of initial populations
n_init_pops = 60
for i in range(n_init_pops):
    wfg.gen_init_pop()
    wfg.save_init_pop("{}/init_{}.dat".format(init_pops_data_folder,i))
```

### Create results folder
```python
# results folder
# adaptive_best_layouts_N60_9_20190422213718.dat : best layout for AGA of run index 9
# result_CGA_20190422213715.dat : run time and best eta for CGA method
results_data_folder = "data/results"
if not os.path.exists(results_data_folder):
    os.makedirs(results_data_folder)

n_run_times = 1  # number of run times
# result_arr stores the best conversion efficiency of each run
result_arr = np.zeros((n_run_times, 2), dtype=np.float32)
```

### Run conventional genetic algorithm (CGA)
```python
# CGA method
CGA_results_data_folder = "{}/CGA".format(results_data_folder)
if not os.path.exists(CGA_results_data_folder):
    os.makedirs(CGA_results_data_folder)
for i in range(0, n_run_times):  # run times
    print("run times {} ...".format(i))
    wfg.load_init_pop("{}/init_{}.dat".format(init_pops_data_folder, i))
    run_time, eta = wfg.conventional_genetic_alg(ind_time=i, result_folder=CGA_results_data_folder)
    result_arr[i, 0] = run_time
    result_arr[i, 1] = eta
time_stamp = datetime.now().strftime("%Y%m%d%H%M%S")
filename = "{}/result_CGA_{}.dat".format(CGA_results_data_folder, time_stamp)
np.savetxt(filename, result_arr, fmt='%f', delimiter="  ")
```
