---
title: Installation
category: Graphics UI
order: 1
---

#### Requirements

- spipy successfully installed

#### Install

- Download from [Github repo](https://github.com/estonshi/spipy_gui/tree/v1.1), or
```bash
[user@linux ~]$ git clone https://github.com/estonshi/spipy_gui.git -b v1.1 --single-branch
```
- Install
```bash
[user@linux ~]$ cd spipy_gui-1.1
[user@linux spipy_gui-1.1]$ ./compile.sh 
```
- Start GUI
```bash
[user@linux spipy_gui-1.1]$ conda activate spipy-v3
(spipy-v3)[user@linux spipy_gui-1.1]$ ./spipy.gui
```

#### Note

The graphics UI doesn't support v3.1 & v3.2 features yet. 