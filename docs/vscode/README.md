# JitFromScratch

Collection of examples from my talks in the [LLVM Social Berlin](https://www.meetup.com/de-DE/LLVM-Social-Berlin/) and [C++ User Group Berlin](https://www.meetup.com/de-DE/berlincplusplus/) that implement various aspects of a JIT compiler based on the LLVM Orc libraries.

## Build and debug with vscode

* download, install and open [Visual Studio Code](https://code.visualstudio.com/download)
* press `Cmd + Shift + P`; type and run `Shell Command: Install 'code' command in PATH`
* install extensions [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools), [
CMake Tools](https://marketplace.visualstudio.com/items?itemName=vector-of-bool.cmake-tools) and [Native Debug](https://marketplace.visualstudio.com/items?itemName=webfreak.debug)
* checkout sources: `git clone https://github.com/weliveindetail/JitFromScratch jitfromscratch`
* copy the below configuration files into the `.vscode` folder: `cp -R jitfromscratch/docs/vscode/*.json jitfromscratch/.vscode/`
* enter your actual `LLVM_DIR` in `jitfromscratch/.vscode/cmake-variants.json`
* open the `jitfromscratch` folder in vscode `code .`
* if it doesn't start automatically, press `Cmd + Shift + P`; type and run `CMake: Scan for Kits`

### .vscode/settings.json
```
{
  "C_Cpp.default.configurationProvider": "vector-of-bool.cmake-tools",
  "cmake.preferredGenerators": [ "Ninja" ]
}
```

### .vscode/cmake-variants.json
```
{
  "LLVM": {
    "default": "LLVM 8.0",
    "choices": {
      "LLVM 8.0": {
        "short": "LLVM 8.0",
        "long": "LLVM Release 8.0",
        "settings": {
          "LLVM_DIR": "/path/to/llvm-build/lib/cmake/llvm"
        }
      }
    }
  },
  "buildType": {
    "default": "debug",
    "choices": {
      "debug": {
        "short": "Debug",
        "long": "Compile in debug mode",
        "buildType": "Debug",
        "linkage": "static"
      }
    }
  }
}
```

### .vscode/launch.json
```
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "LLDB",
      "type": "lldb-mi",
      "request": "launch",
      "target": "${workspaceRoot}/build/JitFromScratch",
      "arguments": "-debug -debug-only=jitfromscratch",
      "cwd": "${workspaceRoot}",
      "valuesFormatting": "prettyPrinters"
    },
    {
      "type": "gdb",
      "request": "attach",
      "name": "Docker",
      "executable": "${workspaceRoot}/build/JitFromScratch",
      "target": ":9999",
      "remote": true,
      "cwd": "${workspaceRoot}",
      "valuesFormatting": "parseText"
    }
  ]
}
```
