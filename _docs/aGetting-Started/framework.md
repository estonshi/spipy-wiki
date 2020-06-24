---
title: Framework
category: Getting Started
order: 2
---

The architecture of spipy package is shown as below:

* [**analyse**]() : *diffraction experiment data analysis*
    * q
        * `cal_q` : calculate frequency q values
        * `cal_q_pat` : calculate frequency q values of a pattern
        * `cal_r` : calculate radius list on detector corresponding to a q-list
        * `oversamp_rate` : calculate oversampling rate
        * `ewald_mapping` : maps pixels in diffraction pattern to 3D k-space
    * saxs
        * `grid` : calculate grid of a pattern
        * `friedel_search` : return the center point (zero frequency) of a pattern
        * `inten_profile_vaccurate` : the averaged intensity radial profile
        * `inten_profile_vfast` : fast calculation of intensity profile
        * `cal_saxs` : the saxs pattern from a pattern set
        * `centering` : shift the input pattern to its center
        * `particle_size` : estimate particle size through saxs pattern
        * `particle_size_sp` : fit the diameter of spherical-like samples by using Iq curve of diffraction patterns
    * orientation
        * `Sphere_randp` : return random/uniform distributed points on a spherical surface
        * `proc_Hammer` : transfer quaternions to Aitoff-Hammer projection coordinates
        * `draw_Hammer` : plot Aitoff-Hammer projection coordinates
        * `draw_ori_Df` : draw figure to show orientation distribution of DragonFly output
    * criterion
        * `r_factor` : overall r-factor between two models
        * `r_factor_shell` : r-factor of shells with different radius between two models
        * `fsc` : Fourier Shell Correlation between two models (in reciprocal space)
        * `r_split` : r-split factors between two models (in reciprocal space)
        * `Pearson_cc` : Pearson correlation coefficient between two arrays
        * `PRTF` : Phase Retrieval Transfer Function of phased dataset
    * rotate
        * `eul2rotm` : transfer euler angles to rotation matrix in intrinsic order
        * `rot_ext` : do an extrinsic rotation to a 3D voxel model
        * `align` : Using grid search (euler angles) to align two models
    * SH_expan
        * `sp_harmonics` : spherical harmonics expansion of a 3D model
* [**image**]() : *diffraction pattern process and io*
    * radp
        * `radial_profile` : calculate intensity radial profile of a pattern/volume
        * `shells` : return indices of pixels in a pattern/volume which forms a shell at radius r
        * `radp_norm` : scaling pattern/volume intensities (at different radius) referring to a given radial profile
        * `circle` : generates a circle/sphere area with given radius, centered by (0,0)
    * quat
        * `invq` : calculate the inverse of a quaternion
        * `quat_mul` : multiply two quaternions
        * `conj` : conjugate quaternion
        * `quat2rot` : transfer quaternion to 3D rotation matrix
        * `rot2quat` : transfer 3D rotation matrix to quaternion
        * `quat2azi` : transfer quaternion to azimuth angle
        * `azi2quat` : transfer azimuth angle to quaternion
        * `rotv` : rotate a 3D vector using a quaternion
        * `Slerp` : linear interpolation on spherical surface between two quaternions
    * classify
        * `cluster_fSpec` ： single-nonsingle hits clustering using linear/non-linear decomposition and spectural clustering
        * `cluster_fTSNE` : single-nonsingle hits clustering using t-SNE decomposition and KNN clustering
        * `diffusion_map` : diffusion map embedding and decomposition
    * preprocess
        * `fix_artifact` : reduces artifacts of dataset, where patterns in the dataset should share the same artifacts
        * `fix_artifact_auto` : fix artifacts without providing the position of artifacts
        * `adu2photon` : evaluate adu value per photon and transfer adu patterns to photon counts
        * `hit_find` : hit finding based on chi-square test
        * `hit_find_pearson` : hit finding based on pearson cc score between pattern and background
        * `cal_correction_factor` : calculate polarization and effective solid angle correction factor
    * io
        * `readccp4` : read ccp4/mrc files
        * `writeccp4` : write intensity volume into a ccp4/mrc file
        * `pdb2density` : read pdb file and return electron density map
        * `cxi_parser` : print cxi/h5 inner path structures
        * `xyz2pdb` : write xyz-coordinates as a pdb file
        * `readpdb_full` : read pdb files and return full infomation
* [**merge**]() : *orientation recovery and merging*
    * emc
        * `new_project` : create a new project
        * `config` : configuration
        * `run` : run emc project
        * `use_project` : switch to an existing project
    * tools
        * `get_slice` : generate slices from a 3D model according to given orientations
        * `merge_slice` : merge slices into a 3D model according to given orientations
        * `poisson_likelihood` : calculate poisson likelihood between a model slice and an experiment pattern
        * `maximization` : calculate updated slice of orientation j after likelihood maximization
        * `get_quaternion` : calculate quaternions which are uniformly distributed in orientation space
* [**phase**]() : *phase retrieval network framework (PRNF) or traditional phasing*
    * phmodel
        * `pInput`,`pOutput`,`pMerge`,`ERA`,`HIO`,`DM`,`RAAR`,`HPR` : different types of phasing models to build phase retrieval network (PRNF)
    * phexec
        * `Runner` : class, used for execuating phase retrieval network
    * phase2d *(deprecated)*
        * `new_project` : create a new project
        * `config_project` : edit configuration file
        * `run_project` : start phasing
        * `use_project` : switch to a existing project
    * phase3d *(deprecated)*
        * the same as phase2d
* [**simulate**]() : *diffraction simulation*
    * sim_adu
        * `go_magic` : simulate diffraction patterns from PDB files using atom reflection or FFT method

