This will serve as a private repository for a Vulkan API template. Currently it just has the rudimentary systems set up. All it does is render a basic square. This is just for me personally to use, so I don't have to set up Vulkan every time I want to do a project with it. I do not recommend you use this, if you want a good Vulkan template there are probably way better templates out there. This was created using the tutorial at [vulkan-tutorial](https://vulkan-tutorial.com/Introduction)

Currently it only supports Linux I think, I don't know I'd need to test it. If it doesn't, I plan to make it compatible for Windows as well. You could probably do it yourself by editing CMakeLists.txt or CMakePresets.json if you really care about it.

![image](/square.png)

# Requirements
- CMake
- Make

# Build Instructions
Open a terminal in the root project folder
Type the commands, 
> mkdir build
>
> cd build
>
> cmake ..
>
> make