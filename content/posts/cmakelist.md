---
title: cmakelist
tag: cpp
date: 2019-03-26 09:36:55
---

## 编写
[官方文档](https://cmake.org/cmake-tutorial/)

用`cmake`编译`C++`的时候，必须写一个`cmakelist.txt`，其具体的写法如下。

首先注释是`#`后面的部分。

在文件最开始指明依赖的`cmake`的版本。`cmake_minimum_required(VERSION 3.9.0)`

然后是指定生成的工程名 `PROJECT(test)`

变量的定义通过`SET(sampleName MyApp)`，前面为变量名，后面为值。

然后添加依赖的`package`，

```
find_package(PCL 1.2 REQUIRED)
FIND_PACKAGE(MRPT REQUIRED base;bayes;obs;gui)
```
下面是指定`include`文件夹，其中既可以使用来自`package`中的定义，又可以使用绝对路径。

```
include_directories(${PCL_INCLUDE_DIRS})
include_directories("G:/Matlab/extern/include")
include_directories("C:/Program Files/MobileRobots/Aria/include")
```
指定`additional library`文件夹，用法同`include_directories`

```
link_directories(${PCL_LIBRARY_DIRS})
link_directories("G:/Matlab/extern/lib/win32/microsoft")
link_directories("C:/Program Files/MobileRobots/Aria/lib")
```

```
# here we add definitions for any package if requred.
add_definitions(${PCL_DEFINITIONS})

# The following line is very important.
# It specifies the executable name. Here the executable is the first parameter i.e. MyApp and a file MyApp.exe will be created on
# compilation in windows platform.
# Followed by the executable name come all your source and header files.
# All cpp fles will be clubbed into source folder and all .h files will be added to header files folder of the project.
add_executable (MyApp Local.cpp part.h grab.h interface.h test.cpp test.h)

# There may be some additional dependencies which you may have to specify for the project, which you may do as in following lines.
# Note that first parameter is the executable name.
target_link_libraries (MyApp ${PCL_LIBRARIES} libeng.lib libmx.lib libmex.lib libmat.lib Aria.lib winmm.lib wsock32.lib)
# Add the required libraries for linking:
TARGET_LINK_LIBRARIES(${sampleName}
${MRPT_LIBS} # This is filled by FIND_PACKAGE(MRPT ...)
"" # Optional extra libs...
)
```

## 将其他平台的项目迁移到 CMake
CMake 可以很轻松地构建出在适合各个平台执行的工程环境。而如果当前的工程环境不是 CMake ，而是基于某个特定的平台，是否可以迁移到 CMake 呢？答案是可能的。下面针对几个常用的平台，列出了它们对应的迁移方案。
### autotools
* am2cmake 可以将 autotools 系的项目转换到 CMake，这个工具的一个成功案例是 KDE 。
* Alternative Automake2CMake 可以转换使用 automake 的 KDevelop 工程项目。
* Converting autoconf tests
### qmake
* qmake converter 可以转换使用 QT 的 qmake 的工程。
### Visual Studio
* vcproj2cmake.rb 可以根据 Visual Studio 的工程文件（后缀名是 .vcproj 或 .vcxproj）生成 CMakeLists.txt 文件。
* vcproj2cmake.ps1 vcproj2cmake 的 PowerShell 版本。
* folders4cmake 根据 Visual Studio 项目文件生成相应的 “source_group” 信息，这些信息可以很方便的在 CMake 脚本中使用。支持 Visual Studio 9/10 工程文件。
### CMakeLists.txt 自动推导
* gencmake 根据现有文件推导 CMakeLists.txt 文件。
* CMakeListGenerator 应用一套文件和目录分析创建出完整的 CMakeLists.txt 文件。仅支持 Win32 平台。
