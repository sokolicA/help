# C++ configuration for VSCode IDE

### 0.1 Install VSCode

View Guide
winget install -e --id Microsoft.VisualStudioCode

### 0.2 Install MinGW64

View Msys2 and MinGW64 guide to install,

## 1. Install Microsoft C/C++ Extension

## 2. Edit C/C++ configurations

(f1 or ctrl+shift+p) (either UI or JSON):

- Change compiler from gcc (c compiler) to g++ (c++ compiler)
- Set C++ Standard : gnu++17
- Ctrl + S to save

The `c_cpp_properties.json` file should look like this:

```
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "C:/msys64/mingw64/bin/g++.exe",
            "cStandard": "gnu17",
            "cppStandard": "gnu++17",
            "intelliSenseMode": "windows-gcc-x64"
        }
    ],
    "version": 4
}
```

## 3. Configure build tasks

See g++ --help or [this link](https://www.cs.bu.edu/fac/gkollios/cs113/Usingg++.html) for information about arguments.

For example:

i) -g (Compilation and link option): Put debugging information for gdb into the object or executable file. Should be specified for both compilation and linking.

ii) -o file-name (Link option, usually): Use file-name as the name of the file produced by g++ (usually, this is an executable file).

**Note:** if the label is not the same as the default label, vscode will create another task and assign preference to that task.

`tasks.json` file should look like this:

```
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "C/C++: g++.exe build active file",
      "type": "cppbuild",
      "command": "C:/msys64/mingw64/bin/g++.exe",
      "args": [
        "-fdiagnostics-color=always",
        "-g",
        "-Wall",
        "-std=c++17",
        "${fileDirname}\\*.cpp",
        "-o",
        "${fileDirname}\\${fileBasenameNoExtension}.exe"
      ],
      "options": {
        "cwd": "C:/msys64/mingw64/bin"
      },
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "problemMatcher": ["$gcc"],
      "detail": "compiler: \"C:/msys64/mingw64/bin/g++.exe\""
    }
  ]
}
```

## 4. Configure the launch.json file

Click on the .cpp file, go to Run > Add configuration

Modify the newly created `launch.json` as follows:

```
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "g++.exe - Build and debug active files",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${fileDirname}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "C:\\msys64\\mingw64\\bin\\gdb.exe",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "C/C++: g++.exe build active file"
    }
  ]
}

```

## 5. Build a task

**Note that VSCode builds the active file! Make sure you click on the file in the Explorer!**

Go to Terminal -> Run Build Task or press ctrl+shift+b. This will build the project folder into the .exe file.

Right click the .exe and select open in Integrated terminal
Type .\main.exe to run the file.

## 6. Debugging

Start debugging by either Run > Debug or F5 or the play button on the left vertical ribbon.

First you have to set breakpoints by clicking right to the row number. A red circle will appear and the debugger will stop before executing the line.
**Make sure to clear all other breakpoints** (if any remained from the previous debugging sessions).

Start debugging by clicking the green start button on the top of the sidebar (RUN AND DEBUG).
The execution will stop at the first breakpoint.

View variable values in the variables pane or by hovering over them in the script.

Then you can step in or step over to run the current line and go to the next line.

i) Step in: Will execute the current line and step into any other functions and files that are used in executing the code.

ii) Step over: Will execute the current line without going into details of every substep and will go to the next line.

Stop debugging by clicking the red circle or by clicking step over until the program ends.

## Use a project manager

Install the _Project Manager_ extension.

See [this link](https://github.com/alefragnani/vscode-project-manager/#project-manager) for more information.

But basically open a new folder, add the preset files and save the folder as a project by opening the Project Manager extension.

