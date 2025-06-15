This will serve as a private repository for a Vulkan API template. Currently it just has the rudimentary systems set up. All it does is render a spinning plane. This is just for me personally to use, so I don't have to set up Vulkan every time I want to do a project with it. I do not recommend you use this, if you want a good Vulkan template there are probably way better templates out there. This was created using the tutorial at [vulkan-tutorial](https://vulkan-tutorial.com/Introduction)

SHOULD support Linux and Windows

![image](/spinningSquare.png)

# Requirements
Ones in question marks are ones I'm not sure about because I haven't tried building without them but I am 90% you'll need them
- CMake
- Vulkan (?)
- GLFW (?)
- Ninja (Windows)

# Build Instructions
## Building on Windows
UNTESTED
Open a terminal in the root project folder
Type the commands,
> mkdir build
>
> cd build
>
> cmake ..
>
> ninja


## Building on Linux
Open a terminal in the root project folder
Type the commands,
> mkdir build
>
> cd build
>
> cmake ..
>
> make