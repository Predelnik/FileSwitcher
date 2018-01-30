cmake_minimum_required (VERSION 3.7)
project (FileSwitcher)

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zc:threadSafeInit-")
endif()

if(MSVC)
  set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO} /GL")
  set(CMAKE_SHARED_LINKER_FLAGS_RELWITHDEBINFO  "${CMAKE_SHARED_LINKER_FLAGS_RELWITHDEBINFO} /LTCG:INCREMENTAL")
  set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /GL")
  set(CMAKE_SHARED_LINKER_FLAGS_RELEASE  "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /LTCG:INCREMENTAL")
endif()

add_definitions ("-D_CRT_NONSTDC_NO_DEPRECATE -DUNICODE -D_UNICODE -D_CRT_SECURE_NO_WARNINGS -D_CRT_NON_CONFORMING_SWPRINTFS -D_CRT_SECURE_NO_DEPRECATE -D_WINSOCK_DEPRECATED_NO_WARNINGS")

file (GLOB_RECURSE source_files *.cpp *.h *.rc)

add_library (FileSwitcher SHARED ${source_files})
target_link_libraries(FileSwitcher comctl32 shlwapi)

if (MSVC)
  target_compile_options (FileSwitcher PRIVATE "/Yustdafx.h" "/FIstdafx.h")
  SET_SOURCE_FILES_PROPERTIES("src/stdafx.cpp" PROPERTIES COMPILE_FLAGS "/Yc")
endif ()

target_include_directories (FileSwitcher PUBLIC Common/Ccpp/ src )

set(FileSwitcher_DLL_OUTPUT_PATH "" CACHE STRING "Output for debug library (Possibly notepad++ plugin folder)")
if (NOT ${FileSwitcher_DLL_OUTPUT_PATH} STREQUAL "")
    set_target_properties (FileSwitcher PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY_DEBUG ${FileSwitcher_DLL_OUTPUT_PATH}
        RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${FileSwitcher_DLL_OUTPUT_PATH}
        RUNTIME_OUTPUT_DIRECTORY_RELEASE ${FileSwitcher_DLL_OUTPUT_PATH})
endif ()

