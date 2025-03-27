# c3-debugging

## C3 debugging in vscode on Linux and Mac

Manually add the breakpoints at the bottom of the debug window (see bottom left of screenshot), unfortunately I have not figured out how to do so via the side of the editor window

## Manually adding breakpoints supports: 
- File and Line based breakpoints: `file_name.c3:line_number` for example: `main.c3:5`
- Package and function based breakpoints: `package_name.function_name` for example `my_library.example_fn`

<img src="https://github.com/user-attachments/assets/e393e8c4-9c9b-46c5-81c1-ad51fa80f73b" alt="Alt Text" height="700">





## Linux with GDB

### Download VSCode extension for debugging support
VSCode extension used: https://marketplace.visualstudio.com/items?itemName=webfreak.debug


### Add the following directories and files
Add file: `.vscode/launch.json`

File content:
```json5
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            // Can get it to stop at entry, but that's it
            // manually add breakpoints at the bottom of the debug pannel
            "name": "gdb debug",
            "type": "gdb",
            "request": "launch",
            "target": "${workspaceRoot}/build/your_project_exe_name",
            "cwd": "${workspaceRoot}",
            "valuesFormatting": "parseText",
            "preLaunchTask": "c3cbuild",
            "stopAtEntry": true,
        }
    ]
}
```

Add file `.vscode/tasks.json`

File content:
```json5
{
    "version": "2.0.0",
    "type":"shell",
    "tasks": [
        {
            "type": "shell",
            "command": "c3c",
            "args": ["build"],
            "label": "c3cbuild"
        }
    ]
}
```


## Linux with GDB and C++ dev tools

### Download VSCode extension for debugging support
VSCode extension used: https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools


### Add the following directories and files
Add file: `.vscode/launch.json`

File content:
```json5
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "gdb with C++ devtools",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/your_project_exe_name",
            "args": [],
            "cwd": "${workspaceFolder}/build",
            "environment": [],
            "MIMode": "gdb",
            "miDebuggerPath": "/usr/bin/gdb",
            "setupCommands": [
                {
                    "text": "-enable-pretty-printing",
                }
            ],
            "preLaunchTask": "c3cbuild",
            "stopAtEntry": true,
        }
    ]
}
```

Add file `.vscode/tasks.json`

File content:
```json5
{
    "version": "2.0.0",
    "type":"shell",
    "tasks": [
        {
            "type": "shell",
            "command": "c3c",
            "args": ["build"],
            "label": "c3cbuild"
        }
    ]
}
```


## Linux and possibly MacOS with LLVM using DAP

### Download VSCode extension for debugging support
VSCode extension used: https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.lldb-dap


### Add the following directories and files
Add file: `.vscode/launch.json`

File content:
```json5
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        // On ubuntu: sudo apt-get install llvm llvm-18 llvm-18-dev lldb lldb-18 llvm-18-tools
        // add to your path /usr/lib/llvm-18/bin/lldb-dap
        // I added this to my .bashrc file
        // export PATH="/usr/lib/llvm-18/bin:$PATH"
        //
        // On Mac I believe llvm tools are already installed
        {
            "name": "lldb dap Debug",
            "type": "lldb-dap",
            "request": "launch",
            "program": "${workspaceFolder}/build/your_project_exe_name",
            // "args": [ "one", "two", "three" ],
            // "env": {
            //    "FOO": "1"
            //    "BAR": ""
            // }
            "preLaunchTask": "c3cbuild",
            "stopOnEntry": true
        }
    ]
}
```

Add file `.vscode/tasks.json`

File content:
```json5
{
    "version": "2.0.0",
    "type":"shell",
    "tasks": [
        {
            "type": "shell",
            "command": "c3c",
            "args": ["build"],
            "label": "c3cbuild"
        }
    ]
}
```


## Debugging on NVIM

- plugins/debuggin.lua: https://github.com/BWindey/nvim-config/blob/main/lua/plugins/debugging.lua
- plugins/adapters/lldb.lua: https://github.com/BWindey/nvim-config/blob/main/lua/plugins/adapters/lldb.lua


## Debugging on Windows
- VSCode extension used: https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools
- Follow the install instructions and run the compiler at least once following these instructions
- https://code.visualstudio.com/docs/cpp/config-msvc



### Add the following directories and files
Add file: `.vscode/launch.json`

File content:
```json5
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "MSVC debugger for C3",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${workspaceFolder}\\build\\project_name_here_to_replace.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "preLaunchTask": "c3cbuild",
        }
    ]
}
```

Add file `.vscode/tasks.json`

File content:
```json5
{
    "version": "2.0.0",
    "type":"shell",
    "tasks": [
        {
            "type": "shell",
            "command": "c3c",
            "args": ["build"],
            "label": "c3cbuild"
        }
    ]
}
```
