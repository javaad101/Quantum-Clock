# Quantum-Clock

## Section 174A — Qualified Research Documentation

**Investigator:** Javaad Ali, Esq.
**Entity:** Advocates Close Corp., New York
**Repository created:** December 9, 2024
**Documentation added:** March 27, 2026 (retrospective)
**Status:** SUCCEEDED as experiment — Metal compute shader for quantum state rotation on GPU
**Contact:** claude.redliner@advocatesclose.nyc

> **Note on timing:** This documentation was prepared retrospectively as part of a systematic review of the investigator's research program for IRC Section 174A compliance. The repository and its contents predate this README. All dates, code, and commit history are original and unmodified.

---

## Research Objective

Implement a quantum clock — a unitary rotation gate (Rz) applied to a two-level quantum state — as a Metal compute shader executing on the GPU. This is one of the investigator's earliest experiments with Metal compute pipelines and GPU programming, predating the Metal text rendering work in AppJavaadica by twelve months.

## Technological Uncertainty

1. **Metal compute pipeline setup**: Can a Metal compute kernel be used to simulate quantum state evolution (unitary rotation) on the GPU? The investigator implemented the full pipeline: `MTLDevice` → `MTLCommandQueue` → `MTLComputePipelineState` → buffer allocation → kernel dispatch.

2. **Struct alignment between Swift and Metal**: The `QuantumState` struct must be identically laid out in Swift (`(Float, Float)` tuple) and Metal (`float state[2]`). The uncertainty was whether Swift's tuple memory layout matches Metal's array layout. It does on ARM64, but this is not guaranteed by either language specification.

## Process of Experimentation

### Approach 1: Rz gate as Metal compute kernel
- **Hypothesis:** A rotation matrix applied to a two-component state vector on the GPU produces correct quantum state evolution.
- **Result:** SUCCEEDED. The kernel correctly applies `[cos(θ), -sin(θ); sin(θ), cos(θ)]` to the state vector. The "clock tick" rotates the quantum state by a configurable angle (default: π/4).
- **Disposition:** This was the investigator's first working Metal compute shader. The pattern (Swift struct ↔ Metal struct, buffer allocation, kernel dispatch) was later used in AppJavaadica for SDF glyph rendering.

## Source Files

| File | Lines | Purpose |
|------|-------|---------|
| `QuantumClock.swift` | ~50 | Metal device setup, buffer allocation, compute pipeline, tick execution |
| `QuantumClock.metal` | ~30 | Rz rotation kernel — cosine/sine rotation matrix on GPU |
| `main.swift` | — | CLI entry point |

## Relationship to Other Research

- **Related:** Quantum-Asset-Pricing (Dec 2024) — CHSH violation and GHZ state simulation
- **Related:** EntangledSpins (Mar 2025) — Metal visualization of spin states
- **Successor:** AppJavaadica (Dec 2025) — Metal SDF glyph rendering for text editor

*Documentation prepared by Javaad Ali, Esq. with assistance from Claude (Anthropic), used as a research tool analogous to an IDE or reference manual.*
