file(GLOB example_ls "*")

# Dependencies

include_directories(../include)
link_libraries(dkl-cmd)

foreach (dir ${example_ls})
    if (IS_DIRECTORY ${dir})
        add_subdirectory(${dir})
    endif ()
endforeach ()