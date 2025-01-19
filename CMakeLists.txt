cmake_minimum_required(VERSION 3.1)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../cimgui ./cimgui)

project(cimplot)

#general settings
file(GLOB IMGUI_SOURCES
    ../cimgui/cimgui.h
    ../cimgui/imgui/imgui.h
	cimplot.cpp
	implot/implot.cpp
	implot/implot_demo.cpp
	implot/implot_items.cpp
)

set(IMGUI_STATIC "no" CACHE STRING "Build as a static library")
set(CMAKE_CXX_STANDARD 11)

#add library and link
if (IMGUI_STATIC)
    add_library(cimplot STATIC ${IMGUI_SOURCES})
else (IMGUI_STATIC)
    add_library(cimplot SHARED ${IMGUI_SOURCES})
endif (IMGUI_STATIC)

add_definitions("-DIMGUI_USER_CONFIG=\"../cimconfig.h\"")
target_compile_definitions(cimplot PUBLIC IMGUI_DISABLE_OBSOLETE_FUNCTIONS=1)


target_include_directories(cimplot PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/implot)
target_include_directories(cimplot PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../cimgui/imgui)
target_include_directories(cimplot PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../cimgui)
target_link_libraries(cimplot PRIVATE cimgui)
set_target_properties(cimplot PROPERTIES PREFIX "")

#install
install(TARGETS cimplot
    RUNTIME DESTINATION  .
    LIBRARY DESTINATION  .
    ARCHIVE DESTINATION  .
)

