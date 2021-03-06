# Assignment 1

*due on 18 November 2020*

In this first assignment your task is to create a version control system (VCS) similar to [Git](https://git-scm.com/) or [Mercurial](https://www.mercurial-scm.org/).
However, this VCS will be much simpler.

For this assignment you are *not* allowed to work in teams.
You are *not* allowed to use code from other people participating in this course.
Always state the origin of non-original code and ideas.

It is recommend to be familiar with Git to understand the intensions behind the following commands.

## `lit`

All functionality of your VCS is bundled into one executable named `lit`.
Similar to `git`, it provides multiple *sub-commands* like `checkout` and `status`.
These sub-commands are always provided as first argument to `lit` and explained below.

### `lit help`

Displays a concise usage information.
Each sub-command is listed and described briefly.

### `lit init`

To initialize a repository run `lit init` inside some directory.
This directory is now the root of the repository.
Additionally, the VCS creates a new folder `.lit` containing all internal data.

All files of the directory are tracked.
There is no need to explicitly add them as you'd do with Git.

### `lit status`

Lists all files that have been added, removed, or modified, with respect to the currently checked out commit.

Keep the output concise, similar to `git status --short`.

### `lit commit`

Creates a new commit containing *all* changes.
In contrast to Git, there is no staging area.

A commit is comprised of a unique identifier, the identifier of the parent commit(s), a timestamp, a message, and the recorded changes.
The message is provided to the `commit` sub-command as additional argument.

Use a counter (starting from 0) prefixed with `r` (for revision) as unique identifier.

```
$ lit commit 'Add coin operated self-destruct feature'
Commit: r42
Date: Mon Sep 28 23:27:53 CEST 2020
```

### `lit show`

This sub-command is used to inspect the given commit.
If no commit is specified, display the currently checked out one.

```
$ lit show r42
Commit: r42
Parents: r41
Date: Mon Sep 28 23:27:53 CEST 2020

Add coin operated self-destruct feature

--- a/main.c
+++ b/main.c
@@ -1,8 +1,19 @@
 #include <stdio.h>
 #include <stdlib.h>

+#include "ship_systems/propulsion/shaw_fujikawa.h"
+
+#include "utils/coin_operator.h"
+
 int main(void)
 {
     puts("Hello World");
+
+    if (coin_operator_triggered()) {
+        puts("Have a nice day (> . =)");
+        prop_engine_t *engine = prop_ftl_get_shaw_fujikawa();
+        prop_detonate(engine);
+    }
+
     return EXIT_SUCCESS;
 }
```

### `lit checkout`

This resets the state of all files to the given commit's state.
All un-committed changes are dropped upon checkout.

If no commit is specified, reset the current state to the currently checked out commit.

You can add new branches by first checking out a previous commit, and then creating a new commit.

### `lit merge`

This command initiates a merge with the currently checked out commit and the specified commit.
A merge is only initiated if there are no un-committed changes.

Auto-merging files that have been modified in both branches is not supported.
If a file has been modified (or created) in both branches, the whole file is treated as a conflict.
If a file has been deleted in both branches you may treat it as no conflict.

If a conflict is encountered, stop the merge process and provide the respective files of the other branch as well as the common base.

```
$ lit merge r21
Merge conflict(s) detected:
    - robot.c

$ ls
robot.c       # currently checked out version.
robot.c.r21   # version of the other branch
robot.c.r18   # common base of both branches

$ # Manually resolving the merge conflict:
$ vimdiff robot.c robot.c.r21 robot.c.r18

$ # Cleanup
$ rm robot.c.r21 robot.c.r18

$ lit commit 'Merge r21 into r20'
```

To complete the merge after manually resolving a conflict, invoke the `commit` sub-command.
To abort the merge, use `checkout`.

If no conflict is encountered, a merge commit is automatically created.

### `lit log`

Displays a graph of all commits, one line per commit.
The currently checked out commit is highlighted.

You don't have to follow the exact format of this example:

```
$ lit log
o??????  ??? r3 "Merge r2 with r1"
??? ???o   r2 "Add chocolate egg dispenser"
o  ???   r1 "Add coin operated self destruct feature"
o?????????   r0 "First Commit"
```

## Implementation

If you encounter a problem where the specification is ambiguous or unclear, make a justifiable decision on what to do.

You are only allowed to use:
- C++ standard library (C++17 standard)
- C standard library (as fallback)
- GNU/Linux operating system's C APIs (POSIX, `mmap`, memfd, eventfd, ???)
- `diff` / `patch` command

You must use [CMake](https://cmake.org/) as build system.

Use [ClangFormat](https://clang.llvm.org/docs/ClangFormat.html) to automatically format your code using the provided [`.clang-format`](../.clang-format) configuration.
There is probably a plugin for your text editor / IDE to automate this process.

Your VCS must store the differences (as reported by `diff -u`).
Storing a full copy of the repository's files for each commit is not allowed.
You don't need to track empty folders, yet files located within folders.

You may treat empty files as being deleted.

You may assume that `lit` is always executed from the root of a repository.

You may assume that only one instance of your VCS operates on a repository at any point in time.
Hence, you don't need to add some form of locking mechanism to prevent concurrent access.

The executable does not depend on any additional resources (except for standard libraries, `diff` and `patch`).
You may assume that `diff` and `patch` are present and available via `PATH`.

**Hint:** It may be a good idea to create a dedicated class for invoking shell commands like `diff` and `patch`.
Simply using `system(3)` may not give you enough control over the process as you also need to interact with `stdin` / `stdout`.
Consider using `popen(3)` (or even `fork(2)`, `exec(2)`, `pipe(2)`, and `dup2(2)`) and getting some inspiration from [Go's `exec` package](https://golang.org/pkg/os/exec/) or [Python's `subprocess` module](https://docs.python.org/3/library/subprocess.html).

## Testing

Along with this specification a rudimentary test script [`lit-test`](lit-test) is provided, have a look at it.
It assumes that the `lit` executable is in your path.
Remember that you can use `bash -x` to debug the test script.

You are encouraged to set up some more test cases for your implementation.

## Evaluation (9 points)

- (3) Can create new commits and checkout old ones (no branches)
- (2) Can create new branches and switch between branches
- (1) Can inspect a commit using `lit show`
- (1) Can merge two branches (no conflict)
- (1) Can merge two branches (with conflict)
- (1) Can display a graph with all commits and their relationships

## Submission

Assuming you are using Git to manage your code, please use the `git archive` command to package your project.
Use the following command, replacing `firstname`, `lastname` accordingly (all lowercase).

    git archive --prefix=lastname_firstname_assignment_1/ --format=zip HEAD > lastname_firstname_assignment_1.zip

Verify that the packaged version is working and send it using the following link:

???? [send email](mailto:alexander.hirsch@uibk.ac.at?subject=703807%20-%20Assignment%201)
