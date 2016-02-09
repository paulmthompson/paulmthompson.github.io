---
layout : page
title : Shaders
---

After we have described the positions of the vertices, we need to describe how those vertices create shapes, and how those shapes map to pixels. This functionality is achieved with shaders, and shaders are coded in the OpenGL Shading Language (GLSL). GLSL code can be quickly uploaded and compiled on the graphics card, so it can actually interface well with a language like Julia to quickly create shaders as needed.

Even Julia users who will ultimately dive into the nitty gritty of OpenGL wrappers hopefully won't have to write shaders of their own. Shaders can get incredibly complex and continue to evolve as programmers figure out how to render more and more realistic visual scenes. Therefore, julia user should be most familiar with what kinds of inputs and outputs enter and leave shaders.

### Vertex Shader

Vertex shaders describe how points in space are assembled together. Ultimately these result in 4D points. If you are working with something that is actually 2D, the user need only supply 2 points. A shader definition in Julia can be written in GLSL code wrapped by a julia macro:

{% highlight julia %}

const vsh = vert"""
{{GLSL_VERSION}}
in vec2 position;
void main(){
    	gl_Position = vec4(position, 0, 1.0);
}
"""

{% endhighlight %}

Variables that are passed from the CPU to the GPU are identified as "in" variables. Any variables that would need to be passed directly onto the texture shader would be identified as "out." Finally, there can be input variables called "uniform" where one value or group of values is passed from the CPU to GPU, but all vertexes in the same buffer will use that same value. In this way, the same value does not have to be passed over and over again. For instance if you want to move a complicated 3D mesh with many many vertices in the x direction by 1.0, it would be much more efficient to pass 1.0 once, rather than over and over again for each vertex.

### Texture Shader

A texture shader takes the vertex shaders and creates pixels on the screen. OpenGL can figure out where the pixels need to go; the user needs to specify the color and opacity.

{% highlight julia %}

const fsh = frag"""
{{GLSL_VERSION}}
out vec4 outColor;
void main() {
    	outColor = vec4(1.0, 1.0, 1.0, 1.0);
}
"""

{% endhighlight %}

### Bringing shaders together

Now that we have written our shaders, we need to have them be appropriately compiled and linked under the hood so OpenGL knows how to use them. The LazyShader function provides this ability:

{% highlight julia %}

l=LazyShader(vsh,fsh)

{% endhighlight %}

Now we have almost everything we need to make shapes appear on the computer screen. Next, we need to bring it all together in a nice Julia type, and make it display on the screen.

Next:
[Simple Shape](6_simple_shape.html)

Previous:
[GPU Buffer](4_opengl_buffer.html)


