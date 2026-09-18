# OpenComfyHands

**All kinds of AI. One place. Fully local.**

OpenComfyHands is a **local-first AI workspace** designed to give users access to many different kinds of AI capabilities from a single application.

Instead of installing and managing separate applications for coding, design, image generation, video generation, audio, speech, vision, and computer interaction, OpenComfyHands brings them together into one unified environment.

The application is built with **Tauri + SolidJS**, with a Rust-based core and a fully modular plugin architecture.

---

## Vision

Make local AI feel like a single, coherent platform rather than a collection of disconnected tools.

OpenComfyHands should allow a user to work with:

* text and reasoning
* coding agents
* image generation and editing
* video generation and editing
* audio understanding
* speech recognition
* text-to-speech
* computer interaction
* design and prototyping
* visual AI workflows
* local AI models
* AI-powered tools and workflows

All of these capabilities should be accessible from the same application.

The underlying implementation should remain replaceable.

---

# Core Principles

## 1. The Core Is Ours

OpenComfyHands is **not a fork or wrapper collection** of OpenHands, ComfyUI, Open WebUI, cptr, or OpenDesign.

Those projects are upstream technologies and sources of functionality that we integrate.

The application architecture, plugin system, UI, orchestration, data model, configuration, lifecycle management, capability system, and integration layer are developed as **OpenComfyHands code**.

```text
              OpenComfyHands
                     │
                 OUR CORE
                     │
       ┌─────────────┼─────────────┐
       │             │             │
    Plugins       Plugins       Plugins
       │             │             │
       ▼             ▼             ▼
   OpenHands      ComfyUI      OpenDesign
```

The core must remain useful even when no external integration is installed.

---

## 2. Every Functionality Is a Plugin

The fundamental extension mechanism is the **OpenComfyHands Plugin System**.

AI functionality should not be hard-coded into the core.

Instead:

```text
Core
 │
 ├── Plugin
 │     └── text generation
 │
 ├── Plugin
 │     └── speech recognition
 │
 ├── Plugin
 │     └── image generation
 │
 ├── Plugin
 │     └── video generation
 │
 ├── Plugin
 │     └── coding agent
 │
 ├── Plugin
 │     └── design tools
 │
 └── Plugin
       └── computer interaction
```

A plugin can provide one or more capabilities.

The core should know **what a plugin can do**, not how that capability is internally implemented.

---

## 3. Models Are Replaceable

OpenComfyHands should not be tied to a specific AI model.

A capability such as:

```text
image.generate
```

may be provided by:

```text
FLUX
SDXL
another local image model
future plugin
```

Likewise:

```text
speech.transcribe
```

could be provided by:

```text
Nemotron ASR
another local ASR model
future plugin
```

The model is an implementation detail.

---

## 4. Local First

OpenComfyHands is designed around **local execution**.

The goal is for users to be able to run the application, models, tools, workflows, files, and generated artifacts on their own hardware without requiring a cloud AI service.

Local resources should be visible and controllable:

```text
CPU
GPU
VRAM
RAM
Storage
Models
Processes
Workspaces
Plugins
```

Cloud services may eventually be supported through plugins, but the architecture must not depend on them.

---

## 5. One Application

The user should not need to understand the architecture underneath.

They should see one application:

```text
OpenComfyHands
│
├── Chat
├── Code
├── Design
├── Images
├── Video
├── Audio
├── Vision
├── Workflows
├── Files
├── Models
└── Plugins
```

Different technologies can operate behind the scenes while maintaining one consistent user experience.

---

# Core Architecture

```text
                         OpenComfyHands
                              │
                    ┌─────────▼─────────┐
                    │    Tauri 2 App    │
                    │                   │
                    │   SolidJS UI      │
                    │   Rust Backend    │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │   OpenComfyHands  │
                    │       Core        │
                    │                   │
                    │ Plugin System     │
                    │ Capability System │
                    │ Runtime Manager   │
                    │ Model Manager     │
                    │ Workflow Manager  │
                    │ Project Manager   │
                    │ Artifact Manager  │
                    │ Configuration     │
                    │ Events / IPC      │
                    └─────────┬─────────┘
                              │
                ┌─────────────┼──────────────┐
                │             │              │
                ▼             ▼              ▼
             Plugins       Plugins        Plugins
                │             │              │
                ▼             ▼              ▼
           OpenHands       ComfyUI       OpenDesign
                │             │              │
                ▼             ▼              ▼
              cptr      Local AI Models   Design Systems
```

The architecture is intentionally layered.

### Core

Own implementation.

### Plugins

Extension boundary.

### Integrations

Adapters that connect external projects to the plugin system.

### Upstream

Separate copies/snapshots of external projects used for development, analysis, compatibility testing, and integration.

---

# Technology Stack

## Desktop

**Tauri 2**

Tauri provides the native desktop application shell while allowing the interface to be built using web technologies and the application logic to remain in Rust.

## Frontend

**SolidJS + TypeScript**

The frontend is responsible for:

* application interface
* workspaces
* navigation
* plugin UI
* model management
* workflow interfaces
* project views
* media previews
* logs and execution state

## Core

**Rust**

The OpenComfyHands core is written in Rust.

Responsibilities include:

* plugin loading
* plugin lifecycle
* capability registration
* runtime management
* model management
* workflow management
* project management
* filesystem integration
* process management
* local execution
* application state
* IPC
* security boundaries
* configuration
* event system

## Async Runtime

**Tokio**

Used for asynchronous processes, I/O, networking, process execution, and plugin communication.

## Serialization

**Serde**

Used for configuration, plugin manifests, messages, state, and data interchange.

## Storage

Initial storage:

```text
SQLite / local application state
Filesystem
Git
```

The storage architecture should remain modular so heavier databases or remote storage can be introduced later without changing the core plugin model.

## Source Control

**Git**

Projects and code remain ordinary Git repositories.

---

# Plugin Architecture

A plugin should describe what it provides.

For example:

```text
Plugin
├── manifest
├── capabilities
├── configuration
├── runtime
├── UI
├── assets
└── lifecycle
```

A conceptual plugin manifest:

```yaml
id: example.image-generator
name: Example Image Generator
version: 1.0.0

capabilities:
  - image.generate
  - image.edit

runtime:
  type: local

ui:
  entry: plugin-ui
```

The core discovers the plugin and registers its capabilities.

```text
Plugin
   ↓
Registration
   ↓
Capabilities
   ↓
Core
   ↓
Available to the user
```

---

# Capabilities

Capabilities form the interface between the core and plugins.

Examples:

```text
text.generate
text.reason
vision.analyze

audio.analyze
audio.generate

speech.transcribe
speech.generate

image.generate
image.edit

video.generate
video.edit

code.generate
code.modify

computer.interact

design.generate
design.edit

workflow.execute
```

These are examples rather than a fixed final specification.

The capability system will evolve during development.

---

# External Integrations

OpenComfyHands will initially investigate and integrate functionality from several major open-source projects.

## OpenHands

AI software-development agents.

Repository:

[OpenHands/OpenHands](https://github.com/OpenHands/OpenHands)

OpenHands currently provides an agent-focused development platform and separates its agent server, tools, workspaces, events, and client-facing control center across repositories. OpenComfyHands will integrate the relevant functionality through its own plugin boundary rather than making OpenHands the application core.

## ComfyUI

Visual AI generation and workflow execution.

Repository:

[Comfy-Org/ComfyUI](https://github.com/Comfy-Org/ComfyUI)

ComfyUI provides a modular node/graph system for AI creation and supports local execution, reusable workflows, and an API suitable for integration into other applications.

## Open WebUI

AI interaction and self-hosted AI interface ecosystem.

Repository:

[open-webui/open-webui](https://github.com/open-webui/open-webui)

Open WebUI provides a broad set of patterns and integrations around local and self-hosted AI systems.

OpenComfyHands will study and integrate selected functionality where appropriate rather than embedding Open WebUI as the application itself.

## cptr

Computer and workspace interaction.

Repository:

[open-webui/computer](https://github.com/open-webui/computer)

cptr provides access to local files, terminals, editors, Git, browser sessions, workspaces, and related computer functionality.

OpenComfyHands will expose relevant functionality through a plugin.

## OpenDesign

Design, prototyping, design systems, skills, and design-oriented workflows.

Repository:

[nexu-io/open-design](https://github.com/nexu-io/open-design)

OpenDesign is particularly relevant to OpenComfyHands because its current architecture is itself highly plugin-oriented, with skills, design systems, rendering templates, and other functionality represented as portable packages.

---

# Upstream Management

External projects are treated as **upstreams**, not as the source of truth for OpenComfyHands.

The repository will maintain tracked upstream snapshots so developers and development agents can inspect the latest compatible source.

Conceptually:

```text
OpenComfyHands/
│
├── src/
├── apps/
├── crates/
├── plugins/
├── integrations/
│
├── upstream/
│   ├── openhands/
│   ├── comfyui/
│   ├── openwebui/
│   ├── cptr/
│   └── open-design/
│
├── docs/
│   └── upstream/
│
└── upstream.lock
```

## Exact Versions Are Pinned

Every OpenComfyHands development state must know exactly which upstream commits it was developed against.

For example:

```text
OpenHands    → commit A
ComfyUI      → commit B
Open WebUI   → commit C
cptr         → commit D
OpenDesign   → commit E
```

The OpenComfyHands repository records these versions.

This makes builds and integration debugging reproducible.

---

## Tracking Latest Upstream Changes

Upstream repositories should be checked regularly.

The update flow is:

```text
Upstream repository
        ↓
detect new commit/tag
        ↓
fetch
        ↓
update local snapshot
        ↓
record exact commit
        ↓
run compatibility tests
        ↓
review changes
        ↓
accept update
```

An upstream update should **never silently modify the working application**.

Instead, updates become explicit changes to OpenComfyHands.

---

## Upstream vs Our Code

Our repository should maintain a hard boundary between:

```text
UPSTREAM
```

and:

```text
OPENCOMFYHANDS
```

For example:

```text
upstream/comfyui/
        │
        ▼
integrations/comfyui/
        │
        ▼
OpenComfyHands Plugin API
        │
        ▼
OpenComfyHands Core
```

Our integration code belongs to OpenComfyHands.

Upstream code remains identifiable as upstream.

This makes upgrades, debugging, attribution, and licensing much easier to manage.

---

# Upstream Changes

We should avoid modifying upstream source directly unless there is a specific reason to maintain a patch.

When a modification is required:

```text
upstream
   ↓
patch
   ↓
integration
```

The patch should be isolated, documented, and tracked.

The preferred order is:

1. use upstream unchanged
2. adapt through our integration layer
3. maintain a small compatibility patch when necessary
4. contribute useful fixes upstream when appropriate

---

# Core Development Before Integration

**We should not begin by integrating every external project.**

First, we build and stabilize the OpenComfyHands core.

The first stage is completely focused on our own architecture.

## Core goals

### 1. Application Shell

Create the Tauri application.

```text
Tauri
└── SolidJS
```

### 2. Core Runtime

Build the Rust application core.

```text
Core
├── configuration
├── state
├── filesystem
├── process management
└── events
```

### 3. Plugin System

Implement:

```text
plugin discovery
plugin manifests
plugin loading
plugin lifecycle
plugin permissions
plugin configuration
plugin UI integration
```

### 4. Capability System

Implement a common way for plugins to declare and expose functionality.

```text
Plugin
   ↓
Capability
   ↓
Core
   ↓
Application
```

### 5. Model System

Create model discovery and management independently of any particular model vendor or runtime.

```text
Model
Provider
Runtime
Capability
```

must remain separate concepts.

### 6. Workflow System

Create a generic workflow abstraction.

A workflow may eventually be backed by:

```text
ComfyUI
OpenDesign
custom plugin
another runtime
```

The core must not assume that every workflow belongs to ComfyUI.

### 7. Artifact System

Build a common system for:

```text
images
videos
audio
documents
code
designs
datasets
generated files
```

Plugins should be able to produce and consume artifacts through a common interface.

### 8. Project System

Build the project/workspace model before integrating external projects.

```text
Project
├── files
├── artifacts
├── models
├── plugins
├── workflows
└── configuration
```

### 9. UI System

Build reusable UI primitives for plugins.

Plugins should be able to contribute their own interfaces without taking control of the entire application.

### 10. Local Execution

The core must be able to:

```text
discover
launch
monitor
stop
restart
```

local processes and runtimes.

---

# Initial Development Strategy

Before integrating OpenHands, ComfyUI, Open WebUI, cptr, or OpenDesign, we should prove the core with **mock plugins**.

For example:

```text
Mock Text Plugin
Mock Image Plugin
Mock Audio Plugin
Mock Video Plugin
```

The application should be able to:

```text
discover plugin
       ↓
register capability
       ↓
display capability
       ↓
execute request
       ↓
receive result
       ↓
store artifact
       ↓
display result
```

Only after this pipeline works reliably should real integrations begin.

This ensures that the external projects fit **our architecture**, rather than determining it.

---

# Initial Project Structure

```text
OpenComfyHands/
│
├── apps/
│   └── desktop/
│       ├── frontend/
│       │   ├── src/
│       │   └── ...
│       │
│       └── src-tauri/
│           └── ...
│
├── crates/
│   ├── core/
│   ├── plugin-api/
│   ├── capability/
│   ├── runtime/
│   ├── models/
│   ├── workflows/
│   ├── artifacts/
│   ├── projects/
│   └── security/
│
├── plugins/
│   ├── examples/
│   └── builtin/
│
├── integrations/
│   ├── openhands/
│   ├── comfyui/
│   ├── openwebui/
│   ├── cptr/
│   └── open-design/
│
├── upstream/
│   ├── openhands/
│   ├── comfyui/
│   ├── openwebui/
│   ├── cptr/
│   └── open-design/
│
├── docs/
│   ├── architecture/
│   ├── plugins/
│   └── upstream/
│
├── scripts/
│   ├── upstream-update/
│   ├── upstream-status/
│   └── compatibility/
│
├── upstream.lock
├── Cargo.toml
└── README.md
```

---

# Initial AI Integrations

Once the core is stable, OpenComfyHands can begin integrating local AI capabilities such as:

```text
MiniCPM5
Nemotron ASR
Granite Vision
LFM2.5 Audio
OmniVoice
FLUX.2 Klein
Bernini
```

These should be implemented as plugins/providers rather than becoming dependencies of the core.

For example:

```text
FLUX
  ↓
Image Generation Plugin

Bernini
  ↓
Video Generation Plugin

Nemotron
  ↓
Speech Recognition Plugin
```

The core only needs to understand the capability contract.

---

# Design Philosophy

OpenComfyHands should remain:

**Local**

Run on the user's hardware.

**Modular**

Everything optional should be replaceable.

**Extensible**

New functionality should be installable through plugins.

**Model-agnostic**

No single model should define the application.

**Runtime-agnostic**

No single AI runtime should define the application.

**Workflow-agnostic**

Workflows may come from different systems.

**Transparent**

Users should be able to see what is running, what model is being used, and where files and artifacts are stored.

**Open**

The platform should be built around open interfaces and interoperable components.

---

# What OpenComfyHands Is Not

OpenComfyHands is not intended to be:

* a fork of OpenHands
* a fork of ComfyUI
* a fork of Open WebUI
* a fork of cptr
* a fork of OpenDesign
* a single-model AI application
* a cloud-only AI service
* a collection of unrelated applications placed in one window

Instead:

```text
                    OpenComfyHands
                          │
                  ┌───────▼───────┐
                  │   OUR CORE    │
                  └───────┬───────┘
                          │
                     Plugin API
                          │
        ┌─────────┬───────┼───────┬─────────┐
        ▼         ▼       ▼       ▼         ▼
    OpenHands  ComfyUI  cptr  OpenDesign  Other
```

---

# Status

**Early development.**

The current priority is to build the OpenComfyHands core, plugin architecture, application shell, UI system, capability system, model/workflow abstractions, and local execution infrastructure.

External integrations will be added after the core architecture is established.

---

# Roadmap

## Phase 0 — Architecture

* [ ] Define core architecture
* [ ] Define plugin specification
* [ ] Define capability specification
* [ ] Define artifact model
* [ ] Define runtime model
* [ ] Define model abstraction
* [ ] Define workflow abstraction
* [ ] Define upstream management strategy

## Phase 1 — Core

* [ ] Tauri desktop application
* [ ] SolidJS UI
* [ ] Rust core
* [ ] application state
* [ ] plugin registry
* [ ] plugin lifecycle
* [ ] capability registry
* [ ] local process management
* [ ] artifact management
* [ ] project management

## Phase 2 — Mock Plugins

* [ ] mock text plugin
* [ ] mock image plugin
* [ ] mock audio plugin
* [ ] mock video plugin
* [ ] end-to-end plugin execution
* [ ] plugin UI system

## Phase 3 — Upstream Integration

* [ ] OpenHands integration
* [ ] ComfyUI integration
* [ ] cptr integration
* [ ] Open WebUI integration
* [ ] OpenDesign integration

## Phase 4 — Local AI

* [ ] local model discovery
* [ ] model installation
* [ ] model configuration
* [ ] model lifecycle
* [ ] multimodal providers
* [ ] GPU/resource monitoring

## Phase 5 — Advanced Workflows

* [ ] multimodal workflows
* [ ] workflow composition
* [ ] artifact pipelines
* [ ] reusable workflows
* [ ] plugin marketplace/registry

---

# Contributing

OpenComfyHands is being developed as an independent project.

Contributions should primarily improve:

```text
Core
Plugin System
UI
Capabilities
Local Runtime
Model System
Workflow System
Integrations
Documentation
Testing
```

When working with upstream projects, keep upstream code and OpenComfyHands code clearly separated.

---

# License


Individual upstream projects retain their own licenses and terms.

OpenComfyHands will document the provenance and licensing requirements of integrated upstream components before redistribution.
