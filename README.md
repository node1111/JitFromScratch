# JitFromScratch

[![Build Status](https://travis-ci.org/weliveindetail/JitFromScratch.svg?branch=master)](https://travis-ci.org/weliveindetail/JitFromScratch/branches/)

Collection of examples from my talks in the [LLVM Social Berlin](https://www.meetup.com/de-DE/LLVM-Social-Berlin/) and [C++ User Group Berlin](https://www.meetup.com/de-DE/berlincplusplus/) that implement various aspects of a JIT compiler based on the LLVM Orc libraries.

## Contribute

The repository follows a perfect history policy to foster traceability and understanding. If you find a bug or want to submit an improvement, please send a pull request against the [respective step](
https://github.com/weliveindetail/JitFromScratch/branches/all?query=step).

## Structure

The examples are organized around a nonsense command line program that compiles the code for a simple function at runtime:

```
template <size_t sizeOfArray>
int *integerDistances(const int (&x)[sizeOfArray], int *y) {
  int items = arrayElements(x);
  int *results = customIntAllocator(items);

  for (int i = 0; i < items; i++) {
    results[i] = abs(x[i] - y[i]);
  }

  return results;
}
```

## Steps

The example project is built in a series of self-contained steps:

0. [Add sources with original integerDistances function](https://github.com/weliveindetail/JitFromScratch/commit/1e459e7)
1. [Add lit tests](https://github.com/weliveindetail/JitFromScratch/commit/822acce)
2. [Add Dockerfiles to run typical use-cases on Ubuntu 18.04](https://github.com/weliveindetail/JitFromScratch/commit/bafa17e)
3. [Add TravisCI config for test-debug docker](https://github.com/weliveindetail/JitFromScratch/commit/81add37)
4. [Initialize LLVM for native target codegen](https://github.com/weliveindetail/JitFromScratch/commit/a57937b)
5. [Add minimal JIT compiler based on LLJIT](https://github.com/weliveindetail/JitFromScratch/commit/a61a3a9)
6. [Create empty module and pass it to the JIT](https://github.com/weliveindetail/JitFromScratch/commit/8c8318b)
7. [In debug mode dump extra info when passing -debug -debug-only=jitfromscratch](https://github.com/weliveindetail/JitFromScratch/commit/0199b52)
8. [Generate function that takes two ints and returns zero](https://github.com/weliveindetail/JitFromScratch/commit/2396b70)
9. [Add basic sanity checks for IR code](https://github.com/weliveindetail/JitFromScratch/commit/cdf832a)
10. [Request and run trivial function](https://github.com/weliveindetail/JitFromScratch/commit/c1ad3e5)
11. [Emit IR code for substraction and use it in the integerDistances function](https://github.com/weliveindetail/JitFromScratch/commit/8611f68)
12. [Emit variable names to make IR more readable](https://github.com/weliveindetail/JitFromScratch/commit/9c10621)
13. [Allow symbol resolution from the host process](https://github.com/weliveindetail/JitFromScratch/commit/a87601e)
14. [Emit call to stdlib function abs in JITed code](https://github.com/weliveindetail/JitFromScratch/commit/7a87c5c)
15. [Emit unrolled loop in JITed code](https://github.com/weliveindetail/JitFromScratch/commit/ce36196)
16. [Emit call to allocator in JITed code](https://github.com/weliveindetail/JitFromScratch/commit/fc86ecb)
17. [Remove wrapper function and call JITed code directly](https://github.com/weliveindetail/JitFromScratch/commit/44a67bb)
18. [Break free from LLJIT](https://github.com/weliveindetail/JitFromScratch/commit/b5bcb5e)
19. [Implement GDB JIT interface](https://github.com/weliveindetail/JitFromScratch/commit/09bb4cc)
20. [Add optimization passes controlled via -Ox command line flag](https://github.com/weliveindetail/JitFromScratch/commit/377e50a)
21. [Simple name-based object cache](https://github.com/weliveindetail/JitFromScratch/commit/5d131f7)
22. [Query cache upfront](https://github.com/weliveindetail/JitFromScratch/commit/4ec55cf)

## Build and run locally

```
$ git clone https://github.com/weliveindetail/JitFromScratch jitfromscratch
$ mkdir build && cd build
$ cmake -GNinja -DLLVM_DIR=/path/to/llvm-build/lib/cmake/llvm ../jitfromscratch
$ ninja JitFromScratch
$ ./JitFromScratch -debug -debug-only=jitfromscratch
```
The project was tested on Ubuntu 18.04, macOS 10.14 and Windows 10. Please find real-world examples for commands and expected output on [Travis CI](https://travis-ci.org/weliveindetail/JitFromScratch).

## Build and run in docker

Docker images with prebuilt variants of LLVM can be pulled from [dockerhub](https://cloud.docker.com/u/weliveindetail/repository/docker/weliveindetail/jitfromscratch) or created manually based on the Dockerfiles in [docker/](https://github.com/weliveindetail/JitFromScratch/tree/master/docker).

## Build and debug from vscode

Install the [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools), [
CMake Tools](https://marketplace.visualstudio.com/items?itemName=vector-of-bool.cmake-tools) and [Native Debug](https://marketplace.visualstudio.com/items?itemName=webfreak.debug) extensions and apply the configuration files in [docs/vscode/](https://github.com/weliveindetail/JitFromScratch/tree/master/docs/vscode).

## Previous Versions

I try to keep modifications on the examples minimal when porting to a newer version of LLVM. This allows to [diff for API changes](https://github.com/weliveindetail/JitFromScratch/tree/master/llvm50#previous-versions) and see how they affect individual steps.

In LLVM 8, however, the Orc library was almost entirely rewritten and I used the opportunity to also refine my examples.

You can still find the old examples here:

* [LLVM 5.0](https://github.com/weliveindetail/JitFromScratch/tree/master/llvm50)
* [LLVM 4.0](https://github.com/weliveindetail/JitFromScratch/tree/master/llvm40)
