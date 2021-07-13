## CMake
___

```
cmake_minimum_required(VERSION 3.16)
project(xper-cpp)


set(CMAKE_CXX_FLAGS_DEBUG_INIT "-Wall")
set(CMAKE_CXX_FLAGS_RELEASE_INIT "-Wall")
set(SOURCE_FILES
        src/main.cpp
        src/window/MyWindow.cpp
        src/headerbar/MyHeaderBar.cpp
        src/drawer/Drawer.cpp
        src/drawer/DrawerContainer.cpp)

add_executable(xper-cpp ${SOURCE_FILES})


find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK3 REQUIRED gtkmm-3.0)

include_directories(${GTK3_INCLUDE_DIRS})
include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(${PROJECT_SOURCE_DIR}/resource)

link_directories(${GTK3_LIBRARY_DIRS})
add_definitions(${GTK3_CFLAGS_OTHER})

target_link_libraries(xper-cpp ${GTK3_LIBRARIES})

file(COPY resource DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(COPY resource DESTINATION ${CMAKE_BINARY_DIR})
file(TO_CMAKE_PATH style FILE_PATH_NORMALIZED)

### install the project
set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/install)

install(TARGETS xper-cpp DESTINATION bin)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include DESTINATION include)
install(DIRECTORY ${GTK3_LIBRARY_DIRS} DESTINATION lib)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/resource DESTINATION files)



```

#### 1) CMAKE_<LANG>_FLAGS_<CONFIG>
Value used to initialize the `CMAKE_<LANG>_FLAGS_<CONFIG>` cache entry the first time a build tree is configured for language
<LANG>. This variable is meant to be set by a toolchain file. CMake may prepend or append content to the value based on 
the environment and target platform.

#### 2) set
Set a normal, cache, or environment variable to a given value. See the cmake-language(7) variables documentation for the
scopes and interaction of normal variables and cache entries.

#### 3) find and use gtkmm

* Download **_gtkmm_** library for Ubuntu from [gtkmm lib](https://gtkmm.org/en/download.html) or from terminal :
> sudo apt-get install libgtkmm-3.0-dev

___

> pkg-config  --cflags gtkmm-3.0

Then in **_CMakeLists.txt_** :

```
find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK3 REQUIRED gtkmm-3.0)
```

#### include directories
Add include directories to the build such as headers of cpp files or gtkmm libraries.

#### link directories
Add directories in which the linker will look for libraries.
Adds the paths in which the linker should search for libraries. Relative paths given to this command are interpreted 
as relative to the current source directory.

#### add definitions
Adds definitions to the compiler command line for targets in the current directory, whether added before or after 
this command is invoked, and for the ones in sub-directories added after. This command can be used to add any flags,
but it is intended to add preprocessor definitions.

#### target link libraries
Specify libraries or flags to use when linking a given target and/or its dependents. Usage requirements from linked 
library targets will be propagated. Usage requirements of a target's dependencies affect compilation of its own sources.

#### file
File manipulation command.

This command is dedicated to file and path manipulation requiring access to the filesystem.
Add your binary files to cpp project.

#### install
If make install is invoked or INSTALL is built, this directory is prepended onto all install directories. 
This variable defaults to _**/usr/local**_ on _**UNIX**_ and **_c:/Program Files/${PROJECT_NAME}_** on Windows. 
See CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT for how a project might choose its own default.

On UNIX one can use the DESTDIR mechanism in order to relocate the whole installation. See DESTDIR for more information.

#### using files and directories in your cpp project
if you want to use a directory containing binary files , xml , ui files and etc , your cmake-build-debug must be include
those directories. 

To use Binary files like image, png and etc.
> file(COPY [dir] DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

or:

> file(COPY [dir] DESTINATION ${CMAKE_BINARY_DIR})

To another files like .css files and etc.
>  file(COPY [dir] DESTINATION ${CMAKE_CACHEFILE_DIR})

other options you can use :
> file(COPY [dir] DESTINATION ${PROJECT_BINARY_DIR}) 
