---
title: Introduction
category: Scripts
order: 1
---

In *spipy/scripts* folder, there are scripts for users to use spipy easily. The installation procedure will generate executable files (in *bin* folder) to run these scripts.

**NOTICE** All of the *mask* file used in spipy scripts are required to contain a two-value array, where **masked pixels** should take the value of **1** while unmasked pixels are 0.

The scripts only receive command line arguments and output result files.

#### Usage and Document

- *spi_phasing*
	- Phase retrieval using multiple algorithms
	- Use '-h' option to see details 
- *spi_simulator*
	- Single particle diffraction simulation
	- Use '-h' option to see details
- *spi_emcsetup*
	- Set up EMC running environment
	- Use '-h' option to see details
- *spi_viewer*
	- 2D/3D data viewer
	- 2D mask maker
	- Forked from [this repo](https://github.com/LiuLab-CSRC/DataViewer)
	- Use '-h' option to see details
- *spi_events*
	- Parse CXI/HDF5/TIF... files and list events
	- Use '-h' option to see details
- *spi_qcenter*
	- Calculate coordinates of central point (q=0) of diffraction patterns
	- Use '-h' option to see details
- *spi_darkcal*
	- Dark calibration of SPI experiment
	- Use '-h' option to see details