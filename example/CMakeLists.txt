project (asm-dom-example)

set(GCCX_GEN_TARGET gccx-main)

gccx(${GCCX_GEN_TARGET} ${CMAKE_CURRENT_SOURCE_DIR}/main.cpx ${CMAKE_CURRENT_BINARY_DIR}/generated)
add_executable(asm-dom-example 
  # Generated location
  ${CMAKE_CURRENT_BINARY_DIR}/generated/main.cpp
)
# depend on the GCCX_GEN_TARGET to ensure main.cpp has been generated
add_dependencies(asm-dom-example ${GCCX_GEN_TARGET})
target_link_libraries(asm-dom-example 
  PRIVATE 
    asm-dom
)

configure_file(
  index.html
  ${CMAKE_CURRENT_BINARY_DIR}/index.html
)
configure_file(
  style.css
  ${CMAKE_CURRENT_BINARY_DIR}/style.css
)

#add_subdirectory(todomv-cpx)
