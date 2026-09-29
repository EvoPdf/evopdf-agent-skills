<p align="center">
  <a href="https://www.evopdf.com/evopdf-next-dotnet"><img src="https://raw.githubusercontent.com/EvoPdf/evopdf-files/main/next/evopdf-next-pdf-library-logo.png" alt="EvoPdf Next" height="72"></a>
</p>

<h1 align="center">EvoPdf Next Agent Skills</h1>

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
| [`.claude-plugin/`](.claude-plugin) | Claude Code plugin manifest; install the skills with one command |
| [`.github/copilot-instructions.md`](.github/copilot-instructions.md) | GitHub Copilot (Chat and coding agent) |
| [`.cursor/rules/`](.cursor/rules), [`.windsurf/rules/`](.windsurf/rules) | Cursor and Windsurf rule files |
| [`llms.txt`](llms.txt), [`llms-full.txt`](llms-full.txt) | Index and full text for any LLM tool that reads llms.txt |

### The skills

| Skill | Use it for |
|---|---|
| **evopdf-next-html-to-pdf** | Convert URLs and HTML strings to PDF in .NET with EvoPdf Next HtmlToPdfConverter |
| **evopdf-next-page-setup** | Choose the PDF page size and how the HTML is laid out and scaled on it with the EvoPdf Next layout methods |
| **evopdf-next-rendering-modes** | Control the Chromium rendering process of EvoPdf Next |
| **evopdf-next-html-loading** | Control how EvoPdf Next fetches the HTML |
| **evopdf-next-untrusted-html** | Harden EvoPdf Next when the HTML comes from users |
| **evopdf-next-headers-footers** | Add headers, footers and HTML stamps to PDF with EvoPdf Next |
| **evopdf-next-bookmarks-and-toc** | Generate a document outline and an automatic table of contents from HTML headings with EvoPdf Next |
| **evopdf-next-html-element-mapping** | Convert or exclude parts of an HTML page with CSS selectors in EvoPdf Next and read back where each element landed in the PDF |
| **evopdf-next-html-to-image** | Render a URL or HTML string to PNG, JPEG or WebP in .NET with EvoPdf Next HtmlToImageConverter |
| **evopdf-next-document-converters** | Convert DOCX, XLSX, RTF and Markdown documents to PDF in .NET with EvoPdf Next |
| **evopdf-next-pdf-create** | Build PDF documents from scratch in .NET with the EvoPdf Next Core PDF API |
| **evopdf-next-pdf-edit** | Open and modify an existing PDF in .NET with EvoPdf Next PdfEditor |
| **evopdf-next-pdf-inspect** | Read the properties of an existing PDF with EvoPdf Next before processing it |
| **evopdf-next-pdf-merge** | Combine PDF documents in .NET with EvoPdf Next |
| **evopdf-next-pdf-annotations** | Add clickable link annotations and sticky-note text annotations to generated or existing PDFs with EvoPdf Next |
| **evopdf-next-pdf-attachments** | Attach files to a PDF with EvoPdf Next |
| **evopdf-next-pdf-forms** | Turn HTML form controls into interactive PDF form fields with EvoPdf Next PdfFormOptions |
| **evopdf-next-fonts** | Work with fonts in EvoPdf Next PDF documents |
| **evopdf-next-pdf-standards** | Produce standards-compliant PDF with EvoPdf Next |
| **evopdf-next-security-signatures** | Protect and sign PDF documents with EvoPdf Next |
| **evopdf-next-pdf-metadata** | Set the PDF document description and how viewers open the file with EvoPdf Next |
| **evopdf-next-pdf-processor** | Extract content from existing PDF documents with the EvoPdf Next PDF Processor |
| **evopdf-licensing** | Apply an EvoPdf Next license key in .NET, understand demo mode and the watermark, and know where the key goes in web applications and services. |
| **evopdf-next-deployment** | Install and deploy EvoPdf Next |
| **evopdf-next-docker** | Run EvoPdf Next in Docker containers on Linux and Windows |
| **evopdf-next-azure** | Run EvoPdf Next on Azure App Service and Azure Functions, on both Linux and Windows plans |
| **evopdf-next-troubleshooting** | Diagnose EvoPdf Next failures |
| **evopdf-classic-to-next-migration** | Migrate .NET applications from EvoPdf Classic (EvoPdf namespace) to EvoPdf Next |
| **evopdf-next-wkhtmltopdf-migration** | Replace wkhtmltopdf, DinkToPdf, Rotativa, TuesPechkin or Pechkin with EvoPdf Next in .NET |

Every skill is written against the current API reference and states which package, namespace and members to use. None contain license keys.

## Installation

### Claude Code
```bash
# from the plugin marketplace in this repository
/plugin marketplace add EvoPdf/evopdf-agent-skills
/plugin install evopdf-next@evopdf
```
Manual alternative; copy the skills into your personal or project skills folder:
```bash
git clone https://github.com/EvoPdf/evopdf-agent-skills
cp -r evopdf-agent-skills/skills/* ~/.claude/skills/      # personal
# or: cp -r evopdf-agent-skills/skills/* .claude/skills/  # per project
```
Claude Code also reads `CLAUDE.md` / `AGENTS.md` when they sit in your project root.

### Claude.ai and Claude Cowork
Upload a skill folder (`skills/<name>/`) in **Settings > Skills**, or attach `llms-full.txt` to a Project as knowledge.

### GitHub Copilot
Copy `.github/copilot-instructions.md` into your repository. Copilot Chat, code review and the coding agent read it automatically.

### Cursor
Copy `.cursor/rules/evopdf-next.mdc` into your project's `.cursor/rules/`. The rule activates on `*.cs` and `*.csproj` files.

### OpenAI Codex, Gemini CLI, Copilot coding agent, Aider, Windsurf, others
Place `AGENTS.md` in your repository root (Windsurf: `.windsurf/rules/evopdf-next.md`). Tools that follow the AGENTS.md convention pick it up without configuration.

### ChatGPT, Gemini, other chats
Paste `llms-full.txt` (or a single `SKILL.md`) as the first message or as project/custom instructions. It is plain Markdown, about 21,000 words; for a single task one `SKILL.md` is enough.

## Quick check
Ask your assistant: *"Convert this HTML string to an A4 PDF with EvoPdf Next and add a footer with page numbers."* A correct answer uses `EvoPdf.Next`, `Licensing.LicenseKey`, the A4 layout of a new converter or `FitBrowserWindowToPage(PdfPageSize.A4)` and `PdfHtmlFooter.Html` with `{page_number}` / `{total_pages}`.

## Related
- [EvoPdf Next documentation](https://www.evopdf.com/help/evopdf-next-dotnet/), [All components](https://www.evopdf.com/evopdf-next-dotnet), [NuGet packages](https://www.nuget.org/profiles/EvoPdf)
- [Classic to Next migration guide](https://www.evopdf.com/evopdf-classic-to-next-migration)
- [wkhtmltopdf to EvoPdf Next migration guide](https://www.evopdf.com/wkhtmltopdf-alternative-dotnet)
- Runnable samples: [evopdf-next-samples](https://github.com/EvoPdf/evopdf-next-samples): quickstarts, every documentation sample, the full demo application

## Contributing and support
Issues and pull requests are welcome for corrections and new scenarios; see [CONTRIBUTING.md](CONTRIBUTING.md). Product support: https://www.evopdf.com/support

## License
The content of this repository is MIT licensed. EvoPdf Next itself is commercial software with a free, time-unlimited evaluation; see https://www.evopdf.com/buy.
