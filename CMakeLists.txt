cmake_minimum_required(VERSION 2.8.9)
project(dkl-cmd LANGUAGES C)

set(CMAKE_C_FLAGS "-nostdlib")
set(CMAKE_C_STANDARD 11)

file(GLOB include_ls RELATIVE "${PROJECT_SOURCE_DIR}" "include/*.h")
file(GLOB header_ls RELATIVE "${PROJECT_SOURCE_DIR}" "src/*.h")
file(GLOB src_ls RELATIVE "${PROJECT_SOURCE_DIR}" "src/*.c" "src/**/*.c" "src/**/**/*.c")

list(FILTER include_ls EXCLUDE REGEX "^include\\/\\@")
list(FILTER header_ls EXCLUDE REGEX "^src\\/\\@")
list(FILTER src_ls EXCLUDE REGEX "^src\\/\\@")

# Dependencies

if (CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    set(exd_examples true)
    set(exd_tests true)
endif()

if (CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    unset(exd_examples)
    unset(exd_tests)
endif()

add_library(dkl-cmd ${header_ls} ${include_ls} ${src_ls})

target_include_directories(dkl-cmd PRIVATE src)

set_target_properties(
        dkl-cmd PROPERTIES
        PUBLIC_HEADER "${include_ls}"
        PRIVATE_HEADER "${header_ls}"
)

install(TARGETS dkl-cmd
        PUBLIC_HEADER DESTINATION include
        PRIVATE_HEADER DESTINATION include/dkl-cmd
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        )

if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/share")
    install(DIRECTORY share/ DESTINATION share/dkl-cmd)
endif ()

if (NOT exd_examples)
    add_subdirectory(examples)
endif ()

if (NOT exd_tests)
    add_subdirectory(tests)
endif ()
