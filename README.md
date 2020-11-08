# cmake-asm-dom-wrapper

This project wraps [asm-dom](https://github.com/mbasso/asm-dom) and [gccx](https://github.com/mbasso/gccx) in CMake for easy integration into C++ projects.

This makes it simple to build dynamic single page web apps entirely in C++ (or [CPX](https://github.com/mbasso/gccx/blob/master/docs/syntax.md), JSX-like syntax for C++), using standard CMake and the [Emscripten](https://emscripten.org/) toolchain for WebAssembly, without writing any JavaScript!

This project is similar to and was inspired by [asm-dom-cmake](https://github.com/ArthurSonzogni/asm-dom-cmake), but makes some different design decisions and is ready to be dropped into any new C++ WASM project as a submodule.

The [button-counter CPX example](example/button-counter/) is taken from asm-dom-cmake with modifications to the build. 
[Live demo](https://jimmyorourke.github.io/cmake-asm-dom-wrapper/example-build/button-counter).

The [todo-mvc CPX example](example/todomvc-cpp) is based on an [original example](https://github.com/mbasso/asm-dom/tree/master/examples/todomvc%20-%20cpx) from asm-dom. [Live demo](https://jimmyorourke.github.io/cmake-asm-dom-wrapper/example-build/todomvc-cpp).

## Installing and Building
* Install CMake, Emscripten, and Ninja (or alternatively use Make) 
* Install `gccx` through npm (npm typically gets installed with Emscripten)
* Git clone recursively (to get the asm-dom submodule) right into your project
* Run `gccx` at build time through CMake and link against `asm-dom` as per [the examples](example/button-counter/CMakeLists.txt)

To build the examples
```
cd cmake-asm-dom-wrapper
mkdir build && cd build
emcmake cmake -G Ninja .. -DBUILD_ASM_DOM_EXAMPLE=1
ninja
```
Then to run, `emrun example/todomvc-cpp/index.html --no_browser` and open `http://localhost:6931` in your browser. Alternatively use Python's `http.server`.
