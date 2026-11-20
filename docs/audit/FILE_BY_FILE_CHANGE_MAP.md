# File-by-File Change Plan — NEXORA AI

## 1. Action Classification Schema
Every repository file is classified under one of the following engineering designations:
- **`KEEP`**: Essential source, logic, asset, or test file retained without any modification.
- **`MODIFY`**: Existing file requiring surgical line edits to update branding, URLs, schemes, or identifiers.
- **`REPLACE`**: Asset or documentation file replaced with NEXORA AI branded equivalent.
- **`FIX`**: Minor compatibility adjustment required for proper execution.
- **`REVIEW`**: File audited and confirmed functional, pending user design choices.
- **`REMOVE`**: Deprecated or legacy file. *(Strict Rule: REMOVE requires explicit User approval; no core files are marked for removal).*

---

## 2. Comprehensive File-by-File Map

### Configuration & Root Files

#### 1. `app.json`
- **Classification**: `MODIFY`
- **Current Purpose**: Master Expo and Android application manifest configuration.
- **Current Configuration**: `name: "Mobile Agent"`, `slug: "mobile-agent"`, `package: "com.tecnicalbot.mobileagent"`, `scheme: "mobile-agent"`, `owner: "tecnicalbot"`, `projectId: "7944daea..."`.
- **Proposed Configuration**: `name: "NEXORA AI"`, `slug: "nexora-ai"`, `package: "com.nexora.ai"`, `scheme: "nexora-ai"`, owner & EAS projectId decoupled.
- **Reason**: Establish NEXORA AI identity and decouple from original developer's EAS account.
- **Dependencies**: Expo CLI, EAS CLI, Android build tools.
- **Functionality Affected**: App installation ID, deep linking, build target.
- **Compatibility Risk**: High (Creates distinct app on device).
- **Security Implications**: Prevents accidental build routing to original developer's EAS account.
- **Testing Requirements**: `node -e "JSON.parse(require('fs').readFileSync('app.json'))"`, prebuild check.
- **Rollback Strategy**: Restore from `checkpoint-before-topic3` branch.

#### 2. `package.json`
- **Classification**: `MODIFY`
- **Current Purpose**: Node.js package manifest, dependencies, scripts, and repository metadata.
- **Current Configuration**: `"name": "Mobile Agent"`, `"url": "https://github.com/TecnicalBot/mobile-agent"`.
- **Proposed Configuration**: `"name": "nexora-ai"`, `"url": "https://github.com/stylesheet48-max/nexora-ai"`.
- **Reason**: Point package repository and name to your GitHub ecosystem.
- **Dependencies**: pnpm.
- **Functionality Affected**: Package metadata, npm/pnpm tooling.
- **Compatibility Risk**: Low.
- **Security Implications**: Ensures dependencies and scripts point to your own repository.
- **Testing Requirements**: JSON syntax validation.
- **Rollback Strategy**: Restore via Git.

#### 3. `LICENSE`
- **Classification**: `MODIFY`
- **Current Purpose**: Open-source license and copyright declaration.
- **Current Configuration**: Copyright (c) 2026 Technical Bot.
- **Proposed Configuration**: Retain original notice; append Copyright (c) 2026 stylesheet48-max.
- **Reason**: Full MIT license compliance while establishing NEXORA AI transformation ownership.
- **Dependencies**: None.
- **Functionality Affected**: Legal compliance.
- **Compatibility Risk**: Low.
- **Security Implications**: None.
- **Testing Requirements**: Plain text review.
- **Rollback Strategy**: Restore via Git.

#### 4. `README.md`
- **Classification**: `REPLACE`
- **Current Purpose**: Repository homepage documentation and quick start guide.
- **Current Configuration**: Mobile Agent branding, TecnicalBot demo YouTube video, original links.
- **Proposed Configuration**: NEXORA AI branding, features overview, independent install instructions.
- **Reason**: Present independent identity on GitHub.
- **Dependencies**: Markdown renderer.
- **Functionality Affected**: Documentation only.
- **Compatibility Risk**: Low.
- **Security Implications**: Eliminates outdated links.
- **Testing Requirements**: Markdown rendering check.
- **Rollback Strategy**: Restore via Git.

---

### Navigation & Deep Linking

#### 5. `src/app/+native-intent.tsx`
- **Classification**: `MODIFY`
- **Current Purpose**: System deep link routing and OAuth callback interceptor.
- **Current Configuration**: `APP_SCHEMES = new Set(["mobile-agent:", "exp+mobile-agent:"])`, base `mobile-agent://app.home`.
- **Proposed Configuration**: `APP_SCHEMES = new Set(["nexora-ai:", "exp+nexora-ai:"])`, base `nexora-ai://app.home`.
- **Reason**: Enable incoming `nexora-ai://` deep links and OAuth redirects.
- **Dependencies**: Expo Router.
- **Functionality Affected**: Deep linking, OAuth completion, share intent routing.
- **Compatibility Risk**: Medium.
- **Security Implications**: Protects against foreign intent schemes.
- **Testing Requirements**: Deep link invocation testing.
- **Rollback Strategy**: Restore via Git.

#### 6. `modules/process-text/android/src/main/java/expo/modules/processtext/ProcessTextActivity.kt`
- **Classification**: `MODIFY`
- **Current Purpose**: Android system context menu text selection action.
- **Current Configuration**: `Uri.parse("mobile-agent:///settings/prompts?capture=1")`.
- **Proposed Configuration**: `Uri.parse("nexora-ai:///settings/prompts?capture=1")`.
- **Reason**: Match new URL scheme so captured text routes into NEXORA AI.
- **Dependencies**: Android Intent system.
- **Functionality Affected**: System text capture.
- **Compatibility Risk**: Low.
- **Security Implications**: Retains `setPackage(packageName)` isolation.
- **Testing Requirements**: Trigger text action from external app.
- **Rollback Strategy**: Restore via Git.

---

### Provider Authentication & OAuth

#### 7. `src/modules/providers/openai-chatgpt-oauth.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: OpenAI ChatGPT Plus PKCE OAuth login handler.
- **Current Configuration**: `Linking.createURL("", { scheme: "mobile-agent" })`.
- **Proposed Configuration**: `Linking.createURL("", { scheme: "nexora-ai" })`.
- **Reason**: Direct browser back to NEXORA AI upon successful OAuth login.
- **Dependencies**: `expo-linking`, `expo-web-browser`.
- **Functionality Affected**: ChatGPT OAuth completion.
- **Compatibility Risk**: Low.
- **Security Implications**: Validates OAuth state against PKCE verifier.
- **Testing Requirements**: Unit tests (`openai-chatgpt-oauth.test.ts`).
- **Rollback Strategy**: Restore via Git.

#### 8. `src/modules/providers/openai-oauth.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: Legacy OpenAI OAuth session management.
- **Current Configuration**: `Linking.createURL("", { scheme: "mobile-agent" })`.
- **Proposed Configuration**: `Linking.createURL("", { scheme: "nexora-ai" })`.
- **Reason**: Scheme synchronization.
- **Dependencies**: `expo-linking`.
- **Functionality Affected**: OAuth return flow.
- **Compatibility Risk**: Low.
- **Testing Requirements**: Unit tests (`openai-oauth-refresh.test.ts`).
- **Rollback Strategy**: Restore via Git.

#### 9. `src/modules/providers/openrouter-oauth.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: OpenRouter PKCE OAuth token exchange.
- **Current Configuration**: `Linking.createURL("", { scheme: "mobile-agent" })`.
- **Proposed Configuration**: `Linking.createURL("", { scheme: "nexora-ai" })`.
- **Reason**: Direct browser back to NEXORA AI upon OpenRouter auth.
- **Dependencies**: `expo-linking`.
- **Functionality Affected**: OpenRouter authentication.
- **Compatibility Risk**: Low.
- **Testing Requirements**: Unit tests (`openrouter-oauth.test.ts`).
- **Rollback Strategy**: Restore via Git.

---

### Remote Catalogs & Updates

#### 10. `src/modules/updates/github-release.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: In-app GitHub Release APK updater.
- **Current Configuration**: `RELEASES_API_URL = "https://api.github.com/repos/TecnicalBot/mobile-agent/releases"`.
- **Proposed Configuration**: `RELEASES_API_URL = "https://api.github.com/repos/stylesheet48-max/nexora-ai/releases"`.
- **Reason**: Query your own GitHub releases for new APK downloads.
- **Dependencies**: GitHub REST API, `expo-file-system`.
- **Functionality Affected**: In-app APK updates.
- **Compatibility Risk**: Medium.
- **Testing Requirements**: Verify against mock or live GitHub Releases API response.
- **Rollback Strategy**: Restore via Git.

#### 11. `src/modules/chat/composer-tip.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: Dynamic composer notice loader.
- **Current Configuration**: Points to `tecnicalbot/mobile-agent` on `raw.githubusercontent.com`.
- **Proposed Configuration**: Points to `stylesheet48-max/nexora-ai` on `raw.githubusercontent.com`.
- **Reason**: Decouple tips from original repo.
- **Dependencies**: `fetchWithTimeout`, local `catalog/chat-tip.json`.
- **Functionality Affected**: Composer tip bar.
- **Compatibility Risk**: Low (Automatic local fallback).
- **Testing Requirements**: Verify network fetch and fallback.
- **Rollback Strategy**: Restore via Git.

#### 12. `src/modules/chat/starter-prompts.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: Dynamic chat starter prompts loader.
- **Current Configuration**: Points to `tecnicalbot/mobile-agent` raw JSON.
- **Proposed Configuration**: Points to `stylesheet48-max/nexora-ai` raw JSON.
- **Reason**: Decouple prompts from original repo.
- **Dependencies**: `fetchWithTimeout`, local `catalog/chat-prompts.json`.
- **Functionality Affected**: Empty chat prompt suggestions.
- **Compatibility Risk**: Low (Automatic local fallback).
- **Testing Requirements**: Verify prompt suggestions.
- **Rollback Strategy**: Restore via Git.

#### 13. `src/modules/mcp/catalog.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: MCP server catalog loader.
- **Current Configuration**: Points to `tecnicalbot/mobile-agent` raw JSON.
- **Proposed Configuration**: Points to `stylesheet48-max/nexora-ai` raw JSON.
- **Reason**: Decouple MCP directory.
- **Dependencies**: `fetchWithTimeout`, local `catalog/mcp-servers.json`.
- **Functionality Affected**: Curated MCP discovery.
- **Compatibility Risk**: Low (Automatic local fallback).
- **Testing Requirements**: Verify MCP presets list.
- **Rollback Strategy**: Restore via Git.

#### 14. `src/modules/on-device/catalog.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: Downloadable LiteRT-LM model directory.
- **Current Configuration**: Points to `tecnicalbot/mobile-agent` raw JSON.
- **Proposed Configuration**: Points to `stylesheet48-max/nexora-ai` raw JSON.
- **Reason**: Decouple model weights directory.
- **Dependencies**: `fetchWithTimeout`, local `catalog/on-device-models.json`.
- **Functionality Affected**: On-device model download list.
- **Compatibility Risk**: Low (Automatic local fallback).
- **Testing Requirements**: `node scripts/verify-model-catalog.mjs`.
- **Rollback Strategy**: Restore via Git.

#### 15. `src/modules/plugins/catalog.ts` & `src/modules/plugins/manifest.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: Community plugin directory and manifest validation.
- **Current Configuration**: Points to `tecnicalbot/mobile-agent` raw JSON and README docs.
- **Proposed Configuration**: Points to `stylesheet48-max/nexora-ai`.
- **Reason**: Decouple plugin ecosystem.
- **Dependencies**: `fetchWithTimeout`, local `catalog/plugins.json`.
- **Functionality Affected**: Plugin discovery.
- **Compatibility Risk**: Low (Automatic local fallback).
- **Testing Requirements**: Verify plugin catalog list.
- **Rollback Strategy**: Restore via Git.

#### 16. `src/modules/skills/catalog.ts` & `catalog/skills.json`
- **Classification**: `MODIFY`
- **Current Purpose**: Agent skills catalog directory and bundled skill definitions.
- **Current Configuration**: References `TecnicalBot` in html-video entry and raw URL.
- **Proposed Configuration**: Points to `stylesheet48-max/nexora-ai`.
- **Reason**: Decouple skill catalog.
- **Dependencies**: `fetchWithTimeout`, local `catalog/skills.json`.
- **Functionality Affected**: Skill discovery and auto-install.
- **Compatibility Risk**: Low (Automatic local fallback).
- **Testing Requirements**: Verify skill installation.
- **Rollback Strategy**: Restore via Git.

#### 17. `src/modules/voice/catalog.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: Whisper voice models directory.
- **Current Configuration**: Points to `tecnicalbot/mobile-agent` raw JSON.
- **Proposed Configuration**: Points to `stylesheet48-max/nexora-ai` raw JSON.
- **Reason**: Decouple voice model weights directory.
- **Dependencies**: `fetchWithTimeout`, local `catalog/voice-models.json`.
- **Functionality Affected**: Voice model download list.
- **Compatibility Risk**: Low (Automatic local fallback).
- **Testing Requirements**: Verify voice models list.
- **Rollback Strategy**: Restore via Git.

---

### UI & Presentation Files

#### 18. `src/components/ui/app-sidebar.tsx`
- **Classification**: `MODIFY`
- **Current Purpose**: Main navigation drawer.
- **Current Configuration**: Header title `<Text>Mobile Agent</Text>`.
- **Proposed Configuration**: Header title `<Text>NEXORA AI</Text>`.
- **Reason**: Display new brand name in primary UI navigation.
- **Dependencies**: NativeWind, Reanimated.
- **Functionality Affected**: Visual branding.
- **Compatibility Risk**: Low.
- **Testing Requirements**: Visual UI inspection.
- **Rollback Strategy**: Restore via Git.

#### 19. `src/providers/app-state/constants.ts`
- **Classification**: `MODIFY`
- **Current Purpose**: Base agent system prompt and application constants.
- **Current Configuration**: System prompt references "Mobile Agent built by Technical Bot", Vercel docs, and TecnicalBot issues.
- **Proposed Configuration**: References "NEXORA AI", `https://github.com/stylesheet48-max/nexora-ai#readme`, and `https://github.com/stylesheet48-max/nexora-ai/issues`.
- **Reason**: Align assistant identity and support links with NEXORA AI.
- **Dependencies**: None.
- **Functionality Affected**: Assistant persona and new chat initial state.
- **Compatibility Risk**: Low.
- **Testing Requirements**: Start new chat and verify system prompt behavior.
- **Rollback Strategy**: Restore via Git.

---

### Database, Storage & Native Core Files

#### 20. `src/app/_layout.tsx`
- **Classification**: `KEEP`
- **Current Purpose**: Root application layout and provider composition.
- **Current Configuration**: `databaseName="nexora-ai.db"`.
- **Preservation Mandate**: Filename `nexora-ai.db` is strictly preserved to maintain data continuity.
- **Status**: Kept intact.

#### 21. `src/core/db/schema.ts`
- **Classification**: `KEEP`
- **Current Purpose**: Drizzle ORM schema for all 20 SQLite tables.
- **Preservation Mandate**: Retained without modification.

#### 22. `src/core/db/migrations.ts`
- **Classification**: `KEEP`
- **Current Purpose**: 28 database schema migrations.
- **Preservation Mandate**: Retained without modification.

#### 23. All 11 Native Modules (`modules/*`)
- **Classification**: `KEEP`
- **Current Purpose**: Background agent service, exact alarms, SAF, Termux, video export, etc.
- **Preservation Mandate**: 100% retained and compiled into NEXORA AI.

#### 24. All 8 Config Plugins (`plugins/*`)
- **Classification**: `KEEP`
- **Current Purpose**: Custom Expo Android build plugins.
- **Preservation Mandate**: 100% retained.
