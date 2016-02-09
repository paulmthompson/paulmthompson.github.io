---
layout : page
title : Manipulating Shapes
---

You most likely are not going through the pains of graphical programming because you simply want a static image to be displayed on the screen; you most likely want it to be able to *do* something. Now we can walk through how we can make our shape change.

### Moving and scaling a buffer

We already saw how we can manipulate data on the GPU. We can simply manipulate the GPU data associated with the shape we have displayed if we wish to move it across the screen.

If we just used the code from the simple shape section as is, we would immediately run into trouble because the renderloop function runs in an infinite loop, and would consequently not accept new input. It does however contain a yield function that can accept other tasks if they arrive. We should therefore run renderloop using the @async macro.

{% highlight julia %}

using GLWindow, GLAbstraction, GeometryTypes

const window = Screen()

const vsh = vert"""
{{GLSL_VERSION}}
in vec2 position;
void main(){
	gl_Position = vec4(position, 0.0, 1.0);
}
"""

const fsh = frag"""
{{GLSL_VERSION}}
out vec4 outColor;
void main() {
	outColor = vec4(1.0, 0.0, 0.0, 1.0);
}
"""

const triangle = std_renderobject(
    Dict{Symbol, Any}(
    :position => GLBuffer(Point2f0[(0.0, 0.5), (0.5, -0.5), (-0.5,-0.5)]),
    	),
LazyShader(vsh, fsh)
)

push!(window.renderlist,triangle)

@async renderloop(window)

{% endhighlight %}

Now if we change the points in the position buffer, we will see this manifested in our GLWindow. If we wanted to move the triangle up and to the right, we could do this:

{% highlight julia %}

gl=triangle.vertexarray.buffers["position"]

for i=1:3
    	GLAbstraction.setindex!(gl,gl[i]+1f0,i:i)
end

{% endhighlight %}

If we wanted to move just to the right:

{% highlight julia %}

for i=1:3
    	GLAbstraction.setindex!(gl,Point2f(gl[i][1],gl[i][2]+1f0),i:i)
end

{% endhighlight %}

Or if we wanted to make the triangle shrink by half:

{% highlight julia %}

for i=1:3
    	GLAbstraction.setindex!(gl,gl[i]*.5,i:i)
end

{% endhighlight %}

Overall this process is kind of cumbersome, and inefficient. Even though the entire shape is really moving the same direction, we receiving and sending each data point. We can do a little better in our shrink example by passing everything at once:

{% highlight julia %}

GLAbstraction.setindex!(gl,gl*.5,1:3)

{% endhighlight %}

We still are having to resend a value for every index on the buffer, and performing all of the computations on the CPU. We can do better than this.

### Uniform Manipulations

In each of the instances from above, we really only are using one value to manipulate our entire triangle: moving all points in the x and y direction by 1.0, scaling all points by 0.5. Could we just pass this value? The answer is yes. Since it would be applied to every point on a buffer, we would designate this as a "uniform" variable in our shader. We are also going to have to tell our shader what to do with it. If we wanted to make a uniform variable to move all of our points in the x direction, we could define the following vertex shader:

{% highlight julia %}

const vsh = vert"""
{GLSL_VERSION}}

in vec2 position;

uniform float t;

void main(){
    	vec2 pos = position;
    	pos.x+=t;
    	gl_Position = vec4(pos, 0.0, 1.0);
}
"""

{% endhighlight %}

We would also define this value in our dictionary in the triangle:

{% highlight julia %}

data=merge(Dict{Symbol, Any}(
    :position => GLBuffer(Point2f0[(0.0, 0.5), (0.5, -0.5), (-0.5,-0.5)]),
    :t => 0.0))

l = LazyShader(vsh, fsh)

const triangle = std_renderobject(data,l)

{% endhighlight %}

We can now move our triangle much more easily:

{% highlight julia %}

#Move right
triangle.uniforms[:t]=1f0

#Move left
triangle.uniforms[:t]=-1f0

{% endhighlight %}

### Matrix manipulations

So with a few uniform variables, we can easily be moving our 2D triangle all over the place. Now what if we don't have a simple 2D triangle, but a large complex 3D shape? Something that can translate or rotate in 3 dimensions. Each degree of freedom would give us a new uniform variable to keep track of. Alternatively, we could use a matrix.

{% highlight julia %}

const vsh = vert"""
{{GLSL_VERSION}}
in vec2 position;
uniform mat4 matrix;

void main(){
    	gl_Position = matrix * vec4(position, 0.0, 1.0);
}
"""

{% endhighlight %}

We then add this to our dictionary:

{% highlight julia %}

mymat=FixedSizeArrays.Mat{4,4,Float32}(eye(Float32,4))

data=merge(Dict{Symbol, Any}(
    :position => GLBuffer(Point2f0[(0.0, 0.5), (0.5, -0.5), (-0.5,-0.5)]),
    :matrix => mymat))

l = LazyShader(vsh, fsh)

const triangle = std_renderobject(data,l)

{% endhighlight %}

Now we have a flexible system where we can upload a new position matrix everytime it is necessary, and this one matrix can take care of rotations and translations at the same time. None of this is really necessary for a simple 2D triangle though. The size of the matrix actually takes up more space than re-uploading the buffer of 2D points. That one 16 float matrix though is much much smaller than a large 3D object with many vertices, made of many many triangles. Let us consider that next.

Next:
[Complex Shapes](8_complex_shape.html)

Previous:
[Simple Shape](6_simple_shape.html)
