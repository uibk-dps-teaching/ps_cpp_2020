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

- Leverage RAII
    - No need for `.close()` or alike with classes like `std::ifstream`.
    - Consider wrapping non-RAII objects with dedicated classes or `std::unique_ptr` with a custom deleter.

- Do not use `.h` for C++ header files
    - C++ header files typically cannot be used by C projects, this should be highlighted by the file extension.
    - Only use `.h` if that header is meant to be used in C projects.
    - I am aware of [the corresponding core guideline](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#sf1-use-a-cpp-suffix-for-code-files-and-h-for-interface-files-if-your-project-doesnt-already-follow-another-convention)
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
    - Personally, I am okay with `#pragma once` but the argument of *not being standard confirm* stands.
    - A header guard should span the *entire* file (maybe except the license header).

- `-Werror`
    - Do not add this by default.
    - Newer compiler versions my emit warnings, causing builds to break.
    - This can potentially impact other people.
    - Only have this enabled on continuous integration to enforce warning-free code on contributions.

- Unused parameters
    - In C++ just omit the name; no more warning.

- Use namespaces
    - Do not prefix symbols with the project name as you'd do in C.
    - Prefer at least one top-level namespace (e.g. `lit`).
    - Do not hesitate adding sub-namespaces (`detail` is rather common for in-file utility functions).
    - Namespaces commonly model the directory structure of a project.

- Avoid `using namespace` inside header files
    - Already talked about this on stream.
    - Personally, I avoid `using namespace` inside headers.
    - Be considerate.
    - Keep namespace aliases inside your namespace.
        - `namespace fs = std::filesystem;` is fine in a header, but not in global scope.

- Exceptions as error reporting for library functions
    - A library function should never terminate the whole application.
    - Throw a custom error indicating failure, or resort to `std::runtime_error`.

- Use `enum class` over `enum`, over preprocessor define
    - `enum class` gets type-checked and is therefore preferred.
    - `enum` is still valid as it allows you to combine multiple members out-of-the-box (flag enum).
    - Do not use preprocessor defines as alternatives to enums.
        - Modern compilers emit debug symbols and warnings for enums, but not for preprocessor defines.

- Avoid `std::endl`
    - Just prefer `\n`.
    - `std::endl` also flushes the stream which can cause performance issues.

- Avoid low-level functions like `strcmp`
    - Usually it's better to convert your data to *better typed* C++ objects (e.g. `std::string`)
        - With `std::string` you can just write `myString == "foo"`.
    - Only use low-level functions directly when encountering performance issues.

- Consider `std::string_view`
    - This can prevent unnecessary conversion to `std::string` from C strings.
    - Take `std::string_view` by value.
    - Only use `std::string_view` as function argument.
    - String may *not* be null-terminated!

- Do not use symbols prefixed with `_` or `__` (e.g. `std::__cxx11::string`).
    - Two underscores are reserved for compiler / standard library internals.
        - These may differ between compiler and versions; therefore, do not use them.
    - Symbols with a single underscore as prefix are to be viewed as *private* or *internal*.
        - Do not use this pattern yourself, it's often unnecessary.
        - Putting stuff into a `detail` or `anonymous` namespace is the way to go.
