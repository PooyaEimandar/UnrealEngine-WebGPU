# Build on Linux

Linux x64 is a validated build and packaging host. The validated environment
is the fork's pinned Ubuntu 24.04 container, described in
[`Engine/Platforms/HTML5/Build/Linux`](https://github.com/PooyaEimandar/UnrealEngine/blob/5.8-webgpu/Engine/Platforms/HTML5/Build/Linux/README.md),
which also runs under Docker with x64 emulation on Apple Silicon. A native
Ubuntu 24.04 x64 machine with the same tools should behave the same, but its
build times were not measured.

What you need:

- A **case-sensitive** filesystem for the checkout. The pipeline refuses
  case-insensitive Linux checkouts because they hide path errors.
- The container image, or its packages: `build-essential`, `clang`, `cmake`,
  `ninja-build`, `git`, `python3`, plus the X11, Wayland and Vulkan development
  headers listed in the Dockerfile. UE's own Clang toolchain is downloaded by
  `Setup.sh`.
- Node.js 24+, Rust 1.99 with Clippy and Emscripten 6.0.10 on `PATH`.
- A non-root user that owns the checkout; UE's commandlets refuse to run as root.
- Plenty of disk and memory. A Shipping run passed with a 32 GiB Docker VM and a
  28 GiB container limit; a Development run needs the line-table debug option
  below, because full-debug Wasm optimization ran out of memory even at 56 GiB.

Clone the fork as shown in
[Clone and build the engine](build-macos.md#clone-and-build-the-engine), then
run from the engine root:

```bash
./Setup.sh --force
python3 Engine/Source/ThirdParty/Dawn/BuildForUE/BuildDawn.py --jobs 4
python3 Engine/Source/ThirdParty/Dawn/BuildForUE/BuildTint.py --jobs 4
Engine/Platforms/HTML5/Build/BuildThirdParty.sh --jobs 4
python3 Engine/Platforms/HTML5/Build/Scripts/RunHTML5CI.py --host Linux --check
```

`--check` only verifies the prepared dependencies. The pipeline below builds the
Linux editor, ShaderCompileWorker and UnrealPak, builds the HTML5 game, then
cooks and packages a copy of the Third Person template into a fresh output
directory, with a `.tar.gz` archive and checksums:

```bash
BINARYEN_CORES=1 MALLOC_ARENA_MAX=2 \
python3 Engine/Platforms/HTML5/Build/Scripts/RunHTML5CI.py \
  --host Linux --configuration Shipping --jobs 8 \
  --output /absolute/path/results/linux-shipping-001
```

For a Development package, use `--configuration Development --wasm-debug-info
line-tables` and a new output directory. To package your own project on Linux,
use UAT from the command line; the editor UI has not been exercised on Linux:

```bash
Engine/Build/BatchFiles/RunUAT.sh BuildCookRun \
  -project=/absolute/path/MyGame/MyGame.uproject -platform=HTML5 \
  -clientconfig=Shipping -cook -stage -pak -archive \
  -archivedirectory=/absolute/path/MyGame-HTML5 -unattended
```

The package contains a Linux `html5-gateway`; start it with `./run.sh` as
[Run the packaged game in a browser](run-packaged-game.md) describes. The
Linux records are in the fork's
[Development](https://github.com/PooyaEimandar/UnrealEngine/blob/5.8-webgpu/Engine/Platforms/HTML5/docs/M10-CI.md#linux-docker-validation-2026-11-02)
and
[Shipping](https://github.com/PooyaEimandar/UnrealEngine/blob/5.8-webgpu/Engine/Platforms/HTML5/docs/M10-CI.md#linux-shipping-validation-2026-11-03)
validation notes. Not validated on Linux: running the editor UI, the native
`-webgpu` preview (Linux GPU rendering) and a browser run on Linux itself; the
Linux-built packages were rendered in Chrome on a Mac.

---

Next: [Run the packaged game in a browser](run-packaged-game.md)
