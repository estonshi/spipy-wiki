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
    * Go into "spipy/test_spipy" folder, for each module there are some example python scripts. Play with them !

![practice](../../images/testfolder.png)