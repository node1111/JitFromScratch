# JitFromScratch

Collection of examples from my talks in the [LLVM Social Berlin](https://www.meetup.com/de-DE/LLVM-Social-Berlin/) and [C++ User Group Berlin](https://www.meetup.com/de-DE/berlincplusplus/) that implement various aspects of a JIT compiler based on the LLVM Orc libraries.

## Contribute

The repository follows a perfect history policy to foster traceability and understanding. If you find a bug or want to submit an improvement, please send a pull request against the [respective step](
https://github.com/weliveindetail/JitFromScratch/branches/all?query=steps).

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

This is achieved in a series of self-contained steps:

0. [Add sources with original integerDistances function](https://github.com/weliveindetail/JitFromScratch/tree/step/A00)
1. [Add Dockerfiles and TravisCI config](https://github.com/weliveindetail/JitFromScratch/tree/step/A01)
2. [Add lit tests](https://github.com/weliveindetail/JitFromScratch/tree/step/A02)
3. [Initialize LLVM for native target codegen](https://github.com/weliveindetail/JitFromScratch/tree/step/A03)
4. [Add minimal JIT compiler based on LLJIT](https://github.com/weliveindetail/JitFromScratch/tree/step/A04)
5. [Create empty module and pass it to the JIT](https://github.com/weliveindetail/JitFromScratch/tree/step/A05)
6. [In debug mode dump extra info when passing -debug -debug-only=jitfromscratch](https://github.com/weliveindetail/JitFromScratch/tree/step/A06)
7. [Generate function that takes two ints and returns zero](https://github.com/weliveindetail/JitFromScratch/tree/step/A07)
8. [Add basic sanity checks for IR code](https://github.com/weliveindetail/JitFromScratch/tree/step/A08)
9. [Request and run trivial function](https://github.com/weliveindetail/JitFromScratch/tree/step/A09)
10. [Emit IR code for substraction and use it in the integerDistances function](https://github.com/weliveindetail/JitFromScratch/tree/step/A10)
11. [Emit variable names to make IR more readable](https://github.com/weliveindetail/JitFromScratch/tree/step/A11)
12. [Allow symbol resolution from the host process](https://github.com/weliveindetail/JitFromScratch/tree/step/A12)
13. [Emit call to stdlib function abs in JITed code](https://github.com/weliveindetail/JitFromScratch/tree/step/A13)
14. [Emit unrolled loop in JITed code](https://github.com/weliveindetail/JitFromScratch/tree/step/A14)
15. [Emit call to allocator in JITed code](https://github.com/weliveindetail/JitFromScratch/tree/step/A15)
16. [Remove wrapper function and call JITed code directly](https://github.com/weliveindetail/JitFromScratch/tree/step/A16)

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
