---
layout : page
title : Vertices and data on the GPU
---

The 3d positions of points on the face of simple shapes (the vertices) are the positions that are important for OpenGL rendering. It is also important to note that describing the 3d positions for openGL is also the step where data is communicated to memory on the graphics card. 

### GLBuffer

Kind of like C, OpenGL is going to want to store all of its data in chunks without caring much what those chunks are. The best Julia types to go between Julia and OpenGL would therefore work the same way (they would not have pointers to data in other locations). Immutable Arrays, or Fixed Size Arrays, do just this. 

{% highlight julia %}

const win = Screen()
gl=GLBuffer(Vec{2,Float32}[(0.0,0.5),(0.5,-0.5),(-0.5,-0.5)])

{% endhighlight %}

It may be more intuitive for the user to express points in some dimensional space as such, and have the data structure be in the form of a Fixed Size Array, that way there is an intutive transfer back and forth between Julia world and OpenGL world. The GeometryTypes package does just this, where 3, two-dimensional points of 32-bit floating point numbers can be expressed as:

{% highlight julia %}

gl=GLBuffer(Point2f0[(0.0, 0.5), (0.5, -0.5), (-0.5,-0.5)])

{% endhighlight %}

### Sending data to and from the GPU

The GLBuffer type is the interface to the CPU. This means that simply indexing the buffer type is actually gathering data from the GPU. The following code:

{% highlight julia %}

gl[1]

{% endhighlight %}

will produce:

FixedSizeArrays.Point{2,Float32}((0.0f0,0.5f0))

We can change the data stored on the GPU as well. The following code:

{% highlight julia %}

GLAbstraction.setindex!(gl,Point2f0(0.0,0.0), 1:1)
gl[1]

{% endhighlight %}

will produce:

FixedSizeArrays.Point{2,Float32}((0.0f0,0.0f0))

### Calculations on the GPU

Image you want to add 1.0 to every point on the buffer in both the x and y direction. Based on the code above, this could be implemented as the following:

{% highlight julia %}

for i=1:3
    	GLAbstraction.setindex!(gl,gl[i]+1f0,i:i)
end

{% endhighlight %}

This is actually pretty inefficient. For every point, you are transfering the data from the GPU to CPU by indexing, adding 1.0 to the Point on the CPU, and then sending the new point back to the GPU to be stored on the buffer. GPUs are actually pretty smart themselves, and it would be much more efficient to just send a value of 1.0 to the GPU, allow it to added to all of the points efficiently using the parallel architecture of the GPU, and then not have to return anything back. Unforunately as of right now, we can't execute pure Julia code on a GPU, but there are optionis to accomplish this idea. The easiest using OpenGL would use shaders, as discussed next.

Next:
[Shaders](5_shaders.html)

Previous:
[Rendering Components](3_render.html)

