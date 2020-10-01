# Assignment 2

*due on 19 February 2021*

For the second assignment you can either pick one of the two provided specifications or come up with your own.
Yes, you are free to come up with a topic for the second assignment.
However, doing so requires you to put together a specification similar to the ones provided and get my approval.
The specification doesn't have to be bulletproof.
But it must contain a bullet list of goals (with points to score) at the bottom which can be checked / evaluated.
You can also adjust one of the provided specifications.

You are allowed to work in teams, yet the team size has to correspond to the amount of work (features) of the topic.
For the provided specifications the recommended team size is 3.

You are allowed to use:
- C++ standard library (C++17 standard)
- C standard library (as fallback)
- [Boost](https://www.boost.org/)
- [SDL](https://www.libsdl.org/)
- [GLFW](https://www.glfw.org/)
- [GLM](https://glm.g-truc.net/)
- [Vulkan](https://www.khronos.org/vulkan/) / [Vulkan-Hpp](https://github.com/KhronosGroup/Vulkan-Hpp)
- [Qt](https://www.qt.io/)
- [ImGui](https://github.com/ocornut/imgui)
- [OpenAL](https://openal.org/)
- [RapidJSON](https://rapidjson.org/)
- [Tiled](https://www.mapeditor.org/)

Feel free to ask me about other libraries / tools.

Your application should work on Linux and Windows unless there is a specific reason why it cannot be cross-platform.
For Linux, assume a recent version of Ubuntu Desktop and that the required dependencies are installed via the system's package manager.
Use the corresponding CMake `find_package` mechanism to find them, it is recommended to leverage [`pkgconf`](https://cmake.org/cmake/help/latest/module/FindPkgConfig.html).
For Windows you can simply ship pre-built libraries that are picked up by CMake automatically.

You must use [CMake](https://cmake.org/) as build system.

Use [ClangFormat](https://clang.llvm.org/docs/ClangFormat.html) to automatically format your code using the provided [`.clang-format`](../.clang-format) configuration.

