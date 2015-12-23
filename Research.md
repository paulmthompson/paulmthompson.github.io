---
layout : page
title : Research
header : Research
group : navigation
weight : 3
---

## Cortical Representations of Bimanual Movements

Independent movements by two arms, such as reaching simultaneously to two different targets, are prone to patterns of interference. For example, it is easier to move two arms in the same direction than to make different movements with the right and left arm. The underlying neural mechanisms are poorly understood. I am investigating the neural correlates during patterns of interference during bimanual movements using multi-electrode arrays in primates.

[SFN 2015 Poster](assets/pdfs/SFN_2015.pdf)

## Electrophysiology Toolkits

There are components of software for electrophysiology experimental setups that are very similar among paradigms, as well as those that are very different. I have often found myself spending far too long trying to integrate cumbersome software that does far more than what I actually need it to do with other similar software to run a relatively straightforward experimental. Consequently, I have so far developed some simple libraries for data acquisition and spike sorting to be used in experimental setups.

<b>Intan.jl</b> - [Github](https://github.com/paulmthompson/Intan.jl) - [Documentation](http://intanjl.readthedocs.org/en/latest/)

<b>SpikeSorting.jl</b> - [Github](https://github.com/paulmthompson/SpikeSorting.jl) - [Documentation](http://spikesortingjl.readthedocs.org/en/latest/)

## Multi-compartment Neuron Models

Computational models of complex neuronal structures are the building blocks of many more complex simulations, such as realistic local field potentials, or large network activity.  Because most successful software for this purpose have been written in compiled languages, they are not always flexible enough to easily integrate with other simulations. Additionally, compiled languages do not allow for rapid prototyping and quickly transitioning code to take advantage of parallel architectures. For these reasons, I have constructed a multi-compartmental simulator in Julia, which has the performance of popular compiled simulators, but has a compact and easy to modify codebase, characteristic of scripting languages.

<b>JNeuron</b> - [Github](https://github.com/paulmthompson/JNeuron) - [Documentation](http://jneuron.readthedocs.org/en/latest/)