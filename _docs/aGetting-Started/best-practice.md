---
title: Best Practice
category: Getting Started
order: 3
---

* Show help information
```python
Ipython 7.8.0
In [1]: import spipy
In [2]: spipy.info.VERSION
In [3]: spipy.help()
In [4]: from spipy import analyse
In [5]: analyse.help()
In [6]: analyse.rotate.help("align")
```

* Import modules
```python
Ipython 7.8.0
In [1]: from spipy import *
```

* Try functions
    * Go into *demo* folder, for each module there are some example python scripts. Play with them !

![practice](../../images/testfolder.png)

* Try scripts
	* After running "make_all.sh" there will be a "bin" folder, try those executable files inside !

* Some results
	* **spipy.phase.phModel** : Phase retrieval results (2D and 3D)
![prnf-re-2d](../../images/2D-phasing.png)
![prnf-re-3d](../../images/3D-phasing.png)
	* **spipy.simulate.sim_adu** : simulation results (intensity pattern and photon pattern)
![simulation](../../images/simulation.png)
	* **spipy.image.io** : read pdb, transfer to density map and view ([DataViewer-download](https://github.com/estonshi/DataViewer))
![readpdb](../../images/pdb2density.jpg)
	* **spipy.image.preprocess** : hit-finding, transfer intensity to photon, fix artifact, ...
![fix-artifact](../../images/fix_art_auto.png)
	* **spipy.analyse.saxs** : find diffraction center, estimate particle size, ...
![particle-size](../../images/particle-size-estimate.png)
	* **spipy.merge.tools** : slicing and merging, likelihood calculation, ...
![slimerg](../../images/slicing_merging.png)
	* more functions please refer to [framework](framework.md)