---
html:
    toc: true
---



# <center><font face="仿宋" font color=orange>Markdown入门教程</font>
# <center><font face ="楷体" sice=5>鑫</font></center>

```
cmake_minimum_required(VERSION 3.15) # 设置CMake的最低版本要求

project(HelloWorld) # 定义项目名称

add_subdirectory(lesson1_1) # 添加子目录lesson1_1，包含其CMakeLists.txt文件
add_subdirectory(lesson1_2) # 添加子目录lesson1_2，包含其CMakeLists.txt文件

set(CMAKE_CXX_STANDARD 11) # 设置C++语言的最低版本要求

add_executable(HelloWorld main.cpp) # 添加可执行文件    

include_directories(../lesson1_1)   # 包含lesson1_1目录，以便在编译时可以找到该目录下的头文件

# 添加一个名为 add_static 的静态库，其源文件为 add.cpp
add_library(add_static add.cpp)

link_libraries(/mnt/hgfs/shared/Cmake/lesson2_1/lib/libadd_static.a)
link_directories(/mnt/hgfs/shared/Cmake/lesson2_1/lib)
target_link_libraries(lesson2_1 libadd_static.a)



cmake .. -DCMAKE_VERBOSE_MAKEFILE=ON # 显示详细的编译信息
```