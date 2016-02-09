---
layout : page
title : Simple Shapes
---

To display a shape, we are going to need to specify the shape in terms that OpenGL can understand and provide a place where we can see the shape displayed. We already learned about how the Screen type in julia provides an OpenGL context, as well as a simple renderloop function that will take care of refreshing the image, gathering inputs, resizing the image if the window is resized, etc. We therefore need to create a RenderObject, and add it to our Screen type

### Creating a RenderObject

To make a simple 2D triangle, we should have 3 2D coordinates. We specify these as a Dictionary with the keyword for the input variable to the shader. For our shader, this keyword should be "position". Our only other mandatory component is our shaders, which can be in the form of a lazy shader:

{% highlight julia %}

const triangle = std_renderobject(
	Dict{Symbol, Any}(
	:position => GLBuffer(Point2f0[(0.0, 0.5), (0.5, -0.5), (-0.5,-0.5)]),
	),
	LazyShader(vsh, fsh)
)

{% endhighlight %}

triangle is a RenderObject type. We can add it to an existing Screen the following way:

{% highlight julia %}

push!(window.renderlist,triangle)

{% endhighlight %}

### Visualizing a Render Object

Bringing it all together, we can use the following code to create a triangle, and view it in an OpenGL context:

{% highlight julia %}

using GLWindow, GLAbstraction, GeometryTypes

const window = Screen()

const vsh = vert"""
{{GLSL_VERSION}}
in vec2 position;
void main(){
	gl_Position = vec4(position, 0, 1.0);
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

renderloop(window)

{% endhighlight %}

Next:
[Manipulating Shapes](7_manipulating_shapes.html)

Previous:
[Shaders](5_shaders.html)
