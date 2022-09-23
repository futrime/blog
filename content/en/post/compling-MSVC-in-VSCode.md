---
author: Futrime
title: Compiling C/C++ programs in VSCode with MSVC
description: Following the official tutorial, then you are to encounter a fatal error...
date: 2021-09-27T16:03:08+08:00
categories:
    - Logging
    - Experience
tags:
    - Visual Studio Code
    - VSCode
    - MSVC
    - compile
image: https://i.loli.net/2021/09/27/fpwJnYGVH6sD4Il.png
draft: false
---

Recently I'm attending my college's Computer Language and Programming Lesson, in which Visual Studio is required to use. However, for just a single file C program, Visual Studio resembles a steam engine used to crack nuts, not only wasting so many advanced features of it but also making the precedure much more complicated. But how to simplify it? I know you would say Visual Studio Code. That's what I'm discussing here. In this blog, I'm going to introduce how to configure VSCode for an easy compiling and debugging process.

In fact, Microsoft has already given us [an official tutorial](https://code.visualstudio.com/docs/cpp/config-msvc) on how to use Windows C++ in Visual Studio Code, yet if following it, you are bound to a fatal error. So did I.

![an image of the official tutorial](https://i.loli.net/2021/09/27/WvsH9czPFgypn5Y.png)

Fortunately, after struggling with the editor and the compiler for a whole morning, I ultimately grasp the key to make it. Here I'm presenting the detailed configuration procedure with Visual Studio 2019 (or the toolset of Visual Studio 2019).

In the first few steps, the tutorial gives us the correct guidance, until step [**Build helloworld.cpp**](https://code.visualstudio.com/docs/cpp/config-msvc#_build-helloworldcpp).

### The actual approach to configure Windows C++ (MSVC)
The configuration of environment variables comes first. Search **path** in the search box in your task bar and open **Edit environment variables for your account**.

![search PATH in the search box](https://i.loli.net/2021/09/27/bfT2hynP4wB3CsM.png)

In the settings window, select **Path** and then click **Edit...**. Then in the pop-up window click **New** to add this value:

```
C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Tools\MSVC\14.29.30133\bin\Hostx64\x64
```

Repeat this step to add another value:

```
C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\Tools\
```

Just like this:
![an example of the environment variables configuration](https://i.loli.net/2021/09/27/OnVulMABgHi6Emr.png)

Click **OK** and then click **OK** again. Restart your Visual Studio Code.

From the main menu, choose **Termial > Configure Default Build Task**, then choose **C/C++: cl.exe build active file** listed in the dropdown. This will create a `tasks.json` file in `.vscode` folder in the root of your project folder. Open it in the editor, replacing all content with belows and save it:

```json
{
	"version": "2.0.0",
	"windows": {
	  "options": {
		"shell": {
		  "executable": "cmd.exe",
		  "args": [
			"/C",
			"VsDevCmd.bat",
			"&&"
		  ]
		}
	  }
	},
	"tasks": [
	  {
		"type": "shell",
		"label": "cl.exe build active file",
		"command": "cl.exe",
		"args": [
		  "/Zi",
		  "/EHsc",
		  "/Fe:",
		  "${fileDirname}\\${fileBasenameNoExtension}.exe",
		  "${file}"
		],
		"problemMatcher": ["$msCompile"],
		"group": {
		  "kind": "build",
		  "isDefault": true
		}
	  }
	]
  }
```

Then return to your C source file, press **Ctrl + Shift + B**, and the program will be compiled.

There is a bit difference between my sample and the offical example of [running VS Code outside the Developer Command Prompt](https://code.visualstudio.com/docs/cpp/config-msvc#_run-vs-code-outside-the-developer-command-prompt): the second argument of **windows.options.shell.args** is no longer a full path. That's all due to a strange bug making the editor always to consider `C:/Program` a command.

The next thing to do is debugging. Press **F5** or **Ctrl + F5** and the editor will prompt a dropdown menu. Select **cl.exe build and debug active file** in it.

![a dropdown menu to choose debuggin configuration](https://i.loli.net/2021/09/27/gHCF5pkXtmUVPWi.png)

VS Code will create a `launch.json` in the identical folder `.vscode`. Open it and replace all the content with belows and save:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "PowerShell Launch Script",
            "type": "PowerShell",
            "request": "launch",
            "script": "VsDevCmd.bat",
            "cwd": "${workspaceFolder}"
        },
        {
            "name": "cl.exe - Build and debug active file",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "console": "externalTerminal"
        }
    ]
}
```

Then try to press **F5** or **Ctrl + F5**, and you will be in the debugging mode with pleasure. Now you can breathe a sigh of relief and enjoy a cup of coffee.

What's more, in the official tutorial exists a more recommended method of simple C/C++ code building using GCC via mingw-w64. I've attempted to set it but failed, though. That's another story.