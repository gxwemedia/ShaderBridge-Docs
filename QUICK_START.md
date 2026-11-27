# ShaderBridge — Quick Start Guide

**ShaderBridge** compiles standard `mainImage` Fragment GLSL (GLSL ES / WebGL conventions) into native Unreal Engine Material assets (`.uasset`) using a **100% offline, deterministic compiler pipeline** powered by unmodified, LunarG-signed open-source Khronos SPIR-V / SPIRV-Cross / glslang tools.

> **Built for AI-Assisted & Procedural Shader Workflows**: Pair **ShaderBridge** with any AI / LLM (ChatGPT, Claude, Gemini, or local models) — generate standard `mainImage` Fragment GLSL with your favorite AI, then let ShaderBridge compile it offline and deterministically into a native UE Material with **zero HLSL syntax hallucinations** and **zero runtime dependencies**.

---

## 1. 10-Second UI Quick Start (No Coding Required)
1. Enable **ShaderBridge** in **Edit -> Plugins** (`PythonScriptPlugin` is enabled automatically) and restart Unreal Editor if prompted.
2. Click the **ShaderBridge** button in the top Level Editor toolbar to open the native 3-tab Slate panel (**Converter** / **Samples** / **Help**).
3. Switch to the **Samples** tab, choose any of the **5 built-in original CC0 samples**, and click **Load Into Converter** (this fills the GLSL editor and default Material Name automatically).
4. Back in the **Converter** tab, optionally pick a **Domain** (`Surface (Unlit)` / `UI` / `Post Process`) and **Output Folder** (default `/Game/ShaderBridgeMaterials`), then click **Convert**.
5. Your native `.uasset` Material is compiled, saved, and synced in the **Content Browser**. Click **Open Material** in the bottom status bar to inspect or edit it immediately.

---

## 2. Requirements & Supported Engine Versions
- **Unreal Engine**: Windows 64-bit (`Win64`) Editor — officially verified via `RunUAT BuildPlugin -Rocket` on **UE 5.8** and **UE 5.9** (check your Fab listing page for the exact supported engine versions selectable in Epic Games Launcher).
- **Operating System**: Windows 10 / 11 (64-bit)
- **Built-in Engine Plugin**: `PythonScriptPlugin` (automatically enabled as a plugin dependency)
- **External Dependencies**: **None.** All required open-source compiler binaries (`glslc.exe`, `spirv-val.exe`, `spirv-opt.exe`, `spirv-cross.exe`) are bundled inside the plugin. No external Vulkan SDK installation, no Python `.venv`, no internet connection, and no API keys required.

---

## 3. Native 3-Tab Panel Overview

### Tab 1: **Converter**
- **Material Name**: Target Unreal Material asset name (default `M_ShaderBridge`, e.g., `M_MyPlasma`).
- **Output Folder**: Target Content Browser folder (default `/Game/ShaderBridgeMaterials`).
- **Domain**: Choose the generated Material Domain:
  - `Surface (Unlit)` — `MD_SURFACE` + `MSM_UNLIT` (default, ideal for meshes, VFX cards, and procedural surfaces)
  - `UI` — `MD_UI` (ready for UMG widgets and HUD elements)
  - `Post Process` — `MD_POST_PROCESS` (ready for Post Process Volumes and screen-space effects)
- **Shader Code**: Paste any single-pass `mainImage` Fragment GLSL defining `void mainImage(out vec4 fragColor, in vec2 fragCoord)`. Includes **Paste** and **Clear** helpers.
- **Convert**: Compiles GLSL offline (`glslc -> spirv-val -> spirv-opt -> spirv-cross`), validates HLSL, builds and saves the native `.uasset` Material, wires all detected uniforms under the **`ShaderBridge`** parameter group, and syncs the Content Browser.
- **Copy HLSL**: Transpiles GLSL to Unreal-ready Custom Node HLSL and copies it directly to your clipboard without creating an asset.
- **Cancel**: Cancels an in-progress background conversion job.
- **Open Material** *(bottom status bar)*: Appears once a Material is generated; opens the new `.uasset` directly in the Material Editor and syncs the Content Browser.

### Tab 2: **Samples** (5 Built-In 100% Original CC0 Shaders)
Click **Load Into Converter** on any sample card to test immediately — zero external downloads required:

| Sample Key | Title | Default Material Name | Uniforms Demonstrated |
| --- | --- | --- | --- |
| `plasma_nebula` | Plasma Nebula | `M_Showcase_PlasmaNebula` | `iTime`, `iResolution` |
| `voronoi_crystal` | Voronoi Crystal | `M_Showcase_VoronoiCrystal` | `iTime`, `iResolution` |
| `sdf_raymarch_orb` | SDF Raymarch Orb | `M_Showcase_SdfRaymarchOrb` | `iTime`, `iResolution` |
| `scifi_holo_grid` | Sci-Fi Holo Grid | `M_Showcase_ScifiHoloGrid` | `iTime`, `iResolution` |
| `textured_uv_flow` | Textured UV Flow | `M_Showcase_TexturedUvFlow` | `iTime`, `iResolution`, `iChannel0` |

### Tab 3: **Help**
Quick in-editor reference covering the `mainImage` signature, uniform conventions, Domain modes, and support links.

---

## 4. Uniform Wiring & Behavior Reference
All generated material parameters are automatically organized under the **`ShaderBridge`** parameter group for clean Material Instances:

| GLSL Uniform | GLSL Type | UE Material Node / Parameter | Default Value | Runtime Behavior in v1.0 |
| --- | --- | --- | --- | --- |
| `iTime` | `float` | Native `Time` (`MaterialExpressionTime`) | Live engine time | **Live-animated** automatically in Editor & runtime |
| `UVs` *(fragCoord)* | `vec2` | Native `TextureCoordinate` (`UV0`) | Mesh / UI UV0 | **Live** surface/UI UVs scaled by `iResolution` |
| `iResolution` | `vec3` | `VectorParameter` (`iResolution`) | `(1024, 1024, 1)` | Tunable constant parameter (not auto-bound to live viewport size; override in Material Instance) |
| `iTimeDelta` | `float` | `ScalarParameter` (`iTimeDelta`) | `0.01667` | Constant parameter (~60 FPS default; drive via Blueprint/MPC if per-frame delta is needed) |
| `iFrame` | `int` / `float` | `ScalarParameter` (`iFrame`) | `0.0` | Constant parameter (drive via Blueprint/Sequencer if frame counter is needed) |
| `iFrameRate` | `float` | `ScalarParameter` (`iFrameRate`) | `60.0` | Constant parameter |
| `iMouse` | `vec4` | `VectorParameter` (`iMouse`, XYZ) + `ScalarParameter` (`iMouse_w`, W) -> `AppendVector` | `(0, 0, 0, 0)` | Exposed parameter for Blueprint/UI interaction (not auto-bound to OS mouse cursor) |
| `iDate` | `vec4` | `VectorParameter` (`iDate`, XYZ) + `ScalarParameter` (`iDate_w`, W) -> `AppendVector` | `(0, 0, 0, 0)` | Exposed parameter (populate via Blueprint if wall-clock date/time is needed) |
| `iChannel0`–`iChannel3` | `sampler2D` | `TextureObjectParameter` (`iChannel0`–`iChannel3`) | Bundled CC0 UV checker (`/ShaderBridge/Textures/checker/checker1`, fallback `/Engine/EngineMaterials/DefaultDiffuse`) | Live 2D texture sampler; works out of the box in blank projects and swaps cleanly in Material Instances |
| `iChannelResolution` | `vec3` | `VectorParameter` (`iChannelResolution`) | `(1024, 1024, 1)` | Tunable constant parameter |
| `iChannelTime` | `float` | `ScalarParameter` (`iChannelTime`) | `0.0` | Constant parameter |
| `iSampleRate` | `float` | `ScalarParameter` (`iSampleRate`) | `44100.0` | Constant parameter |

---

## 5. Optional Python Console & Batch Automation
For scripted or batch conversion inside Unreal Editor (**Output Log -> Python**), use the **`shaderbridge_converter`** module. Use **`convert_shader_sync(...)`** for deterministic synchronous batch loops (avoiding single-job async contention):

```python
from ShaderBridge import sample_shaders
import shaderbridge_converter

# 1. List all 5 built-in CC0 samples
for item in sample_shaders.list_samples():
    print(item["key"], "->", item["material_name"])

# 2. Synchronous batch conversion across all 5 samples
for item in sample_shaders.list_samples():
    sample = sample_shaders.get_sample(item["key"])
    result = shaderbridge_converter.convert_shader_sync(
        glsl_code=sample["glsl"],
        material_name=sample["material_name"],
        output_folder="/Game/ShaderBridgeMaterials",
        material_domain="surface",  # "surface" | "ui" | "post_process"
    )
    print(item["key"], "->", result.get("material_asset_path"))
```

For single non-blocking UI-style conversion from Python, `shaderbridge_converter.convert_shader(...)` and `shaderbridge_converter.preview_hlsl(glsl_code, material_domain="surface")` are also available.

---

## 6. Language & Localization (EN / ZH)
ShaderBridge supports **English (default)** and **Simplified Chinese**:
- **Automatic Detection**: Follows Unreal Editor's active UI culture (`zh*` -> Simplified Chinese; all other cultures -> English).
- **Manual Override**: Set environment variable `SHADERBRIDGE_LANG=en` or `SHADERBRIDGE_LANG=zh` before launching Unreal Editor.

---

## 7. Generated Material Structure & Zero Runtime Dependency
- **Automatic Parameter Organization**: All generated `ScalarParameter`, `VectorParameter`, and `TextureObjectParameter` (`iChannel0`–`iChannel3`) nodes are grouped cleanly under **`ShaderBridge`**.
- **Out-of-the-Box Blank-Project Textures**: `iChannel0`–`iChannel3` automatically bind to ShaderBridge's bundled original CC0 UV checker texture (`/ShaderBridge/Textures/checker/checker1`), with safe engine fallback (`/Engine/EngineMaterials/DefaultDiffuse`), so `Textured UV Flow` and your own textured shaders render crisply even in brand-new empty projects.
- **Zero Runtime Dependency**: Generated assets are standard native Unreal Engine `.uasset` Materials. Your packaged game does **not** require ShaderBridge at runtime and cooks cleanly for all target platforms (Windows, Console, Mobile, Linux, macOS).

---

## 8. Scope, Troubleshooting & Third-Party GLSL Licensing
- **Error: "Shader code is missing mainImage"**
  - Ensure your pasted GLSL defines `void mainImage(out vec4 fragColor, in vec2 fragCoord)`.
- **Scope in v1.0**
  - **Supported**: Single-pass `mainImage` Fragment GLSL (GLSL ES / WebGL conventions) including standard math, structs, loops, helper functions, `iChannel0`–`iChannel3` 2D textures, and `Surface (Unlit)` / `UI` / `Post Process` material domains on Win64 Unreal Editor.
  - **Not Supported in v1.0**: Multi-pass ping-pong feedback buffers (`BufferA/B/C/D`), audio/FFT, webcam, video, keyboard, or cubemap inputs; macOS or Linux Unreal Editor hosts.
- **Third-Party GLSL Licensing Responsibility**
  - All 5 built-in samples and the bundled default UV checker texture are **100% original CC0-1.0 works** safe for commercial use. If you import or convert GLSL source code authored by third parties (many public shader galleries default to non-commercial licenses such as **CC BY-NC-SA 3.0** unless stated otherwise), **you are solely responsible for verifying and complying with the original author's license** before commercial use.
- **Where are logs and state files stored?**
  - All runtime logs and state files write cleanly to `<YourProject>/Saved/ShaderBridge/` (never inside the Engine Plugins installation directory).

---

## 9. Third-Party Licenses & Support
- **Third-Party Licenses**: ShaderBridge bundles 4 unmodified, `LunarG, Inc.` DigiCert EV-signed open-source Khronos SPIR-V / SPIRV-Cross / glslang command-line executables (`glslc.exe`, `spirv-val.exe`, `spirv-opt.exe`, `spirv-cross.exe`). Verbatim upstream license texts (`Apache-2.0.txt`, `glslang-LICENSE.txt`, `MIT-LICENSE.txt`, `SPIRV-Tools-LICENSE.txt`, `SPIRV-Cross-LICENSE.txt`), attributions, and SHA-256 checksums are bundled in `Resources/ThirdParty/VulkanSDK/THIRD_PARTY_NOTICES.md` and `Resources/ThirdParty/VulkanSDK/Licenses/`.
- **Issue Tracker & Support**:
  - GitHub Issues: https://github.com/gxwemedia/ShaderBridge-Docs/issues
  - Email: `gxwemedia@gmail.com` *(typical response within 24–48 business hours)*
  - When reporting a conversion issue, please attach your minimal GLSL snippet (confirming you have rights to share it) and the relevant log from `<YourProject>/Saved/ShaderBridge/`.
