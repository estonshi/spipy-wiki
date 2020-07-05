---
title: merge
category: Documents
order: 3
---

> merge.tools

- **get_slice** (model, rotations, det_size, det_center=None, mask=None, slice_coor_ori=None, euler_order="zxz")
    - `model` : diffraction volume, numpy array, shape=(Nx,Ny,Nz)
    - `rotations` : list, mixed with quaterions or euler angles, for example [[w, qx, qy, qz], [alpha, beta, gamma], ...], length = Nq
    - `det_size` : detector size, [size_x, size_y], in pixels
    - `det_center` : detector center, [Cx, Cy] or None, in pixels
    - `mask` : 0/1 two-value numpy array, same shape with data, 1 means masked pixel
    - `slice_coor_ori` : pre-mapped coordinates in k-space of detector pixels without rotation, shape=(3,N), N=det_size[0]*det_size[1], optional
    - `euler_order` : rotation order if you are using euler angles, str, 'zxz' or 'xyz', etc

    [__return__] generate slices from a 3D model according to given orientations, return slices, numpy array, shape=(Nq,size_x,size_y)

- **merge_slice** (model, rotations, slices, weights=None, det_center=None, mask=None, slice_coor_ori=None, euler_order="zxz")
    - `model` : diffraction volume to be merged, numpy array, shape=(Nx,Ny,Nz)
    - `rotations` : list, mixed with quaterions or euler angles, for example [[w, qx, qy, qz], [alpha, beta, gamma], ...], length = Nq
    - `slices` : numpy array, some patterns, shape=(size_x,size_y)
    - `weights` : initial interpolation weights of all voxels in the model, same shape with model, optional
    - `det_center` : detector center, [Cx, Cy] or None, in pixels
    - `mask` : 0/1 two-value numpy array, same shape with data, 1 means masked pixel
    - `slice_coor_ori` : pre-mapped coordinates in k-space of detector pixels without rotation, shape=(3,N), N=det_size[0]*det_size[1], optional
    - `euler_order` : rotation order if you are using euler angles, str, 'zxz' or 'xyz', etc

    [__return__] merge sereval slices into a 3D model according to given orientations, input 'model' is modified directly, no return

- **poisson_likelihood** (W_j, K_k, beta=1, weight=None)
    - `W_j` : model slice in orientation j, 2d pattern or flattened array, do masking in advance (set masked area to 0 or just give flattened array without masked points)
    - `K_k` : kth experiment pattern, 2d pattern or flattened array, do masking in advance
    - `beta` : float, suggested values are from 1 to 50
    - `weight` : float, the weight of orientation j, given when orientations are not uniformly sampled

    [__return__] poisson likelihood between a model slice and an experiment pattern, return R_jk = weight * ( Product{ W_j<sup>K_k</sup>exp(-W_j) } )<sup>beta</sup>, a float

- **maximization** (K_ks, Prob_ks)
    - `K_ks` : ALL useful experiment patterns, numpy array, shape=(N,Np), where N is the number of patterns and Np is the number of useful pixels per pattern. Reshape pattern to array and do masking in advance !
    - `Prob_ks` : probabilities of ALL useful patterns (after normalizing in every orientation) in orientation j, shape=(N,)

    [__return__] updated slice of orientation j after likelihood maximization, numpy array, shape=(Np,)

- **get_quaternion** (Num_level)
    - `Num_level` : int, controls the number of output quaternions ( 2*Num_level<sup>3</sup> )

    [__return__] quaternions which are uniformly distributed in orientation space, return quaternions array, shape=(2*Num_level<sup>3</sup>,4)

> merge.emc

```ini
# EMC basic parameters
- 'parameters|detd'         : distance between sample and detector [unit : mm]

- 'parameters|lambda'       : wave length of laser [unit : angstrom]

- 'parameters|detsize'      : detector size (Sx,Sy) [unit : pixel]

- 'parameters|pixsize'      : pixel size of detector [unit : mm]

- 'parameters|stoprad'      : radius of a circle region at the center of pattern that will not be used in orientation recovery, but will be merged to final scattering volume [unit : pixel]. This option only works when 'make_detector|in_mask_file'=None

- 'parameters|polarization' : correction due to incident beam polarization, value from 'x', 'y' or 'none'

- 'emc|num_div'             : level that used to generate quaternions, also known as n, where Mrot=10(n+5n^3)

- 'emc|need_scaling'        : whether need to scale patterns' intensities

- 'emc|beta'                : beta value of emc method

- 'emc|beta_schedule'       : for example, if 'emc|beta_schedule' = '1.414 10', that means for every 10 iterations, beta = beta * 1.414

# EMC advanced parameters
- 'parameters|ewald_rad'    : Radius of curvature of the Ewald sphere in voxels, used to control oversampling rate in reciprocal space. For default, oversampling rate is controlled to be the same with experimental patterns. Larger ewald_rad means less oversampling and smaller reciprocal scattering matrix.

- 'make_detector|in_mask_file' : path of mask file (a numpy/binary file, .npy, .byt, .bin) that operate on input patterns. Usually, most experiments need mask file. In your mask file, 0 marks unmasked area, 1 marks areas that will not be used for orientation recovery but will used in merging, 2 marks areas that will not be used for both orientation recovery and merging. For binary file the dtype should be 'uint8', for numpy file it can be any type. Default is None.

- 'make_detector|center'    : the center of detector, for example '128 128'. As default, the geometry center is used.

- 'emc|sym_icosahedral'     : bool (0/1), whether to force icosahedral symmetry in orientation recovery.

- 'emc|selection'           : 'even_only', 'odd_only' and 'None', where 'even' / 'odd' means only patterns whose index is even / odd will be used. 'None' means all patterns will be used.

- 'emc|start_model_file'    : path of file that store initial model which will be used at the start of emc program. The format should be '.bin' binary file for C 'fopen' to read.
```

- **new_project** (data_path, inh5, path=None, name=None)
    - `data_path` : string, path of your dataset file, MUST be HDF5 file, and the data type must be 'int32' or 'int64'
    - `inh5` : string, where pattern locates inside h5 file, patterns should be stored in a numpy.ndarray, shape=(Nd,Nx,Ny)
    - `path` : string, create work directory under this path, set None to use current dir
    - `name` : string, give a name to your project, set None to let program choose one for you

    [__return__] create a new project, no return

- **config** (params)
    - `params` : dict, parameters to configure

    [__return__] doing configuration, no return

- **run** (num_proc, num_thread, iters, nohup=False, resume=False, cluster=True)
    - `num_proc` : int, number of processes
    - `num_thread` : int, how many threads in each process
    - `iters` : int, how many reconstruction iterations
    - `nohup` : bool, whether run in the background
    - `resume` : bool, whether run from previous break point
    - `cluster` : bool, whether you will submit jobs using job scheduling system, if True, the function will only generate a command file at your work path without submitting it, and ignore nohup value; if False, the program will run directly

    [__return__] start emc, return command string if 'cluster' is True

    [__notice__] As this program costs a lot of memories, use as less processes and much threads as possible. Recommended strategy : num_proc * num_thread ~ number of cores in all of your CPUs. Let one cluster node support 1~2 processes.

- **use_project** (project_path)
    - `project_path` : string, the path of project directory that you want to switch to

    [__return__] switch to a existing project, no return