---
name: evopdf-classic-to-next-migration
description: "Migrate .NET code from EvoPdf Classic (namespace EvoPdf, PdfConverter/HtmlToPdfConverter, PdfHeaderOptions) to EvoPdf Next: package, namespace, license key, page sizing model, headers/footers, option map. Use whenever legacy EvoPdf code must move to EvoPdf Next."
---

# EvoPdf Classic → EvoPdf Next migration

Read `references/option-map.md` for the full property table before editing option code.

## What stays the same
`ConvertUrl`, `ConvertHtml(html, baseUrl)`, `…ToFile`; the option groups (`PdfDocumentOptions`, `PdfSecurityOptions`, `PdfViewerPreferences`, `HtmlViewerWidth`, `ConversionDelay`, `NavigationTimeout`, `JavaScriptEnabled`, authentication, headers, cookies); the license covers both.

## What changes
1. **Package and namespace** — `EvoPdf.HtmlToPdf` → `EvoPdf.Next.HtmlToPdf.<Platform>`; `using EvoPdf;` → `using EvoPdf.Next;`. Classic tools had sub-namespaces (`EvoPdf.PdfMerge`, `EvoPdf.PdfToText`, `EvoWordToPdf`, `EvoExcelToPdf`, `EvoPdfClient`); Next has one namespace.
2. **License key** — instance property `converter.LicenseKey = "…"` → static `Licensing.LicenseKey = "…"` once per process.
3. **Page sizing** — Classic `FitWidth` (default true) scaled content to the page; Next sizes the page to the content by default (`AutoResizePdfPageWidth = true`, 96 DPI → 1024 px = 768 pt). For a fixed A4: `AutoResizePdfPageWidth = false; AutoResizePdfPageHeight = false; PdfPageSize = PdfPageSize.A4`. `SinglePage` → `AutoResizePdfPageHeight = true` (with width `true`). `AutoSizePdfPage` → default behaviour. `ClipHtmlView` / `StretchToFit` → no equivalent needed.
4. **Headers and footers** — Classic composed `PdfHeaderOptions` from `HtmlToPdfElement`, `TextElement` (`&p;`, `&P;`), `LineElement`, `HeaderHeight`, `HeaderBackColor`; Next describes each header/footer as **one HTML template**: `PdfDocumentOptions.PdfHtmlHeader.Html`/`HtmlSourceUrl`, `Height` or `AutoSizeContentHeight`, `{page_number}`/`{total_pages}`, `ShowInFirstPage`/`OddPages`/`EvenPages`, `ReserveSpaceAlways`, `AutoResizePdfMargins`; `PageNumberingStartIndex` → `PageNumberOffset`/`TotalPagesOffset`; `PrepareRenderPdfPageEvent` (per-page header on/off) → `ShowIn…` flags.
5. **Engine behaviour** — Chromium renders modern CSS/JS exactly as Chrome: flexbox/grid/custom properties, ES2015+, `@page` and `break-*` rules are honoured; fonts and line breaks follow Chrome, so pagination can shift by a line; lazy images load by default; `LocalFilesEnabled`/`AllowInsecureContent` govern local and mixed content; HTTP/2 and TLS 1.3 sites load directly.

## Before / after
```csharp
// Classic
using EvoPdf;
var c = new HtmlToPdfConverter();
c.LicenseKey = "…";
c.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
c.PdfDocumentOptions.FitWidth = true;
c.PdfFooterOptions.AddElement(new TextElement(0, 5, "Page &p; of &P;", font));
byte[] pdf = c.ConvertUrl(url);

// Next
using EvoPdf.Next;
Licensing.LicenseKey = "…";
var c = new HtmlToPdfConverter();
c.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
c.PdfDocumentOptions.AutoResizePdfPageWidth = false;
c.PdfDocumentOptions.AutoResizePdfPageHeight = false;
c.PdfDocumentOptions.PdfHtmlFooter.Html = "<div style='font:9px Arial'>Page {page_number} of {total_pages}</div>";
c.PdfDocumentOptions.PdfHtmlFooter.Height = 30;
byte[] pdf = c.ConvertUrl(url);
```

## Procedure
1. Swap package and `using`; move the license key to `Licensing.LicenseKey` at startup.
2. Build; fix the members flagged in `option-map.md`.
3. Convert two or three representative documents; settle page sizing first, then port headers/footers.
4. Only then touch CSS: drop workarounds written for the old engine.

Reference: https://www.evopdf.com/evopdf-classic-to-next-migration
