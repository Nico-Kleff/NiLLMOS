# NiLLMOS

### A minimal Linux-based OS designed exclusively for running LLMs locally.

> Plug in. Boot up. Use. No setup. No Linux knowledge required.

-----

## The Problem

Running a local LLM today requires:

- Linux expertise (kernel, drivers, CUDA setup)
- Python, dependency hell, version conflicts
- Hours of configuration and maintenance
- Constant updates and frustration

This is a massive barrier. Most people give up and use ChatGPT, Claude, or Gemini instead — sending their private conversations, medical questions, and personal thoughts to servers they don’t control.

**That needs to change.**

-----

## What is NiLLMOS?

NiLLMOS is a minimal, purpose-built operating system that runs a Large Language Model and nothing else.

Think of it like a **router for AI** — you plug it in, it boots in seconds, and it just works. No installation. No Linux. No configuration hell.

```
You buy or build a machine.
Plug in power and network.
Open your browser → http://nillmos.local
Start chatting with your private AI.
Done.
```

Your data never leaves your device. No company can read your conversations. No terms of service. No subscriptions. **You own it completely.**

-----

## Architecture

NiLLMOS is built in clearly separated, modular layers. Each layer can be independently updated or replaced without affecting the others.

```
┌─────────────────────────────────────────────────┐
│           WebUI  │  OpenAI-compatible API        │
├─────────────────────────────────────────────────┤
│              Inference Engine                   │
│           (llama.cpp based)                     │
├─────────────────────────────────────────────────┤
│         Compute Abstraction Layer (CAL)         │
│   CUDA Module │ ROCm Module │ oneAPI Module     │
├─────────────────────────────────────────────────┤
│        Hardware Abstraction Layer (HAL)         │
├─────────────────────────────────────────────────┤
│              Minimal Linux Kernel               │
│     PCIe │ LAN │ WLAN │ NVMe │ GPU Driver      │
├─────────────────────────────────────────────────┤
│                   Hardware                      │
│          GPU  │  RAM  │  SSD  │  NIC            │
└─────────────────────────────────────────────────┘
```

### Key Design Principles

**Modularity** — Every layer has a stable interface. Swap out the GPU driver, the compute backend, or the inference engine independently.

**Minimalism** — The Linux kernel is stripped to only what an LLM needs: PCIe, network, NVMe, GPU driver. No display server, no USB stack, no sound system, no package manager.

**Privacy by default** — No telemetry, no cloud dependency, no external connections unless explicitly configured.

**Plug & Play** — First boot experience like a modern router. A setup wizard guides through language, WiFi (optional), and model selection in under 2 minutes.

-----

## Features (Planned)

### Networking

- **LAN** (primary) — stable, low latency
- **WLAN** (optional) — single supported chip per hardware target for reliability

### User Interface

- **Web UI** — accessible from any browser on the local network, no app needed
- Chat interface (similar to OpenWebUI)
- Model management (load, remove, switch)
- System status (VRAM usage, temperature, requests/sec)
- Update management

### API

- **OpenAI-compatible REST API** — any app that supports ChatGPT works immediately
- Endpoints: `/v1/chat/completions`, `/v1/models`, `/v1/embeddings`, `/v1/health`
- API key management via WebUI

### Updates

- **A/B partition system** — updates can never brick the device
- One-click updates via WebUI
- Modular updates: kernel, inference engine, WebUI, and model weights updated independently

### Hardware Support (Planned)

- AMD GPUs via ROCm (open source, primary target)
- Intel GPUs via oneAPI
- NVIDIA GPUs via minimal CUDA runtime (secondary target)

-----

## Why AMD and Intel First?

**AMD ROCm is fully open source** — it can be studied, modified, and integrated into a minimal stack without vendor permission.

**Intel oneAPI** is designed from the ground up as a hardware-agnostic abstraction layer — architecturally aligned with NiLLMOS’s CAL concept.

NVIDIA CUDA requires a full OS stack by design and is closed source. NVIDIA support may come later through community contribution.

-----

## Comparison

|                      |Today (Linux + CUDA)|NiLLMOS      |
|----------------------|--------------------|-------------|
|Boot time             |30–60 seconds       |2–5 seconds  |
|RAM overhead          |500MB–1GB           |~10–50MB     |
|Setup time            |Hours               |2 minutes    |
|Linux knowledge needed|Yes                 |No           |
|Attack surface        |Large               |Minimal      |
|Inference latency     |OS jitter present   |Deterministic|

-----

## Vision

NiLLMOS is not just a technical project. It is infrastructure for digital sovereignty.

Today, 4–5 companies control access to capable AI. NiLLMOS means:

- Every person can **own** a fully capable LLM
- No company can revoke access or raise prices
- No censorship, no usage policies, no data collection
- Medical, legal, and personal questions — truly private

Like Linux for servers, or Firefox for the web — NiLLMOS should be the open foundation that makes private AI accessible to everyone.

-----

## Current Status

🟡 **Concept / RFC phase** — No code yet.

This repository currently contains the architecture design and vision document. The goal is to attract contributors who can help build each layer.

**This is intentional.** The design needs to be right before the code is written.

-----

## Roadmap

### Phase 1 — Foundation (Months 1–3)

- [ ] Finalize HAL and CAL interface specifications
- [ ] Minimal Linux kernel configuration with Buildroot
- [ ] llama.cpp running as sole userspace process
- [ ] Basic HTTP API serving responses over LAN
- [ ] Boot time under 5 seconds

### Phase 2 — Usability (Months 4–6)

- [ ] Web UI (chat + admin)
- [ ] WLAN support (selected chipset)
- [ ] OpenAI-compatible API fully implemented
- [ ] A/B update system
- [ ] First public demo

### Phase 3 — Hardware Expansion (Months 6+)

- [ ] AMD ROCm minimal integration
- [ ] Intel oneAPI integration
- [ ] Multi-model support
- [ ] Community hardware targets

-----

## How to Contribute

NiLLMOS needs people with different skills:

|Skill                      |What’s needed                                       |
|---------------------------|----------------------------------------------------|
|**Linux kernel / embedded**|Kernel configuration, Buildroot, driver minimization|
|**C / C++**                |Inference engine integration, HAL/CAL implementation|
|**GPU / ROCm / PTX**       |Minimal compute kernels for LLM inference           |
|**Web / JavaScript**       |WebUI (chat interface + admin panel)                |
|**Networking**             |Minimal TCP/IP stack, WLAN driver integration       |
|**Documentation**          |Architecture docs, setup guides, tutorials          |

**You don’t need to be an expert to contribute.** Documentation, testing, and ideas are just as valuable as code.

Open an issue, start a discussion, or reach out.

-----

## Inspired By

- **Linux** — proof that open, community-built OS infrastructure works
- **OpenWRT** — minimal Linux for a single purpose
- **llama.cpp** — LLM inference without framework overhead
- **Buildroot** — minimal embedded Linux systems
- **Groq / Cerebras** — proof that minimal inference stacks are faster

-----

## License

NiLLMOS is licensed under the **GNU General Public License v3.0**.

You are free to use, modify, and distribute this project. Any derivative work must remain open source under the same license. Devices running NiLLMOS must allow users to install modified versions.

See <LICENSE> for full terms.

-----

## Author

Created by **Nico Kleff** — born out of a simple question:  
*Why does running a private AI have to be this hard?*

-----

*NiLLMOS — Private AI for everyone.*
