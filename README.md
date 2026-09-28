# 🪐 NEXA (v0.1)

> **The Next-Generation Spatial Programming Language for 120FPS Worlds.**  
> Blazingly fast like C++, elegant like Swift, intuitive like Python, and memory-safe like Rust.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status: Experimental](https://img.shields.io/badge/Status-v0.1--alpha-orange.svg)]()
[![Target: 120FPS](https://img.shields.io/badge/Render-120FPS%20GPU%20Direct-green.svg)]()
[![Engine: Rust%20%2B%20Tauri](https://img.shields.io/badge/Toolchain-Rust%20%2B%20Tauri-red.svg)]()

---

## ⚡ What is NEXA?

NEXA is a domain-specific spatial programming language engineered from scratch for the Spatial Computing era (VisionOS, Quest, WebGPU). 

Traditional frameworks (React Three Fiber, Unity, Flutter) force painful compromises between developer experience and real-time performance. NEXA completely eliminates garbage collection spikes, closure allocations, and async microtask delays inside rendering loops.

- 🏎️ **Deterministic 120FPS**: Pure GPU-direct frame loop (`on frame(dt)`) with zero GC pause.
- 🎨 **Unified Spatial Canvas**: Declarative 2D Glassmorphism UI and 3D PBR meshes in a single syntax tree.
- 🧠 **Native AI Core (LensCore)**: Zero-copy tensor and prompt integration in the same memory space.
- 🛡️ **Zero-Cost Safety**: Rust-like pattern matching, algebraic enums, and ownership-aware arena allocators without lifetime hell.

---

## 🚀 Code Preview

Here is how you build a reactive, interactive 3D spatial entity with zero boilerplate:

```nexa
import { PBRMaterial, OctahedronGeometry } from "nexa/graphics"
import { LensCore } from "nexa/ai"

// Reactive State
let mut hovered: bool = false
let mut active: bool = false

// Unified 3D Scene & Spatial UI
let mainScene = Scene {
    crystalMesh: Mesh.new(
        geometry: OctahedronGeometry(radius: 1.2, detail: 0),
        material: PBRMaterial(
            color: if hovered { "#FF0077" } else { "#00F0FF" },
            roughness: 0.1,
            metallic: 0.9
        ),
        onClick: || { active = !active },
        onPointerEnter: || { hovered = true },
        onPointerLeave: || { hovered = false }
    )
}

// Native 120FPS GPU Direct Loop (Zero Allocation)
on frame(dt: float) {
    mainScene.crystalMesh.rotation.y += dt * 1.5
    mainScene.crystalMesh.position.y = Math.sin(Time.elapsed() * 2.0) * 0.2
}
