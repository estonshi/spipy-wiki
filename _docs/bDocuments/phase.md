---
title: phase
category: Documents
order: 4
---

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

---
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
