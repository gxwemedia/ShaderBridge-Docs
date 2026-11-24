# ShaderBridge — Quick Start Guide

**ShaderBridge** converts Shadertoy-compatible GLSL shaders into native Unreal Engine Material assets (`.uasset`) using a 100% offline, deterministic compiler pipeline powered by LunarG-signed open-source Khronos SPIR-V / SPIRV-Cross / glslang tools.

---

## 1. 10-Second UI Quick Start (No Coding Required)
1. Enable **ShaderBridge** in **Edit -> Plugins** (`PythonScriptPlugin` is enabled automatically) and restart Unreal Editor if prompted.
2. Click the **ShaderBridge** button in the top Level Editor toolbar.
3. In the **ShaderBridge** panel, pick any of the **5 built-in original CC0 samples** from the **Built-in Sample** dropdown and click **Load Sample** (this automatically fills **Shader Code** and **Material Name**).
4. Optionally adjust **Output Folder** (default `/Game/ShaderBridgeMaterials`) and click **Convert To UE Material**.
5. Your compiled native Material `.uasset` is saved, automatically highlighted in the **Content Browser**, and ready to apply to any mesh, UI widget, or post-process workflow.

---

## 2. Requirements & Supported Engine Versions
- **Unreal Engine**: Windows 64-bit (`Win64`) Editor — officially verified via `RunUAT BuildPlugin` on **UE 5.8** and **UE 5.9** (see your Fab listing page for the exact supported engine versions selectable in Epic Games Launcher).
- **Operating System**: Windows 10 / 11 (64-bit)
- **Built-in Engine Plugin**: `PythonScriptPlugin` (automatically enabled as a plugin dependency)
- **External Dependencies**: **None.** All required open-source compiler binaries are bundled inside the plugin. No external Vulkan SDK installation, no Python `.venv`, no internet connection, and no API keys required.

---

## 3. Panel Controls Overview
- **Built-in Sample + `Load Sample`**: Loads any of the 5 original CC0 sample shaders directly into the panel:
  | Sample Key | Title | Default Material Name | Uniforms Demonstrated |
  | --- | --- | --- | --- |
  | `plasma_nebula` | Plasma Nebula | `M_Showcase_PlasmaNebula` | `iTime`, `iResolution` |
  | `voronoi_crystal` | Voronoi Crystal | `M_Showcase_VoronoiCrystal` | `iTime`, `iResolution` |
  | `sdf_raymarch_orb` | SDF Raymarch Orb | `M_Showcase_SdfRaymarchOrb` | `iTime`, `iResolution` |
  | `scifi_holo_grid` | Sci-Fi Holo Grid | `M_Showcase_ScifiHoloGrid` | `iTime`, `iResolution` |
  | `textured_uv_flow` | Textured UV Flow | `M_Showcase_TexturedUvFlow` | `iTime`, `iResolution`, `iChannel0` |
- **Material Name**: Name of the generated Unreal Material asset (e.g., `M_MyPlasma`).
- **Output Folder**: Target Content Browser folder (default `/Game/ShaderBridgeMaterials`).
- **Shader Code**: Paste any single-pass Shadertoy-compatible GLSL defining `void mainImage(out vec4 fragColor, in vec2 fragCoord)`.
- **Copy HLSL**: Converts GLSL to Unreal-ready Custom Node HLSL, copies it to your clipboard, and displays live status feedback in the panel without creating an asset.
- **Convert To UE Material**: Compiles GLSL offline, builds and saves the native Material asset, wires all detected uniforms (`iTime`, `iTimeDelta`, `iFrame`, `iFrameRate`, `iResolution`, `iMouse`, `iDate`, `iChannel0`–`iChannel3`, `UVs`) grouped under **`Shadertoy`**, and syncs the Content Browser to the new asset.
- **Cancel**: Cancels an active background conversion job.

---

## 4. Optional Python Console Automation
For batch workflows inside **Output Log -> Python** (inside Unreal Editor):

```python
from ShaderBridge import sample_shaders
import shadertoy_converter

# List all 5 built-in CC0 samples
for item in sample_shaders.list_samples():
    print(item["key"], "->", item["material_name"])

# Convert a sample directly into a UE Material in a custom output folder
sample = sample_shaders.get_sample("plasma_nebula")
shadertoy_converter.convert_shader(
    glsl_code=sample["glsl"],
    material_name=sample["material_name"],
    output_folder="/Game/ShaderBridgeMaterials",
)
```

---

## 5. Language & Localization (EN / ZH)
ShaderBridge supports **English (default)** and **Simplified Chinese**:
- **Automatic Detection**: Automatically follows Unreal Editor's active UI culture (`zh*` -> Simplified Chinese; all other cultures -> English).
- **Manual Override**: Set environment variable `SHADERBRIDGE_LANG=en` or `SHADERBRIDGE_LANG=zh` before launching Unreal Editor.

---

## 6. Generated Material Structure & Zero Runtime Dependency
- **Automatic Parameter Organization**: All generated `ScalarParameter`, `VectorParameter`, and `TextureObjectParameter` (`iChannel0`–`iChannel3`) nodes are grouped cleanly under the **`Shadertoy`** category for tidy Material Instances.
- **Safe Blank-Project Fallbacks**: `iChannel*` texture parameters automatically receive valid fallback engine textures so generated materials compile cleanly even in empty projects.
- **Zero Runtime Dependency**: Generated assets are standard Unreal Engine `.uasset` Materials. Your packaged game does **not** require ShaderBridge at runtime and cooks cleanly for all target platforms.

---

## 7. Scope, Troubleshooting & Third-Party GLSL Licensing
- **Error: "Shader code is missing mainImage"**
  - Ensure your pasted GLSL defines `void mainImage(out vec4 fragColor, in vec2 fragCoord)`.
- **Unsupported Shadertoy Features in v1.0**
  - v1.0 supports **single-pass** shaders (`mainImage`) and `iChannel0`–`iChannel3` 2D textures. Multi-pass ping-pong buffers (`BufferA/B/C/D`), audio, webcam, video, keyboard, and cubemap inputs are not supported in v1.0.
- **Third-Party GLSL Licensing Reminder**
  - The 5 built-in samples are 100% original CC0 works safe for commercial use. If you convert public GLSL written by other authors (e.g., Shadertoy community shaders, which default to **CC BY-NC-SA 3.0**), you are solely responsible for verifying and complying with the original author's license before commercial use.
- **Where are logs and state files stored?**
  - All logs and state files write to `<YourProject>/Saved/ShaderBridge/` (never inside the Engine Plugins directory).

---

## 8. Third-Party Licenses & Support
- **Third-Party Licenses**: ShaderBridge bundles unmodified LunarG-signed open-source Khronos SPIR-V / SPIRV-Cross / glslang command-line utilities (`glslc.exe`, `spirv-val.exe`, `spirv-opt.exe`, `spirv-cross.exe`). Complete Apache 2.0 and glslang BSD-3-Clause license texts, attributions, and SHA-256 checksums are bundled in `Resources/ThirdParty/VulkanSDK/THIRD_PARTY_NOTICES.md` and `Resources/ThirdParty/VulkanSDK/Licenses/`.
- **Support**: `gxwemedia@gmail.com` (typical response within 24–48 business hours). When reporting a conversion issue, please attach your GLSL snippet and `<YourProject>/Saved/ShaderBridge/` log.
