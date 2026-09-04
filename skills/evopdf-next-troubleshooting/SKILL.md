---
name: evopdf-next-troubleshooting
description: "Diagnose and fix EvoPdf Next HTML to PDF problems in .NET: watermark still shown, navigation timeout, CSS or images missing when converting a string, async content not rendered, content truncated or too small, unexpected margins, page breaks, fonts and Unicode, out of memory, HTTPS and authentication. Use when a user reports EvoPdf output or an exception that looks wrong."
---

# EvoPdf Next — troubleshooting HTML to PDF

Ask for: the call used (`ConvertUrl` / `ConvertHtml`), the platform, whether the page renders correctly in Chrome, and the exception text. Then match the symptom below. All members are on `HtmlToPdfConverter` unless stated; `o` = `converter.PdfDocumentOptions`.

| Symptom | Cause | Fix |
|---|---|---|
| Demo watermark although a license was bought | key not set, set after the first conversion, or a demo key left in code | `Licensing.LicenseKey = "…"` once at startup (static); search the code for other assignments |
| "Navigation timeout" / page never finishes | slow page, blocked resource, infinite script | raise `NavigationTimeout` (seconds); check the URL loads in a browser from the server; block third-party hosts with `BlockedHosts` |
| Converting an HTML **string**: CSS and images missing | relative URLs have no base | pass the base URL: `ConvertHtml(html, "https://www.example.com/")`; for local files set `LocalFilesEnabled = true` and use `file:///` or the folder as base |
| AJAX / charts / lazy content missing | conversion started before scripts finished | `ConversionDelay = 2` (seconds), or `TriggeringMode = TriggeringMode.Manual` and call `evoPdfConverter_startConversion()` from the page when ready; keep `JavaScriptEnabled = true`; `LoadLazyImages` is `true` by default |
| Content smaller than in the browser / page much wider than A4 | page width follows `HtmlViewerWidth` (default 1024 px → 768 pt) | for a fixed size: `o.AutoResizePdfPageWidth = false; o.AutoResizePdfPageHeight = false; o.PdfPageSize = PdfPageSize.A4`; make the HTML responsive or lower `HtmlViewerWidth` |
| Content truncated at the right | HTML wider than the viewer and page width fixed | raise `HtmlViewerWidth` to the page's natural width, or keep `AutoResizePdfPageWidth = true`, or let the CSS adapt to the viewer width |
| Extra margin at top/left | page margins plus the HTML body margin | set `o.TopMargin`/`o.LeftMargin` (points) and `body { margin: 0 }` in the HTML |
| Wrong page breaks; images cut between pages | no break rules in the HTML | use CSS: `break-before/after: page`, `break-inside: avoid` (or `page-break-*`) on the elements to keep together; `o.RepeatTableHeaderFooter = true` repeats `<thead>`/`<tfoot>` |
| Only screen styles, `@media print` ignored | media type defaults to screen | `MediaType = "print"` |
| Fonts replaced / Unicode boxes | font not installed on the server (Linux especially) | install the fonts on the server or embed web fonts (`@font-face`, WOFF2) in the HTML; the converter embeds used fonts as subsets automatically |
| Right-to-left, Arabic, Indic, Thai | — | supported by the Chromium engine; use `dir="rtl"` / correct `lang` and fonts that contain the glyphs |
| Out of memory / very slow | huge page, many high-resolution images, unbounded height | reduce image sizes, split the document, set `MaxHtmlViewerHeight`, convert in a 64-bit process with enough RAM; one converter per conversion |
| HTTPS page fails with a certificate error | self-signed or internal CA | `IgnoreCertificateErrors = true` (development) or install the CA on the server |
| Page behind a login | no credentials | `AuthenticationOptions` (basic/NTLM), or forward the session: `HttpRequestCookies.Add(...)`, `HttpRequestHeaders.Add("Authorization", ...)` — the converter does **not** share the browser's session automatically |
| Mixed content blocked | HTTPS page loading HTTP resources | `AllowInsecureContent = true` |
| Links in the PDF undesired | — | remove or neutralise the anchors in CSS/HTML before conversion (links come from the HTML) |
| Frameset / iframe content missing | frames are separate documents | convert the frame's URL directly, or include the content inline |
| Landscape / custom size | — | `o.PdfPageOrientation = PdfPageOrientation.Landscape`; custom: `o.PdfPageSize = new PdfPageSize(width, height)` in points, with `AutoResizePdfPageWidth = false` |
| Append other PDFs to the result | — | open the result in `PdfEditor` and `AddPdfTemplate` the others, or merge with the Core API (see the pdf-features skill) |
| "converter instances are not reusable" exception | a second conversion on the same converter object | create a new converter for every conversion (also in loops and cached services) |
| Linux: works on Windows, fails on Linux | missing system packages or execute permissions | follow the `evopdf-next-linux` skill, in that order |

Always test the page in Chrome first: EvoPdf Next renders what Chrome renders. If Chrome shows the problem too, fix the HTML/CSS.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/ · Support Q&A: https://www.evopdf.com/support
