---
title: phase
category: Documents
order: 4
---

**Functional based programming mode**

* _Phase retrieval algorithms_ :
    * _ER (error reduction)_
    * _DM (difference map)_
    * _RAAR (relaxed averaged alternating reflections)_

> phase.phase2d

-> _2d pattern phase retrieval_

```ini
# 2d phase retrieval basic parameters
- 'input|shape' : input pattern shape, default='123,123'

- 'input|padd_to_pow2' : zero padding to make the size of pattern to be a 2^n number, default=True

- 'input|inner_mask' : pixels whose radius<inner_mask are allowed to float while phasing, default=5

- 'input|outer_mask' : pixels whose radius>outer_mask are set to zero, default=64

- 'input|outer_outer_mask' : pixels whose radius are between outer_mask and outer_outer_mask are allowed to float, default=None

- 'input|mask_edges' : bool, whether allow pixels between outer_mask and outer_outer_mask to float, default=True

- 'phasing|repeats' : how many times of independent phasing for **every process**, default=40

- 'phasing|iters' : schedual iterations for 1 phasing loop, default is '100RAAR 200DM 200ERA' which means '100 times RAAR algorithm -> 200 times DM algorithm -> 200 times ER algorithm'

- 'phasing_parameters|support_size' : set restriction to the number of pixels inside final support of shrinkwrap process, default=200

- 'phasing_parameters|beta' : beta value for RAAR algorithm, float from 0 to 1, default=0.8

# 2d phase retrieval advanced parameters
- 'input|subtract_percentile' : subtract the X percentile value of data for all pixels, float value from 0 to 100, default=None

- 'input|spherical_support' : radius of spherical support that added at the initiation, default=None

- 'input|init_model' : filepath of initial REAL space model. Support .npy, .bin and .mat file formats. The size of init model should agree with 'input|shape' after 'input|padd_2_pow', default=None

- 'phasing_parameters|background' : evaluate background while phasing, default=True
```

> phase.phase3d

-> _3d volume phase retrieval_

```ini
# 3d phase retrieval basic parameters
- 'input|shape' : input pattern shape, default='120,120,120'

- 'input|padd_to_pow2' : zero padding to make the size of pattern to be a 2^n number, default=True

- 'input|inner_mask' : pixels whose radius<inner_mask are allowed to float while phasing, default=3

- 'input|outer_mask' : pixels whose radius>outer_mask are set to zero, default=64

- 'input|outer_outer_mask' : pixels whose radius are between outer_mask and outer_outer_mask are allowed to float, default=None

- 'input|mask_edges' : bool, whether allow pixels between outer_mask and outer_outer_mask to float, default=False

- 'phasing|repeats' : how many times of independent phasing for **every process**, default=40

- 'phasing|iters' : schedual iterations for 1 phasing loop, default is '100RAAR 200DM 200ERA' which means '100 times RAAR algorithm -> 200 times DM algorithm -> 200 times ER algorithm'

- 'phasing_parameters|voxel_number' : set restriction to the number of pixels inside final support of shrinkwrap process, default=2000

- 'phasing_parameters|beta' : beta value for RAAR algorithm, float from 0 to 1, default=0.8

# 3d phase retrieval advanced parameters
- 'input|subtract_percentile' : subtract the X percentile value of data for all pixels, float value from 0 to 100, default=None

- 'input|spherical_support' : radius of spherical support that added at the initiation, default=None

- 'input|init_model' : filepath of initial REAL space model. Support .npy, .bin and .mat file formats. The size of init model should agree with 'input|shape' after 'input|padd_2_pow', default=None

- 'phasing_parameters|background' : evaluate background while phasing, default=True
```

**phase.phase3d and phase.phase2d have same *APIs*** : 

- **new_project** (data_path, mask_path=None, path=None, name=None)
    - `data_path` : path of your original diffraction data, support '.npy' or '.mat', or '.bin' files. For '.bin' file, dtype should be *float*
    - `mask_path` : path of user-defined mask file, a 0/1 numpy array where 1 means masked point
    - `path` : create work directory under this path, set None to use current dir
    - `name` : give a name to your project, set None to let program choose one for you

    [__return__] create project, no return

- **config_project** (params)
    - `params` : dict, parameters (see above) to configure

    [__return__] configuration, no return

- **run_project** (num_proc=1, nohup=False, cluster=False)
    - `num_proc` : int, this function maps multi-processes automatically, so you need to figure out how many processes to run in parallel
    - `nohup` : bool, whether run in background
    - `cluster` : bool, whether you will submit jobs using job scheduling system, if True, the function will only generate a command file at your work path without submitting it; if False, the program will run directly

    [__return__] start phase retrieval, no return

- **use_project** (project_path)
    - `project_path` : string, the path of project directory that you want to switch to

    [__return__] switch to a existing project, no return

---
**Modelized programmming mode**

> phase.phmodel

(**NOTE** : All of the class in phmodel has a "run" function, which will be called by **phexec.Runner** to run a node, users don't need to use it.)

- class **phInput** : input node of a phasing model
    - \_\_init\_\_ (self, config\_dict, name=None)
        - `config_dict` : parameter dict

        ```
        { 
            "pattern_path" : xxx.npy,
            
            "mask_path" : xxx.npy (1 is masked area),
            
            "center" : [62,62],
            
            "center_mask" : 5,
            
            "edge_mask" : [60,64],
            
            "subtract_percentile" : False,
            
            "fixed_support_r" : 20,
            
            "background" : True,
            
            "initial_model" : xxx.npy
        }
        ```
        
        - `name` : name of this node, default is "Input"
    - after (self, father_node)
        - `father_node` : set father node of this node
        
        [__return__] self

- class **phOutput** : output node of a phasing model
    - \_\_init\_\_ (self, name=None)
    - after (self, father_node)

- class **ERA** : ERA algorithm phasing node
    - \_\_init\_\_ (self, iteration, support_size, name=None)
        - `iteration` : how many iterations for ERA to run, int
        - `support_size` : (estimation) number of pixels within final retrieved sample, int
    - after (self, father_node)

- class **DM** : DM algorithm phasing node
    - \_\_init\_\_ (self, iteration, support_size, name=None)
    - after (self, father_node)

- class **RAAR** : RAAR algorithm phasing node
    - \_\_init\_\_ (self, iteration, support_size, beta, name=None)
        - `beta` : beta value, a float from 0~1. If beta==0, then RAAR degenerate to ERA
    - after (self, father_node)

> phase.phexec

- class **Runner** : running phasing model, support mpi4py parallel
    - \_\_init\_\_ (self, inputnode = None, outputnode = None, loadfile = None, change_dataset = None)
        - `inputnode` : a "phInput" instance, as input node of the whole model
        - `outputnode` : a "phOutput" instance, as output node of the whole model
        - `loadfile` : str, a model file path to load
        - `change_dataset` : a dict, {"pattern\_path" : "xxx.npy", "mask\_path" : "xxx.npy"/None, "initial\_model" : "xxx.npy"/None}. Only valid when 'loadfile' is given.

        [__NOTE__] You can specify either `inputnode` + `outputnode` or `loadfile` + `change_dataset` to initiate a Runner.
        
    - run (self, repeat=1)
        - `repeat` : times of independent phasing **of this mpi rank**, int
    - dump\_model (self, model\_file, skeleton = False)
        - `model_file` : str, file path to save this model
        - `skeleton` : bool, whether to save data of this model, save (False) or not save (True)

        [__return__] A file will be generated to describe the model you created. You can share this file to others to establish a same model.

```python
# Examles of using modelized programming,
# you can also find this at "spipy/test_spipy/phase/test_phmodel.py"

import numpy as np
from spipy.phase import phexec, phmodel

from mpi4py import MPI
comm = MPI.COMM_WORLD
mrank = comm.Get_rank()
msize = comm.Get_size()

if __name__ == "__main__":

    config_input = {
        "pattern_path" : "pattern.npy",
        "mask_path" : "pat_mask.npy",
        "center" : [61,61],
        "center_mask" : 5,
        "edge_mask" : None,
        "subtract_percentile" : False,
        "fixed_support_r" : None,
        "background" : True,
        "initial_model" : None
    }
    iters = [100,200,200]
    support_size = 100
    beta = 0.8
    newdataset = {"pattern_path" : "pattern.npy", "mask_path" : "pat_mask.npy", "initial_model" : None}

    
    l1 = phmodel.pInput(config_input)
    l2 = phmodel.RAAR(iters[0], support_size, beta).after(l1)
    l3 = phmodel.DM(iters[1], support_size).after(l2)
    l4 = phmodel.ERA(iters[2], support_size).after(l3)
    l5 = phmodel.pOutput().after(l4)

    runner = phexec.Runner(inputnode = l1, outputnode = l5)
    out = runner.run(repeat = 2)
    
    if mrank == 0:
        runner.plot_result(out)
        
    # Dump model and load it

    runner.dump_model("temp_model.json", skeleton=True)
    runner2 = phexec.Runner(inputnode = None, outputnode = None, \
                            loadfile = "temp_model.json", change_dataset = newdataset)
    out = runner2.run(repeat = 1)
```