---
title: image
category: Documents
order: 2
---

> image.radp

- **radial_profile** (data, center, mask=None)
    - `data` : input pattern, numpy array, shape=(Nx,Ny) or (Nx,Ny,Nz)
    - `center` : center (q=0) coordinates, (Cx,Cy) or (Cx,Cy,Cz), unit=pixel
    - `mask` : 0/1 two-value numpy array, same shape with data, 1 means masked pixel

    [__return__] intensity radial profile of a pattern (2d/3d), return numpy array, shape=(Nr,3), the three columns are r(in pixels), Iq and std-error

- **shells** (rads, data_shape, center)
    - `rads` : radii list of output shells, [r1,r2,...], unit=pixel
    - `data_shape` : shape of your pattern, [Nx,Ny] or [Nx,Ny,Nz], unit=pixel
    - `center` : center (q=0) coordinates, (Cx,Cy) or (Cx,Cy,Cz), unit=pixel

    [__return__] xy indices of a pattern which forms a shell/circle at radius=rads,return list [shell1, shell2, ...], for each item it is a numpy array, shape=(N<sub>i</sub>,)

- **radp_norm** (ref_Iq, data, center, mask=None)
    - `ref_Iq` : reference radial profile, numpy array, shape=(Nr,)
    - `data` : pattern that need to be normalized, numpy array, shape=(Nx,Ny) or (Nx,Ny,Nz)
    - `center` : center (q=0) coordinates, (Cx,Cy) or (Cx,Cy,Cz), unit=pixel
    - `mask` : 0/1 two-value numpy array, same shape with data, 1 means masked pixel

    [__return__] scaling pattern/volume intensities (at different radii) referring to a given radial profile, return scaled pattern, same shape with 'data'

- **circle** (data_shape, rad)
    - `data_shape` : output dimension, int, 2 (circle) or 3 (sphere)
    - `rad` : float, radius of generated area, in pixels

    [__return__] generate a circle/sphere solid area with given radius, center is origin. Return points' index inside this area, numpy array, shape=(N,data_shape)

> image.quat

*In this mudule, quaternion format is [w,q0,q1,q2], where w=cos(theta/2)*

- **invq** (q)
    - `q` : input quaterion

    [__return__] inverse of a quaternion, q<sup>-1</sup>

- **quat_mul** (q1, q2)
    - `q1`, `q2` : input quaternions

    [__return__] multiply two quaternions, return q1 * q2

- **conj** (q)
    - `q` : input quaterion

    [__return__] conjugate of a quaternion, q<sup>*</sup>

- **quat2azi** (q)
    - `q` : input quaterion

    [__return__] transfer quaternion to azimuth angle, return numpy.array([theta,x,y,z])

- **azi2quat** (azi)
    - `azi` : input azimuth angle [theta,x,y,z]

    [__return__] transfer azimuth angle to quaternion

- **quat2rot** (q)
    - `q` : input quaterion

    [__return__] transfer quaternion to 3D rotation matrix, return numpy matrix, shape=(3,3)

- **rot2quat** (rot)
    - `rot` : rotation matrix, numpy array, shape=(3,3)

    [__return__] transfer 3D rotation matrix to quaternion

- **rotv** (vector, q)
    - `vector` : input vector to rotate, numpy.array([x,y,z])
    - `q` : input quaternion

    [__return__] vector after rotation, numpy array, shape=(3,)

- **Slerp** (q1, q2, t)
    - `q1` & `q2` : two input quaternions
    - `t` : float between 0~1, interpolation weight from q1 to q2

    [__return__] linear interpolation on spherical surface between two quaternions, return new quaternion

> image.classify

- **cluster_fSpec** (dataset, mask=None ,low_filter=0.3, decomposition='SVD', ncomponent=2, nneighbors=10, LLEmethod='standard', clustering=2, njobs=1, verbose=True)
    - `dataset` : raw dataset, numpy array, shape=(Nd,Nx,Ny), Nd>1000 is recommended
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `low_filter` : float 0~1, the percent of area at the central part of diffraction pattern that is used for clustering
    - `decomposition` : decoposition method, choosen from 'LLE' (Locally Linear Embedding), 'SVD' (Truncated Singular Value Decomposition) and 'SpecEM' (Spectral Embedding)
    - `ncomponent` : number of dimension left after decomposition
    - `nneighbors` : number of neighbors of each point, considered only when decomposition method is 'LLE'
    - `LLEmethod` : LLE method, choosen from 'standard' (standard locally linear embedding algorithm), 'modified' (modified locally linear embedding algorithm), 'hessian' (Hessian eigenmap method) and 'ltsa' (local tangent space alignment algorithm)
    - `clustering` : int, whether to do clustering (<0 or >0) and how many classes (value of this paramater)
    - `njobs` : int, number of subprocesses
    - `verbose` : bool

    [__return__] single-nonsingle hits clustering using linear/non-linear decomposition and spectural clustering, return (dataset_decomp, label). "dataset_decomp" is dataset after decomposition, numpy array, shape=(Nd,ncomponent); "label" is predicted label from clustering algorithm, numpy array, shape=(Nd,), return [] if 'clustering'<0

- **cluster_fTSNE** (dataset, mask=None, low_filter=0.3, no_dims=2, perplexity=20, use_pca=True, initial_dims=50, max_iter=500, theta=0.5, randseed=-1, clustering=2, njobs=1, verbose=True)
    - `dataset` : raw dataset, numpy array, shape=(Nd,Nx,Ny), 500<Nd<5000 is recommended
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `low_filter` : float 0~1, the percent of area at the central part of diffraction pattern that is used for clustering
    - `no_dims` : number of dimensions left after decomposition
    - `perplexity` : positive int, usually has positive correlation with dataset amount, >10 and <50 will be fine
    - `use_pca` : bool, whether to use PCA to generate initiate features
    - `initial_dims` : positive int, output dimensions of PCA, ignored if use_pca=False
    - `max_iter` : int, max times of iterations, suggested >500
    - `theta` : float 0~1, the speed/accuracy trade-off parameter, theta=1 means highest speed with lowest accuracy
    - `randseed` : int, if it is >=0, then use it as initial value's generating seed; else use current time as random seed
    - `clustering` : int, whether to do clustering (<0 or >0) and how many classes (value of this param)
    - `njobs` : int, number of subprocesses
    - `verbose` : bool

    [__return__] single-nonsingle hits clustering using t-SNE decomposition and KNN clustering, return (dataset_decomp, label). "dataset_decomp" is dataset after decomposition, numpy array, shape=(Nd,no_dims); "label" is predicted label from clustering algorithm, numpy array, shape=(Nd,), return [] if 'clustering'<0

> image.preprocess

- **hit_find** (dataset, background, radii_range=None, mask=None, cut_off=None)
    - `dataset` : raw dataset, numpy array, shape=(Nd,Nx,Ny)
    - `background` : dark pattern for calibration, numpy array, shape=(Nx,Ny)
    - `radii_range` : radii of annular area used for hit-finding, list/array, [center_x, center_y, inner_r, outer_r], unit=pixel. You could give center_x,center_y=None and it will search for pattern center automatically
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `cut_off` : chi-square cut-off, positve int/float, default=None and a mix-gaussian analysis is used for clustering. Usually a number >= 10 is good

    [__return__] hit finding based on chi-square test, high score means hit, return label, numpy array, shape=(Nd,). Value of 1 means a hit

- **hit_find_pearson** (dataset, background, radii_range=None, mask=None, max_cc=0.5)
    - `dataset` : raw dataset, numpy array, shape=(Nd,Nx,Ny)
    - `background` : dark pattern for calibration, numpy array, shape=(Nx,Ny)
    - `radii_range` : the same with 'radii_range' in 'hit_find' function
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `max_cc` : float from -1~1, max cc for patterns to be identified as hits, lower means more strict

    [__return__] hit finding based on pearson cc test, lower score means hit. Return label, numpy array, shape=(Nd,). Value of 1 means a hit

- **fix_artifact** (dataset, estimated_center, artifacts, mask=None)
    - `dataset` : a dataset of patterns, numpy array, shape=(Nd,Nx,Ny)
    - `estimated_center` : estimated pattern center in pixels, (Cx,Cy)
    - `artifacts` : the locations of bad pixels in pattern (indices), numpy array, shape=(Na,2), the first colum is x coordinates of artifacts and second colum is y coordinates
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel

    [__return__] replace bad pixels with centrosymmetric pixels to reduce artifacs, input 'dataset' is modified directly and returned. NOTE patterns in the dataset must have same artifacts.

- **fix_artifact_auto** (dataset, estimated_center, njobs=1, mask=None, vol_of_bins=100)
    - `dataset` : a dataset of patterns, numpy array, shape=(Nd,Nx,Ny), Nd>1000 is recommended
    - `estimated_center` : estimated pattern center in pixels, (Cx,Cy)
    - `njobs` : number of subprocesses
    - `mask` : 0/1 two-value numpy array, shape=(Nx,Ny), 1 means masked pixel
    - `vol_of_bins` : int, the number of similar patterns that will be processed together in a group, >100 will be good
    
    [__return__] fix artifacts without providing the position of bad pixels, data with similar intensity and pattern will be grouped together and use averaged radial profile to analysis artifacts at every pixel. Input dataset is modified directly and returned.

- **cal_correction_factor** (det_size, polarization, detd, pixsize, center=None)
    - `det_size` : detector size, [Nx, Ny]
    - `polarization` : 'x' or 'y' or 'none'
    - `detd` : float, detector distance, in mm
    - `pixsize` : float, pixel size in mm
    - `center` : detector center, [Cx,Cy], default is None and use geometric center

    [__return__] calculate polarization correction and solid angle effective area correction factor, return numpy array, shape=(Nx,Ny), all values <=1.0

> image.io

- **readccp4** (file_path)
    - `file_path` : string, path of ccp4/mrc file to read

    [__return__] read ccp4/mrc files, return a dict, {'volume':indensity matrix, 'header':header information}

- **writeccp4** (volume, save_file)
    - `volume` : the 3D intensity volume, numpy array, shape=(Nx,Ny,Nz)
    - `save_file` : specify the path+name of ccp4/mrc file that you want to save to, for example save_file='./test.ccp4'

    [__return__] no return

- **readpdb_full** (pdb_file)
    - `pdb_file` : string, path of pdb file to read

    [__return__] read pdb files, return a dict {res-index:[res-name,{atom-name:[atom-index,atom-mass,x,y,z,occupancy,b-factor],...}],...}

- **pdb2density** (pdb_file, resolution, lamda=None)
    - `pdb_file` : string, path of pdb file to read
    - `resolution` : float, the resolution of returned density map, in Angstrom
    - `lamda` : float, laser wavelength in Angstrom
    
    [__return__] transfer pdb to electron density map, return density map, numpy array, shape depends on resolution

- **xyz2pdb** (xyz_array, atom_type, b_factor=None, save_file="./convert.pdb")
    - `xyz_array` : numpy array, shape=(N,3), three columns are x,y,z coordinates
    - `atom_type` : list, which atoms would you like to write in the file. If there is only one item, then all atoms are the same; otherwise you should give a list containing the same number of atom types with the xyz_array length. For example, you can either give ['C'] or ['C','H','H','O','H'] for a 5-atom pdb file. No matter upper or lower case.
    - `b_factor` : array or list, b-factors in pdb file, shape=(N,), default is None then all b-factors=1
    - `save_file` : str, file path to save

    [__return__] write 3D xyz coordinates as a pdb file, no return

- **cxi_parser** (cxifile, out='std')
    - `cxifile` : str, cxi file path
    - `out` : str, give 'std' for terminal print or give a file path to redirect to

    [__return__] print cxi/h5 inner path structures, no return