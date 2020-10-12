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
- [ncurses](https://invisible-island.net/ncurses/)
- [OpenAL](https://openal.org/)
- [nlohmann/json](https://github.com/nlohmann/json) / [RapidJSON](https://rapidjson.org/)
- [Assimp](https://www.assimp.org/)
- [sbt-image](https://github.com/nothings/stb/blob/master/stb_image.h)
- [Ogg](https://xiph.org/ogg/) / [Vorbis](https://xiph.org/vorbis/) / [Opus](https://opus-codec.org/)
- [Tiled](https://www.mapeditor.org/)
- [OpenSSL](https://www.openssl.org/)
- [SQLite](https://www.sqlite.org/)
- [Catch2](https://github.com/DigitalInBlue/Celero)
- [Google Test](https://github.com/google/googletest)
- [Google Benchmark](https://github.com/google/benchmark)
- [spdlog](https://github.com/gabime/spdlog) / [plog](https://github.com/SergiusTheBest/plog)
- [fmt](https://github.com/fmtlib/fmt)
- [AngelScript](https://www.angelcode.com/angelscript/)
- [Lua](http://www.lua.org/) / [sol2](https://github.com/ThePhD/sol2)
- [cereal](https://github.com/USCiLab/cereal)
- [protobuf](https://github.com/protocolbuffers/protobuf)
- [Font Chef](https://github.com/mobius3/font-chef)

Feel free to ask me about other libraries / tools.

Your application should work either on Linux (64-Bit) or Windows (64-Bit), preferably both unless there is a specific reason why it cannot be cross-platform.
For Linux, assume a recent version of Ubuntu Desktop and that the required dependencies are installed via the system's package manager.
Use the corresponding CMake `find_package` mechanism to find them.
Prefer [`pkgconf`](https://cmake.org/cmake/help/latest/module/FindPkgConfig.html) over custom *FindPackage* scripts.
For Windows you can simply ship pre-built libraries that are picked up by CMake automatically.

You must use [CMake](https://cmake.org/) as build system.

Use [ClangFormat](https://clang.llvm.org/docs/ClangFormat.html) to automatically format your code using the provided [`.clang-format`](../.clang-format) configuration.

## Team Composition + Specification

Send me an email with your team composition and your specification as early as possible.
Use the following link:

📧 [send email](mailto:alexander.hirsch@uibk.ac.at?subject=703807%20-%20Assignment%202%20Team%20Composition)

## Submission

### Packaging

Assuming you are using Git to manage your code, please use the `git archive` command to package your project.
Use the following command, replacing `XX` with your team number (with leading zero, e.g. `02`).

    git archive --prefix=team_XX_assignemnt_2/ --format=zip HEAD > team_XX_assignemnt_2.zip

### Build Test Submission

Submit a non-final version of your project around 2 weeks before the final deadline.
I will verify that your project builds on my test system(s) and let you know if I run into any issues.
Use the following link, again replacing `XX` with your team number.

📧 [send email](mailto:alexander.hirsch@uibk.ac.at?subject=703807%20-%20Team%20XX%20Assignment%202%20Build%20Test)


### Final Submission

Verify that the packaged version is working.
Use the following link, again replacing `XX` with your team number.

📧 [send email](mailto:alexander.hirsch@uibk.ac.at?subject=703807%20-%20Team%20XX%20Assignment%202%20Final)
