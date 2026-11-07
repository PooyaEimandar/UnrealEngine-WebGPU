# Platform status

The current branch is **`5.8-webgpu`**, based on **Unreal Engine 5.8.3**.

| Build host | Status |
|---|---|
| macOS on Apple Silicon | Current development host: editor, native WebGPU preview, packaging and browser checks are all verified here |
| Linux x64 | Supported for building and packaging: the Linux editor, shader tools and both HTML5 Development and Shipping packages were validated in the fork's pinned Ubuntu 24.04 container (Docker, x64 emulation on Apple Silicon). See [Build on Linux](build-linux.md). Running the editor UI, native WebGPU preview and a browser on Linux are not validated yet. |
| Windows x64 | Supported for building and packaging with the Visual Studio toolchain, `RunHTML5CI.py --host Win64`, `run.bat` and a Windows host executable. See [Build on Windows](build-windows.md). A recorded end-to-end Windows run is still outstanding. |

## Feature support and roadmap

Status describes the current implementation. Planned work has no committed
release date. **Not scheduled** means there is no committed implementation
plan; **Unverified** means compatibility still needs testing.

| Feature | Status | Current behavior | Planned behavior |
|---|---|---|---|
| WebGPU rendering | Supported | Uses Unreal's mobile renderer in the browser and through Dawn natively. Both share rendering limitations. | - |
| Editor packaging | Supported | HTML5 cooking and packaging, Development/Shipping builds and local server launch scripts. | - |
| Packaged hosting | Supported | Bundled Rust host built on Sib serves Wasm, JavaScript and streamed assets. HTTPS prefers HTTP/2 with HTTP/1.1 fallback; localhost asset-only HTTP is also supported. The host is built for the packaging machine on all three hosts; the Mac and Linux binaries pass the packaged serving checks. | Record the Windows host checks. |
| Core gameplay | Supported | C++/Blueprint gameplay, standard skeletal animation, core Chaos physics and Slate/UMG remain included. | - |
| Standard input | Supported | Keyboard, mouse, touch, standard gamepads, pointer lock and fullscreen. | - |
| Audio playback | Supported | UE audio mixer output through stereo Web Audio. | - |
| HTTP requests | Supported | Standard UE HTTP APIs use browser fetch. | - |
| Persistent saves | Supported | UE save APIs use browser-managed persistent storage. | - |
| Asset streaming | Supported | Cooked pak/IoStore content loads through HTTP ranges, with bounded caching, retries and progress reporting. | - |
| Crash diagnostics | Supported | Browser error reporting, captured logs and development symbol support. | - |
| Multiplayer | Supported | Browser WebTransport datagrams or secure WebSocket connect through a Sib gateway to a native UE UDP server. Replication, RPCs, native/browser co-play and explicit reconnect pass local tests. | Real-user/WAN testing, longer endurance and capacity measurements, account-service integration and broader platform coverage. |
| Video and streaming media | Partial | Standard UE Media Player and Media Texture support files, video URLs, HLS and DASH. Native HLS or packaged Shaka feeds the shared WebGPU frame path, with texture FPS limits and buffer options. Packaged MP4/WebM and HLS/DASH materials are verified on Chrome/Mac, including one-face cube playback, seek, rate, reopen and looping checks. Optional audio uses the browser directly. | Real live-stream and adaptive-bitrate tests, broader browser/device coverage, WebRTC and UE spatial media audio. |
| Advanced content delivery | Planned | No managed downloadable packs, persistent content cache or prefetch policies. | Add downloadable content, versioned caching, prefetch and storage-budget management. |
| WebGPU virtual geometry | Under evaluation | No custom virtual geometry implementation is integrated. | Investigate meshlets, GPU LOD/culling and geometry streaming. Unreal Nanite compatibility and performance parity are not promised. |
| Unreal Nanite | Not scheduled | Unsupported by the current rendering path. | - |
| Desktop rendering features | Not scheduled | Lumen, hardware ray tracing, Path Tracer, Virtual Shadow Maps and TSR are unavailable. | - |
| Additional rendering features | Not scheduled | Virtual textures, mesh distance fields, volumetric clouds, strand-based hair, Compute Framework mesh deformers, sorted-triangle OIT and Substrate specular profiles are disabled or unsupported. | - |
| Advanced input | Not scheduled | IME composition, mobile virtual keyboards, controller vibration and nonstandard gamepad mappings are unavailable. | - |
| Additional audio features | Not scheduled | Microphone capture, voice chat, surround output and output-device selection are unavailable. | - |
| Rendering optimizations | Not scheduled | Parallel RHI execution, browser PSO precaching and GPU timestamp queries are unavailable. | - |
| Browser Launch On | Not scheduled | No browser device appears in the editor's Launch On menu. Packaging and server scripts work. | - |
| Dynamic modules and Live Coding | Not scheduled | The browser executable is monolithic. Dynamic module loading, hot reload and Live Coding are unavailable. | - |
| Larger Wasm memory | Partial | Default memory mode has a 4 GiB heap ceiling. Optional native Memory64 requires additional browser support. | - |
| Native plugins and SDKs | Requires adaptation | Plugins with desktop binaries or OS dependencies cannot be assumed compatible. | No general porting commitment; individual integrations require evaluation. |

Limits that come from the browser itself, independent of this implementation,
are listed in [Browser restrictions](browser-restrictions.md).
