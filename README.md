# ShaderBridge — Official Documentation & Support

**ShaderBridge** is a 100% offline, deterministic standard `mainImage` Fragment GLSL (GLSL ES / WebGL conventions) to Unreal Engine Material (`.uasset`) converter plugin for Unreal Engine (Win64 Editor), powered by unmodified open-source Khronos SPIR-V / SPIRV-Cross / glslang compiler tools digitally signed by `LunarG, Inc.`.

> **Built for AI-Assisted & Procedural Workflows**: Generate standard `mainImage` Fragment GLSL with any AI / LLM (or write your own), then let **ShaderBridge** compile it offline and deterministically into a native UE Material (`Surface (Unlit)`, `UI`, or `Post Process`) with zero HLSL syntax hallucinations and zero runtime dependencies.
>
> **Repository Scope**: This public repository hosts official buyer documentation, release notes, and the issue tracker for **ShaderBridge** (distributed on Epic Fab). It contains no plugin source code.

## Quick Links
- **[Quick Start Guide (QUICK_START.md)](./QUICK_START.md)** — Installation, 10-second 3-tab UI walkthrough, Uniform behavior reference, synchronous Python batch API (`shaderbridge_converter`), and troubleshooting.
- **[Changelog & Roadmap (CHANGELOG.md)](./CHANGELOG.md)** — v1.0.0 release notes and planned v1.5 / v2.0 direction.
- **[Report a Shader Issue / Request Support](https://github.com/gxwemedia/ShaderBridge-Docs/issues)** — Have a single-pass `mainImage` GLSL shader that failed to compile, or a workflow question? Open an Issue and we typically respond within 24–48 business hours.
- **Direct Email Support**: `gxwemedia@gmail.com`

## Trademarks & Third-Party GLSL Licensing Notice
ShaderBridge is an independent Unreal Engine Editor plugin and is not affiliated with, endorsed by, or sponsored by The Khronos Group Inc., LunarG, Inc., or Epic Games, Inc. All 5 built-in samples and the bundled default UV checker texture are 100% original CC0-1.0 works safe for commercial use. Users are solely responsible for verifying and complying with the license terms of any third-party GLSL source code they import or convert (note that many public shader galleries default to non-commercial licenses such as CC BY-NC-SA 3.0 unless stated otherwise).
