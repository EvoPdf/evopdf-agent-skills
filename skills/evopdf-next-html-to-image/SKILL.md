---
name: evopdf-next-html-to-image
description: "Convert HTML pages, URLs and HTML strings to PNG or JPEG images (screenshots, thumbnails, full-page captures) in .NET with EvoPdf Next HtmlToImageConverter: image format, viewer size, entire-page capture, element selection. Use for HTML to image, web page screenshot or HTML thumbnail tasks in C#."
---

# EvoPdf Next — HTML to image

Package: `EvoPdf.Next.HtmlToPdf.*` (the HTML to Image converter ships with the HTML to PDF component) or the all-components package. Namespace `EvoPdf.Next`. Class `HtmlToImageConverter` — same loading options as `HtmlToPdfConverter`.

```csharp
using EvoPdf.Next;

var converter = new HtmlToImageConverter();
converter.HtmlViewerWidth = 1280;                // layout width in pixels
converter.CaptureEntirePage = true;             // whole page height, not just the viewer area
byte[] png = converter.ConvertUrl("https://www.evopdf.com", ImageType.Png);   // ImageType: Png, Jpeg, Webp
File.WriteAllBytes("page.png", png);

byte[] jpg = converter.ConvertHtml("<h1>Hello</h1>", "https://www.example.com/", ImageType.Jpeg);
converter.ConvertUrlToFile("https://www.evopdf.com", ImageType.Png, "page.png");   // …Async variants exist
```

## Sizing
- `HtmlViewerWidth` (px) sets the layout width; `HtmlViewerHeight` the captured height when `CaptureEntirePage = false`; `MaxHtmlViewerHeight` caps very long pages; `AutoResizeHtmlViewerHeight` fits the viewer to the content.
- `CaptureEntirePage` / `CaptureEntirePageMode` decide between "visible area" and "full page" screenshots.
- Thumbnails: render at the real width, then resize the returned bytes with your imaging library — the converter produces 1:1 pixels.

## Same loading controls as HTML to PDF
`ConversionDelay`, `TriggeringMode`, `NavigationTimeout`, `JavaScriptEnabled`, `MediaType`, `ScriptToExecuteAfterLoad`, `AuthenticationOptions`, `HttpRequestHeaders`, `HttpRequestCookies`, `HttpPostFields`, `LocalFilesEnabled`, `AllowInsecureContent`, `IgnoreCertificateErrors`, `BlockedHosts`.

## Select what to capture
`ConvertedElementsSelector` (CSS selector: only these elements are rendered), `ExcludedElementsSelector` / `RemoveExcludedElements` (hide or remove elements such as cookie banners before the capture).

## Pitfalls
- Formats: `ImageType.Png`, `ImageType.Jpeg`, `ImageType.Webp`. JPEG has no transparency — use PNG or WebP for transparent backgrounds.
- Fonts and lazy images follow the HTML to PDF rules (see the troubleshooting and Linux skills).
- License key: `Licensing.LicenseKey` once per process; demo mode stamps the image.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/
