# Create your first sample in the editor

Launch the editor you just built from the same Terminal environment:

```bash
cd "$HOME/Developer/UnrealEngine-WebGPU-Source"
source "$HOME/Developer/emsdk/emsdk_env.sh"
Engine/Binaries/Mac/UnrealEditor
```

On Windows, launch `Engine\Binaries\Win64\UnrealEditor.exe` from the prompt
described in [Build on Windows](build-windows.md).

1. Create a **Games → Third Person** project using **Blueprint** and the
   default variant (**None**). Name it `HTML5ThirdPerson` and save it outside
   the engine source directory, for example under `~/UnrealProjects`.
2. For this basic sample, open **Edit → Plugins** and disable **Gameplay
   StateTree** if enabled. The template enables it, but the shared `UnrealGame`
   executable built in
   [Build the editor, packaging tools and browser runtime](build-macos.md#build-the-editor-packaging-tools-and-browser-runtime)
   does not include it. Projects that use its features need their own target
   build. Save and restart the editor when prompted.
3. Open `Content/ThirdPerson/Lvl_ThirdPerson` and save the project.
4. In **Project Settings → Maps & Modes**, set **Game Default Map** to
   `Lvl_ThirdPerson`. Set **Editor Startup Map** to the same level if desired.
5. In **Project Settings → Packaging**, use **Development** for the first
   build. Keep **Use Pak File** enabled. In **List of maps to include in a
   packaged build**, add `/Game/ThirdPerson/Lvl_ThirdPerson`.
6. Check **Platforms → SDK Management → HTML5**. The installed SDK should
   show **6.0.10** when using the pinned emsdk setup from
   [Prepare macOS](build-macos.md#prepare-macos). A blank AutoSDK entry is
   expected: this platform uses a manually installed SDK.

To reopen this project with the correct engine and SDK environment:

```bash
cd "$HOME/Developer/UnrealEngine-WebGPU-Source"
source "$HOME/Developer/emsdk/emsdk_env.sh"
Engine/Binaries/Mac/UnrealEditor \
  "$HOME/UnrealProjects/HTML5ThirdPerson/HTML5ThirdPerson.uproject"
```

Use your actual project path if you chose a different location. Normal editor
Play runs the native preview; it does not launch the browser build.

---

Next: [Package the sample from the editor](package-project.md)
