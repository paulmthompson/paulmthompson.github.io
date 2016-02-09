---
layout : page
title : Preparing to Render
---

### What do we need to render an object?

So if we want to make a simple shape, say a triangle, appear on the screen, what necessary components are involved? Importantly note that during this step somewhere is where the flow of data from CPU to GPU will occur. Below we will explore where and why.

OpenGL breaks down the knowns into these groups: 1) you know how to describe the shape you want to render, 2) you can describe where the shape is located in space, 3) you can describe how to go from a shape to pixels on the screen. While this sounds really difficult, OpenGL will actually take care of most of this information for you. OpenGL provides the GSLS language, which is pretty similar to C, to describe "shaders." A Vertex Shader describes the shape of an object. A Texture shader describes how to go from the vertex shader to pixels on the screen. Shaders can also perform mathematical calculations and logical statements; however shader code is executed on the GPU. So our workflow is then

1) Describe coordinates of shape in space (on CPU)
2) Send coordinates to GPU
3) Use coordinates to manipulate shape (on GPU)
4) Use shape to make pixels on the screen (on GPU)

Usually the "shape" that a GPU knows about is a triangle, and more complex shapes are created by putting triangles together. This allows for a lot of tricks: consider a large complex object that is made of many triangles. If you wanted to move the object around in space and visualize this, you could change the coordinates of every triangle making up the object and send the change in all of these coordinates from the CPU to the GPU. Alternatively, you could send the change in the x,y and z position for the object to the GPU, and apply the spatial transformation to every vertex of the object on the GPU in parallel. As you would expect, this second option is much faster.

### Coordinates

### Shaders




### Constructing an object

So something we render in OpenGL is described by its points in space, how those points are used to make shapes (vertex shader), and how those shapes are used to fill pixels on the screen (texture shader).


Next:
[GPU Buffer](4_opengl_buffer.html)

Previous:
[Creating Windows](2_window.html)
