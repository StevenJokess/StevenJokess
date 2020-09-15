---
title: 'Powershell配置 和 Terminal配置'
date: 2020-07-01 21:25:40
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
效果图：![效果图](https://blog.dltech.xyz/post-images/1593611155593.png)
# Powershell
参考了：https://zhuanlan.zhihu.com/p/144610989
因为PowerShell默认禁止运行脚本，需要首先开启权限，管理员模式运行PowerShell
然后输入命令，并输入Y确认

`set-ExecutionPolicy RemoteSigned`

接着
安装了visual studio code并且添加了环境变量的可以直接在PowerShell中接着输入（推荐）

`code $profile`

```
cls  #清除微软广告

$path = $pwd.path
if ( $path.split("\")[-1] -eq "System32" ) {
    # change default path to desktop
    $desktop = "C:\Users\" + $env:UserName + "\Desktop\"
    cd $desktop
}

Set-PSReadLineOption -Colors @{
    Command             = "#e5c07b"
    Number              = "#cdd4d4"
    Member              = "#e06c75"
    Operator            = "#e06c75"
    Type                = "#78b6e9"
    Variable            = "#78b6e9"
    Parameter           = "#e06c75"  #命令行参数颜色
    ContinuationPrompt  = "#e06c75"
    Default             = "#cdd4d4"
    Emphasis            = "#e06c75"
    #Error
    Selection           = "#cdd4d4"
    Comment             = "#cdd4d4"
    Keyword             = "#e06c75"
    String              = "#78b6e9"
}

function prompt
{
    #Write-Host("$pwd>")
    $path = $pwd.path
    if ( -not $path.EndsWith("\") ) {
        "" + $path.split("\")[-1] + " > "
    }
    else {
        "" + $path.split("\")[0] + " > "
    }
}
···

---

# Terminal:
https://www.bilibili.com/read/cv4651123/
https://www.chuchur.com/article/windows-terminal-beautify

```
{
    "$schema": "https://aka.ms/terminal-profiles-schema",

    "defaultProfile": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",

    // You can add more global application settings here.
    // To learn more about global settings, visit https://aka.ms/terminal-global-settings

    // If enabled, selections are automatically copied to your clipboard.
    "copyOnSelect": false,

    // If enabled, formatted data is also copied to your clipboard
    "copyFormatting": false,

    // A profile specifies a command to execute paired with information about how it should look and feel.
    // Each one of them will appear in the 'New Tab' dropdown,
    //   and can be invoked from the commandline with `wt.exe -p xxx`
    // To learn more about profiles, visit https://aka.ms/terminal-profile-settings
    "profiles":
    {
        "defaults":
        {
            // Put settings here that you want to apply to all profiles.
            "acrylicOpacity": 0.8, //背景透明度
            "useAcrylic": true, // 启用毛玻璃
            "backgroundImage": "https://area.sinaapp.com/bingImg/", //bing每日
            "backgroundImageOpacity": 0.5//图片透明度
        },
        "list":
        [
            {
                // Make changes here to the powershell.exe profile.
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": false
            },
            {
                // Make changes here to the cmd.exe profile.
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "name": "Command Prompt",
                "commandline": "cmd.exe",
                "hidden": false
            },
            {
                "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                "hidden": false,
                "name": "Azure Cloud Shell",
                "source": "Windows.Terminal.Azure"
            }
        ]
    },

    // Add custom color schemes to this array.
    // To learn more about color schemes, visit https://aka.ms/terminal-color-schemes
    "schemes": [],

    // Add custom keybindings to this array.
    // To unbind a key combination from your defaults.json, set the command to "unbound".
    // To learn more about keybindings, visit https://aka.ms/terminal-keybindings
    "keybindings":
    [
        // Copy and paste are bound to Ctrl+Shift+C and Ctrl+Shift+V in your defaults.json.
        // These two lines additionally bind them to Ctrl+C and Ctrl+V.
        // To learn more about selection, visit https://aka.ms/terminal-selection
        { "command": {"action": "copy", "singleLine": false }, "keys": "ctrl+c" },
        { "command": "paste", "keys": "ctrl+v" },

        // Press Ctrl+Shift+F to open the search box
        { "command": "find", "keys": "ctrl+shift+f" },

        // Press Alt+Shift+D to open a new pane.
        // - "split": "auto" makes this pane open in the direction that provides the most surface area.
        // - "splitMode": "duplicate" makes the new pane use the focused pane's profile.
        // To learn more about panes, visit https://aka.ms/terminal-panes
        { "command": { "action": "splitPane", "split": "auto", "splitMode": "duplicate" }, "keys": "alt+shift+d" }
    ]
}
```
