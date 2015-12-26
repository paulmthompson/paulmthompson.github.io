---
layout : page
title : Neurolucida File Import
---

## Overview

A common file type of 3D neuronal morphologies is from [Neurolucida](http://www.mbfbioscience.com/neurolucida) software. These have the .asc file extension. I will briefly outline how JNeuron parses these files.

## File format

If you take a look at a neurolucida file, it looks kind of Lisp-y, with values between parentheses of varying levels of indentation. Here is a snippet from a modelDB [file](https://senselab.med.yale.edu/ModelDB/ShowModel.cshtml?model=139653&file=/L5bPCmodelsEH/morphologies/cell2.asc):

<pre>
( (Color RGB (100, 100, 100))
  (Dendrite)
  (  469.57   100.92   -11.00     0.29)  ; Root
  (  470.73   100.28   -11.37     1.75)  ; 1, R
  (
    (  472.19   100.28    -6.97     1.17)  ; 1, R-1
    (  474.82    99.02    -4.55     1.17)  ; 2
    (  478.61    99.34    -2.27     1.17)  ; 3
    (  482.70    99.02    -2.13     1.17)  ; 4
    (  485.33    98.70    -1.67     1.17)  ; 5
    (
      (  487.66    99.34    -1.67     0.88)  ; 1, R-1-1
      (  489.41    99.34    -0.12     0.88)  ; 2
      (  489.70   100.28    -0.07     0.88)  ; 3
      (  490.29   102.18     0.60     0.88)  ; 4
      (  492.33   103.76     1.92     1.17)  ; 5
      (  494.37   104.08     2.40     1.17)  ; 6
      (  497.00   103.76     4.20     1.17)  ; 7
      (  498.46   103.13     9.35     1.17)  ; 8
      (
        (  498.75   101.86    11.10     0.88)  ; 1, R-1-1-1
        (  499.04   100.92    11.22     0.88)  ; 2
        (  500.12   100.60    11.37     0.88)  ; 3
        (  502.16   101.86    11.45     0.88)  ; 4
</pre>

Whatever is between two parentheses is part of the same component of the file. In this example, we start with an open parentheses that is never closed, meaning that everything that follows is part of the same trunk. For the most part, JNeuron only cares about the lists of 4 values and a few of the labels. 

Four cell labels matter to JNeuron: Axon, Cell Body, Dendrite, and Apical. As you can see, this is a dendrite. Axon, Dendrite and Apical follow the same method to parse. Cell Body is a little different and will be discussed separately below.

Each time a new indented lone parenthesis is seen, that indicates that there is a branch from the current trunk. Right hand parentheses indicate that a segment is now complete and the next values listed will be part of the previous trunk.

The lines of 4 values signify the x, y and z position of the current section, along wth the diameter. On the far right side of this example, you can see the numbers signifying the index of this data point in the section, and even labels for each section. I have not always found these consistently in data files.

## Soma

## JNeuron Parser
