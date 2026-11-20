# Feature Inventory & Preservation Audit — NEXORA AI

## Preservation Mandate
Under Section 2 and Section 5 of the Master Directive, **no existing feature may be removed, deprecated, weakened, or rewritten**. Every capability listed below has been verified in the cloned source code and must remain 100% operational in **NEXORA AI**.

---

## Complete Feature Matrix

| # | Feature Subsystem | Technical Implementation | Key Source Files | Status |
| :- | :--- | :--- | :--- | :--- |
| 1 | **Multi-Provider Cloud AI** | Google Gemini, OpenAI, Anthropic Claude, xAI Grok, OpenAI-Compatible, Ollama via Vercel AI SDK Core | `src/modules/providers/`, `src/modules/runtime/` | Preserved |
| 2 | **OAuth Provider Authentication** | Browser PKCE OAuth flow for OpenAI ChatGPT Plus/Team accounts and OpenRouter | `src/modules/providers/openai-chatgpt-oauth.ts`, `openrouter-oauth.ts` | Preserved |
| 3 | **On-Device LLM (Offline AI)** | Local LiteRT-LM runtime executing Gemma 2B, Gemma 3, LFM3 models directly on mobile NPU/GPU/CPU | `src/modules/on-device/`, `plugins/with-litert-lm.js`, `catalog/on-device-models.json` | Preserved |
| 4 | **Context Compaction & Token Budgeting** | Sliding window context management, automatic heuristic summary generation, token pruning | `src/modules/context/compaction.ts`, `prompt-budget.ts`, `summarizer.ts` | Preserved |
| 5 | **Long-Term AI Memory** | Semantic/textual memory extraction, persistence in SQLite `memories` table, dynamic prompt injection | `src/modules/memory/`, `src/core/db/schema.ts` | Preserved |
| 6 | **MCP (Model Context Protocol) Client** | Full MCP v1.32 client supporting HTTP and SSE transports, headers auth, and token proxying | `src/modules/mcp/`, `catalog/mcp-servers.json` | Preserved |
| 7 | **Phone Tools MCP Server (Local Bridge)** | Exposes Android battery status, device info, notifications, camera, and files to external agents | `src/modules/bridge/mcp-server.ts`, `native-http-server.ts` | Preserved |
| 8 | **Agent Skills System** | Markdown-defined skills (`SKILL.md`) with keyword auto-matching, tool whitelisting, and execution | `src/modules/skills/`, `catalog/skills.json` | Preserved |
| 9 | **Dynamic JS Plugin System** | Sandboxed JavaScript plugins with `@mobile-agent-plugin` manifests and daily auto-update checks | `src/modules/plugins/`, `catalog/plugins/` | Preserved |
| 10 | **Background Agent Execution** | Android Foreground Service with continuous WakeLock, battery exemption, and persistent notification | `modules/background-agent-service/`, `plugins/with-background-agent.js` | Preserved |
| 11 | **Job Scheduler & Exact Alarms** | Cron-based autonomous agent execution using `AlarmManager.setExactAndAllowWhileIdle` | `modules/scheduler-alarm/`, `src/modules/scheduler/` | Preserved |
| 12 | **Voice STT (Speech-to-Text)** | System speech recognition (`expo-speech-recognition`) and offline on-device Whisper (`whisper.rn`) | `src/modules/voice/`, `modules/persistent-model-download/` | Preserved |
| 13 | **Voice TTS & Spoken Announcements** | Native TTS and automatic spoken task completion/permission alerts using Android TTS engine | `modules/local-tts/`, `src/modules/voice/local-tts.ts` | Preserved |
| 14 | **Antigravity Coding Harness** | JSON-RPC stdio bridge to Google Antigravity CLI agent via local network HTTP/SSE | `src/hooks/use-antigravity-harness.ts`, `tools/antigravity-bridge/` | Preserved |
| 15 | **Claude Code ACP Harness** | Agent Communication Protocol bridge to Anthropic Claude Code CLI over LAN | `src/hooks/use-claude-harness.ts`, `tools/claude-bridge/` | Preserved |
| 16 | **OpenCode SDK Integration** | Direct client bridge to OpenCode server sessions and session lifecycle management | `src/hooks/use-opencode.ts`, `@opencode/client` | Preserved |
| 17 | **Termux Stream Bridge** | Headless stdio execution channel communicating directly with Android Termux terminal app | `modules/termux-stream/`, `src/hooks/use-termux-stream.ts` | Preserved |
| 18 | **SAF (Storage Access Framework)** | Read/write/list operations across external Android shared directories and SD cards | `modules/saf-file-operations/`, `src/modules/files/` | Preserved |
| 19 | **HTML & Canvas to MP4 Video Export** | Headless WebGL/Canvas/Three.js frame capture exported as H.264 MP4 videos | `modules/html-video-export/`, `src/modules/preview/` | Preserved |
| 20 | **Scene Preview & Audio Mix Checking** | In-app video storyboard previews, timeline scrubber, and multi-track audio waveform analysis | `src/components/ui/video-preview-dialog.tsx`, `audio-analysis-host.tsx` | Preserved |
| 21 | **Document Processing & Diffing** | Full reading, section extraction, modification, and diffing for PDF, DOCX, XLSX, and PPTX files | `src/modules/documents/`, `pdf-lib`, `docx`, `xlsx` | Preserved |
| 22 | **Android System Text Selection** | `ACTION_PROCESS_TEXT` system menu integration to capture highlighted text from other apps | `modules/process-text/`, `src/app/+native-intent.tsx` | Preserved |
| 23 | **Android Share Sheet Target** | `ACTION_SEND` intent filter receiving incoming text, files, and images directly into active chat | `modules/share-target/`, `plugins/with-share-target.js` | Preserved |
| 24 | **Dual-Track Application Updates** | In-app GitHub Release APK updater with background download + Expo EAS OTA update state machine | `src/modules/updates/`, `src/providers/check-for-updates.tsx` | Preserved |

---

## Detailed Subsystem Specifications

### 1. AI Execution Engine & Providers
- **Cloud Providers**: Native drivers for Google Gemini (`@ai-sdk/google`), OpenAI (`@ai-sdk/openai`), Anthropic Claude (`@ai-sdk/anthropic`), xAI Grok (`@ai-sdk/xai`), and OpenAI-Compatible (`@ai-sdk/openai-compatible`).
- **Local Model Provider**: Ollama integration supporting local network addresses and local inference.
- **On-Device LiteRT-LM**: Supports quantized `.litertlm` models (Gemma 2 2B, Gemma 3 1B/4B, LFM3). File integrity verified via pinned SHA-256 hashes and byte lengths.
- **Multimodal Capabilities**: Image attachment, audio recording input, document attachment, and live camera QR scanning.

### 2. Desktop Host Coding Bridges
- The application contains three independent bridges to desktop coding agents:
  1. **Google Antigravity**: Configured in [src/hooks/use-antigravity-harness.ts](file:///storage/emulated/0/arya1/src/hooks/use-antigravity-harness.ts). Talks to `tools/antigravity-bridge/` on default port `7801`.
  2. **Anthropic Claude Code**: Configured in [src/hooks/use-claude-harness.ts](file:///storage/emulated/0/arya1/src/hooks/use-claude-harness.ts). Talks to `tools/claude-bridge/` on default port `7802`.
  3. **OpenCode**: Configured in [src/hooks/use-opencode.ts](file:///storage/emulated/0/arya1/src/hooks/use-opencode.ts). Connects via `@opencode/client`.
- Pairing is secured via QR code scanning using standard HMAC token validation.

### 3. Background Services & Alarm Engine
- **Background Agent Service** (`modules/background-agent-service`): Implements an Android Foreground Service displaying a persistent notification. Acquires a partial wake lock (`PowerManager.PARTIAL_WAKE_LOCK`) so long-running tasks continue when the screen turns off.
- **Scheduler Alarm** (`modules/scheduler-alarm`): Uses Android's `AlarmManager.setExactAndAllowWhileIdle` and a broadcast receiver listening for `ACTION_BOOT_COMPLETED` so recurring schedules persist across device reboots.

### 4. Media & Video Export Pipeline
- The app features a native video rendering engine (`modules/html-video-export`) capable of turning HTML5/Canvas/Three.js code into MP4 files without requiring an external video rendering server.
- Supported features:
  - Chroma keying / green screen removal.
  - Multi-track audio mixing and volume normalization.
  - Scene storyboarding and timeline slicing.
