# 现代C++开发模板
一个采用CMake的快速开始的C++开发模板，目的为能够快速轻松的开始现代C++编程。

## 特色

* 采用 `CMake` 来构建项目
* 给出一个 `Clang-Format` 配置示例，灵感来自基础的 Google 模型，并进行了小幅调整。每个人都可以自由地删除它（以使用 LLVM 默认设置）或提供他们自己的替代方案。
* 集成了静态分析器，支持 `Clang-Tidy` 和 `Cppcheck`，默认选项是前者。`Clang-Tidy`的详解介绍可参考该[blog](https://blog.csdn.net/stallion5632/article/details/139545885)。
* 支持 `Doxygen`，通过 `ENABLE_DOXYGEN` 选项来启用。`Doxygen`的详解介绍可参考该[blog](https://blog.csdn.net/FSKEps/article/details/125137129)。
* 支持单元测试，通过 `GoogleTest` 或 `Cath2` 来实现。
* 集成 `Codecov CI` ，通过使用 `ENABLE_CODE_COVERAGE` 选项来启用。
* 支持包管理。
* 使用 `GitHub Actions` 实现 `Windows`、`Linux` 和 `MacOS` 的 CI 工作流程，利用缓存特性，确保最短的运行时间。
* 提供了详细的 `.md` 模板，以便你使用。
* 采用宽松许可证，以便你能够尽可能容易地集成。
* 可以构建为只包含头文件的库或可执行文件，不仅仅是静态或共享库。
* 集成 `Ccache`，加速重构时间。

## 快速开始

### 前置条件

* 版本大于 **3.15** 的 `CMake`
* C++编译器，至少需要支持 `C++ 17` 标准

### 安装

执行以下命令，克隆此仓库：  
`git clone https://github.com/filipdutescu/modern-cpp-template/`

克隆完成后，在 `include/` 下创建项目文件夹，并且在 `cmake/SourcesAndHeaders.cmake` 文件中修改对应内容，比如头文件、源文件有哪些。

接着，将 `cmake` 文件夹下的 `ProjectConfig.cmake.in` 的名称修改为自己项目的名称，如  `MyNewProjectConfig.cmake.in` 。同时，将 `.github/workflows/` 下的文件进行对应修改，特别是 `ubuntu.yml`，应该将 `-DProject_ENABLE_CODE_COVERAGE=1` 修改为 `-DMyNewProject_ENABLE_CODE_COVERAGE=1`。

最后，将 CMakeLists.txt 中的以下内容进行修改：  

```bash
project(
  "Project"
  VERSION 0.1.0
  LANGUAGES CXX
)
```

将 `"Project"` 修改为自己的项目名称，例如：  

```bash
project(
  MyNewProject
  VERSION 0.1.0
  LANGUAGES CXX
)
```

如果你想使用 CMake 来安装一个已经构建的项目，可以执行以下命令：  

```bash
cmake --build build --target install --config Release

# a more general syntax for that command is:
cmake --build <build_directory> --target install --config <desired_config>
```

## 构建项目

在进行上面的步骤后，就可以执行与下面类似的命令来构建项目： 

```bash
mkdir build/ && cd build/
cmake .. -DCMAKE_INSTALL_PREFIX=/absolute/path/to/custom/install/directory
cmake --build . --target install
```

> **注意：** 当你想自定义安装路径时才使用 `DCMAKE_INSTALL_PREFIX` 参数，否则，该参数可以省略


更多参数可以在 `cmake/StandardSettings.cmake` 文件中查询。某些参数还需在其对应的 `*.cmake` 文件进行修改。

## 生成文档

执行以下命令即可：

```bash
mkdir build/ && cd build/
cmake .. -D<project_name>_ENABLE_DOXYGEN=1 
cmake --build . --target doxygen-docs
```

> **注意：** 这将会在项目根目录下，生成目录 docs/

## 进行测试

默认使用 `GoogleTest` 来进行单元测试。可在  `cmake/StandardSettings.cmake` 文件中进行修改。可以执行以下命令来进行测试：

```bash
cd build          # if not in the build directory already
ctest -C Release  # or `ctest -C Debug` or any other configuration you wish to test

# you can also run tests with the `-VV` flag for a more verbose output (i.e.
#GoogleTest output as well)
```
