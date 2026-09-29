---
name: evopdf-classic-to-next-migration
description: "Migrate .NET applications from EvoPdf Classic (EvoPdf namespace) to EvoPdf Next: packages and namespace, license key, the complete map from the Classic page setup and scaling options to the Next layout methods, the header and footer model, and engine behaviour differences."
---

# EvoPdf Next: migrating from EvoPdf Classic

EvoPdf Classic and EvoPdf Next share the `HtmlToPdfConverter` class name, the conversion methods and most option names, but the two rendering engines place the HTML on the PDF page in different ways and describe headers and footers differently. This topic covers the two areas where an integration moved from Classic changes its output or its code: the page setup with its scaling rules and the headers and footers. The namespace, package and license key changes are covered in the Classic to Next migration guide on the website.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

Read `references/option-map.md` for the full property table before editing option code.

## What stays the same
`ConvertUrl`, `ConvertHtml(html, baseUrl)`, `...ToFile`; the option groups (`PdfDocumentOptions`, `PdfSecurityOptions`, `PdfViewerPreferences`, `HtmlViewerWidth`, `ConversionDelay`, `NavigationTimeout`, `JavaScriptEnabled`, authentication, headers, cookies); the license covers both.

## What changes
1. **Package and namespace**: `EvoPdf.HtmlToPdf` becomes `EvoPdf.Next.HtmlToPdf.<Platform>`; `using EvoPdf;` becomes `using EvoPdf.Next;`. Classic tools had sub-namespaces (`EvoPdf.PdfMerge`, `EvoPdf.PdfToText`, `EvoWordToPdf`, `EvoExcelToPdf`, `EvoPdfClient`); Next has one namespace.
2. **License key**: the instance property `converter.LicenseKey = "..."` becomes the static `Licensing.LicenseKey = "..."`, set once per process.
3. **Page sizing**: Classic `FitWidth` (default true) laid the page out at 1024 pixels and shrank it to the page width. A new Next converter does the same through `FitBrowserWindowToPage(PdfPageSize.A4)`: the page is laid out in a 1024 pixel browser window and drawn at 77.47 percent on A4. `FitWidth = false` with `AutoSizePdfPage = true` becomes `PageWidthFromBrowserWindow()`; `StretchToFit` becomes a zoom above 100; `SinglePage` becomes `PageWidthFromBrowserWindow(1024, singlePage: true)`. `FitHeight`, `ClipHtmlView` and the destination rectangle have no equivalent. The complete map is in the page setup migration section of this skill.
4. **Headers and footers**: Classic composed `PdfHeaderOptions` from `HtmlToPdfElement`, `TextElement` (`&p;`, `&P;`), `LineElement`, `HeaderHeight`, `HeaderBackColor`; Next describes each header/footer as **one HTML template**: `PdfDocumentOptions.PdfHtmlHeader.Html`/`HtmlSourceUrl`, `Height` or `AutoSizeContentHeight`, `{page_number}`/`{total_pages}`, `ShowInFirstPage`/`OddPages`/`EvenPages`, `ReserveSpaceAlways`, `AutoResizePdfMargins`; `PageNumberingStartIndex` becomes `PageNumberOffset`/`TotalPagesOffset`; `PrepareRenderPdfPageEvent` (per-page header on/off) becomes `ShowIn...` flags.
5. **Engine behaviour**: Chromium renders modern CSS/JS exactly as Chrome: flexbox/grid/custom properties, ES2015+, `@page` and `break-*` rules are honoured; fonts and line breaks follow Chrome; lazy images load by default; `LocalFilesEnabled`/`AllowInsecureContent` govern local and mixed content; HTTP/2 and TLS 1.3 sites load directly.

## Before / after
```csharp
// Classic
using EvoPdf;
var c = new HtmlToPdfConverter();
c.LicenseKey = "...";
c.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
c.PdfDocumentOptions.FitWidth = true;
c.PdfFooterOptions.AddElement(new TextElement(0, 5, "Page &p; of &P;", font));
byte[] pdf = c.ConvertUrl(url);

// Next
using EvoPdf.Next;
Licensing.LicenseKey = "...";
var c = new HtmlToPdfConverter();
c.FitBrowserWindowToPage(PdfPageSize.A4);
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

## Migrate HTML to PDF Page Setup and Headers from EvoPdf Classic

### The Classic Default: 1024 Pixel Layout Fitted to an A4 Page

```csharp
using EvoPdf;

HtmlToPdfConverter converter = new HtmlToPdfConverter();
converter.LicenseKey = "...";

converter.HtmlViewerWidth = 1024;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.PdfDocumentOptions.FitWidth = true;

byte[] pdf = converter.ConvertUrl(url);
```

```csharp
using EvoPdf.Next;

Licensing.LicenseKey = "...";
HtmlToPdfConverter converter = new HtmlToPdfConverter();

byte[] pdf = converter.ConvertUrl(url);

// The same settings made explicitly
converter.FitBrowserWindowToPage(PdfPageSize.A4);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.HtmlViewerZoom = 77.47;
converter.HtmlViewerWidth = 1024;
```

### A Page with a Fixed Width Larger Than the Viewer

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// Same as Classic: A4 page, a 1600 pixel window scaled to it, drawn at 49.58 percent
converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 1600);

// Or a 1200 point wide page with the content at 1:1
converter.PageWidthFromBrowserWindow(1600);
```

### AutoSizePdfPage and SinglePage

```csharp
// Classic
converter.PdfDocumentOptions.FitWidth = false;
converter.PdfDocumentOptions.AutoSizePdfPage = true;
converter.PdfDocumentOptions.SinglePage = true;

// Next
converter.PageWidthFromBrowserWindow(1024, singlePage: true);
```

### StretchToFit

```csharp
// Classic
converter.PdfDocumentOptions.FitWidth = true;
converter.PdfDocumentOptions.StretchToFit = true;

// Next: a 600 pixel window enlarged to the A4 page, drawn at 132 percent
converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 600);
```

### A Header with HTML and Page Numbers

```csharp
using EvoPdf;
using System.Drawing;

converter.PdfDocumentOptions.ShowHeader = true;
converter.PdfHeaderOptions.HeaderHeight = 60;
converter.PdfHeaderOptions.HeaderBackColor = Color.White;

HtmlToPdfElement headerHtml = new HtmlToPdfElement(headerHtmlUrl);
headerHtml.FitHeight = true;
converter.PdfHeaderOptions.AddElement(headerHtml);

float headerWidth = converter.PdfDocumentOptions.PdfPageSize.Width -
    converter.PdfDocumentOptions.LeftMargin - converter.PdfDocumentOptions.RightMargin;
LineElement headerLine = new LineElement(0, 59, headerWidth, 59);
headerLine.ForeColor = Color.Gray;
converter.PdfHeaderOptions.AddElement(headerLine);

converter.PdfDocumentOptions.ShowFooter = true;
converter.PdfFooterOptions.FooterHeight = 40;
TextElement footerText = new TextElement(0, 15, "Page &p; of &P;",
    new Font(new FontFamily("Times New Roman"), 10, GraphicsUnit.Point));
footerText.TextAlign = HorizontalTextAlign.Right;
converter.PdfFooterOptions.AddElement(footerText);
```

```csharp
using EvoPdf.Next;

PdfHtmlHeaderFooter header = converter.PdfDocumentOptions.PdfHtmlHeader;
header.HtmlSourceUrl = headerHtmlUrl;
header.Height = 60;
header.FitHeight = true;

PdfHtmlHeaderFooter footer = converter.PdfDocumentOptions.PdfHtmlFooter;
footer.Html = "<div style=\"font-family: 'Times New Roman'; font-size: 10pt; text-align: right\">" +
    "Page {page_number} of {total_pages}</div>";
footer.HtmlBaseUrl = baseUrl;
footer.Height = 40;
```

```html
<!DOCTYPE html>
<html>
<body style="margin: 0; background: white; border-bottom: 1px solid gray; font-family: Arial; font-size: 12pt">
    <div style="padding: 8px">Quarterly report</div>
</body>
</html>
```

All 13 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/migrate-html-to-pdf-from-classic.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
