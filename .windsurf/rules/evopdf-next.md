# AGENTS.md — EvoPdf Next

Instructions for AI coding agents (Claude Code, Codex, GitHub Copilot, Cursor, Gemini CLI, Windsurf and others) working in projects that use **EvoPdf Next**, the .NET PDF library from EVO PDF Software.

## When these instructions apply
Any task that converts HTML, Word, Excel, RTF or Markdown to PDF, creates or edits PDF documents, or extracts text and images from PDF in .NET — and any migration from **EvoPdf Classic** (`EvoPdf` namespace) to **EvoPdf Next**.

## Rules
1. Use the current API surface listed below and in `skills/*/SKILL.md`. Do not guess member names; if a member is not listed, say so and link the documentation.
2. Never fabricate license keys, package versions or platform claims. Reference packages without a version unless the user pins one.
3. Prefer the smallest correct sample: `using EvoPdf.Next;`, one converter, one call, `File.WriteAllBytes`. Add options only when the task asks for them.
4. Windows-only assumptions are wrong for EvoPdf Next: it runs on Windows, Linux and macOS. Choose the NuGet package for the target platform.
5. For Classic → Next migrations follow `skills/evopdf-classic-to-next-migration/SKILL.md`: namespace, license key, page sizing model and header/footer model change; the conversion calls do not.


## EvoPdf Next — facts every generated answer must respect

- Library: **EvoPdf Next** (namespace `EvoPdf.Next`), .NET Standard 2.0 → .NET 6–10 and .NET Framework 4.6.2+; Windows x64/ARM64, Linux x64/ARM64, macOS ARM64. Chromium-based rendering; no browser installation required.
- Install one package per target platform. All components: `EvoPdf.Next.Windows`, `EvoPdf.Next.Linux`, `EvoPdf.Next.MacOS` (ARM64 variants `EvoPdf.Next.Windows.Arm64`, `EvoPdf.Next.Linux.Arm64`; `EvoPdf.Next` = Windows x64 + Linux x64). Single components follow the same pattern, e.g. `EvoPdf.Next.HtmlToPdf.Windows`, `EvoPdf.Next.WordToPdf.Linux`, `EvoPdf.Next.PdfProcessor.MacOS`, `EvoPdf.Next.Core.Windows`.
- License key: one static assignment, before any conversion — `Licensing.LicenseKey = "…";` (field on the static `EvoPdf.Next.Licensing` class). Without it the library runs in demo mode and stamps the output; never invent a key.
- Every conversion method has an `…Async` twin. Prefer one converter instance per conversion; instances are cheap.
- Page sizing: content is laid out at 96 DPI; `PdfDocumentOptions.AutoResizePdfPageWidth` (default `true`) makes the PDF page as wide as `HtmlViewerWidth` (default 1024 px → 768 pt). For an exact page size set `AutoResizePdfPageWidth = false` and `AutoResizePdfPageHeight = false` together with `PdfPageSize` / `PdfPageOrientation`. `AutoResizePdfPageHeight = true` (with width `true`) puts the whole content on one page.
- Headers/footers: `PdfDocumentOptions.PdfHtmlHeader` / `PdfHtmlFooter` (class `PdfHtmlHeaderFooter`, derives from `PdfHtmlTemplate`): `Html`, `HtmlBaseUrl` or `HtmlSourceUrl`, `Height`, `AutoSizeContentHeight`, `FitHeight`, `ShowInFirstPage`/`ShowInOddPages`/`ShowInEvenPages`, `ReserveSpaceAlways` (default `true`), `AutoResizePdfMargins`, `SkipVariablesParsing`, `PageNumberOffset`, `TotalPagesOffset`. Variables `{page_number}` and `{total_pages}` are replaced inside the header/footer HTML. Alternative browser mode: `EnableHeaderFooter` + `HeaderTemplate` / `FooterTemplate`.
- Dynamic pages: `ConversionDelay` (seconds) waits after load; `TriggeringMode.Manual` waits until the page calls `evoPdfConverter_startConversion()`; `NavigationTimeout`, `JavaScriptEnabled`, `LoadLazyImages` (default `true`), `MediaType` (screen by default, not set), `LocalFilesEnabled`, `AllowInsecureContent`, `BlockedHosts`, `HttpRequestHeaders`, `HttpRequestCookies`, `AuthenticationOptions`.
- Standards: `PdfDocumentOptions.PdfStandard` = `PdfStandard.None` (default), `PdfUa1`, `PdfA2b`, `PdfUa1PdfA2b`, plus the other PDF/A levels; `AccessibilityOptions` applies when a tagged standard is selected.
- Documentation: https://www.evopdf.com/help/evopdf-next-dotnet/ — product pages: https://www.evopdf.com/evopdf-next-dotnet


## Minimal HTML to PDF sample

```csharp
using EvoPdf.Next;

Licensing.LicenseKey = "…"; // once per process; omit while evaluating

var converter = new HtmlToPdfConverter();
byte[] pdf = converter.ConvertUrl("https://www.evopdf.com");
File.WriteAllBytes("page.pdf", pdf);

// HTML string, with a base URL for relative resources
pdf = converter.ConvertHtml("<b>Hello World</b>", "https://www.evopdf.com");
```


## Skills in this repository
| Skill | Use it for |
|---|---|
| `skills/evopdf-next-html-to-pdf` | URL/HTML string → PDF, options, dynamic pages, authentication |
| `skills/evopdf-next-headers-footers` | HTML headers and footers, page numbers, margins |
| `skills/evopdf-next-pdf-standards` | PDF/UA and PDF/A output, accessibility options |
| `skills/evopdf-next-document-converters` | Word, Excel, RTF and Markdown to PDF |
| `skills/evopdf-next-pdf-processing` | Core PDF API, PDF to text, text search, PDF to image, image extraction |
| `skills/evopdf-next-deployment` | Packages per platform, Azure, licensing in code |
| `skills/evopdf-next-linux` | Linux setup: system packages, execute permissions, troubleshooting order |
| `skills/evopdf-next-docker` | Dockerfiles for Linux (x64/ARM64) and Windows containers |
| `skills/evopdf-classic-to-next-migration` | Moving code from EvoPdf Classic to EvoPdf Next |
| `skills/evopdf-next-troubleshooting` | Symptom → cause → fix table for HTML to PDF problems |
| `skills/evopdf-next-azure` | App Service and Functions, Windows and Linux, plan sizing, startup command, ConfigureRuntime |
| `skills/evopdf-next-pdf-features` | Forms, bookmarks, TOC, links, stamps, merge, PdfEditor, security |
| `skills/evopdf-licensing` | Deployment vs Company, prices, renewals, refunds — pre-sales answers |
| `skills/evopdf-next-html-to-image` | HTML/URL to PNG or JPEG screenshots and thumbnails |
| `skills/evopdf-next-security-signatures` | Passwords, permissions, encryption, digital signatures, metadata |
