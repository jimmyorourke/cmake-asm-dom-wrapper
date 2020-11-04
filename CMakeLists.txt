cmake_minimum_required(VERSION 3.13)

project(asm-dom)

# link against this library to use asm-dom constructs
add_library(asm-dom
  asm-dom/cpp/asm-dom.cpp
  asm-dom/cpp/asm-dom-server.cpp
)
target_include_directories(asm-dom PUBLIC 
  asm-dom/cpp
)
target_link_options(asm-dom PUBLIC
   --bind
   -s EXTRA_EXPORTED_RUNTIME_METHODS=['UTF8ToString']
)

find_program(GCCX_FOUND gccx)
if (NOT GCCX_FOUND)
  message(FATAL_ERROR "gccx not found! Install with npm and ensure it is in the path")
endif()

# Runs gccs on .CPX source file to generate cpp file in ${CMAKE_CURRENT_BINARY_DIR}
function(gccx target cpx_source)
  get_filename_component(name ${cpx_source} NAME_WE)

  add_custom_command(
    OUTPUT
      ${CMAKE_CURRENT_BINARY_DIR}/${name}.cpp
    COMMAND
      gccx ${CMAKE_CURRENT_SOURCE_DIR}/${cpx_source} -o ${CMAKE_CURRENT_BINARY_DIR}/${name}.cpp
    DEPENDS
      ${CMAKE_CURRENT_SOURCE_DIR}/${cpx_source}
    USES_TERMINAL
    )
  # Wrap with custom targets to avoid running the command multiple times if multiple targets depend on the output
  # https://samthursfield.wordpress.com/2015/11/21/cmake-dependencies-between-targets-and-files-and-custom-commands/
  add_custom_target(${target}
    DEPENDS
      ${CMAKE_CURRENT_BINARY_DIR}/${name}.cpp
  )
endfunction()

if (BUILD_ASM_DOM_EXAMPLE)
    add_subdirectory(example)
endif()