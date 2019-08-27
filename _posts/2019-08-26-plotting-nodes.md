---
layout: post
title: Plotting molecular wave function nodes
image: /post-data/2019-08-26-plotting-nodes/O-CAS.jpg
---

Recently, I had to plot some molecular wave function nodes for a paper we were working on.
My goal was to illustrate the change in nodal structure from HF to exact nodes.
Here we will go through the steps to achieve this.
Examples files are available here.

1. Using [PySCF](https://github.com/pyscf/pyscf) for oxygen atom (O.py), run HF and large CASSCF (for "exact" nodes).
2. Convert these wave functions to [QWalk](https://github.com/QWalk/mainline) format using [autogenv2](https://github.com/lkwagner/autogenv2) (done at the end of O.py files).
3. Run QWalk VMC method for a few steps to get reasonable walker positions. 
Use approximately ~10 walkers, since you might want to try different walkers to plot the wave function nodes since they depend on the electron positions. 
This is similar to a 3D object having different 2D shadows depending on the orientation (qwalk.hf).
```
method { VMC
         NCONFIG 10
         NSTEP 100
         NBLOCK 100
}
include qwalk.sys
trialfunc { include qwalk.slater }
```
4. Use QWalk NODES method to scan the space. 
One can use 1 electron or 2 (up and down) electrons to scan. 
Here we use 2 electron scan since this will show higher features of the nodes.
To do this use `DOUBLEMOVES` keyword. 
Note that NODES method will use the very first walker from .config file.
`CONTOURS` will pick which electrons to use in the scan.
QWalk will list all spin up electrons and then all spin down electrons in .config files.
Here we use 1st (up) and 6th (down) electrons.
```
method {
    NODES
    resolution 0.05
    minmax {-5.0.0 5.0.0 -5.0.0 5.0.0 -5.0.0 5.0.0}
    DOUBLEMOVES { 1 1 1}
    contours { 1 6 }
    readconfig qwalk.hf.config
    label VMC
    #dynamics { UNR }
    storeconfig mynodes.config
}
include qwalk.sys
trialfunc { include qwalk.slater }
```

