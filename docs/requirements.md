# Before you build

| Requirement | What you need |
|---|---|
| Source access | A GitHub account linked to Epic Games, with access to Epic's engine repository and this fork |
| Build host | An Apple Silicon Mac, a Windows 10/11 x64 PC (see [Build on Windows](build-windows.md)) or a Linux x64 machine with a case-sensitive filesystem (see [Build on Linux](build-linux.md)) |
| Xcode (macOS) | Full Xcode with its macOS SDK, selected as the active developer directory. The fork's Apple SDK configuration targets Xcode 26.1.1. Command Line Tools alone are insufficient. |
| Rosetta (macOS) | Required by the Intel `protoc` executable bundled with UE's build dependencies |
| Visual Studio (Windows) | Visual Studio 2022 with the C++ desktop workload and Windows SDK that Unreal Engine 5.8 requires; its x64 Native Tools prompt is used for the shader tools |
| Build tools | Git, Python 3.9+ (3.11+ for the pipeline script), CMake 3.22+ and Ninja; on Linux also clang and the packages listed in the fork's container image |
| Rust/Cargo | Stable Rust toolchain to build the bundled native [Sib host](https://github.com/PooyaEimandar/sib); the MSVC toolchain on Windows |
| Emscripten | Use **6.0.10**, the version used for project validation; Node.js is supplied by emsdk |
| Browser | Browser with WebGPU, WebAssembly JSPI and transferable OffscreenCanvas |

Local validation used macOS 27.0.1, Emscripten 6.0.10 and Chrome
155.0.8059.81. Safari version 27.0.1 & Firefox Nightly 155.0a1 also rendered the sample. Linux validation used Ubuntu 24.04 x64 in
Docker with UE's Clang 20.1.8, Emscripten 6.0.10, Node 24.22.2 and Rust 1.99.1;
Chrome 156 on a Mac rendered the Linux-built packages. These are recorded
results, not minimum supported browser versions. The generated page checks
required capabilities before downloading the game.

---

Next: [Get access to the source](source-access.md)
