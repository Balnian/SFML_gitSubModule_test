cmake_minimum_required(VERSION 3.0)

add_subdirectory(extlib/SFML)

include_directories(extlib/SFML/include)

project(MyProject)

set(SRC
    main.cpp
    )

function(copy_to_output target item_to_copy)
add_custom_command(TARGET ${target} POST_BUILD 
            COMMAND ${CMAKE_COMMAND} -E copy_if_different
            $<TARGET_FILE:${item_to_copy}>              
            $<TARGET_FILE_DIR:${target}>)
endfunction(copy_to_output)

if(UNIX)
	include(CheckCXXCompilerFlag)
	check_cxx_compiler_flag("-std=c++1y" COMPILER_SUPPORTS_CXX1Y)
	check_cxx_compiler_flag("-std=c++14" COMPILER_SUPPORTS_CXX14)
	check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
	check_cxx_compiler_flag("-std=c++0X" COMPILER_SUPPORTS_CXX0X)
	if(COMPILER_SUPPORTS_CXX1Y)
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++1y")
	elseif(COMPILER_SUPPORTS_CXX14)
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
	elseif(COMPILER_SUPPORTS_CXX11)
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
	elseif(COMPILER_SUPPORTS_CXX0X)
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
	endif()
endif()

if(MSVC)
	
endif()


#set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_INSTALL_PREFIX}
add_executable(MyProject ${SRC})
target_link_libraries(MyProject sfml-graphics sfml-window sfml-system)

copy_to_output(MyProject sfml-graphics)
copy_to_output(MyProject sfml-window)
copy_to_output(MyProject sfml-system)

