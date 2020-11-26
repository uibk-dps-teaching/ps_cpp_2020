# Feedback

- Use an auto formatting tool!!11!
    - Reading inconsistently formatted code gets cumbersome over long periods of time.
    - Prefer tabs for indentation, see <https://www.reddit.com/r/javascript/comments/c8drjo>.
    - Personally I prefer a tab-size ≥ 4.

- Check return values!!! (especially `system()`)
    - I hope I don't have to explain why.
    - The exit-code of an application is often used to indicate failure.
    - Your application should also adhere to this.

- Remove commented-out code.
    - You are probably using a version control system anyway.
    - At least keep them out of the final submission.

- Avoid putting lots of logic in header files.
    - You have to do this often enough when using templates.
    - It negatively impacts your development cycle; changes to a header usually cause a large number of translation units to get re-compiled.
    - You loose some benefits of shared libraries.

- Avoid overusing libraries.
    - By introducing *library boundaries* to your code, you create additional *interfaces* that may require additional effort to maintain.
      As always, ensure this is worth the tradeoff.
    - Look at typical open-source libraries to get a feeling for how big a typical library actually is.
    - For the size of Boost, this makes sense; for `lit` sub-commands, not so much.

- Avoid classes with only static member functions.
    - While possible, this is kind of an anti-pattern in C++.
    - Use namespaces instead.

- Nesting depth / length of functions.
    - Keeping functions short and linear enhances readability of your code a lot.
    - See [Linux Kernel Coding Style](https://www.kernel.org/doc/html/v4.10/process/coding-style.html#functions).

- Naming conventions.
    - Consider [Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#nl-naming-and-layout-rules).
    - Consider [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Naming)
    - Adhere to what-ever style a code-base is already using.
    - Here my personal conventions for reference:
        - Types: `PascalCase`
        - Functions: `camelCase`
        - Constants: `ALL_CAPS`
        - Variables: `camelCase`
        - Function-like macros: `camelCase`
        - Other macros: `ALL_CAPS`
        - Namespaces: `snail_case`
            - Trying to avoid underscores
        - `enum class` members: `PascalCase`
        - `enum` members: `ALL_CAPS`
            - Prefixed with enum name (e.g. `PRESENT_MODE_IMMEDIATE`, `PRESENT_MODE_MAILBOX`)
        - Private member variables prefixed with `m_`
            - This allows getters to be named without the `get` prefix (e.g. `Vec3 position() const;`).
            - Setters get prefixed with `set` (e.g. `void setPosition(const Vec3 &);`).

- Leverage RAII!
    - No need for `.close()` or alike with classes like `std::ifstream`.
    - Consider wrapping non-RAII objects with dedicated classes or `std::unique_ptr` with a custom deleter.

- Do not use `.h` for C++ header files.
    - C++ header files typically cannot be used by C projects, this should be highlighted by the file extension.
    - Only use `.h` if that header is meant to be used in C projects.
    - I am aware of [the corresponding core guideline](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#sf1-use-a-cpp-suffix-for-code-files-and-h-for-interface-files-if-your-project-doesnt-already-follow-another-convention).
    - Personally, I prefer `.hpp` and `.cpp`.

- Include order
    - See [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Names_and_Order_of_Includes).
    - Only use `< >` for system-wide installed libraries, use `" "` otherwise.

- Member order
    - See [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Declaration_Order).
    - Show the public interface first.
    - Be consistent across the code-base.

- Header guards
    - See [Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#sf8-use-include-guards-for-all-h-files)
    - Personally, I am okay with `#pragma once` but the argument of *not being standard conform* stands.
    - A header guard should span the *entire* file (maybe except the license header).

- `-Werror`
    - Do not add this by default.
    - Newer compiler versions my emit warnings, causing builds to break.
    - This can potentially impact other people.
    - Only have this enabled on continuous integration to enforce warning-free code on contributions.

- Unused parameters
    - In C++ just omit the name; no more warning.

- Use namespaces!
    - Do not prefix symbols with the project name as you'd do in C.
    - Prefer at least one top-level namespace (e.g. `lit`).
    - Do not hesitate adding sub-namespaces (`detail` is rather common for in-file utility functions).
    - Namespaces commonly model the directory structure of a project.

- Avoid `using namespace` inside header files.
    - I've already talked about this on stream.
    - Personally, I avoid `using namespace` inside headers.
    - Be considerate.
    - Keep namespace aliases inside your namespace.
        - `namespace fs = std::filesystem;` is fine in a header, but not in global scope.

- Use exceptions as error reporting for library functions.
    - A library function should never terminate the whole application.
    - Throw a custom error indicating failure, or resort to `std::runtime_error`.

- Libraries should not directly use `stdin` / `stdout` / `stderr`.
    - Input / output should always be done via function arguments / return value.
    - Use some configurable logging mechanism.

- Use `enum class` over `enum`, over preprocessor define.
    - `enum class` gets type-checked and is therefore preferred.
    - `enum` is still valid as it allows you to combine multiple members out-of-the-box (flag enum).
    - Do not use preprocessor defines as alternatives to enums.
        - Modern compilers emit debug symbols and warnings for enums, but not for preprocessor defines.

- Avoid `std::endl`.
    - Just prefer `\n`.
    - `std::endl` also flushes the stream which can cause performance issues.

- Avoid low-level functions like `strcmp`.
    - Usually it's better to convert your data to *better typed* C++ objects (e.g. `std::string`)
        - With `std::string` you can just write `myString == "foo"`.
    - Only use low-level functions directly when encountering performance issues.

- Consider `std::string_view`.
    - This can prevent unnecessary conversions to `std::string` from C strings.
    - Take `std::string_view` by value.
    - Only use `std::string_view` as function argument.
    - String may *not* be null-terminated!

- Do not use symbols prefixed with `_` or `__` (e.g. `std::__cxx11::string`).
    - Two underscores are reserved for compiler / standard library internals.
        - These may differ between compiler and versions; therefore, do not use them.
    - Symbols with a single underscore as prefix are to be viewed as *private* or *internal*.
        - Do not use this pattern yourself, it's often unnecessary.
        - Putting stuff into a `detail` or anonymous namespace is the way to go.

- Use Out-of-source builds!
    - Do not pollute your source files with build artifacts.
    - Out-of-source builds allow you to have different build configurations side-by-side.
    - Build folders should not be tracked by your version control system (use `.gitignore`).

## Project structure

### Single Library

This structure is preferred for projects that focus around a single library.
There can be multiple executables which use this library.

```
.
├── apps/                   # All executables go here, usually one .cpp file per executable.
│   ├── myapp1.cpp
│   └── myapp2.cpp
├── cmake/                  # Contains all the project specific CMake modules.
├── docs/                   # Handwritten documentation goes here.
├── include/                # This includes all public headers defining the interface of the library.
│   └── mylib/              # If there is more than one header file, put them in a folder with the library's name.
│       ├── bar.hpp
│       └── foo.hpp
├── src/                    # Contains all sources (and private headers) of the library.
│   ├── bar.cpp
│   ├── foo.cpp
│   └── utils.hpp
├── tests/                  # Commonly contains unit tests. Adjust structure as needed.
│   ├── bar_test.cpp
│   └── foo_test.cpp
├── vendor/                 # All third party libraries that are maintained along with this project go here.
└── CMakeLists.txt
```

Separating the public headers clarifies which elements are part of the library's interface and which are implementation details.
Furthermore, upon installing the library, we can simply copy the contents of `include` to the corresponding installation target.

## Multiple Libraries

This is very similar to the single library case. Just replicate `include`, `src`, `tests` for each library.

```
.
├── apps/
│   ├── myapp1.cpp
│   └── myapp2.cpp
├── cmake/
├── docs/
├── mylib1/
│   ├── include/
│   │   └── mylib1/
│   ├── src/
│   └── tests/              # Contains only tests specific to mylib1.
├── mylib2/
│   ├── include/
│   │   └── mylib2/
│   ├── src/
│   └── tests/
├── tests/                  # Contains tests that utilize more than one library.
├── vendor/
└── CMakeLists.txt
```

## Framework

Sometimes the library approach with its clear separation between public interface and implementation details does not fit the project.
Yet you may still keep some form of separation between components.

For creating a video game, a common approach is to divide code into *game specific* code and *engine* code.
Engine code deals with low-level elements and provides (often generic) tools, while the game specific code typically covers one dedicated use-case.
Building the engine as a library is certainly a possibility, but I suggest keeping the border fuzzy and allowing game code to handle implementation details.

```
.
├── cmake/
├── docs/
├── myapp/                 # Always use dedicated folders here. With this approach applications get bigger quickly.
│   └── myapp.cpp
├── myframework/
│   ├── bar.cpp
│   ├── bar.hpp
│   ├── foo.cpp
│   └── foo.hpp
├── tests/
│   ├── bar_test.cpp
│   └── foo_test.cpp
├── vendor/
└── CMakeLists.txt
```

Essentially we ditch the `include` directory and just keep headers next to the source files.
For simplicity we can use the whole repository as include path, giving all translation units access to everything.
This is a very open approach that may only fit some projects.

You'd then use `#include "myframework/foo.hpp"` for instance.

You may of course build more than just one app; yet, the focus lies on the combination of app and framework.

Consider watching [John Romero's talk at 2016 GDC Europe](https://www.youtube.com/watch?v=E2MIpi8pIvY) regarding some paradigms to follow during development.
