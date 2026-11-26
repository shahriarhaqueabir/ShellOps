# Migration Plan: Ollama → llama.cpp

## Status: PLANNED (not started) — Config pre-updated 2026-11-18

## Current State
- **AI Endpoint:** `http://127.0.0.1:11434` (Ollama)
- **AI Model:** `OpsPowershell` (custom Ollama model via Modelfile)
- **SetupCompleted:** false
- **ModelFile:** null

## Pre-Updated Config (2026-11-18)
The following config changes have already been applied:
- `Config/ops-settings.json` updated:
  - `AIEndpoint` → `http://127.0.0.1:18000` (was `http://127.0.0.1:11434`)
  - `AIModel` → `Qwythos-9B-Claude-Mythos-5-1M-Q4_K_M.gguf` (was `OpsPowershell`)
  - Added `LlamaServerPort: 18000`
  - Added `LlamaServerFallbackPort: 18001`
  - Added `ModelsDir: <models directory>`
  - Added `_migration` note

Code still calls Ollama endpoints — migration of AI.ps1 etc. is still needed.

## Target State
- Use llama.cpp `llama-server` on a configurable port (default: 18000)
- Model: GGUF file from `<models directory>\`
- OpenAI-compatible API (`/v1/chat/completions`, `/v1/completions`)

## Migration Steps

### Phase 1: llama-server Integration
1. Add `llama-server` binary path to ops-settings.json (or auto-detect from PATH)
2. Create `Start-LlamaServer` function in AI.ps1
   - Launch `llama-server -m <model> --port <port> --host 127.0.0.1`
   - Track PID for cleanup
   - Health check via `GET /health`
3. Create `Stop-LlamaServer` function
4. Auto-start on module import if AI features are used

### Phase 2: API Call Migration
Current Ollama calls (in AI.ps1, Workflows.ps1, etc.):
```powershell
# Ollama format
Invoke-RestMethod -Uri "http://127.0.0.1:11434/api/generate" -Method POST -Body @{
    model = "OpsPowershell"
    prompt = $prompt
    stream = $false
}
```

New llama.cpp format:
```powershell
# OpenAI-compatible format
Invoke-RestMethod -Uri "http://127.0.0.1:18000/v1/chat/completions" -Method POST -Body @{
    model = "OpsPowershell"
    messages = @(@{ role = "user"; content = $prompt })
    stream = $false
} | Select-Object -ExpandProperty choices | Select-Object -First 1 | ForEach-Object { $_.message.content }
```

### Phase 3: Model Selection
- Default model: `Qwythos-9B-Claude-Mythos-5-1M-Q4_K_M.gguf` (already in `<models directory>\`)
- Allow user to specify model via `ops-settings.json` → `AIModel` field (now a filename instead of Ollama model name)
- Update `Onboarding.ps1` to let user pick from available GGUF files

### Phase 4: Modelfile → System Prompt
- The old `OpsPowershell` Ollama model had a custom system prompt
- Extract that system prompt and pass it as the `system` message in chat completions
- Store in `ops-settings.json` → `SystemPrompt` field

### Phase 5: Testing
- Test each AI-dependent function: AI.ps1, Workflows.ps1, Security.ps1, System.ps1, Report.ps1
- Verify streaming works for long responses
- Test model switching

## Files to Modify
- `Config/ops-settings.json` — add LlamaServerPort, SystemPrompt, update AIModel format
- `Modules/PowershellOps/Public/AI.ps1` — core migration
- `Modules/PowershellOps/Public/Onboarding.ps1` — model selection UI
- `Modules/PowershellOps/Public/Workflows.ps1` — AI calls
- `Modules/PowershellOps/Public/Security.ps1` — AI calls
- `Modules/PowershellOps/Public/System.ps1` — AI calls
- `Modules/PowershellOps/Public/Report.ps1` — AI calls
- `Modules/PowershellOps/Private/Helpers.ps1` — server management helpers

## Dependencies
- llama.cpp v1.0.0 at `<llama.cpp install directory>` (on PATH)
- At least one GGUF model in `<models directory>\`

## Estimated Effort
- Phase 1-2: 2-3 hours
- Phase 3-4: 1-2 hours
- Phase 5: 1 hour
- Total: ~4-6 hours (focused session)
