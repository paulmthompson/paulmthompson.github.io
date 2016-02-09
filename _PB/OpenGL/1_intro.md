---
layout : page
title : OpenGL in Julia
---

### Introduction

Graphical programming is a lot like parallel programming in high level languages: from a bird's eye view it seems like it should be pretty simple to implement, and have numerous benefits (4 cores should give 4x the performance right?); however, often small and subtle code practices can be the difference between something that "works" and something that does not, especially from the perspective of a new user. 

Good parallel programming in Julia does not mean that the user has to perform a lot of MPI calls; usually similar performance can be achieved within the Julian architecture, but knowledge of some of the low level routines can help the programmer to quickly iterate and modify high level code.

Robust high level interfaces for graphical programming are being developed in Julia, but it is still useful to understand some of what is happening "under the hood," even if you never plan on writing any OpenGL code. These tutorials will guide you through the low level workings that a Julia user should be familiar with. These tutorials are in no way a replacement for learning OpenGL. A good introduction to OpenGL for someone with limited graphic experience and some programming experience should check out the tutorials for the Rust Language wrapper:

https://github.com/tomaka/glium/tree/master/book

Even without a background in Rust, they are very informative.

### What OpenGL is and what OpenGL is not

OpenGL is an API for communicating with a graphics card. It is mature (1.0 release was in '97), maintained (4.5 release was in 2014), and widely adopted. OpenGL is also language independent and cross platform.

However, some of the flexibility of OpenGL comes from the fact that it does not do some of the things a new user would expect it to do. For example, OpenGL itself does not provide an interface for input or even creating windows. Likewise, while OpenGL is language agnostic, it requires libraries to interface with it for each language.

### How can Julia communicate with OpenGL?

There are mature C bindings for OpenGL, which differ depending on the operating system. Since Julia can easily call C, these are the preferred method of interface. GLX is the Linux library, WGL is the windows library, and OpenGL headers are included in the core MacOSX operating system.

### Window system

Because of the wide support of OpenGL, it again does not natively handle the creation of windows to display contexts. There are many libraries to provide this functionality, such as GLFW, freeglut, GLUT, SDL, Qt, etc. Some of these are simple and barebones (e.g. GLFW), while some of them are part of massive widget toolkits (e.g. Qt). 

### Moving parts

So, if we want to use Julia with OpenGL, let us now consider the underlying structure discussed above, and what components will be necessary. First we need a Julia interface with OpenGL, but really we know that openGL is language agnostic, so we need an interface from Julia -> OpenGL C bindings -> OpenGL.

1. OpenGL
2. C bindings (platform dependent)
3. Julia wrapper for C bindings (ModernGL.jl)

Now remember that we can't actually do much with the above tools because we need some kind of window creation library like GLFW. Right now, GLFW is the only supported Julia wrapper for making OpenGL windows, but it doesn't have to be. In the future, we may want to leave room for a standard way that Julia can make OpenGL windows, regardless of the underlying library being used

4. GLFW library
5. Julia wrapper for GLFW (GLFW.jl)
6. Julia module for making an OpenGL window in a library agnostic way (GLWindow.jl)

Finally, there is going to be glue code that arises from manipulating data on a GPU with a language that does not natively do this. This leaves us with a list of necessary components to use Julia with OpenGL:

1. OpenGL
2. C bindings (platform dependent)
3. Julia wrapper for C bindings (ModernGL.jl)
4. GLFW library
5. Julia wrapper for GLFW (GLFW.jl)
6. Julia module for making an OpenGL window in a library agnostic way (GLWindow.jl)
7. Julia module with glue for manipulation of data on GPU (GLAbstraction.jl)

So I expect that most Julia users are like me: I would never like to think about all of the sublties written above; I just want some big graph to update on my computer screen, and because I have a graphics card in my computer, I expect it to happen fast. To use openGL, I would ideally interface with one module:

1. Julia module for doing all of the stuff Julia does well like plotting, interacting, displaying images etc, but doing it faster, and extended to include 3D objects and camera manipulation; this happens because OpenGL...I think

This is approximately the goal of GLVisualize.jl: to allow for high level manipulation of familiar Julian-like data structures while taking advantage of OpenGL under the hood for fast and powerful rendering with a GPU. The items 1-7 in the list above are still going to need to exist, and power users can take advantage of them for fine tuning.

### GLVisualize Interface

A high level interface for creating visualizations will center around the following components:

1. A screen to show the visualization
2. The visualization itself
3. A control loop that manages the appropriate updating, callbacks, interactivity, etc

The package itself will take care of most of items 1 and 3, while the nature of item 2 will be more dependent on what the user intends to visualize. 

While the goal is to allow the package to manage most of the complexities of working with OpenGL, with so many layers of abstraction, it can be useful to have some general understanding of what is going on under the hood. The documentation of a similar package in the Rust Language puts it well:

"Glium gives you access to all the tools but doesn't prevent you from doing horribly slow things. Some knowledge of modern techniques is required if you want to reach maximum performances."

With that, lets dive into some OpenGL with Julia!

Next:
[Creating Windows](2_windows.html)


