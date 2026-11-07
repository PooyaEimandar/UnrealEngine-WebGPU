# Build on Windows

Windows 10 or 11 x64 builds and packages HTML5 games with the Visual Studio
toolchain. Install [Visual Studio 2022](https://visualstudio.microsoft.com/)
with the C++ workload Unreal Engine 5.8 requires, Git, Python 3.11+, CMake
3.22+, Ninja and [stable Rust](https://rust-lang.org/tools/install/) with its
MSVC toolchain. Install the pinned Emscripten with emsdk, then activate it in
every prompt you build from:

```bat
git clone https://github.com/emscripten-core/emsdk.git %USERPROFILE%\Developer\emsdk
cd %USERPROFILE%\Developer\emsdk
emsdk install 6.0.10
emsdk activate 6.0.10
emsdk_env.bat
emcc --version
```

Emscripten 6.0.10 installs native `emcc.exe` launchers on Windows; the fork
finds those as well as the `.bat` launchers of older SDKs, through `EMSDK`,
`EMSCRIPTEN` or `PATH`. Clone the fork as shown in
[Clone and build the engine](build-macos.md#clone-and-build-the-engine), then
run the following from an **x64 Native Tools Command Prompt for VS 2022** at
the engine root, after `emsdk_env.bat`:

```bat
Setup.bat
GenerateProjectFiles.bat
Engine\Build\BatchFiles\RunUBT.bat -Mode=ValidatePlatforms -Platforms=Win64+HTML5
python Engine\Source\ThirdParty\Dawn\BuildForUE\FetchDawn.py --platform win64
python Engine\Source\ThirdParty\Dawn\BuildForUE\BuildTint.py --jobs 6
python Engine\Platforms\HTML5\Build\BuildThirdParty.py --jobs 6
```

SDK validation must report both **Win64 VALID** and **HTML5 VALID**. Windows
uses Dawn's prebuilt release library (`FetchDawn.py`); `BuildTint.py` builds the
Tint and SPIR-V tools with the Visual Studio compiler, and `BuildThirdParty.py`
builds the WebAssembly libraries without a POSIX shell. Then build the editor,
the packaging tools and the browser runtime:

```bat
Engine\Build\BatchFiles\RunUBT.bat UnrealEditor Win64 Development -NoUBA
Engine\Build\BatchFiles\RunUBT.bat -Target="ShaderCompileWorker Win64 Development" -Target="UnrealPak Win64 Development" -NoUBA
Engine\Build\BatchFiles\RunUBT.bat UnrealGame HTML5 Development -NoUBA
```

Launch `Engine\Binaries\Win64\UnrealEditor.exe` from the same prompt and
continue with [Create your first sample in the editor](create-sample-project.md)
and [Package the sample from the editor](package-project.md); the packaged
folder contains `run.bat` and a Windows `html5-gateway.exe`, which
[Run the packaged game in a browser](run-packaged-game.md) starts with
`run.bat`. The fork's `RunHTML5CI.py --host Win64` runs the same build, cook
and package pipeline unattended, as [Build on Linux](build-linux.md) shows for
`--host Linux`. The Mac and Linux validation records exist; a recorded
end-to-end Windows run is still outstanding, so report Windows problems in
this repository's issues.

---

Next: [Create your first sample in the editor](create-sample-project.md)
