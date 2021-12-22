# CMake基础

- CMake指令房放在`CMakeLists.txt`文件中，文件名区分大小写，必须命名正确

- 指定CMake所需的最低版本
    - `cmake_minimum_required(VERSION 3.5 FATAL_ERROR)`
    - 如果使用的CMake低于该版本，则会发出致命错误，FATAL_ERROR可以省略

- 指定项目的名称和支持的编程语言
    - `project(name LANGUAGES CXX)`
    - CXX代表C++，LANGUAGES CXX可以省略，建议显示声明语言

- 指示CMake创建一个新可执行文件，该可执行文件通过编译链接源文件产生
    - `add_executable(exe_name a.cpp b.cpp)`
    - 源文件一般为多个，可以用绝对路径，也用可以相对路径，相对于CMakeLists.txt文件

- 通过命令行运行CMake
    - `mkdir build`
    - `cd build`
    - `make ..`
    - 也可以一行命令完成以上操作，且该行命令是夸平台的标准使用方式
        - `cmake -H. -Bbuild`
        - `-H.`表示在当前目录中搜索根CMakeLists.txt
        - `-Bbuild`告诉CMake在一个名为build的目录中生成所有的文件

- CMake是一个构建系统生成器，描述构建系统应当如何操作才能编译代码，CMake为所选的构建系统生成相应的指令。默认情况下，在Windows上使用Visual Studio作为生成器。在类Unix系统上使用Makefile生成器

- 构建目标
    - `cmake --build .`
    - `all`默认目标，将在项目中构建所有目标
    - `clear`删除所有生成的文件
    - `rebuild_cache`将调用CMake为源文件生成依赖
    - `edit_cache`这个目标允许直接编辑缓存
    - `test`运行测试套件
    - `install`将执行项目安装规则
    - `package`生成可分发的包

- 切换生成器
    - `cmake -G 生成器 ..`

- `CMakeCache.txt`CMake在这个文件中进行缓存，与生成器无关
- `CMakeFiles`包含由CMake配置期间生成的临时文件
- `cmake_install.cmake`CMake脚本处理安装规则，并在安装时使用
- 