# build from source
```
git clone https://github.com/google-deepmind/mujoco.git
cd mujoco/
git checkout 3.3.0
mkdir build
cmake ..
cmake --build .
cmake .. -DCMAKE_INSTALL_PREFIX=/opt/mujoco
sudo cmake --install .
```


- error 
```
CMake Error at build/_deps/glfw3-src/CMakeLists.txt:223 (message):
  Xinerama headers not found; install libxinerama development package
-- Using X11 for window creation
CMake Error at build/_deps/glfw3-src/CMakeLists.txt:233 (message):
  Xcursor headers not found; install libxcursor development package
CMake Error at build/_deps/glfw3-src/CMakeLists.txt:238 (message):
  XInput headers not found; install libxi development package

```
- solution
```
sudo apt-get install libxinerama-dev
sudo apt install libxcursor-dev
sudo apt-get install libxi-dev
```