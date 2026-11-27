# ShaderBridge — Changelog & Planned Roadmap

All notable changes to **ShaderBridge** are documented in this file.

---

## [1.0.0] — Initial Fab Marketplace Release
### Added
- **100% Offline Deterministic Compiler Pipeline**: Bundled unmodified `LunarG, Inc.` DigiCert EV-signed Windows x64 Khronos SPIR-V / SPIRV-Cross / glslang CLI utilities (`glslc.exe`, `spirv-val.exe`, `spirv-opt.exe`, `spirv-cross.exe`) compiling standard `mainImage` Fragment GLSL (GLSL ES / WebGL conventions) into native Unreal Engine `.uasset` Materials with zero external setup, zero `.venv`, zero API keys, and zero cloud dependencies.
- **Native UE5 3-Tab Slate Panel (`Converter` / `Samples` / `Help`)**:
  - One-click Level Editor toolbar button (with 24px transparent icon).
  - **Converter** tab with **Material Name** (default `M_ShaderBridge`), **Output Folder** (default `/Game/ShaderBridgeMaterials`), **Domain** selector (`Surface (Unlit)` / `UI` / `Post Process`), **Convert**, **Copy HLSL**, **Cancel**, **Paste**, and **Clear**.
  - Bottom status bar with one-click **Open Material** button (opens the generated `.uasset` directly in the Material Editor and syncs the Content Browser).
  - **Samples** tab featuring interactive cards for all 5 built-in original CC0 shaders with one-click **Load Into Converter**.
  - **Help** tab with quick in-editor syntax, uniform, and support reference.
- **Automatic Material Graph Wiring & `ShaderBridge` Parameter Group**: Detects and wires `iTime`, `iTimeDelta`, `iFrame`, `iFrameRate`, `iResolution`, `iMouse`, `iDate`, `iChannel0`–`iChannel3`, `iChannelResolution`, `iChannelTime`, `iSampleRate`, and `UVs` cleanly under the **`ShaderBridge`** parameter group.
- **Bundled Original CC0 Default UV Checker Texture**: Ships a native UE5 CC0 UV checker texture (`/ShaderBridge/Textures/checker/checker1`, with `/Engine/EngineMaterials/DefaultDiffuse` fallback) so `iChannel0`–`iChannel3` shaders work crisply out of the box in blank projects.
- **5 Built-in 100% Original Sample Shaders (CC0-1.0)**:
  - `plasma_nebula` (`M_Showcase_PlasmaNebula`)
  - `voronoi_crystal` (`M_Showcase_VoronoiCrystal`)
  - `sdf_raymarch_orb` (`M_Showcase_SdfRaymarchOrb`)
  - `scifi_holo_grid` (`M_Showcase_ScifiHoloGrid`)
  - `textured_uv_flow` (`M_Showcase_TexturedUvFlow`)
- **Editor Python Automation (`shaderbridge_converter`)**: Exposes `convert_shader(...)`, synchronous batch `convert_shader_sync(...)`, and `preview_hlsl(...)` with `material_domain` support (`"surface"` / `"ui"` / `"post_process"`).
- **Internationalization (EN / ZH)**: English default with automatic Simplified Chinese detection from Unreal Editor's active culture, plus `SHADERBRIDGE_LANG=en|zh` environment override (honored by both C++ Slate UI and Python backend).
- **Read-Only Engine Plugin Directory Safety**: All runtime state, logs, and exchange files write exclusively to `<Project>/Saved/ShaderBridge/`.
- **Clean Fab Packaging & Verbatim Open-Source Compliance**: Editor-only module (`Win64`), zero runtime dependency for generated `.uasset` Materials, full C++ Copyright headers, `Config/FilterPlugin.ini`, `Icon128.png` / `Icon40.png`, public `DocsURL` / `SupportURL`, and verbatim upstream license texts (`Apache-2.0.txt`, `glslang-LICENSE.txt`, `MIT-LICENSE.txt`, `SPIRV-Tools-LICENSE.txt`, `SPIRV-Cross-LICENSE.txt`) with exact SHA-256 checksums in `Resources/ThirdParty/VulkanSDK/THIRD_PARTY_NOTICES.md`.

---

## Planned Roadmap *(Direction Only — No Fixed Delivery Dates)*
> *Note: Items below represent planned development directions and are not binding feature or schedule commitments.*

### [1.5.0] — Planned Free Update (Core Capability Expansion)
- **Native Material Graph Lowering (`graph_lowering`)**: Optionally lower suitable shader expressions into native Unreal Engine Material Graph nodes alongside Custom Node fallback.
- **Automatic Shader Parameter Exposure**: Detect and surface tunable shader constants as first-class Unreal Material parameters for rapid look-dev in Material Instances.
- **Lightweight `BufferA` Single-Buffer Runtime Workflow**: Editor-verified render-target update loop for single-buffer feedback effects.
- **Ongoing Engine Version Support**: Compatibility updates for upcoming Unreal Engine 5.x releases.

### [2.0.0] — ShaderBridge Pro / Enterprise (Standards-Based Agent Integration)
- **Native ACP + MCP + Skills Tool Surface**: Expose ShaderBridge's deterministic offline compiler core, existing Material / Substrate diagnostics, and headless CI shader compilation gates through official **ACP (Agent Client Protocol)**, **MCP (Model Context Protocol)**, and **Skills** — allowing any compliant AI coding agent or studio pipeline to compile and audit UE materials without any proprietary in-editor chat shell.
