<div align="center">
  <img src="./docs/readme-assets/logo.svg" alt="make_windows_silky_Patch logo" width="240" />
  <h1>make_windows_silky_Patch</h1>
  <p><strong>把 Windows 工作台的卡顿感，拆成可执行的清理、证据、发布与编码门禁补丁。</strong></p>
  <p>
    <a href="./scripts">Scripts</a>
    ·
    <a href="./patches">AgentWorkOS Patches</a>
    ·
    <a href="./checklists/windows-silky-preflight.md">Checklist</a>
    ·
    <a href="./docs/evidence-map.md">Evidence Map</a>
  </p>
  <p>
    <img alt="Platform" src="https://img.shields.io/badge/platform-Windows-2563eb" />
    <img alt="PowerShell" src="https://img.shields.io/badge/scripts-PowerShell-0f766e" />
    <img alt="Patch type" src="https://img.shields.io/badge/patches-skill%20%7C%20agent%20%7C%20rule%20%7C%20SOP-7c3aed" />
    <img alt="License" src="https://img.shields.io/badge/license-MIT-111827" />
  </p>
</div>

## 这是什么

`make_windows_silky_Patch` 是一个 Windows 工作台顺滑补丁包。它不是玄学加速器，也不改注册表；它把本地经验里反复造成“Windows 越用越不顺”的部分提炼成可复用资产：

- 重复的 EXE/ZIP/APK/预览文件，让资源管理器和项目入口越来越乱。
- 没有 checksum、截图、安装结果、release note 的发布产物，导致每次都要重新判断哪个能用。
- README、截图、发布说明、GitHub 发布前置检查分散，发布前总要重新开一轮整理。
- 中文文档和 UI 文案的 mojibake/乱码太晚才发现。
- 大项目没有入口卡，回到项目时只能靠记忆找状态。

这套补丁的目标是让 Windows 上的 AI/product/dev 工作区更轻、更可追踪、更少重复打开上下文。

## 快速开始

在任意 Windows 工作区运行审计：

```powershell
pwsh -ExecutionPolicy Bypass -File .\scripts\Invoke-WindowsSilkyAudit.ps1 -Root "C:\path\to\workspace" -Days 14
```

给一个发布产物生成证据清单：

```powershell
pwsh -ExecutionPolicy Bypass -File .\scripts\New-ArtifactEvidenceManifest.ps1 -Artifact ".\dist\App-v1.0.0-windows-x64.exe" -SmokeResult "pending"
```

发布前跑门禁：

```powershell
pwsh -ExecutionPolicy Bypass -File .\scripts\Invoke-WindowsSilkyPreflight.ps1 -ProjectRoot .
```

## 补丁地图

| Patch | 解决的卡顿 | 入口 |
| --- | --- | --- |
| Workspace Silky Audit | 找出重复安装包、WebView2 残留、无证据产物、乱码风险、大项目无入口 | [`scripts/Invoke-WindowsSilkyAudit.ps1`](./scripts/Invoke-WindowsSilkyAudit.ps1) |
| Artifact Evidence Manifest | 每个 EXE/ZIP/MSI 都有 hash、来源、安装/冒烟状态、截图、发布目标 | [`scripts/New-ArtifactEvidenceManifest.ps1`](./scripts/New-ArtifactEvidenceManifest.ps1) |
| Encoding Gate | 发布前拦住中文乱码、复制损坏、mojibake | [`scripts/Test-EncodingGate.ps1`](./scripts/Test-EncodingGate.ps1) |
| Windows Silky Preflight | README、视觉证明、发布产物证据、编码检查合并成一个 gate | [`scripts/Invoke-WindowsSilkyPreflight.ps1`](./scripts/Invoke-WindowsSilkyPreflight.ps1) |
| AgentWorkOS Assets | 把顺滑经验固化成 Skill、Agent、Rule、Prompt、SOP | [`patches/`](./patches) |

## 提炼依据

本仓库从 `13-summarize-method-ablation` 的经验资产中只提取 Windows 顺滑相关内容，并只发布蒸馏后的证据：

| Evidence | Signal |
| --- | --- |
| Workspace scan | 24 个项目、68,310 个文件、100 个发布产物、293 个重复产物组、96 个无证据发布产物、44 个潜在编码问题 |
| Windows artifact clues | `RepoAtlas-v*-windows-x64.exe` 多版本重复、`.WebView2` 目录残留、`ChatGPT Installer` 多份重复、`gitmarket-windows-x86_64` 压缩包/EXE 分散 |
| Workflow clusters | Release/repo prep、visual proof、encoding cleanup、local model handoff、workflow assetization 反复出现 |

完整证据说明见 [`docs/evidence-map.md`](./docs/evidence-map.md)。

## 仓库结构

```text
make_windows_silky_Patch/
├─ scripts/        # 可运行的 Windows 审计、证据清单、编码门禁、发布门禁
├─ patches/        # Skill / Agent / Rule / Prompt 补丁
├─ checklists/     # 人类可执行检查表
├─ sops/           # 周期性工作台重置 SOP
├─ templates/      # evidence manifest 和 project card 模板
└─ docs/           # 证据地图、AgentWorkOS 七层提炼、设计原则
```

## 不做什么

- 不自动删除文件。
- 不修改注册表、系统策略、电源计划或 Defender 设置。
- 不提交私有路径、token、cookie、原始 session log。
- 不把“建议”当成完成状态，必须落到脚本、清单、规则、Skill 或 SOP。

## 适合谁

- Windows 上做很多本地 AI/移动端/桌面工具实验的人。
- 经常堆出多个 `*-windows-x64.exe`、`.zip`、`.WebView2`、`preview.html` 的人。
- 每次发布都要重新补 README、截图、release note、checksum 的人。
- 想把 Codex/AgentWorkOS 经验沉淀成可复用补丁的人。

## License

MIT
