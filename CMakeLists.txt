project(imgui)

add_library(imgui STATIC
${CMAKE_CURRENT_SOURCE_DIR}/imgui.h
${CMAKE_CURRENT_SOURCE_DIR}/imgui.cpp
${CMAKE_CURRENT_SOURCE_DIR}/imconfig.h
${CMAKE_CURRENT_SOURCE_DIR}/imgui_demo.cpp
${CMAKE_CURRENT_SOURCE_DIR}/imgui_draw.cpp
${CMAKE_CURRENT_SOURCE_DIR}/imgui_internal.h
${CMAKE_CURRENT_SOURCE_DIR}/imgui_tables.cpp
${CMAKE_CURRENT_SOURCE_DIR}/imgui_widgets.cpp
${CMAKE_CURRENT_SOURCE_DIR}/imstb_rectpack.h
${CMAKE_CURRENT_SOURCE_DIR}/imstb_textedit.h
${CMAKE_CURRENT_SOURCE_DIR}/imstb_truetype.h

${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.h ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.h ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_vulkan.h ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_vulkan.cpp
)

set(CMAKE_MODULE_PATH $ENV{VULKAN_SDK})
find_package(Vulkan REQUIRED)

target_include_directories(imgui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} ${Vulkan_INCLUDE_DIRS})

target_link_libraries(imgui glfw)
