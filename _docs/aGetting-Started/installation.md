---
title: Installation
category: Getting Started
order: 1
---

#### Requirements

- Linux (Ubuntu/Centos/...) or MacOS
- Anaconda environment, conda version > 4.4
- gcc/g++ >= 4.8
- (optional) openmpi >= 2.0 and gsl >= 1.0

#### Install

- Download from [github repository](https://github.com/LiuLab-CSRC/spipy), or
```bash
[user@linux ~]$ git clone https://github.com/LiuLab-CSRC/spipy -b v3
```
- Go into the folder and run installation script
```bash
[user@linux ~]$ cd spipy
[user@linux spipy]$ chmod u+x ./make_all.sh
[user@linux spipy]$ conda activate base
# For Linux with openmpi and gsl installed
(base)[user@linux spipy]$ ./make_all.sh
# For MacOS and Linux without openmpi and gsl installed
(base)[user@linux spipy]$ ./make_all.sh -x
```
- Notice ! Do **NOT** **delete** or **move** original spipy folder after installation !