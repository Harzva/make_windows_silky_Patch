# Token Saver Positioning

`make_windows_silky_Patch` saves tokens by reducing repeated rediscovery.

In Windows AI workspaces, the expensive part is often not one command. It is the same context being loaded again and again:

- which artifact is current;
- whether an installer has proof;
- why there are many similar ZIP/EXE files;
- whether Chinese copy is safe to publish;
- what the release gate was last time;
- which repeated workflow should become a rule, SOP, Skill, or script.

## Token Saver Contract

When a workflow would otherwise require a long explanation, convert it into a smaller durable artifact.

| Repeated context | Durable artifact |
| --- | --- |
| Folder state | `WINDOWS_SILKY_AUDIT.md` and `windows-silky-audit.json` |
| Release binary state | `*.evidence.json` beside the artifact |
| Evidence shape | `schemas/artifact-evidence.schema.json` |
| Publish readiness | `scripts/Invoke-WindowsSilkyPreflight.ps1` |
| Encoding risk | `scripts/Test-EncodingGate.ps1` |
| Reusable judgment | Skill, Agent, Rule, Prompt, SOP, or checklist |

## README Message

Use this project sentence when explaining the repo:

> Make Windows silky = save your tokens. Do not feed the same messy workspace to the model again; turn it into evidence, gates, and reusable patches.

## Boundaries

Saving tokens does not mean hiding risk.

- Keep destructive operations user-controlled.
- Keep evidence close to artifacts.
- Keep private raw paths, logs, cookies, and tokens out of public docs.
- Call unverified work a plan, not proof.
