project (todomvc)

set(GCCX_GEN_TARGET gccx-todomvc)

gccx(${GCCX_GEN_TARGET} 
     "${CMAKE_CURRENT_SOURCE_DIR}/src/task.cpx;${CMAKE_CURRENT_SOURCE_DIR}/src/todos.cpx" 
     ${CMAKE_CURRENT_BINARY_DIR}/generated
)

add_executable(todomvc 
  src/app.cpp
  src/helpers.cpp
  # Generated location
  ${CMAKE_CURRENT_BINARY_DIR}/generated/task.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/generated/todos.cpp
)
# depend on the GCCX_GEN_TARGET to ensure main.cpp has been generated
add_dependencies(todomvc ${GCCX_GEN_TARGET})
target_link_libraries(todomvc
  PRIVATE 
    asm-dom
)
# ensure generated files can find local includes
target_include_directories(todomvc 
  PRIVATE
    src
)

configure_file(
  index.html
  ${CMAKE_CURRENT_BINARY_DIR}/index.html
)
