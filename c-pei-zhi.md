---
title: 'C++ 配置'
date: 2020-07-01 20:07:42
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
```json
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
            "intelliSenseMode": "gcc-x64",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "compilerPath": "C:/mingw64/bin/g++.exe"
        }
    ],
    "version": 4
}
```
---

```json
{//task.json
	// See https://go.microsoft.com/fwlink/?LinkId=733558
	// for the documentation about the tasks.json format
	// https://code.visualstudio.com/docs/editor/tasks
	"version": "2.0.0",
	"tasks": [{
			"type": "shell",
			"label": "C/C++: g++.exe build active file", // 任务名称，与launch.json的preLaunchTask相对应
			"command": "g++",//要使用的编译器，c用gcc
			"args": [
				"${file}",
				"-o",// 指定输出文件名，不加该参数则默认输出a.exe，Linux下默认a.out
				"${fileDirname}/${fileBasenameNoExtension}.exe",
				"-g",    // 生成和调试有关的信息
				"-Wall", // 开启额外警告
				"-static-libgcc",     // 静态链接libgcc，一般都会加上
				"-fexec-charset=GBK", // 生成的程序使用GBK编码，不加这一条会导致Win下输出中文乱码
				// "-std=c11", // C++最新标准为c++17，或根据自己的需要进行修改
			], // 编译的命令，其实相当于VSC帮你在终端中输了这些东西
			"group": {
				"kind": "build",
				"isDefault": true // 不为true时ctrl shift B就要手动选择了
			},
			"presentation": {
				"echo": true,
				"reveal": "always", // 执行任务时是否跳转到终端面板，可以为always，silent，never。具体参见VSC的文档
				"focus": false,     // 设为true后可以使执行task时焦点聚集在终端，但对编译C/C++来说，设为true没有意义
				"panel": "shared"   // 不同的文件的编译信息共享一个终端面板
			},
			"problemMatcher": []
			//"type": "process", // process是vsc把预定义变量和转义解析后直接全部传给command；shell相当于先打开shell再输入命令，所以args还会经过shell再解析一遍
			// "problemMatcher":"$gcc" // 此选项可以捕捉编译时终端里的报错信息；但因为有Lint，再开这个可能有双重报

	}],
}

```

---

```json
{//launch.json
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    // https://github.com/Microsoft/vscode-cpptools/blob/master/launch.md
    "version": "0.2.0",
    "configurations": [{
            "name": "(gdb) Launch",// 配置名称，将会在启动配置的下拉菜单中显示
            "preLaunchTask": "C/C++: g++.exe build active file",//调试前执行的任务，就是之前配置的tasks.json中的label字段
            "type": "cppdbg",//配置类型，cppdbg对应cpptools提供的调试功能；可以认为此处只能是cppdbg
            "request": "launch",//请求配置类型，可以为launch（启动）或attach（附加）
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",//调试程序的路径名称
            "args": [],//程序调试时传递给程序的命令行参数，一般设为空即可
            "stopAtEntry": false,// 设为true时程序将暂停在程序入口处，相当于在main上打断点
            "cwd": "${workspaceFolder}",// 调试程序时的工作目录，此为工作区文件夹；改成${fileDirname}可变为文件所在目录
            "environment": [],// 环境变量
            "externalConsole": true,//true显示外置的控制台窗口，false显示内置终端
            "internalConsoleOptions": "neverOpen", // 如果不设为neverOpen，调试时会跳到“调试控制台”选项卡，你应该不需要对gdb手动输命令吧？
            "MIMode": "gdb",
            "miDebuggerPath": "gdb.exe",//调试器路径，Windows下后缀不能省略，Linux下则不要:https://github.com/microsoft/vscode-cpptools/issues/2778
            "setupCommands": [
                {// 模板自带，好像可以更好地显示STL容器的内容，具体作用自行Google
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }]
}
```

---

sftp.json
```json
{
    "name": "tfae.dtx",
    "protocol": "sftp",
    "host": "10.xxxxxxxx.22",
    "port": 36000,
    "username": "xxxxxxxx",
    "password": "xxxxxx",
    "uploadOnSave": true,
    "ignore": [
        "\\.VSCode",
        "\\.git",
        "\\.DS_Store",
        "\\.svn",
        "\\.history",
        "\\.IAB",
        "\\.IAD",
        "\\.IMB",
        "\\.IMD",
        "\\.PFI",
        "\\.PO",
        "\\.PR",
        "\\.PRI",
        "\\.PS",
        "\\.WK3"
    ],
    "remotePath": "/root/home/denniszhu/tarsCpp"
}
```
---