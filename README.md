# UnrealEngine-WebGPU

**WebGPU rendering and HTML5/WebAssembly support for Unreal Engine.**

[![Sponsor PooyaEimandar](https://img.shields.io/badge/Sponsor-PooyaEimandar-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/PooyaEimandar)
[![Documentation license: CC BY 4.0](https://img.shields.io/badge/Documentation-CC_BY_4.0-blue)](LICENSE)

UnrealEngine-WebGPU is an independent project bringing a WebGPU
renderer and an HTML5 target platform to Unreal Engine. The renderer runs
natively through Dawn and in the browser through WebAssembly and WebGPU.

This public repository contains project information and documentation.
The engine implementation lives in the restricted
[Unreal Engine fork](https://github.com/PooyaEimandar/UnrealEngine/tree/5.8-webgpu),
where source access follows Epic Games' licensing and GitHub access rules.

## What the project provides

- A WebGPU rendering hardware interface, shared between native and browser builds.
- Offline shader compilation from Unreal's HLSL to WGSL.
- An HTML5 platform built with Emscripten and WebAssembly threads.
- Cooking and packaging into a static web application.
- Browser keyboard, mouse, touch and standard gamepad input.
- Resize, pixel-density, fullscreen and pointer-lock handling.
- Web Audio output through Unreal's audio mixer and an AudioWorklet.
- Ongoing work on standard Unreal platform services, including HTTP.

The browser target runs packaged games. The Unreal Editor remains a native
application. This is experimental development work; compatibility and
performance are still being validated.

## Current status

As of **October 16, 2026**, the working branch is **`5.8-webgpu`**, based on
**Unreal Engine 5.8.3**. Development and build verification currently focus
on macOS.

## Access the source

1. Follow [Epic's Unreal Engine GitHub access guide](https://www.unrealengine.com/ue-on-github)
   to link your Epic Games and GitHub accounts and accept the organization invitation.
2. While signed in to GitHub, confirm that you can open
   [EpicGames/UnrealEngine](https://github.com/EpicGames/UnrealEngine).
3. Open [PooyaEimandar/UnrealEngine](https://github.com/PooyaEimandar/UnrealEngine)
   and select the **`5.8-webgpu`** branch.
4. Follow the HTML5 platform's build instructions in that branch.

Once access is available, clone the branch with:

```bash
git clone --branch 5.8-webgpu --single-branch \
  https://github.com/PooyaEimandar/UnrealEngine.git
```

A GitHub **404** can mean that your signed-in account does not have access.
If Epic's repository is accessible but this fork is not, report the access
problem here so the fork's permissions can be checked. Access is subject to
Epic's terms; this public repository does not grant an Unreal Engine license.

## Contribute

Browser/device compatibility reports, reproducible bug reports, documentation
improvements and testing feedback are welcome in this repository's issues.
Include the engine branch or revision, operating system, browser version,
GPU and reproduction steps when reporting a problem.

Submit engine implementation changes and discussions containing Engine Code
in the restricted fork. Keep public reports focused on behavior and diagnostics
that can be shared publicly.

## Support development

If this project is useful to you, you can support its continued development,
testing and maintenance through **GitHub Sponsors**:

### [Sponsor PooyaEimandar](https://github.com/sponsors/PooyaEimandar)

Sponsorship supports development and is separate from purchasing a plugin
license. It does not automatically include paid add-ons or support plans.

Bug reports, testing on additional hardware and contributions to the
documentation also help move the project forward.

## License and attribution

The original documentation in this public repository is copyright
**2026 [PooyaEimandar](https://github.com/PooyaEimandar)** and licensed under
[Creative Commons Attribution 4.0 International](LICENSE). Reuse is permitted
with appropriate attribution, a license link and an indication of changes.

This documentation license does **not** apply to Unreal Engine source code,
engine modifications, commercial plugins, premium add-on code,
Epic-provided assets or third-party components.
Unreal Engine and its modifications remain subject to the
[Unreal Engine EULA](https://www.unrealengine.com/eula/unreal); third-party
components retain their respective licenses.

Unreal Engine is a trademark or registered trademark of Epic Games, Inc.
This is an independent community project and is not an official Epic Games
product or endorsed by Epic Games.
