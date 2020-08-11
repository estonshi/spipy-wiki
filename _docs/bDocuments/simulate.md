---
title: simulate
category: Documents
order: 5
---

> simulate.sim_adu

```ini
# simulation parameters
- 'detd' : float, distance between sample and detector [unit : mm], default=200.0

- 'lambda' : float, wave length of laser [unit : angstrom], default=2.5

- 'detsize' : int list, detector size in width and height [unit : pixel], default=[128,128]

- 'detcenter' : float list, detector center location [Cx,Cy] [unit : pixel], default=None and uses geometry center

- 'pixsize' : float, pixel length [unit : mm], default=0.3

- 'stoprad' : int, radius of a circle region at the center of pattern that to be masked out [unit : pixel], default=0

- 'polarization' : correction due to incident beam polarization, value from 'x', 'y' or 'none', default=None

- 'num_data' : number of patterns to generate, default=100

- 'fluence' : laser fluence [unit : photons/pulse], usually 1.0e14 ~ 1.0e16 is reasonable for most situations, default=1.5e11

- 'photons' : bool, generate photon patterns (True) or adu patterns without poisson noise (False), default=False

- 'absorption' : bool, whether consider photon absorption, default=True

- 'adu_per_eV' : float, detector ADU of 1eV energy, default=0.001

- 'phy.scatter_factor' : bool, whether to consider scattering factors in atomic diffraction, default=True

- 'phy.b_factor' : float, B-factor value to describe atom displacement in atomic diffraction [unit : angstrom^2], default=20.0, displacement = sqrt( B_factor / 79.0 )

- 'phy.ram_first' : bool, whether to save memory while running atomic diffraction simulation, default=True

- 'phy.projection' : bool, whether to genrate projections of the sample at each orientation in atomic diffraction, default=True
```

- **go_magic** (method, save_dir, pdb_file, param, euler_mode='random', euler_order='zxz', euler_range=None, predefined=None, verbose=True)
    - `method` : 'atomic' or 'fft', which means 'atomic diffraction' or 'fourier transform' method
    - `save_dir` : the folder path where to save simulation result
    - `pdb_file` : path of input pdb file
    - `param` : dict, simulation parameters (see above)
    - `euler_mode` : how to genreate euler angles, chosen from 'random' or 'predefined'
    - `euler_order` : rotation order, such as 'zxz' or 'zyz' ...
    - `euler_range` : range of euler angles, numpy.array([[alpha_min,alpha_max],[beta_min,beta_max],[gamma_min,gamma_max]]), shape=(3,2), or give None to use native range
    - `predefined` : if euler_mode is 'predefined', then you have to specify all euler angles used for object rotation, shape=(Ne,3)
    - `verbose` : bool, whether to print detail information while running

    [__return__] patterns and other useful information are saved in a h5 file named by OS timestamp, no return

    [__NOTICE__] support mpi4py, you need to run 'mpiexec -np $nproc python sim_script.py' to start parallel