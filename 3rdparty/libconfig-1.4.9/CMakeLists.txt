# CMake build file for libconfig++

cmake_minimum_required(VERSION 2.8)
project(libconfig++)

set(EXECUTABLE_OUTPUT_PATH "${PROJECT_BINARY_DIR}/../../bin")
set(LIBRARY_OUTPUT_PATH "${PROJECT_BINARY_DIR}/../../bin")

set(LIBCONFIG++_SOURCES
    "grammar.c"
    "libconfig.c"
    "libconfigcpp.c++"
    "scanctx.c"
    "scanner.c"
    "strbuf.c"
    )

add_definitions(
    /DLIBCONFIGXX_EXPORTS
    /DLIBCONFIG_STATIC
    )

if (MSVC)
  add_definitions(
      /wd4290 /wd4267 /wd4244
      /D_CRT_SECURE_NO_WARNINGS
      /DYY_NO_UNISTD_H
      /DYY_USE_CONST
      /Gd /EHsc
  )
endif (MSVC)

foreach(file ${LIBCONFIG++_SOURCES})
    if(file MATCHES ".*\\.c$")
        set_source_files_properties(${file} PROPERTIES
            LANGUAGE C
        )
    else()
        set_source_files_properties(${file} PROPERTIES
            LANGUAGE CXX
        )
    endif()
endforeach()

if (WIN32)
    add_library(libconfig++ SHARED ${LIBCONFIG++_SOURCES})
else()
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")

    add_library(libconfig++ STATIC ${LIBCONFIG++_SOURCES})
endif()

set_target_properties(
    libconfig++ PROPERTIES
    PREFIX ""
    FOLDER "3rdParty"
)