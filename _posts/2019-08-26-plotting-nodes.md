---
layout: post
title: Plotting molecular wave function nodes
image: /post-data/2019-08-26-plotting-nodes/O-CAS.jpg
---

Recently, I had to plot some molecular wave function nodes for a paper we were working on.
My goal was to illustrate the change in nodal structure from HF to exact nodes.
Here we will go through the steps to achieve this.
Examples files are available here.

1. Using [PySCF](https://github.com/pyscf/pyscf) for oxygen atom, run HF and large CASSCF (for "exact" nodes).
2. Convert these wave functions to [QWalk]() format using [autogenv2](https://github.com/lkwagner/autogenv2) (done at the end of python files).

