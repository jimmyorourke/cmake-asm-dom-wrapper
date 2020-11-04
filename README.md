# cmake-asm-dom-wrapper

This project wraps [asm-dom](https://github.com/mbasso/asm-dom) and [gccx](https://github.com/mbasso/gccx) in CMake for easy integration into C++ projects.

This makes it simple to build dynamic single page web apps entirely in C++ (or CPX, JSX like syntax for C++), using standard CMake and the [Emscripten](https://emscripten.org/) toolchain for WebAssembly, without writing any JavaScript!

This project is similar to and was inspired by [asm-dom-cmake](https://github.com/ArthurSonzogni/asm-dom-cmake), but makes some different design decisions and is ready to be dropped into any new C++ WASM project as a submodule.
The [CPX example](example/) is taken from asm-dom-cmake with only modifications to the build. See [@ArthurSonzogni](https://github.com/ArthurSonzogni)'s [live demo](http://arthursonzogni.github.io/asm-dom-cmake/).



## Installing and Building
* Install CMake, Emscripten, and Ninja (or alternatively use Make) 
* Install gccx through npm (npm typically gets installed with Emscripten)
* Git clone recursively (to get the asm-dom submodule) right into your project
* Run `gccx` at build time through CMake and link against asm-dom as per [the example](example/CMakeLists.txt)

To build the example
```
cd cmake-asm-dom-wrapper
mkdir build && cd build
emcmake cmake -G Ninja .. -DBUILD_ASM_DOM_EXAMPLE=1
ninja
```
Then to run it, `emrun example/index.html --no_browser` and open `http://localhost:6931` in your browser. Alternatively use Python's `http.server`.
