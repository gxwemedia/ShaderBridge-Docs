# ShaderBridge — Changelog & Planned Roadmap

All notable changes to **ShaderBridge** are documented in this file.

---

## [1.0.0] — Initial Fab Marketplace Release
### Added
- **100% Offline Deterministic Shader Compilation Engine**: Bundled Windows x64 offline compiler utilities with zero external setup, zero API keys, and zero cloud dependencies.
- **Native Unreal Editor Toolbar & Slate Panel**: One-click access from the Level Editor toolbar with **Convert To UE Material**, **Preview HLSL** (clipboard copy + Output Log preview), live progress feedback, and **Cancel** support.
- **Automatic Material Graph Wiring**: Detects and wires `iTime`, `iTimeDelta`, `iFrame`, `iResolution`, `iMouse`, `iDate`, `iChannel0`–`iChannel3`, and `UVs`, with safe fallback textures for blank projects.
- **5 Built-in 100% Original Sample Shaders (CC0)**:
  - `plasma_nebula` (`M_Showcase_PlasmaNebula`)
  - `voronoi_crystal` (`M_Showcase_VoronoiCrystal`)
  - `sdf_raymarch_orb` (`M_Showcase_SdfRaymarchOrb`)
  - `scifi_holo_grid` (`M_Showcase_ScifiHoloGrid`)
  - `textured_uv_flow` (`M_Showcase_TexturedUvFlow`)
- **Internationalization (EN / ZH)**: English default with automatic Simplified Chinese detection from Unreal Editor's active culture, plus `SHADERBRIDGE_LANG=en|zh` environment override.
- **Read-Only Engine Plugin Directory Safety**: All runtime state, logs, and exchange files write exclusively to `<Project>/Saved/ShaderBridge/` and `<Project>/Intermediate/ShaderBridgeExchange/`.
- **Clean Fab Packaging & Compliance**: Editor-only module (`Win64`), zero runtime dependency for generated `.uasset` Materials, silent background execution, full C++ Copyright headers, `Icon128.png`, and bundled `THIRD_PARTY_NOTICES.md`.

---

## [1.1.0] — Planned Roadmap *(Direction Only — No Fixed Delivery Dates)*
> *Note: Items below represent planned development directions and are not binding feature or schedule commitments.*

- **Lightweight `BufferA` Runtime Workflow**: Editor-verified live render-target update loop for single-buffer Shadertoy effects.
- **Custom Compiler Path Override**: Optional plugin setting for studios preferring a custom local compiler toolchain path.
- **Expanded Original Sample Library**: Additional CC0 procedural patterns (noise, caustics, UI effects) and Material Instance presets.
- **Ongoing Engine Version Support**: Day-one compatibility updates for upcoming Unreal Engine 5.x releases.
