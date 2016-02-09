---
layout : page
title : Creating Windows
---

### Windows with GLFW.jl

OpenGL will require a window for any visualization to be rendered. There are numerous libraries available to make OpenGL windows; currently GLFW is supported in Julia. GLFW is pretty lightweight, and also cross platform. You can find its full documentation here: 

The GLFW.jl package is available at . Let's go through the first example:

{% highlight julia %}

import GLFW

# Create a window and its OpenGL context
window = GLFW.CreateWindow(640, 480, "GLFW.jl")

# Make the window's context current
GLFW.MakeContextCurrent(window)

# Loop until the user closes the window
while !GLFW.WindowShouldClose(window)

    	# Render here

    	# Swap front and back buffers
    	GLFW.SwapBuffers(window)

    	# Poll for and process events
    	GLFW.PollEvents()
end

{% endhighlight %}

Note what GLFW does and does not do: GLFW is responsible for creating the screen, keeping it open, and gathering events. GLFW does not actually draw anything, all of that is left to openGL.

The most unintuitive line for someone new to graphical programming is SwapBuffers. What does this do exactly? Multiple buffering is a technqiue that separates data that is being viewed by the user, with data that is being changed under the hood. So each iteration, your graphical drawing will not happen to the screen pixel by pixel, (as may cause artifacts as one part of the screen will be updated before another part), but instead will happen to a "back buffer" that will store all of the pixel values until drawing is completed, (slow step) and then GLFW will move this back buffer to the front so that it si displayed on the screen (fast step). So every time after you render with OpenGL, if you actually want to see what you have done, you need to swap buffers.

### Windows with GLWindow.jl

The above example is incredibly barebones. How do you integrate mouse events? How can a window be resized? How do you actually render onto the screen? The GLwindow package provides scaffolding for most of these operations in a Julian control flow. To create a window, and intiate a control loop, simply use:

{% highlight julia %}

using GLWindow

w=Screen()

renderloop(w)

{% endhighlight %}

A screen is our main data type that holds properties for the screen which lives in GLFW world and the objects that are rendered to it, that live in OpenGL world. renderloop will initiate a call to a loop that will continuously run until the window is closed, mcuh like the loop above. It will perform the GLFW functions of checking for events, swapping buffers, keeping the window open etc, while also providing the nitty-gritty function calls necessary for all OpenGL rendering like reseting pixel values, getting ready to draw the buffer, etc.

So if we are going to render anything, we need to make something to render.

Next:
[Rendering Components](3_render.html)

Previous:
[Introduction](1_intro.html)


