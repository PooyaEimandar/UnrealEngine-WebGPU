# Build on macOS

This page covers the macOS host, from installing the tools to building the
editor, the packaging tools and the browser runtime. For a Windows host, follow
[Build on Windows](build-windows.md) instead. For a Linux host, follow
[Build on Linux](build-linux.md) instead, then package from the command line as
that page shows.

## Prepare macOS

Install full [Xcode](https://developer.apple.com/xcode/), open it once and
complete its component installation and license prompts. Then select it:

```bash
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
xcodebuild -version
```

Adjust the path if Xcode is installed elsewhere. Install Rosetta if it is
missing, following [Apple's instructions](https://support.apple.com/en-us/102527):

```bash
softwareupdate --install-rosetta
```

Install [Homebrew](https://docs.brew.sh/Installation) and follow its shell
setup instructions, then install the build utilities:

```bash
brew install cmake ninja python
git --version
python3 --version
cmake --version
ninja --version
```

Install stable Rust and Cargo using the
[official rustup instructions](https://rust-lang.org/tools/install/), then open
a new Terminal and verify:

```bash
rustc --version
cargo --version
```

The HTML5 build and packaging steps compile the host automatically. Cargo uses the
repository's lockfile and pinned
[Sib revision `806b59fa59f0`](https://github.com/PooyaEimandar/sib/commit/806b59fa59f060994f663c2faad52dcee00e4845);
a separate Sib checkout is unnecessary. To use a prebuilt host, set
`HTML5_GATEWAY_BINARY` to its absolute path before building or packaging, and
keep its `HTML5Host-Licenses.txt` alongside it.

Install the pinned Emscripten version using the
[official emsdk workflow](https://emscripten.org/docs/getting_started/downloads.html):

```bash
mkdir -p "$HOME/Developer"
git clone https://github.com/emscripten-core/emsdk.git "$HOME/Developer/emsdk"
cd "$HOME/Developer/emsdk"
./emsdk install 6.0.10
./emsdk activate 6.0.10
source ./emsdk_env.sh
emcc --version
node --version
```

Run `source "$HOME/Developer/emsdk/emsdk_env.sh"` in each new Terminal session
used to build or launch the editor. Activating emsdk does not automatically
give Finder-launched applications its environment.

An existing Homebrew Emscripten installation can also be used. Check its version
with `emcc --version`; the current formula may differ from the validated version.
The editor detects standard Homebrew locations automatically, while custom
SDK installations must be available through `EMSDK`, `EMSCRIPTEN` or `PATH`.

## Clone and build the engine

Clone the **engine fork**, separate from this public documentation repository:

```bash
git clone --branch 5.8-webgpu --single-branch \
  https://github.com/PooyaEimandar/UnrealEngine.git \
  "$HOME/Developer/UnrealEngine-WebGPU-Source"
cd "$HOME/Developer/UnrealEngine-WebGPU-Source"
source "$HOME/Developer/emsdk/emsdk_env.sh"
```

Run the following commands from that source directory, in order. Finish each
step successfully before continuing. Keep the editor closed while building.

### Download engine dependencies and check the SDK

```bash
./Setup.sh
Engine/Platforms/HTML5/HTML5Setup.sh
Engine/Build/BatchFiles/RunUBT.sh -Mode=ValidatePlatforms -Platforms=HTML5
```

`Setup.sh` downloads UE's dependencies, including its bundled .NET SDK.
`HTML5Setup.sh` checks prerequisites; it does not install them. SDK validation
must report **HTML5 VALID** before proceeding.

### Build WebGPU dependencies

```bash
python3 Engine/Source/ThirdParty/Dawn/BuildForUE/BuildDawn.py --jobs 6 --test
python3 Engine/Source/ThirdParty/Dawn/BuildForUE/BuildTint.py --jobs 6
Engine/Platforms/HTML5/Build/BuildThirdParty.sh --jobs 6
```

These build native Dawn, the Tint/SPIR-V shader tools and the libraries needed
by the WebAssembly target. The scripts use the repository's pinned dependency
versions. Lower `--jobs` if memory is limited.

### Build the editor, packaging tools and browser runtime

```bash
./GenerateProjectFiles.sh
Engine/Build/BatchFiles/RunUBT.sh UnrealEditor Mac Development -NoUBA
Engine/Build/BatchFiles/RunUBT.sh \
  -Target="ShaderCompileWorker Mac Development" \
  -Target="UnrealPak Mac Development" -NoUBA
Engine/Build/BatchFiles/RunUBT.sh UnrealGame HTML5 Development -NoUBA
```

The first build can take a substantial amount of time. The editor build includes
the HTML5 target modules; `UnrealGame HTML5 Development` produces the shared
browser executable used by the simple Blueprint sample in
[Create your first sample in the editor](create-sample-project.md). C++
projects or projects with additional runtime plugins may require their own game
target.

Do not run another build or cook against this checkout at the same time.
For host-specific failures, including the known Xcode 27 post-build issue,
see the fork's [troubleshooting guide](https://github.com/PooyaEimandar/UnrealEngine/blob/5.8-webgpu/Engine/Platforms/HTML5/docs/TROUBLESHOOTING.md).

---

Next: [Create your first sample in the editor](create-sample-project.md)
