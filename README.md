<div align="center">
  <img src="./docs/readme-assets/logo.svg" alt="make_windows_silky_Patch logo" width="240" />
  <h1>make_windows_silky_Patch</h1>
  <p><strong>Windows AI 工作台顺滑补丁包：让重复安装包、缺证据发布、乱码、项目回看成本，变成脚本、门禁和 AgentWorkOS 资产。</strong></p>
  <p>
    <a href="./scripts">Scripts</a>
    ·
    <a href="./patches">AgentWorkOS Patches</a>
    ·
    <a href="./docs/windows-patch-10-factors.md">10 Factors</a>
    ·
    <a href="./docs/upgrade-roadmap.md">Roadmap</a>
    ·
    <a href="./docs/evidence-map.md">Evidence Map</a>
  </p>
  <p>
    <img alt="Platform" src="https://img.shields.io/badge/platform-Windows-2563eb" />
    <img alt="PowerShell" src="https://img.shields.io/badge/scripts-PowerShell-0f766e" />
    <img alt="Patch type" src="https://img.shields.io/badge/patches-skill%20%7C%20agent%20%7C%20rule%20%7C%20SOP-7c3aed" />
    <img alt="Safety" src="https://img.shields.io/badge/default-report--first-f59e0b" />
    <img alt="License" src="https://img.shields.io/badge/license-MIT-111827" />
  </p>
</div>

<p align="center">
  <img src="./docs/readme-assets/patch-flow.svg" alt="Windows silky patch flow" width="900" />
</p>

## 这是什么

`make_windows_silky_Patch` 不是“系统优化玄学包”。它不默认改注册表、不关 Defender、不乱动电源计划。

它解决的是 Windows 上 AI/product/dev 工作区最常见的真实卡顿：文件越来越多、发布产物越来越像谜题、README 和截图总是最后补、中文乱码发布前才发现、大项目回看全靠记忆。

这个仓库把这些经验沉淀成一套可复用补丁：

- PowerShell 审计脚本；
- artifact evidence manifest；
- README / visual proof / release preflight 门禁；
- 编码检查；
- Skill、Agent、Rule、Prompt、SOP；
- Windows Patch 10 Factors。

## 30 秒开始

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

## Patch Map

| Patch | 解决的卡顿 | 入口 |
| --- | --- | --- |
| Workspace Silky Audit | 找出重复安装包、WebView2 残留、无证据产物、乱码风险、大项目无入口 | [`scripts/Invoke-WindowsSilkyAudit.ps1`](./scripts/Invoke-WindowsSilkyAudit.ps1) |
| Artifact Evidence Manifest | 每个 EXE/ZIP/MSI 都有 hash、来源、安装/冒烟状态、截图、发布目标 | [`scripts/New-ArtifactEvidenceManifest.ps1`](./scripts/New-ArtifactEvidenceManifest.ps1) |
| Encoding Gate | 发布前拦住中文乱码、复制损坏、mojibake | [`scripts/Test-EncodingGate.ps1`](./scripts/Test-EncodingGate.ps1) |
| Windows Silky Preflight | README、视觉证明、发布产物证据、编码检查合并成一个 gate | [`scripts/Invoke-WindowsSilkyPreflight.ps1`](./scripts/Invoke-WindowsSilkyPreflight.ps1) |
| AgentWorkOS Assets | 把顺滑经验固化成 Skill、Agent、Rule、Prompt、SOP | [`patches/`](./patches) |

## Windows Patch 10 Factors

完整版本见 [`docs/windows-patch-10-factors.md`](./docs/windows-patch-10-factors.md)。

| Factor | 准则 | 一句话 |
| ---: | --- | --- |
| 1 | Evidence Before Cleanup | 先盘点，再清理 |
| 2 | Idempotent By Default | 重复运行不制造新混乱 |
| 3 | User-Controlled Destruction | 删除、移动、系统修改必须由用户明确确认 |
| 4 | One Canonical Artifact | 每组版本产物只保留一个当前可信件 |
| 5 | Evidence Beside Artifacts | 产物旁边必须有 hash、来源、冒烟、截图、决策 |
| 6 | Entrypoint First | 大项目先有 README 或 PROJECT_CARD |
| 7 | Encoding Is A Gate | 中文乱码是发布阻断项 |
| 8 | Proof Is Part Of Release | 截图、日志、release note 不是善后，是发布定义 |
| 9 | Agent-Readable Assets | 脚本、清单、SOP、Rule、Skill 都要可被 Agent 复用 |
| 10 | Repetition Becomes Infrastructure | 重复两次写规则，三次做 Skill，阻塞发布就做门禁 |

## 提炼依据

本仓库从 `13-summarize-method-ablation` 的经验资产中只提取 Windows 顺滑相关内容，并只发布蒸馏后的证据：

| Evidence | Signal |
| --- | --- |
| Workspace scan | 24 个项目、68,310 个文件、100 个发布产物、293 个重复产物组、96 个无证据发布产物、44 个潜在编码问题 |
| Windows artifact clues | `RepoAtlas-v*-windows-x64.exe` 多版本重复、`.WebView2` 目录残留、`ChatGPT Installer` 多份重复、`gitmarket-windows_x86_64` 压缩包/EXE 分散 |
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
└─ docs/           # 证据地图、10 Factors、升级路线、AgentWorkOS 提炼
```

## 可以继续升级什么

详细路线见 [`docs/upgrade-roadmap.md`](./docs/upgrade-roadmap.md)。

| Priority | Upgrade | 价值 |
| --- | --- | --- |
| P0 | Archive planner | 从“发现重复文件”升级到“生成安全 dry-run 归档计划” |
| P0 | JSON Schema | 让 evidence manifest 可验证、可被 CI 拦截 |
| P0 | Test fixtures | 用假工作区测试脚本，不扫真实私有目录 |
| P1 | HTML report | 本地用卡片/筛选看报告，比长 Markdown 更快 |
| P1 | PowerShell module | 从复制脚本升级到 `Import-Module` |
| P1 | GitHub Action | 每次 release tag / PR 自动跑 preflight |
| P1 | Codex hook candidate | 发布类任务自动提醒 Agent 跑 Windows silky gate |
| P2 | WebView2 classifier | 自动判断 `.WebView2` 残留是否属于当前发布 |
| P2 | README proof generator integration | 截图/缩略图和 README 证明进入发布流程 |
| P2 | Local dashboard | 横跨多个项目查看 artifact trust state |

## 不做什么

- 不自动删除文件。
- 不默认修改注册表、系统策略、电源计划、启动项或 Defender 设置。
- 不提交私有路径、token、cookie、原始 session log。
- 不把“建议”当成完成状态，必须落到脚本、清单、规则、Skill 或 SOP。

## 适合谁

- Windows 上做很多本地 AI、移动端、桌面工具实验的人。
- 经常堆出多个 `*-windows-x64.exe`、`.zip`、`.WebView2`、`preview.html` 的人。
- 每次发布都要重新补 README、截图、release note、checksum 的人。
- 想把 Codex/AgentWorkOS 经验沉淀成可复用补丁的人。

## License

MIT
