# Upgrade Roadmap

This repository is useful as a first patch kit. The next upgrades should make it more automatic, more testable, and easier to install.

## High-Value Upgrades

| Priority | Upgrade | Why it matters | First deliverable |
| --- | --- | --- | --- |
| P0 | Archive planner | The audit currently reports duplicates but does not produce a safe move plan | `New-WindowsSilkyArchivePlan.ps1` with dry-run JSON |
| P0 | Schema validation | Evidence manifests should be machine-checkable | JSON Schema for `artifact-evidence.v1` |
| P0 | Test fixtures | Scripts need repeatable local tests without scanning a real private workspace | `tests/fixtures/duplicate-artifacts` |
| P1 | HTML report | Markdown is good for GitHub, but local review is faster with grouped cards and filters | `WINDOWS_SILKY_AUDIT.html` |
| P1 | PowerShell module packaging | Users should be able to install commands instead of copying scripts | `MakeWindowsSilky.psd1` and exported functions |
| P1 | GitHub Action | Public repos should run preflight in CI before release tags | `.github/workflows/windows-silky-preflight.yml` |
| P1 | Codex hook candidate | Release tasks should automatically remind agents to run preflight | Hook spec in `patches/hooks/` |
| P2 | WebView2 classifier | `.WebView2` folders need a smarter owner/current/stale decision | WebView2 residue rule with artifact linkage |
| P2 | README proof generator integration | Visual proof should be produced or checked as part of release prep | Integration note for screenshot/thumbnail tools |
| P2 | Local dashboard | A small local index could show artifact trust state across many projects | Static `silky-index.html` generated from audit JSON |

## Suggested Next 7 Days

| Day | Action | Done when |
| --- | --- | --- |
| 1 | Add JSON Schema for evidence manifests | Invalid manifests fail preflight |
| 2 | Add script fixtures and basic smoke tests | Audit, encoding gate, and preflight run against fixtures |
| 3 | Build archive dry-run planner | Duplicate groups produce safe move commands without executing them |
| 4 | Add GitHub Action | README/preflight/encoding gate run on pull requests |
| 5 | Package scripts as a PowerShell module | Commands can be imported with one line |
| 6 | Add hook candidate | Agents know when to run Windows silky gate |
| 7 | Generate HTML report | Local review becomes easier than reading a long Markdown table |

## Product Direction

The strongest direction is not "Windows optimizer." It is:

> A trust layer for Windows AI workspaces: every artifact has proof, every repeated workflow becomes a patch, and every public release has a gate.

That positioning keeps the repository safe and believable while still solving the real friction.
