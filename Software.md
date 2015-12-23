---
layout : page
title : Software
header : Software
group : navigation
weight: 4
---



## JNeuron

This is a toolbox for detailed multicompartmental neuron models, and related calculations such as extracellular potentials and network activity. JNeuron is implemented from scratch in Julia, and has comparable, or even faster, performance than compiled simulators such as Neuron. 

[Github](https://github.com/paulmthompson/JNeuron) - [Documentation](http://jneuron.readthedocs.org/en/latest/)

## SpikeSorting.jl

This is a Julia implementation of various spike sorting methods. All methods have been optimized for speed and prepared for online implementation. Every method is modular, such that a given algorithm for spike detection is compatible with any feature extraction method or clustering method, etc. I hope for this to be useful both in acquiring data, and also benchmarking new spike sorting ideas.

[Github](https://github.com/paulmthompson/SpikeSorting.jl) - [Documentation](http://spikesortingjl.readthedocs.org/en/latest/)

## Intan.jl

This is an interface for an electrophysiology acquisition system based on Intan amplifiers. [OpenEphys](http://www.open-ephys.org/) is a mature and open source alternative for the same purpose. My hope with Intan.jl is that the pure scripting language implementation makes it much less verbose, and easier to modify.

[Github](https://github.com/paulmthompson/Intan.jl) - [Documentation](http://intanjl.readthedocs.org/en/latest/)
