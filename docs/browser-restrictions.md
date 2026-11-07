# Browser restrictions

These restrictions apply independently of the project's implementation progress:

- **Networking:** ordinary web pages cannot directly use Unreal's native UDP transport. This fork bridges WebTransport or secure WebSocket through a separate gateway. Browser listen servers, raw TCP/UDP sockets and LAN discovery are unavailable.
- **Native integration:** desktop DLLs, external processes and unrestricted filesystem access are unavailable. Integrations must use WebAssembly-compatible code and browser APIs.
- **User interaction:** audio playback may require a click or key press. Fullscreen and pointer lock also follow browser activation and permission policies. See [browser audio policies](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Best_practices#autoplay_policy).
- **Video:** codec support depends on the browser, OS and hardware. Cross-origin video URLs must allow CORS to be used as textures; HLS/DASH also require it for manifests and referenced segments. Codec support and DRM restrictions still apply; a playable URL alone is not sufficient. Unmuted playback follows browser autoplay rules. See [browser video requirements](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video).
- **Storage:** saves and cached assets are subject to browser quotas, eviction and users clearing site data. See [browser storage behavior](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API/Origin_private_file_system).
- **Hosting and capabilities:** this build requires HTTPS or localhost, cross-origin isolation, WebGPU, transferable OffscreenCanvas and WebAssembly JSPI. WebGPU availability alone does not guarantee compatibility.

The default 4 GiB memory ceiling and disabled rendering parallelism are current
implementation constraints, rather than universal browser limits. See
[Platform status](platform-status.md) for the implementation roadmap.
