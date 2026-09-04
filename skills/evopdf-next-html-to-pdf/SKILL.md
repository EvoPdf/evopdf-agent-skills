---
name: evopdf-next-html-to-pdf
description: "Convert HTML pages, URLs and HTML strings to PDF in .NET with EvoPdf Next (HtmlToPdfConverter): options, page size, dynamic pages, authentication, forms, links, images. Use for any C#/.NET HTML to PDF task."
---

# EvoPdf Next — HTML to PDF

Package: `EvoPdf.Next.HtmlToPdf.Windows` / `.Linux` / `.MacOS` (or the all-components `EvoPdf.Next.Windows` / `.Linux` / `.MacOS`). Namespace `EvoPdf.Next`. Class `HtmlToPdfConverter`.

## Minimal

```csharp
using EvoPdf.Next;

Licensing.LicenseKey = "…"; // once per process; omit while evaluating

var converter = new HtmlToPdfConverter();
byte[] pdf = converter.ConvertUrl("https://www.evopdf.com");
File.WriteAllBytes("page.pdf", pdf);

// HTML string, with a base URL for relative resources (new instance: converters are single-use)
converter = new HtmlToPdfConverter();
pdf = converter.ConvertHtml("<b>Hello World</b>", "https://www.evopdf.com");
```


## Methods
- `byte[] ConvertUrl(string url)` · `ConvertUrlToFile(url, path)` · `ConvertHtml(string html, string baseUrl)` · `ConvertHtmlToFile(html, baseUrl, path)` — each with an `…Async` variant.
- `HtmlElementsInfo` (after `HtmlElementsInfoSelector`) returns the PDF position of HTML elements; `ConvertedElementsSelector` / `ExcludedElementsSelector` convert or skip parts of the page.

## Page size and margins
```csharp
var o = converter.PdfDocumentOptions;
o.PdfPageSize = PdfPageSize.A4;
o.PdfPageOrientation = PdfPageOrientation.Portrait;
o.AutoResizePdfPageWidth = false;   // otherwise the page width follows HtmlViewerWidth (1024 px = 768 pt at 96 DPI)
o.AutoResizePdfPageHeight = false;  // true (with width true) = whole content on one page
o.LeftMargin = o.RightMargin = o.TopMargin = o.BottomMargin = 36; // points
o.PrintBackgrounds = true;
o.PreferCssPageSize = true;         // honour @page in the HTML
converter.HtmlViewerWidth = 1024;   // layout width in pixels
```

## Dynamic pages, timing, loading
```csharp
converter.ConversionDelay = 2;                  // seconds after load
converter.TriggeringMode = TriggeringMode.Manual; // page calls evoPdfConverter_startConversion()
converter.NavigationTimeout = 120;
converter.JavaScriptEnabled = true;
converter.LoadLazyImages = true;                // default
converter.MediaType = "print";                  // default: screen (not set)
converter.ScriptToExecuteAfterLoad = "document.body.classList.add('pdf')";
```

## Pages behind a login, headers, cookies
```csharp
converter.AuthenticationOptions.Username = "user";
converter.AuthenticationOptions.Password = "secret";
converter.HttpRequestHeaders.Add("Authorization", "Bearer …");
converter.HttpRequestCookies.Add("session", "…");
converter.LocalFilesEnabled = true;       // file:// resources
converter.AllowInsecureContent = true;    // mixed content
converter.IgnoreCertificateErrors = true; // self-signed dev servers
```

## Output features (all on `PdfDocumentOptions` unless noted)
- `GenerateDocumentOutline`, `GenerateTableOfContents` / `TableOfContents`, `RepeatTableHeaderFooter`, `GeneratePdfFormFields` / `PdfFormOptions`, `PdfStandard` (see the pdf-standards skill), headers/footers (see the headers-footers skill).
- `converter.PdfSecurityOptions` (passwords, permissions), `converter.DigitalSignature`, `converter.PdfDocumentInfo` (title, author), `converter.PdfViewerPreferences`.

## Pitfalls
- Set `Licensing.LicenseKey` once, before the first conversion; the property is static.
- A page that renders fine in Chrome renders the same way here; if output looks wrong, check `MediaType`, `PrintBackgrounds` and `@page` rules in the CSS.
- **One converter instance per conversion.** A second `Convert…` call on the same `HtmlToPdfConverter` throws "HTML to PDF converter instances are not reusable"; create a new instance every time (they are cheap) and never share one across threads.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/ · Product page: https://www.evopdf.com/evopdf-next-html-to-pdf-dotnet
