# openCV-Built-by-minGW


用minGW64和CMake编译openCV源码，得到编译结果(`.a .dll`)，方便VSCode使用g++编译器引入openCV库

## 文件说明

本仓库是本人的备份库，为了解决VSCode使用minGW(g++)编译器时遇到的问题。

openCV官方github提供了vc16的库（./opencv/build/x64/vc16/lib/`*.lib`），但是没有提供minGW的库。

所以，本仓库备份了本人用minGW64和CMake编译openCV源码得到的结果。本人是备份了编译之后install文件夹下的文件。

 环境：windows10、VSCode

 minGW版本：[x86_64-1310-win32-seh-ucrt-rt_v11-rev1](https://github.com/niXman/mingw-builds-binaries/releases/tag/13.1.0-rt_v11-rev1)

 openCV版本：[4.9.0](https://github.com/opencv/opencv/releases/tag/4.9.0)

 如何编译：[OpenCV使用CMake和MinGW-w64的编译安装 | HuiHut](https://blog.huihut.com/2018/07/31/CompiledOpenCVWithMinGW64/)

 HuiHut编译好的：[huihut/OpenCV-MinGW-Build(github.com)](https://github.com/huihut/OpenCV-MinGW-Build)

 建议自己编译。编译openCV源代码可以确保库在你的开发平台运行正常，这对于跨平台应用程序的开发很重要，因为不同平台二进制文件可能不兼容。编译时，可勾选特定的硬件支持，例如CUDA支持、openGL支持等。

## 如何使用

主要用到两个文件夹

```
./include
./x64/mingw/lib
```

.vscode的文件夹下配置文件

tasks.json文件：

```json
//tasks.json
{
  	// See https://stackoverflow.com/questions/51622111/opencv-c-mingw-vscode-fatal-error-to-compile/51801863#51801863
    // See https://gist.github.com/agtbaskara/4a2ec9a3a9a963069e719c0477185321
    "version": "2.0.0",
    "tasks": [
        {
          	//task标签
            "label": "build it",
            "type": "shell",
            "command": "g++",
            "args":[
              	//编译Source下所有.cpp文件
              	"${workspaceRoot}/Source/*.cpp",
                "-g",
                "-o", "main.exe",
              	//头文件目录
              	"-I", "${workspaceRoot}/Include",
              	//openCV头文件目录(.h .hpp)
                "-I", "D:/openCV-Built-by-minGW/include",
              	//openCV库路径(.a .dll)
                "-L", "D:/openCV-Built-by-minGW/x64/mingw/lib",
              	//openCV库 (前面加-l，不需要加后缀)   
                "-llibopencv_calib3d490",
                "-llibopencv_core490",
                "-llibopencv_dnn490",
                "-llibopencv_features2d490",
                "-llibopencv_flann490",
                "-llibopencv_gapi490",
                "-llibopencv_highgui490",
                "-llibopencv_imgcodecs490",
                "-llibopencv_imgproc490",
                "-llibopencv_ml490",
                "-llibopencv_objdetect490",
                "-llibopencv_photo490",
                "-llibopencv_stitching490",
                "-llibopencv_video490",
                "-llibopencv_videoio490"
            ],
          	"options":{
              	//生成文件放在这
              	"cwd": "${workspaceRoot}/Build"
            }
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
launch.json文件:

```json
//launch.json
{
    // See https://gist.github.com/agtbaskara/de04db8b6a31522dd1e62c43aa6e0f89
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
          	//对生产的文件进行调试
            "program": "${workspaceRoot}/Build/main.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceRoot}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
          	//minGW的gbd.exe路径
            "miDebuggerPath": "D:/mingw64/bin/gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": false
                }
            ],
          	//task标签(和tasks.json中一致)
            "preLaunchTask": "build it"
        }
    ]
}
```

c_cpp_properties.json文件:

```json
//c_cpp_properties.json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
            		//自己的头文件
                "${workspaceRoot}/Include",
                //openCV的头文件
                "D:/openCV-Built-by-minGW/include"
            ],
          	"browse": {
              	"path": [
                		//库文件
                  	"D:/openCV-Built-by-minGW/x64/minge/lib"
              	]
            }
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            //"windowsSdkVersion": "10.0.17134.0",
  					//编译器位置,不加后缀
            "compilerPath": "D:/mingw64/bin/g++",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```
