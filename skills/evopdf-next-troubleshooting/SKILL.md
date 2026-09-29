---
name: evopdf-next-troubleshooting
description: "Diagnose EvoPdf Next failures: startup and native runtime errors, permission problems, blank or incomplete output, timeouts, missing fonts and images, and the reusable-instance exception."
---

# EvoPdf Next: troubleshooting

This troubleshooting guide for EvoPdf Next for .NET contains the necessary information to allow you identify and solve the possible issues that you might encounter during the usage of our software. If you cannot find the solution to your issue in this guide then you can contact our support at any time to help you solve the problem.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

Ask for: the call used (`ConvertUrl` / `ConvertHtml`), the platform, whether the page renders correctly in Chrome, and the exception text. Then match the symptom below. All members are on `HtmlToPdfConverter` unless stated; `o` = `converter.PdfDocumentOptions`.

| Symptom | Cause | Fix |
|---|---|---|
| Demo watermark although a license was bought | key not set in the process that converts, or the evaluation key of the samples left in the code | `Licensing.LicenseKey = "..."` once at startup, before the first conversion; search the code for other assignments |
| "Navigation timeout" / page never finishes | slow page, blocked resource, infinite script | raise `NavigationTimeout` (seconds); check the URL loads in a browser from the server; block third-party hosts with `BlockedHosts` |
| Converting an HTML **string**: CSS and images missing | relative URLs have no base | pass the base URL: `ConvertHtml(html, "https://www.example.com/")`; local files load by default (`LocalFilesEnabled`); use `file:///` URLs or the folder as base |
| AJAX / charts / lazy content missing | conversion started before scripts finished | `ConversionDelay = 2` (seconds), or `TriggeringMode = TriggeringMode.Manual` and call `evoPdfConverter_startConversion()` from the page when ready; keep `JavaScriptEnabled = true`; `LoadLazyImages` is `true` by default |
| Content smaller than in the browser | a new converter lays the page out in a 1024 pixel browser window and draws it at 77.47 percent on A4 (`FitBrowserWindowToPage`) | to print at the width of the page without scaling: `converter.LayoutAtPageWidth(PdfPageSize.A4)`; to keep the page at the window size: `converter.PageWidthFromBrowserWindow()` |
| Everything smaller when the page has a wide table or image | the browser window grows to the width of the content and the whole layout is scaled to the page (`AutoResizeHtmlViewerWidth`, true by default); `ConversionInfo.PrintZoom` gives the zoom drawn | put the wide content on a wider page: `converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Landscape)`, or on a page as wide as the content: `converter.PageWidthFromBrowserWindow(1400)` |
| Extra margin at top/left | page margins plus the HTML body margin | set `o.TopMargin`/`o.LeftMargin` (points) and `body { margin: 0 }` in the HTML |
| Wrong page breaks; images cut between pages | no break rules in the HTML | use CSS: `break-before/after: page`, `break-inside: avoid` (or `page-break-*`) on the elements to keep together; `o.RepeatTableHeaderFooter = true` repeats `<thead>`/`<tfoot>` |
| Only screen styles, `@media print` ignored | media type defaults to screen | `MediaType = "print"` |
| Fonts replaced / Unicode boxes | the font is not installed on the server | install the fonts on the server or embed web fonts (`@font-face`, WOFF2) in the HTML; the converter embeds used fonts as subsets automatically |
| Right-to-left, Arabic, Indic, Thai | - | supported by the Chromium engine; use `dir="rtl"` / correct `lang` and fonts that contain the glyphs |
| HTTPS page fails with a certificate error | self-signed or internal CA | `IgnoreCertificateErrors = true` (development) or install the CA on the server |
| Page behind a login | no credentials | `AuthenticationOptions` with the user name and password, or forward the session: `HttpRequestCookies.Add(...)`, `HttpRequestHeaders.Add("Authorization", ...)`; the converter does **not** share the browser's session automatically |
| Mixed content blocked | HTTPS page loading HTTP resources | `AllowInsecureContent = true` |
| Links in the PDF undesired | - | remove or neutralise the anchors in CSS/HTML before conversion (links come from the HTML) |
| Landscape / custom size | - | `converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Landscape)`; custom: `new PdfPageSize(width, height)` in points, passed to the layout method |
| Append other PDFs to the result | - | `o.AddStartPdf(pdfBytes)` / `o.AddEndPdf(pdfBytes)` before the conversion, or `PdfMerge` afterwards (see the `evopdf-next-pdf-merge` skill) |
| "converter instances are not reusable" exception | a second conversion on the same converter object | create a new converter for every conversion (also in loops and cached services) |
| Linux: works on Windows, fails on Linux | missing system packages or execute permissions | follow the Linux section of the `evopdf-next-deployment` skill, in that order |

Always test the page in Chrome first: EvoPdf Next renders what Chrome renders. If Chrome shows the problem too, fix the HTML/CSS.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/, Support Q&A: https://www.evopdf.com/support

- Troubleshooting: https://www.evopdf.com/help/evopdf-next-dotnet/html/troubleshooting.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
