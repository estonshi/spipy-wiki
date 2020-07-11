---
title: analyse
category: Documents
order: 1
---

> analyse.q

- **cal_q** (detd, lamda, det_r, pixsize)
    - `detd` : distance between detector and sample, unit=mm
    - `lamda` : wave length, unit=Angstrom
    - `det_r` : the highest resolution (radius on detector) that you want to calculate to, unit=pixel
    - `pixsize` : physical length of a detector pixel, unit=mm

    [__return__] reciprocal q array, shape=(det_r,), unit=nm<sup>-1</sup>

- **cal_q_pat** (detd, lamda, pixsize, det_size, center=None)
    - `detd` : distance between detector and sample, unit=mm
    - `lamda` : wave length, unit=Angstrom
    - `pixsize` : physical length of a detector pixel, unit=mm
    - `det_size` : detector size in pixels, (size_x, size_y)
    - `denter` : center of pattern, (Cx, Cy)

    [__return__] reciprocal q array of a pattern, shape=(Nq,), unit=nm<sup>-1</sup>

- **cal_r** (qlist, detd, lamda, pixsize)
    - `qlist` : numpy array, shape=(Nq,), unit=nm<sup>-1</sup>
    - `detd` : distance between detector and sample, unit=mm
    - `lamda` : wave length, unit=Angstrom
    - `pixsize` : physical length of a detector pixel, unit=mm

    [__return__] inverse calculation of cal_q, return r, shape=qlist.shape

- **oversamp_rate** (sample_size, detd, lamda, pixsize)
    - `sample_size` : diameter of your experiment sample, unit=nm
    - `detd` : distance between detector and sample, unit=mm
    - `lamda` : wave length, unit=Angstrom
    - `pixsize` : physical length of a detector pixel, unit=mm

    [__return__] oversampling rate, float

- **ewald_mapping** (detd, lamda, pixsize, det_size, center=None)
    - `detd` : distance between detector and sample, unit=mm
    - `lamda` : wave length, unit=Angstrom
    - `pixsize` : physical length of a detector pixel, unit=mm
    - `det_size` : detector size in pixels, (size_x, size_y)
    - `center` : center of pattern, (Cx, Cy), default=None

    [__return__] map detector pixels into 3D k-space, return (q_coor, qmax, qmin, q_len). "*q_coor*" is 3D coordinates in k-space, shape=(3,size_x,size_y); "*qmax*" is the maximum q value of detector; "*qmin*" is the minimum q value of detector; "*q_len*" is the side length of k space matrix in pixel.

> analyse.saxs

- **friedel_search** (pattern, estimated_center, mask=None, small_r=None, large_r=None)
    - `pattern` : input pattern, numpy array, shape=(Nx,Ny)
    - `estimated_center` : estimated center of the pattern, (Cx, Cy), near to real center
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel. Nan/Inf/negtive pixels should be maksed
    - `small_r` : int, radius of search area for center allocation candidates, in pixel
    - `large_r` : int, radius of area for sampling friedel twin points, in pixel

    [__return__] central point (q=0) of the pattern, (Cx, Cy)
    
- **center_refine** (pattern, center, mask=None, sampling=300, roir=20)
	- `pattern` : input pattern, numpy array, shape=(Nx,Ny)
	- `center` : center from friedel_search, [Cx,Cy]
	- `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel. Nan/Inf/negtive pixels should be maksed
	- `sampling` : Number of sampled pixels for refinement
	- `roir` : Radius of a circle area for pixel sampling, in pixel

- **inten_profile_vaccurate** (dataset, mask, *exp_param)
    - `dataset` : numpy array, shape=(Nd,Nx,Ny)
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `*exp_param` : detd (mm) , lamda (A), det_r (pixel), pixsize (mm)

    [__return__] calculate averaged intensity radial profile of diffraction dataset, return numpy array, shape=(Nq,2), first column is q value, second column is radial intensities

- **inten_profile_vfast** (dataset, mask, *exp_param)
    - same with 'inten_profile_vaccurate'

    [__return__] same with 'inten_profile_vaccurate', but assumed all patterns in the dataset have same center point

- **cal_saxs** (data)
    - `data` : patterns, numpy array, shape=(Nd,Nx,Ny)

    [__return__] averaged pattern of diffraction dataset, shape=(Nx,Ny)

- **centering** (pat, estimated_center, mask=None, small_r=None, large_r=None)
    - `pat` : input pattern, numpy array, shape=(Nx,Ny)
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `small_r` : int, radius of search area for center allocation candidates, in pixel
    - `large_r` : int, radius of area for sampling friedel twin points, in pixel

    [__return__] shift the q=0 point to geometric center and output new pattern (Note that the new pattern may be smaller than original one), return (newpat, newmask). "newpat" and "newmask" are both numpy array, shape=(Nx',Ny')

- **particle_size** (saxs, estimated_center, exparam=None, high_filter_cut=0.3, power=0.7, mask=None)
    - `saxs` : saxs pattern, numpy array, shape=(Nx,Ny)
    - `estimated_center` : estimated center of saxs pattern, (Cx, Cy), error within 20 pixels
    - `exparam` : experimetnal parameters, str, "detector-sample distance(mm),lambda(A),pixel length(mm)". For example, "578,7.9,0.3"
    - `high_filter_cut` : float between (0,1), which determine the FWHM of high pass filter, larger value means smaller filter width
    - `power` : float between (0,1), a power conducted on pattern to enhance the contribution of high q values
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel

    [__return__] estimate particel size, return (particle_size, auto_correlation_radial_profile). "particle_size" is the estimated particle diameter in nm; "auto_correlation_radial_profile" is a numpy array with shape=(Nd,2), the 1st column is particle diameter in nm (if exparam is given) and 2nd colum is the auto-correlation value. The locations of peaks in auto_correlation_radial_profile give most trustable values of possible particle sizes

- **particle_size_sp** (dataset, exparam, fitarea, badsearchr, method, mask=None, center=None, verbose=True)
    - `dataset` : a set of patterns, numpy array, shape=(Nd,Nx,Ny)
    - `exparam` : experimental parameters, a list [detector-distance(mm), lambda(A), pixel-length(mm)]
    - `fitarea` : a list [nr, nR], define an ring area (ROI) to do the fitting
    - `badsearchr` : int, if there are more than 10 peaks (within fit area) in the radial profile whose radii<=badsearchr , the pattern will be dropped, unit=pixel
    - `method` : fitting method, str, chosen from 'q0' (use the first Iq minimum position to estimate diameter) or 'lsq' (fit theoretical Iq curve to given patterns to estimate diameter)
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `center` : center location of diffraction patterns, (Cx,Cy)
    - `verbose` : bool, whether to display progress bar

    [__return__] fit the diameters of spherical-like samples using Iq curve of their diffraction patterns, return diameters in nm, shape=(Nd,)

> analyse.orientation

- **Sphere_randp** (algo, radius, num)
    - `algo` : algorithm, string, "random" or "uniform-1" (standard Fibonacci spherical mapping) or "uniform-2" (modified Fibonacci spherical mapping)
    - `radius` : radius of the sphere, unit=pixels
    - `num` : number of points you want to generate

    [__return__] random/uniform distributed points on a pherical surface, return (xyz, azi). "xyz" is Euclid coordinates of points, numpy array, shape=(Np,3); "azi" is azimuth coordinates, numpy array, shape=(Np,2)

- **proc_Hammer** (qlist, data)
    - `qlist` : quaternion list, numpy.array( [ w[..], qx[..], qy[..], qz[..] ] ); or xyz coordinates, numpy.array( [ x[..], y[..], z[..] ] ). That is, shape=(Nq, 4) or (Nq,3)
    - `data` : the corresponding value for every quaternion (orientation), this function do not care what the values in data really mean. shape=(Nq,1)

    [__return__] transfer quaternions to Aitoff-Hammer projection coordinates, return (logi_lati, x_y). "logi_lati" is numpy.array([ logitute[..], latitute[..], value[..] ]), shape=(Nq,3); "x_y" is numpy.array([ x[..], y[..], value[..] ]), shape=(Nq,3)

- **draw_hammer** (logi_lati, save_dir=None)
    - `logi_lati` : Logitute, latitute and value, shape=(Nq,3) ( The first output of proc_Hammer )
    - `save_dir` : string, the path if you want to save figure

    [__return__] None

- **draw_ori_Df** (ori_bin, q_level)
    - `ori_bin` : string, path of 'orientation_xxx.bin' file from spipy.merge.emc output
    - `q_level` : the 'num_div' parameter used in spipy.merge.emc, int

    [__return__] None

> analyse.criterion

- **r_factor** (F_cal, F_obs)
    - `F_cal` : voxel models from calculation, numpy array
    - `F_obs` : voxel models from observation, numpy array

    [__return__] overall r-factor of two models, a float between [0,1]

- **r_factor_shell** (F_cal, F_obs, rlist)
    - `F_cal` : voxel models from calculation, numpy array
    - `F_obs` : voxel models from observation, numpy array
    - `rlist` : radii list of shells, list/array, length=Nr

    [__return__] r-factor of shells with different radius between two models, return a numpy array, shape=(Nr,)

- **fsc** (F1, F2, rlist)
    - `F1` : the first voxel model, numpy array
    - `F2` : the second voxel model, numpy array
    - `rlist` : radii list of shells, list/array, length=Nr

    [__return__] Fourier Shell Correlation between two models (in reciprocal space), numpy array, shape=(Nr,)

- **r_split** (F1, F2, rlist)
    - `F1` : the first voxel model, numpy array
    - `F2` : the second voxel model, numpy array
    - `rlist` : radii list of shells, list/array, length=Nr

    [__return__]  R-split factors between two models (in reciprocal space), numpy array, shape=(Nr,)

- **Pearson_cc** (exp_d, ref_d, axis=-1)
    - `exp_d` : the first (numpy) array, support any dimensional array
    - `ref_d` : the second (numpy) array, same shape with exp_d
    - `axis` : use which axis to calculate cc, default is -1 (the last dimension), if axis!=-1 then use all dimensions

    [__return__] Pearson correlation coefficient between two arrays, numpy array, dimension is N-1 for axis=-1 (N is the dimension of input array), return a float if axis!=-1

- **PRTF** (phased_reciprocal, center, mask=None)
    - `phased_reciprocal` : the reciprocal dataset after N times' independent phasing, dtype=numpy.complex128, shape=(N,Npx,Npy) (2d data) or shape=(N,Npx,Npy,Npz) (3d data)
    - `center` : the coordinates of zero frequency, (Cx,Cy)
    - `mask` : 0/1 two-value numpy array, shape=(Npx,Npy) or (Npx,Npy,Npz), 1 means masked pixel

    [__return__] Phase Retrieval Transfer Function of phased dataset (2D/3D), return radial profile of PRTF matrix, shape=(Np,3), three columns are (r, prtf, std-err)

> analyse.rotate

- **eul2rotm** (ea, order)
    - `ea` : euler angles, intrinsic rotation, [alpha,beta,gamma], unit=rad
    - `order` : rotation order, such as 'zxz', 'zyz' or 'xyz'

    [__return__] transfer euler angles to rotation matrix in intrinsic order, return rotation matrix, numpy array, shape=(3,3)

- **rot_ext** (ea, order, vol, ref=None)
    - `ea` : euler angles, intrinsic rotation, [alpha,beta,gamma], unit=rad
    - `order` : rotation order, such as 'zxz', 'zyz', 'xyz'
    - `matrix` : 3d voxel model to be rotated, numpy array, shape=(Nx,Ny,Nz)
    - `ref` : reference 3d model, if given, program will compare rotated model with reference model by ploting their x/y/z slices

    [__return__] extrinsic rotation to a 3D voxel model, return rotated model, numpy array, shape=(Nx,Ny,Nz)

- **align** (fix, mov, grid_unit=[0.3, 0.1], nproc=2, resize=40, order='zxz')
    - `fix` : fixed model, numpy array, shape=(Nx,Ny,Nz)
    - `mov` : rotated model while aligning, numpy array, shape=(Nx,Ny,Nz)
    - `grid_unit` : grid size (in rad) of alpha,beta,gamma euler angles. This is a list, default=[0.3, 0.1], which means the program will firstly search with grid_size = 0.3 rad, then refine the euler angles with smaller grid_size (= 0.1 rad) from what it just gets that best align two models. Of course you can use more loops such as [0.3, 0.1, 0.02, ...]
    - `nproc` : number of processes to run in parallel, NOTE that the program parallels in alpha angles
    - `resize` : resize your input models to a smaller size to accelerate
    - `order` : rotation order while aligning

    [__return__] align two models by euler angle grid search, return (r_factor, ea, new_mov). "r_factor" is overall r-factor between fixed and best aligned mov model, float; "ea" is best aligned euler angle, [al,be,ga]; "new_mov" is mov model after aligned, numpy array, shape=(Nx,Ny,Nz)

> analyse.SH_expan

- **sp_hamonics** (data, r, L=10)
    - `data` : {'volume':voxel data intensity, 'mask':voxel data mask}, if no mask please set 'mask' : None; shape=(Nx,Ny,Nz)
    - `r` : radius of the shell you want to expand, unit=pixel
    - `L` : level of hamonics, recommended >=10

    [__return__] spherical harmonics expansion 'C(l,m)' of a 3D model, return C(l), shape=(L,)