find_package(glfw3 3.3 REQUIRED)

add_precompiled_vulkan_spirv(rpcsx-gpu-shaders
    shaders/fill_red.frag.glsl
    shaders/flip_std.frag.glsl
    shaders/flip_alt.frag.glsl
    shaders/flip.vert.glsl
    shaders/rect_list.geom.glsl
)

add_executable(rpcsx-gpu2
    Cache.cpp
    main.cpp
    Device.cpp
    FlipPipeline.cpp
    Pipe.cpp
    Registers.cpp
    Renderer.cpp
)

target_link_libraries(rpcsx-gpu2
PUBLIC
    rpcsx-gpu-shaders
    amdgpu::bridge
    rx
    gcn-shader
    glfw
    amdgpu::tiler::cpu
    amdgpu::tiler::vulkan
    rdna-semantic-spirv
    gnm::vulkan
    gnm
)

install(TARGETS rpcsx-gpu2 RUNTIME DESTINATION bin)
set_target_properties(rpcsx-gpu2 PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
target_base_address(rpcsx-gpu2 0x0000060000000000)
add_subdirectory(lib)
