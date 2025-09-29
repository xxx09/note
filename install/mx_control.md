# 1. Cmake update
- prepare
```
sudo apt-get install libgl1-mesa-dev
sudo apt-get install libssl-dev
```
-download package `wget https://cmake.org/files/v3.28/cmake-3.28.5.zip`

- install
```
cd cmake-3.28.5
chmod 777 ./configure
./configure
make
sudo make install
```
- replace default cmake version
```
sudo update-alternatives --install /usr/bin/cmake cmake /usr/local/bin/cmake 1 --force
```

# 2. Onnxruntime
- `git checkout v1.19.2`
- error
```
gmake[2]: *** [CMakeFiles/eigen-populate.dir/build.make:100: eigen-populate-prefix/src/eigen-populate-stamp/eigen-populate-download] Error 1
gmake[1]: *** [CMakeFiles/Makefile2:83: CMakeFiles/eigen-populate.dir/all] Error 2
gmake: *** [Makefile:91: all] Error 2
CMake Error at /usr/local/share/cmake-3.28/Modules/FetchContent.cmake:1679 (message):
  Build step for eigen failed: 2
Call Stack (most recent call first):
  /usr/local/share/cmake-3.28/Modules/FetchContent.cmake:1819:EVAL:2 (__FetchContent_directPopulate)
  /usr/local/share/cmake-3.28/Modules/FetchContent.cmake:1819 (cmake_language)
  external/eigen.cmake:12 (FetchContent_Populate)
  external/onnxruntime_external_deps.cmake:500 (include)
  CMakeLists.txt:598 (include)
  ```
  - solution; 使用sha1sum来计算.zip的结果，原始文件中的计算结果有误， replace l24 of cmake/deps.txt with below[which is the main branch]
```
eigen;https://gitlab.com/libeigen/eigen/-/archive/e7248b26a1ed53fa030c5c459f7ea095dfd276ac/eigen-e7248b26a1ed53fa030c5c459f7ea095dfd276ac.zip;32b145f525a8308d7ab1c09388b2e288312d8eba
```

- error
```
CMake Error at onnxruntime_unittests.cmake:1762 (add_dependencies):
  The dependency target "absl::bounded_utf8_length_sequence" of target
  "test_execution_provider" does not exist.
Call Stack (most recent call first):
  CMakeLists.txt:1787 (include)
  CMake Error at onnxruntime_common.cmake:138 (target_link_libraries):
  Target "onnxruntime_common" links to:

    absl::demangle_rust

  but the target was not found.  Possible reasons include:

    * There is a typo in the target name.
    * A find_package call is missing for an IMPORTED target.
    * An ALIAS target is missing.

Call Stack (most recent call first):
  CMakeLists.txt:1787 (include)
```
- solution, `abseil的版本问题, cmake`， 将.bashrc中conda环境去掉；再编译就会自动下载deps.txt对应的abseil版本

- error 
```
onnxruntime/onnxruntime/test/util/file_util.cc:3:10: fatal error: gtest/gtest.h: No such file or directory
    3 | #include <gtest/gtest.h> 
onnxruntime/onnxruntime/test/util/include/asserts.h:9:10: fatal error: gmock/gmock.h: No such file or directory
```
- soluton
```
sudo apt install libgtest-dev
sudo apt install libgmock-dev
```
但版本不对，仍旧会报以下错误
```
/gmock/internal/gmock-internal-utils.h:405:37: error: ‘IndexSequence’ has not been declared 
```
- solution
```
git clone https://github.com/google/googletest.git
cd googletest
git checkout v1.15.0
mkdir build
cd build/
cmake ..
make -j8
sudo make install
```
可能有用的地方， /anaconda3/include/gmock/ 将其名称改为gmock1,否则系统路径会找到这个位置[但其gmock版本较旧]






