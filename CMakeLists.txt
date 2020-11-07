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


function(gccx sources_target cpx_sources cpp_output_dir)
  add_custom_target(${sources_target})

  # gccx doesn't have a nice way to operate on multiple files at once, and the directory input is inflexible when other
  # exist in the dir, so call it one by one on each file
  foreach(cpx_source ${cpx_sources})
    get_filename_component(name ${cpx_source} NAME_WE)
    gccx_internal(${name}-target ${cpx_source} ${cpp_output_dir})
    add_dependencies(${sources_target} ${name}-target)  
  endforeach()
endfunction()

# Runs gccs on .CPX source file to generate cpp file in ${CMAKE_CURRENT_BINARY_DIR}
function(gccx_internal target cpx_source cpp_output_dir)
  get_filename_component(name ${cpx_source} NAME_WE)

  add_custom_command(
    OUTPUT
      ${cpp_output_dir}/${name}.cpp
    COMMAND
      echo "gccx ${cpx_source} -o ${cpp_output_dir}/${name}.cpp"
    COMMAND
      gccx ${cpx_source} -o ${cpp_output_dir}/${name}.cpp
    DEPENDS
      ${cpx_source}
    USES_TERMINAL
    )
  # Wrap with custom targets to avoid running the command multiple times if multiple targets depend on the output
  # https://samthursfield.wordpress.com/2015/11/21/cmake-dependencies-between-targets-and-files-and-custom-commands/
  # And don't simply put the command in the custom target directly or it will run everytime since custom targets are 
  # always out of date
  add_custom_target(${target}
    DEPENDS
      ${cpp_output_dir}/${name}.cpp
  )
endfunction()

if (BUILD_ASM_DOM_EXAMPLE)
    add_subdirectory(example)
endif()