project (button-counter)

set(GCCX_GEN_TARGET gccx-button-counter)

gccx(${GCCX_GEN_TARGET} 
     ${CMAKE_CURRENT_SOURCE_DIR}/main.cpx 
     ${CMAKE_CURRENT_BINARY_DIR}/generated
)

add_executable(button-counter 
  # Generated location
  ${CMAKE_CURRENT_BINARY_DIR}/generated/main.cpp
)
# depend on the GCCX_GEN_TARGET to ensure main.cpp has been generated
add_dependencies(button-counter ${GCCX_GEN_TARGET})
target_link_libraries(button-counter 
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
