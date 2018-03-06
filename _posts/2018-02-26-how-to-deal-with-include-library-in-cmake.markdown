---
layout: post
title: How to deal with include and library in cmake project
date: 2018-03-06 12:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: we-in-rest.jpg # Add image post (optional)
tags: [C++, Compiling] # add tag
---

### Multiple compiling steps integrated into one steps in cmake system.

Usually non-script language like C++, C and etc need to be compiled to use. In linux system, for C++ project, we normally use g++ from GNU to do compiling, and there are multiple steps:
* Pre-processing: via the GNU C Preprocessor (cpp.exe), which includes the headers (#include) and expands the macros 
> cpp hello.c > hello.i 

The resultant intermediate file "hello.i" contains the expanded source code. 
 
* Compilation: g++ or gcc do compilation to generate Assembly code(.s), 
> gcc -S hello.i

The -S option specifies to produce assembly code, instead of object code. The resultant assembly file is "hello.s".

* Assembler collect .s files to generate machine code (.o .obj)
 > as -o hello.o hello.s 
 
* Finally, linker(ld) will do linking that link **machine code** with **static library (.lib, .a)** to generate executable machine code.
> ld -o hello.exe hello.o ...libraries... 

###Headers (.h), static libraries(.lib, .a) and shared library (.dll, .so) 

Library: collection of pre-compiled object files that can be linked into your programs via the linker. Like prinf() and sqrt() 

Two types of external libraries: **static and shared library **

Static: (.a or .lib) machine code of static libraries are to be included into excutable. 

Static lib can be created by archive program: ar.exe 

Shared: (.so or .dll) when program is linked to a share lib, a small table is created in excutable. When excutable running, OS load machine code needed via dynamic linking.  

Shared libraries can be updated without recompiling the program again. 

_GCC, by default, links to the shared library if it is available. _

> cpp –v  
//list the default include-paths used by GNU C Preprocessor 

For now, we already know what steps we need to compile a C++ project with gcc compiling system.

### Cmake compiling system
Here is the official tutorial: https://cmake.org/cmake-tutorial/

normally, we use this kind of minimal **CMakeLists.txt** file:
```
cmake_minimum_required(VERSION 2.6)
project(my_project)
find_package(OpenCV 3.0.0 REQUIRED)
message("OpenCV version: ${OpenCV_VERSION}")
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIR})
set(SRC
  main.cpp
)
add_executable(${PROJECT_NAME} ${SRC})
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS})
```
however, we may encounter situation that you have multiple version of Opencv on different path in your system. What can we do?
we can use a few function to forcefully make the cmake point to the location you want.
Here is another modified **CMakeLists.txt** file:
```
cmake_minimum_required(VERSION 2.6)
project(tut)

set(OpenCV_INCLUDE_DIRS
  /usr/local/include
  /usr/local/include/opencv2
)

set(OpenCV_LIB_DIR
  /usr/local/lib
)

set(OpenCV_LIBS
  opencv_core
  opencv_highgui
  opencv_imgcodecs
)

include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIR})
add_executable(${PROJECT_NAME} src/main.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS})
```
we can also specify exact version of package you want:
```
find_package(OpenCV EXACT version REQUIRED)
```
Or alternatively in terminal, before you run **cmake ..**, run this
```bash
xport CMAKE_PREFIX_PATH=/usr/local:$CMAKE_PREFIX_PATH
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
```
This should give preference to your custom OpenCV installation when doing the find_package(OpenCV 3.0.0 REQUIRED)


###Little tips for you
#### Use ldconfig after make install of library _every time__
Can be used to load shared object libxxxxx.so into memory after make install.
#### Point to specific package with CMAKE_PREFIX_PATH as option of cmake ..
In build or release dir: 
Use cmake –DCMAKE_PREFIX_PATH="path to your dir containing xxxconfig.cmake" .. 


![kevin hart]({{site.baseurl}}/assets/img/kh.jpg)
