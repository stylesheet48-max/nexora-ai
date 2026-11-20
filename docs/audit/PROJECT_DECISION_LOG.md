# Project Decision Log — NEXORA AI

## Chronological Engineering & Governance Records

Every significant architectural, branding, and technical decision is recorded in this log with its rationale, approval timestamp, and implementation status.

---

### Record 001: Foundation Codebase & Scope Boundary
- **Date**: 2026-11-20
- **Decision**: Use the existing cloned `mobile-agent` source code (`https://github.com/TecnicalBot/mobile-agent.git`) as the sole foundation. Do not create a separate application from scratch.
- **Rationale**: Preserves thousands of hours of native Android module development, custom LiteRT-LM runtime linkage, exact alarm scheduling, and video export engineering.
- **Constraints**: Strict "No New Features" scope. No dependencies added or removed without explicit approval.
- **Status**: **APPROVED & ACTIVE**

---

### Record 002: Core Application Identity Coordinates
- **Date**: 2026-11-20
- **Decision**: Define the final application identity as:
  - Application Name: **`NEXORA AI`**
  - Application Slug: **`nexora-ai`**
  - Android Package Name / Application ID: **`com.nexora.ai`**
  - iOS Bundle Identifier: **`com.nexora.ai`**
  - Custom URL Scheme: **`nexora-ai://`**
  - GitHub Ecosystem: **`stylesheet48-max/nexora-ai`**
- **Rationale**: Fully establishes the User's proprietary brand and GitHub ecosystem across all OS registries and manifests.
- **Status**: **APPROVED & CONFIGURED**

---

### Record 003: Backend & Database Architecture Preservation
- **Date**: 2026-11-20
- **Decision**:
  1. Maintain 100% serverless, Local-First client architecture.
  2. Keep existing SQLite database filename as **`nexora-ai.db`**.
  3. Retain Drizzle ORM and all 28 schema migrations.
  4. Retain `expo-secure-store` Android Keystore encryption for secrets.
  5. Do NOT create a remote backend server or connect Turso/Supabase/PowerSync at this stage.
- **Rationale**: Renaming the database file or introducing a cloud sync layer risks data loss, latency, and unnecessary hosting costs. Local-First architecture is private, fast, and stable.
- **Status**: **APPROVED & LOCKED**

---

### Record 004: Decoupling Original EAS Project & Expo Owner
- **Date**: 2026-11-20
- **Decision**: Temporarily remove `"owner": "tecnicalbot"`, `"updates": { ... }`, and `"extra.eas.projectId"` (`7944daea-4e83-4c42-8bb5-604ac6ec6ea8`) from `app.json`. Do not create or link a new EAS project yet.
- **Rationale**: Prevents accidental build or OTA update routing to the original developer's Expo account. Local Gradle and local EAS APK builds remain 100% functional without cloud EAS dependencies.
- **Status**: **APPROVED & APPLIED**

---

### Record 005: Deep-Link & OAuth Scheme Synchronization
- **Date**: 2026-11-20
- **Decision**: Update all deep-link and OAuth callback configurations to use `nexora-ai://` consistently across:
  - `src/app/+native-intent.tsx`
  - `src/modules/providers/openai-chatgpt-oauth.ts`
  - `src/modules/providers/openai-oauth.ts`
  - `src/modules/providers/openrouter-oauth.ts`
  - `modules/process-text/android/.../ProcessTextActivity.kt`
- **Rationale**: Guarantees external browser logins (ChatGPT, OpenRouter) and system text selection intents route seamlessly into NEXORA AI.
- **Status**: **APPROVED & APPLIED**

---

### Record 006: Pre-Implementation Git Checkpoint
- **Date**: 2026-11-20
- **Decision**: Create a dedicated Git checkpoint branch `checkpoint-before-topic3` before applying any file modifications.
- **Rationale**: Provides an immediate, zero-risk rollback path to the pristine cloned state if any unexpected regressions occur.
- **Status**: **VERIFIED & SECURED**

---

### Record 007: Comprehensive 17-Document Master Audit Suite
- **Date**: 2026-11-20
- **Decision**: Author an exhaustive 17-document audit suite in `docs/audit/` covering every architectural, native, legal, security, and testing subsystem before executing subsequent transformation phases.
- **Rationale**: Eliminates guesswork, documents all 28 original references, establishes testing protocols, and enforces strict engineering governance.
- **Status**: **IN PROGRESS (PHASE B)**
