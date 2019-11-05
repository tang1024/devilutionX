Windows via Visual Studio
=========================
Download the 32bit MSVC Development Libraries
---------------------------------------------
- SDL2
- SDL2_mixer
- SDL2_ttf
- Libsodium

`vcpkg install sdl2:x86-windows sdl2-mixer:x86-windows sdl2-ttf:x86-windows libsodium:x86-windows`

Bash Setting
------------
CMake Alias
```````````
alias cmake='/z/entry/sw/cmake/bin/cmake.exe'

Environment Variable
````````````````````
export sodium_DIR="Z:\\entry\\sw\\vcpkg-2019.10\\packages\\libsodium_x86-windows"

message(WARNING "sodium_DIR: "$ENV{sodium_DIR}"")

CMake Variable
--------------
set(SDL2_ROOT_DIR "Z:\\entry\\sw\\vcpkg-2019.10\\packages\\sdl2_x86-windows")

set(SDL2_ttf_ROOT_DIR "Z:\\entry\\sw\\vcpkg-2019.10\\packages\\sdl2-ttf_x86-windows")

set(SDL2_mixer_ROOT_DIR "Z:\\entry\\sw\\vcpkg-2019.10\\packages\\sdl2-mixer_x86-windows")

.cmake Files
------------
Findsodium.cmake
````````````````
::

    find_path(sodium_INCLUDE_DIR sodium.h
        HINTS ${sodium_DIR}
        PATH_SUFFIXES include
    )
    message("sodium_INCLUDE_DIR: ${sodium_INCLUDE_DIR}")
    
    set(_DEBUG_PATH_SUFFIX "debug/lib")
    message("_DEBUG_PATH_SUFFIX: ${_DEBUG_PATH_SUFFIX}")
    find_library(sodium_LIBRARY_DEBUG libsodium.lib
        HINTS ${sodium_DIR}
        PATH_SUFFIXES ${_DEBUG_PATH_SUFFIX}
    )
    message("sodium_LIBRARY_DEBUG: ${sodium_LIBRARY_DEBUG}")
    
    set(_RELEASE_PATH_SUFFIX "lib")
    message("_RELEASE_PATH_SUFFIX: ${_RELEASE_PATH_SUFFIX}")
    find_library(sodium_LIBRARY_RELEASE libsodium.lib
        HINTS ${sodium_DIR}
        PATH_SUFFIXES ${_RELEASE_PATH_SUFFIX}
    )
    message("sodium_LIBRARY_RELEASE: ${sodium_LIBRARY_RELEASE}")

FindSDL2.cmake
``````````````
::

    if(WIN32 OR ANDROID OR IOS OR (APPLE AND NOT _sdl2_framework))
	set(SDL2_EXTRA_REQUIRED SDL2_SDLMAIN_LIBRARY)
	find_library(SDL2_SDLMAIN_LIBRARY
		NAMES
		SDL2main
		PATHS
		${SDL2_ROOT_DIR}
		ENV SDL2DIR
		PATH_SUFFIXES lib lib/manual-link ${SDL2_LIB_PATH_SUFFIX})
    endif()

CMake find
----------
find_path

::

    find_path(sodium_INCLUDE_DIR sodium.h
        HINTS ${${XPREFIX}_INCLUDE_DIRS}
    )

find_library

find_package