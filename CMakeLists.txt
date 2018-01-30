cmake_minimum_required (VERSION 3.7)
project (FileSwitcher)

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zc:threadSafeInit-")
endif()

if(MSVC)
    if (NOT CMAKE_BUILD_TYPE EQUAL "DEBUG")
      set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /GL")
      set(CMAKE_SHARED_LINKER_FLAGS  "${CMAKE_DLL_LINKER_FLAGS} /LTCG:INCREMENTAL")
    endif()
endif()

add_definitions ("-D_CRT_NONSTDC_NO_DEPRECATE -DUNICODE -D_UNICODE -D_CRT_SECURE_NO_WARNINGS -D_CRT_NON_CONFORMING_SWPRINTFS -D_CRT_SECURE_NO_DEPRECATE -D_WINSOCK_DEPRECATED_NO_WARNINGS")

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4 /WX /w44062 /utf-8")
endif()

file (GLOB_RECURSE source_files src/*.cpp src/*.h)

add_library (FileSwitcher SHARED ${source_files})
target_include_directories (FileSwitcher PUBLIC Common/Ccpp/ src )
target_compile_options (FileSwitcher PUBLIC /permissive- /Zc:twoPhase-)

set(FileSwitcher_DLL_OUTPUT_PATH "" CACHE STRING "Output for debug library (Possibly notepad++ plugin folder)")
if (NOT ${FileSwitcher_DLL_OUTPUT_PATH} STREQUAL "")
    set_target_properties (FileSwitcher PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY_DEBUG ${FileSwitcher_DLL_OUTPUT_PATH}
        RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${FileSwitcher_DLL_OUTPUT_PATH}
        RUNTIME_OUTPUT_DIRECTORY_RELEASE ${FileSwitcher_DLL_OUTPUT_PATH})
endif ()

