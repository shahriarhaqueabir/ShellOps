# Release Readiness / Desired State

- **Repo:** `private/ShellOps`
- **Owner:** `TBD`
- **Status:** `draft`
- **Last verified:** `2026-11-25` (recon only; no runtime verification)
- **Target platforms:** `TBD`
- **Distribution modes:** `source checkout | release build` (decide per repo)

## 0) Recon snapshot (auto-scaffolded)

> Groundwork only. Verify before treating as fact.

- **Path:** `E:\Projects\repos\private\ShellOps`
- **Branch:** `main`
- **Dirty files:** `2` (do not include accidental WIP in release snapshot)
- **Remote:** `https://github.com/shahriarhaqueabir/ShellOps.git`
- **Detected markers:** `(none detected)`
- **Workflows:** `ci.yml`
- **License files:** `LICENSE`
- **Has docs/tests/src:** `yes/no/no`
- **Top dirs:** `.github,.idea,.memory,Config,docs,Memory,modelfiles,Modules,plans,private,Reports`
- **Package scripts (if any):** `(none)`
- **README lead:** `# ShellOps - PowerShell Operations Dashboard / <p align="center"> /   <img src="ShellOps.jpg" alt="ShellOps" width="400"> / </p> / > **Advanced Operational Intelligence & Universal AI Companion for Windows.** / > Private. High-Performance. Local Agentic Stack. 81 utility tools · 7`
- **Recon note:** Contains operational tooling; release snapshot must scrub machine-specific config and private reports/memory.

## 1) User promise

- **Source checkout:** Inspect README Quick reference; likely `pwsh -File ...` or module import. Verify exact entrypoint.
- **Release build:** CI workflow exists; no release workflow detected.
- **First-run experience:** `TBD - detect -> diff -> provision missing/drifted -> verify -> handoff`
- **Non-goals:** `TBD - what this release does not provision/support`

## 2) Desired-state manifest

| Component | Required | Version/Constraint | Source of truth | Install/Provision (idempotent) | Verify | Notes/Rollback |
|---|---:|---|---|---|---|---|
| OS/runtime | TBD | TBD | README/docs/script | Detect; install or guide; never silently destructive | command/check |  |
| Toolchain/package manager | TBD | TBD | (none detected) | idempotent install | `TBD` |  |
| App dependencies | TBD | lockfile/manifest | repo manifest | install only when missing/drifted | `TBD` |  |
| External services/CLIs | TBD | TBD | docs/scripts | preserve user config; install fallback only if empty | health check |  |
| Models/assets/binaries | TBD | hash/size/version | manifest/LFS/release asset | download/verify only missing/drifted | hash + smoke |  |
| Config/secrets | yes | no secrets committed | env/config docs | create example/default only when missing; never overwrite | secret scan |  |
| Network/ports/permissions | TBD | TBD | docs/code | document; request minimal permissions | smoke |  |

**Recon provisioning hints:** PowerShell 7+, Windows dependencies, optional Ollama/local model/modelfiles, config/module paths, permissions.

## 3) Provision flow (XaaS-style)

**Flow:** `detect -> diff -> provision missing/drifted -> verify -> handoff`

- **Detect:** `TBD - script/check that inspects current machine/repo state`
- **Diff:** missing, wrong version, unhealthy, or drifted from desired state.
- **Provision:** only missing/drifted items; safe to rerun; user-visible actions.
- **Verify:** required checks pass before user handoff.
- **Handoff:** user-driven "Test all systems / Go" or explicit ready state.

### Verification checklist

| Check | Required | Command/Method | Pass criteria | Status |
|---|---:|---|---|---|
| Bootstrap/install | yes | `TBD` | completes cleanly on fresh env | TBD |
| App starts | yes | `TBD` | process/window/service healthy | TBD |
| Core smoke flow | yes | `TBD` | primary user flow works | TBD |
| Existing tests/build | if present | CI workflow - inspect; add Pester/smoke checks for dashboard/tools as applicable. | exit 0 / expected pass count | TBD |
| Cleanup/stop/cancel | if applicable | `TBD` | no orphan process/resource leak | TBD |

## 4) Clean snapshot / release contents

### Include (candidate)
- `README*`, `LICENSE*`, user docs, required source/runtime assets, supported install/run scripts, tests/verification scripts you support.

### Exclude (candidate)
- `.memory/`, `Memory/` user data, `Reports/` generated/private, `private/`, logs, model caches, secrets.
- Add repo-specific excludes after review.

### Packaging notes
- **Source checkout artifact:** `TBD`
- **Release artifact:** `TBD`
- **Integrity:** `TBD - SHA256/signature/provenance`
- **Size/bandwidth:** `TBD - call out large runtimes/models/assets`

## 5) Security / config hygiene

- [ ] No credentials/tokens/PII/personal paths in tracked release files.
- [ ] Existing user config preserved; defaults installed only when missing.
- [ ] Download URLs, hashes, and permissions explicit.
- [ ] Telemetry/network behavior documented (or explicitly none).
- [ ] Logs do not leak secrets.

## 6) Licensing / redistribution

| Component | License/Terms | Bundled/Downloaded | Redistribution OK | Action |
|---|---|---|---:|---|
| Repo code | LICENSE | n/a | TBD | verify |
| Third-party deps/runtime | TBD | TBD | TBD | inventory |
| Models/assets/fonts/binaries | TBD | TBD | TBD | verify before release |

**Recon note:** LICENSE present.

## 7) Open decisions / blockers

- [ ] Confirm source-checkout vs release-build user promise.
- [ ] Define desired-state manifest and idempotent provision checks.
- [ ] Run verification commands and record evidence.
- [ ] Confirm clean snapshot include/exclude list.
- [ ] Complete licensing/redistribution review.
