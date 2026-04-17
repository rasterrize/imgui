cmake_minimum_required(VERSION 3.31)

project(imgui)

option(IMGUI_USE_VOLK "Use volk with the vulkan backend" FALSE)
option(IMGUI_USE_BACKEND_GLFW "Use the GLFW backend" FALSE)
option(IMGUI_USE_BACKEND_OPENGL3 "Use the OpenGL3 backend" FALSE)
option(IMGUI_USE_BACKEND_VULKAN "Use the Vulkan backend" FALSE)
option(IMGUI_USE_STDLIB "Use support for C++ std types" FALSE)

# ImGui defaults to a filename of <volk.h>, but VulkanSDK has the header file under a volk subdirectory
set(IMGUI_VOLK_FILENAME <volk/volk.h>)

add_library(imgui STATIC
    imgui.h
    imgui.cpp
    imconfig.h
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_tables.cpp
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
)

if(IMGUI_USE_BACKEND_GLFW)
    target_sources(imgui PRIVATE
        backends/imgui_impl_glfw.h
        backends/imgui_impl_glfw.cpp
    )

    target_link_libraries(imgui glfw)
endif()

if(IMGUI_USE_BACKEND_OPENGL3)
    target_sources(imgui PRIVATE
        backends/imgui_impl_opengl3.h
        backends/imgui_impl_opengl3.cpp
    )
endif()

if(IMGUI_USE_BACKEND_VULKAN)
    find_package(Vulkan REQUIRED COMPONENTS volk)

    target_sources(imgui PRIVATE
        backends/imgui_impl_vulkan.h
        backends/imgui_impl_vulkan.cpp
    )

    if(IMGUI_USE_VOLK)
        target_compile_definitions(imgui PRIVATE
            IMGUI_IMPL_VULKAN_USE_VOLK
            IMGUI_IMPL_VULKAN_VOLK_FILENAME=${IMGUI_VOLK_FILENAME}
        )

        target_link_libraries(imgui Vulkan::volk)
    endif()

    target_link_libraries(imgui Vulkan::Vulkan)
endif()

if(IMGUI_USE_STDLIB)
    target_sources(imgui PRIVATE
        misc/cpp/imgui_stdlib.h
        misc/cpp/imgui_stdlib.cpp
    )
endif()

target_include_directories(imgui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
