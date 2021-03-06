cmake_minimum_required(VERSION 3.0)
project(war2tools C)

include(cmake/check_compiler_warnings.cmake)

add_definitions(-g) # FIXME
add_definitions(-std=gnu99) # FIXME
add_definitions(-D_BSD_SOURCE)
add_definitions(-D_DEFAULT_SOURCE)

check_compiler_warnings(
    -Wall
    -Wextra
    -Winit-self
    -Wfloat-equal
    -Wunused
    -Wunused-parameter
    #-Wsign-conversion
    -Wchar-subscripts
    -Wdouble-promotion
    -Wformat-security
    -Wformat-nonliteral
    -Wmissing-braces
    -Wmissing-include-dirs
    -Wparentheses
    -Wsequence-point
    -Wtrigraphs
    -Wmaybe-uninitialized
    -Wsuggest-final-types
    -Wsuggest-final-methods
    #-Wconversion
    -Wlogical-op
    -Wlogical-not-parentheses
    -Wmemset-transposed-args
    -Wsizeof-pointer-memaccess
    -Wmissing-field-initializers
    -Wmultichar
    #-Wpacked
    #-Wpacked-bitfield-compat
    #-Wpadded
    -Wredundant-decls
    -Winline
    -Winvalid-pch
    -Woverlength-strings
    -Wcast-align
)


set(LIBPUD_DESCRIPTION "Warcraft II PUD Manipulation Utilities")
set(LIBPUD_VERSION_MAJOR 0)
set(LIBPUD_VERSION_MINOR 99)

set(LIBWAR2_DESCRIPTION "Warcraft II Data Files Manipulation Utilities")
set(LIBWAR2_VERSION_MAJOR 0)
set(LIBWAR2_VERSION_MINOR 99)

find_package(PkgConfig)
find_package(JPEG)
find_package(PNG)

pkg_check_modules(CHECK check)

set(LIBWAR2_LIBRARIES libwar2 libpud)
set(LIBWAR2_INCLUDE_DIRS "")
set(LIBPUD_LIBRARIES libpud)

if (JPEG_FOUND)
   set(LIBWAR2_INCLUDE_DIRS ${LIBWAR2_INCLUDES_DIRS} ${JPEG_INCLUDES_DIR})
   set(LIBWAR2_LIBRARIES ${LIBWAR2_LIBRARIES} ${JPEG_LIBRARIES})
endif ()
if (PNG_FOUND)
   set(LIBWAR2_INCLUDES_DIRS ${LIBWAR2_INCLUDES_DIRS} ${PNG_INCLUDES_DIR})
   set(LIBWAR2_LIBRARIES ${LIBWAR2_LIBRARIES} ${PNG_LIBRARIES})
endif ()

set(PUD_LIBRARIES libpud ${LIBWAR2_LIBRARIES})
set(PUD_INCLUDES_DIRS ${LIBWAR2_INCLUDE_DIRS})

add_custom_target(uninstall
   COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_SOURCE_DIR}/cmake/uninstall.cmake
)

add_subdirectory(libpud)
add_subdirectory(libwar2)
add_subdirectory(pc)
add_subdirectory(pud)
add_subdirectory(include)
add_subdirectory(tools)
add_subdirectory(doc)
add_subdirectory(quicklook)

if (CHECK_FOUND)
   enable_testing()
   add_definitions(-DTESTS_BUILD_DIR=\"${CMAKE_BINARY_DIR}/tests\")
   add_subdirectory(tests)
endif ()
