code
Markdown
# 📐 NEXA Language Specification (v0.1)

This document defines the formal language syntax, runtime semantics, and component specifications for **NEXA (v0.1)**.

---

## 1. Module Imports & Custom Types

```nexa
import { LensCore } from "nexa/ai"
import { PBRMaterial, BoxGeometry } from "nexa/graphics"

// Custom Struct (State Definition)
struct AppState {
    themeColor: string,
    particleCount: int,
    isAiActive: bool,
}

// Algebraic Data Types (Enums with Payload)
enum SystemStatus {
    Ready,
    Processing,
    Error(code: int),
}
2. State & Variable Declarations
code
Nexa
// Immutable (let) and Mutable (let mut)
let appName: string = "LensOS Spatial Dashboard"
let mut frameCount: int = 0
let mut status: SystemStatus = SystemStatus.Ready

// Struct Initialization
let mut state: AppState = AppState {
    themeColor: "#00F0FF",
    particleCount: 1000,
    isAiActive: true,
}
3. Declarative UI & 3D Scene Hybrid
NEXA unifies 3D PBR meshes and 2D spatial glassmorphism UI into a single declarative pipeline.
code
Nexa
let mainScene = Scene {
    // 3D Mesh Object (Direct WebGPU Binding)
    cubeMesh: Mesh.new(
        geometry: BoxGeometry(width: 1.5, height: 1.5, depth: 1.5),
        material: PBRMaterial(
            color: state.themeColor,
            roughness: 0.2,
            metallic: 0.9
        )
    ),

    // Declarative 2D Spatial UI (Panel / Text)
    Panel {
        style: {
            position: "absolute",
            top: 20,
            left: 20,
            padding: 16,
            background: "#000000AA",
            blur: 15,
            borderRadius: 8
        },

        Text {
            content: appName,
            fontSize: 18,
            color: "#FFFFFF"
        },

        Text {
            content: `FPS: 120 | Frames: ${frameCount}`,
            fontSize: 12,
            color: "#888888"
        }
    }
}
4. Pure Functions & Exhaustive Pattern Matching
code
Nexa
// Pure Function with Type Signatures
fn calculateRotationSpeed(dt: float, multiplier: float) -> float {
    return dt * 2.0 * multiplier
}

// Pattern Matching
fn getStatusText(currentStatus: SystemStatus) -> string {
    match currentStatus {
        SystemStatus.Ready => return "System Nominal",
        SystemStatus.Processing => return "LensCore Processing...",
        SystemStatus.Error(code) => return `Error Code: ${code}`,
    }
}
5. Native AI Core (LensCore) Integration
Zero-copy, shared-memory spatial AI inference.
code
Nexa
async fn runAiAnalysis() {
    status = SystemStatus.Processing
    
    // Direct in-memory request to LensCore
    let prediction = await LensCore.predictUIState(
        currentFrames: frameCount,
        context: "Optimize rendering pipeline"
    )

    if prediction.confidence > 0.85 {
        state.themeColor = prediction.suggestedColor
        status = SystemStatus.Ready
    } else {
        status = SystemStatus.Error(code: 404)
    }
}
6. Native 120FPS Render Loop (GPU Direct)
Synchronized WebGPU execution loop with zero-allocation guarantee.
code
Nexa
on frame(dt: float) {
    // 1. Mutable state update
    frameCount += 1
    let speed = calculateRotationSpeed(dt, 1.5)

    // 2. Direct transform mutation (No reconciliation overhead)
    mainScene.cubeMesh.rotation.x += speed
    mainScene.cubeMesh.rotation.y += speed * 0.5

    // 3. Periodic AI execution trigger
    if frameCount % 60 == 0 && state.isAiActive {
        spawn Task(runAiAnalysis())
    }
}
