<p align="center">
  <a href="https://www.evopdf.com/evopdf-next-dotnet"><img src="https://raw.githubusercontent.com/EvoPdf/evopdf-files/main/next/evopdf-next-pdf-library-logo.png" alt="EvoPdf Next" height="72"></a>
</p>

<h1 align="center">EvoPdf Next — Agent Skills</h1>

<p align="center">
  Teach AI coding assistants to write correct <b>EvoPdf Next</b> code and to migrate <b>EvoPdf Classic</b> applications.<br>
  Works with Claude Code, GitHub Copilot, Cursor, Codex, Gemini CLI, Windsurf and any chat that accepts instructions.
</p>

<p align="center">
  <a href="https://www.nuget.org/packages/EvoPdf.Next"><img src="https://img.shields.io/nuget/v/EvoPdf.Next?label=EvoPdf.Next&logo=nuget" alt="NuGet"></a>
  <a href="https://www.evopdf.com/help/evopdf-next-dotnet/"><img src="https://img.shields.io/badge/docs-evopdf.com-1E6FB8" alt="Documentation"></a>
  <img src="https://img.shields.io/badge/platforms-Windows%20%7C%20Linux%20%7C%20macOS-555" alt="Platforms">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="MIT"></a>
</p>

---

## What is in this repository

| Path | Purpose |
|---|---|
| [`AGENTS.md`](AGENTS.md) | Universal instructions read by Codex, Gemini CLI, Copilot coding agent, Cursor and others |
| [`CLAUDE.md`](CLAUDE.md) | Entry point for Claude Code (points to `AGENTS.md`) |
| [`skills/`](skills) | Sixteen **Agent Skills** (`SKILL.md` + `references/`), one per task area |
| [`.claude-plugin/`](.claude-plugin) | Claude Code plugin manifest — install the skills with one command |
| [`.github/copilot-instructions.md`](.github/copilot-instructions.md) | GitHub Copilot (Chat and coding agent) |
| [`.cursor/rules/`](.cursor/rules) · [`.windsurf/rules/`](.windsurf/rules) | Cursor and Windsurf rule files |
| [`llms.txt`](llms.txt) · [`llms-full.txt`](llms-full.txt) | Index and full text for any LLM tool that reads llms.txt |

### The skills

| Skill | Covers |
|---|---|
| **evopdf-next-html-to-pdf** | URL / HTML string to PDF, page size, dynamic pages, authentication, forms, links |
| **evopdf-next-headers-footers** | HTML headers and footers, page numbers, margins, browser templates |
| **evopdf-next-pdf-standards** | PDF/UA and PDF/A output, accessibility options |
| **evopdf-next-document-converters** | Word, Excel, RTF and Markdown to PDF |
| **evopdf-next-core-pdf-api** | Create and edit PDFs with the Core API: `PdfDocument`, `PdfEditor`, text, images, shapes, templates, attachments |
| **evopdf-next-pdf-processor** | PDF to text (layout or reading order), text search with positions, pages to PNG, embedded image extraction — with a full overload reference |
| **evopdf-next-deployment** | Packages per platform, Azure, license key |
| **evopdf-next-linux** | Linux setup: system packages, execute permissions, troubleshooting in the right order |
| **evopdf-next-docker** | Complete Dockerfiles for Linux (x64/ARM64) and Windows Server Core containers |
| **evopdf-classic-to-next-migration** | Moving EvoPdf Classic code to EvoPdf Next, with a full option map |
| **evopdf-next-troubleshooting** | Symptom → cause → fix for HTML to PDF problems (timeouts, missing CSS, page size, fonts, memory, auth) |
| **evopdf-next-azure** | App Service and Functions on Windows and Linux: plans, startup command, `ConfigureRuntime` |
| **evopdf-next-pdf-features** | Fillable forms, bookmarks, table of contents, links, stamps, merge, `PdfEditor`, security |
| **evopdf-licensing** | Pre-sales answers: Deployment vs Company, prices, renewals, refunds |
| **evopdf-next-html-to-image** | HTML and URLs to PNG/JPEG: screenshots, full-page captures, thumbnails, element selection |
| **evopdf-next-security-signatures** | Passwords, permissions, encryption, digital signatures with PFX and timestamps, metadata |

Every skill is written against the current API reference and states which package, namespace and members to use. None contain license keys.

## Installation

### Claude Code
```bash
# from the plugin marketplace in this repository
/plugin marketplace add EvoPdf/evopdf-agent-skills
/plugin install evopdf-next@evopdf
```
Manual alternative — copy the skills into your personal or project skills folder:
```bash
git clone https://github.com/EvoPdf/evopdf-agent-skills
cp -r evopdf-agent-skills/skills/* ~/.claude/skills/      # personal
# or: cp -r evopdf-agent-skills/skills/* .claude/skills/  # per project
```
Claude Code also reads `CLAUDE.md` / `AGENTS.md` when they sit in your project root.

### Claude.ai and Claude Cowork
Upload a skill folder (`skills/<name>/`) in **Settings → Skills**, or attach `llms-full.txt` to a Project as knowledge.

### GitHub Copilot
Copy `.github/copilot-instructions.md` into your repository. Copilot Chat, code review and the coding agent read it automatically.

### Cursor
Copy `.cursor/rules/evopdf-next.mdc` into your project's `.cursor/rules/`. The rule activates on `*.cs` and `*.csproj` files.

### OpenAI Codex, Gemini CLI, Copilot coding agent, Aider, Windsurf, others
Place `AGENTS.md` in your repository root (Windsurf: `.windsurf/rules/evopdf-next.md`). Tools that follow the AGENTS.md convention pick it up without configuration.

### ChatGPT, Gemini, other chats
Paste `llms-full.txt` (or a single `SKILL.md`) as the first message or as project/custom instructions. It is plain Markdown, about 4,000 words.

## Quick check
Ask your assistant: *"Convert this HTML string to an A4 PDF with EvoPdf Next and add a footer with page numbers."* A correct answer uses `EvoPdf.Next`, `Licensing.LicenseKey`, `AutoResizePdfPageWidth = false`, and `PdfHtmlFooter.Html` with `{page_number}` / `{total_pages}`.

## Related
- [EvoPdf Next documentation](https://www.evopdf.com/help/evopdf-next-dotnet/) · [All components](https://www.evopdf.com/evopdf-next-dotnet) · [NuGet packages](https://www.nuget.org/profiles/EvoPdf)
- [Classic to Next migration guide](https://www.evopdf.com/evopdf-classic-to-next-migration)
- Runnable samples: [evopdf-next-samples](https://github.com/EvoPdf/evopdf-next-samples) — quickstarts, every documentation sample, the full demo application

## Contributing and support
Issues and pull requests are welcome for corrections and new scenarios — see [CONTRIBUTING.md](CONTRIBUTING.md). Product support: https://www.evopdf.com/support

## License
The content of this repository is MIT licensed. EvoPdf Next itself is commercial software with a free, time-unlimited evaluation — see https://www.evopdf.com/buy.
