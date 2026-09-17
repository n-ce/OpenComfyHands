# OpenComfyHands

**One place for local AI.**

OpenComfyHands is a **local-first, modular AI workspace** that brings different kinds of AI capabilities together in a single application.

Instead of requiring users to switch between separate tools for coding, image generation, video generation, audio, vision, speech, and computer interaction, OpenComfyHands provides one unified environment for using them together.

The project combines ideas and technology from **OpenHands, ComfyUI, Open WebUI, and cptr**, while building a new orchestration layer and native desktop experience around them.

## Vision

Make powerful AI capabilities accessible from **one application, running locally on the user's own hardware**.

OpenComfyHands is designed to let users:

* run AI models locally
* work with text, images, video, audio, and vision
* use AI coding agents
* interact with local computers and workspaces
* execute visual AI workflows
* connect different AI capabilities into pipelines
* add or replace models without changing the core application
* keep their projects, files, and generated content local

## Core Idea

OpenComfyHands treats AI capabilities as modular components.

A user should not need to think about which underlying model or implementation performs a task.

For example:

```text
"Generate a video"
        ↓
OpenComfyHands
        ↓
video.generate
        ↓
available local workflow/provider
        ↓
result
```

The same principle applies to:

```text
reasoning
vision
speech recognition
text-to-speech
image generation
image editing
video generation
video editing
audio understanding
coding
computer interaction
```

Models and runtimes can therefore be added, removed, or replaced independently of the main application.

## Architecture

```text
                    OpenComfyHands
                           │
                    Rust + egui
                           │
                    ┌──────▼──────┐
                    │  Swarm Core │
                    └──────┬──────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
     Agents            Workflows           Models
        │                  │                  │
        ▼                  ▼                  ▼
   OpenHands            ComfyUI          Local runtimes
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                          cptr
                           │
                    Computer / Workspace
```

The application itself is designed to remain independent from any particular model.

## Technology

### Desktop Application

* Rust
* egui
* eframe
* wgpu

### Core

* Rust
* Tokio
* Serde
* asynchronous execution
* modular provider system

### Storage

* PostgreSQL for application state and metadata
* Git for source-code projects
* Git worktrees for isolated agent work
* S3-compatible object storage for large artifacts

### AI and Execution

* OpenHands for coding-agent capabilities
* ComfyUI for node-based media workflows
* cptr for computer/workspace interaction
* local model runtimes through pluggable providers

## Multimodal AI

OpenComfyHands is intended to support a broad range of local AI capabilities.

Examples include:

| Capability           | Example        |
| -------------------- | -------------- |
| Reasoning            | MiniCPM5       |
| Speech Recognition   | Nemotron ASR   |
| Vision               | Granite Vision |
| Audio                | LFM2.5 Audio   |
| Text-to-Speech       | OmniVoice      |
| Image Generation     | FLUX.2 Klein   |
| Video Generation     | Bernini        |
| Coding Agents        | OpenHands      |
| Computer Interaction | cptr           |

These are examples of integrations rather than requirements of the core architecture.

## Workflows

A major goal of OpenComfyHands is allowing capabilities to be chained together.

For example:

```text
Microphone
    ↓
Speech Recognition
    ↓
Language Model
    ↓
Image Generation
    ↓
Video Generation
    ↓
Text-to-Speech
```

Or:

```text
Repository
    ↓
Coding Agent
    ↓
Tests
    ↓
Build
    ↓
Generated Documentation
    ↓
Visual Assets
```

The application should make these pipelines visible and controllable rather than hiding them behind a single chat interface.

## Modular Design

OpenComfyHands separates:

```text
Models
Providers
Runtimes
Agents
Workflows
Tools
Capabilities
Artifacts
```

This means a model can be replaced without redesigning the application.

For example:

```text
vision.analyze
       │
       ├── Granite Vision
       ├── another local vision model
       └── future provider
```

The application depends on the **capability contract**, not on one specific implementation.

## Native Local Experience

OpenComfyHands is being designed as a native desktop application rather than a browser-first AI service.

The interface will provide a unified workspace for:

```text
Projects
Files
Tasks
Agents
Models
Workflows
Media
Artifacts
Terminal
Events
Logs
```

The goal is to make local AI feel like an integrated development and creative environment rather than a collection of disconnected applications.

## Privacy

Local execution is a core design principle.

Whenever supported by the user's hardware and configuration, processing should happen on the local machine without requiring data to be sent to external AI services.

Users should be able to see:

* which model is being used
* where it is running
* what files it can access
* what tools it can use
* what artifacts it produces

## Project Status

**Early development / experimental.**

The initial focus is building the underlying application architecture, interfaces, provider system, workflow system, and desktop UI.

Models and external runtimes are designed to be integrated incrementally.

## Roadmap

### Phase 1 — Foundation

* [ ] Rust application
* [ ] egui desktop interface
* [ ] project/workspace management
* [ ] core event system
* [ ] capability system
* [ ] provider interfaces

### Phase 2 — AI Integration

* [ ] OpenHands integration
* [ ] ComfyUI integration
* [ ] cptr integration
* [ ] local model providers
* [ ] multimodal pipelines

### Phase 3 — Workspace

* [ ] task management
* [ ] agent management
* [ ] workflow editor
* [ ] artifact browser
* [ ] terminal
* [ ] live execution logs

### Phase 4 — Advanced Orchestration

* [ ] task graphs
* [ ] agent coordination
* [ ] resource management
* [ ] isolated workspaces
* [ ] workflow composition

## Why OpenComfyHands?

The name combines the project's main influences:

**OpenHands**
AI coding and agent capabilities.

**ComfyUI**
Visual and multimodal workflow execution.

**Open WebUI**
Unified interaction and AI workspace concepts.

**cptr**
Computer and workspace interaction.

OpenComfyHands brings these ideas together into a single **local AI environment**.

## Contributing

OpenComfyHands is an experimental open-source project.

Contributions, integrations, workflow definitions, model providers, UI components, and ideas are welcome as the project develops.

## License

License: **TBD**
