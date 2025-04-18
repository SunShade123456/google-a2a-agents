# Google A2A Agent

This repository implements a simple Agent-to-Agent (A2A) communication example based on Google's A2A protocol. A2A is Google's proposed standard for enabling AI agents to communicate with each other through a standardized API.

This document provides an in-depth technical exploration of Google’s Agent2Agent (A2A) protocol and reference implementation, covering its architecture, core specifications, sample agents and host applications, message flow, and practical setup instructions.

## 📝 Overview

The Agent2Agent (A2A) protocol is an open standard developed by Google to enable seamless communication and interoperability between independent AI agents built on diverse frameworks or by different vendors. By defining a common JSON‑RPC over HTTP(S) interface, A2A allows agents to discover each other’s capabilities, negotiate interaction modes (text, forms, files, audio/video), and collaborate on tasks securely and efficiently.

## ⚙️ Core Concepts

### Agent Card

An Agent Card is a public metadata file (typically hosted at `/.well-known/agent.json`) that describes an agent’s capabilities, supported endpoints, authentication requirements, and skills. Clients use the Agent Card for service discovery and capability negotiation.
### A2A Server & Client

- **A2A Server**: An HTTP endpoint implementing A2A protocol methods (`tasks/send`, `tasks/sendSubscribe`, `tasks/pushNotification/set`). It receives client requests, orchestrates task execution, and returns updates or artifacts.
- **A2A Client**: An application or agent that consumes A2A services by sending requests to a server’s URL and handling responses or streaming events.

### Task Lifecycle

Tasks are the central units of work exchanged between agents. Each task has a unique ID and passes through states: `submitted` → `working` → (`input-required` if extra data is needed) → `completed` or `failed` or `canceled`.

### Messages and Parts

- **Message**: A turn in the conversation with roles (`user` for client, `agent` for server) and a list of `Parts`.
- **Part**: The atomic content unit within a `Message` or `Artifact`. Can be:
  - `TextPart` for plain text.
  - `FilePart` carrying file bytes or URIs.
  - `DataPart` for structured JSON (e.g., web forms).

### Artifacts

Artifacts represent outputs that an agent generates during a task (e.g., generated files, final structured data), each containing one or more `Parts`.

### Streaming & Push Notifications

- **Streaming**: For long-running tasks, clients can subscribe via `tasks/sendSubscribe` and receive Server-Sent Events (SSE) with `TaskStatusUpdateEvent` or `TaskArtifactUpdateEvent` messages for real-time progress.
- **Push Notifications**: Servers supporting `pushNotifications` can send proactive updates to a client-provided webhook URL configured via `tasks/pushNotification/set`.

## 📜 Specification

The full A2A protocol is defined by a JSON schema located at `specification/json/a2a.json`, detailing data structures for agent cards, tasks, messages, parts, artifacts, and error handling. This 46.9 KB schema serves as the formal specification for any compliant implementation.
