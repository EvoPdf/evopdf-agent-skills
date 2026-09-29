---
name: evopdf-next-wkhtmltopdf-migration
description: "Replace wkhtmltopdf, DinkToPdf, Rotativa, TuesPechkin or Pechkin with EvoPdf Next in .NET: the wkhtmltopdf options mapped to HtmlToPdfConverter settings, the smart shrinking and zoom equivalents, margins, headers and footers with page numbers without the query string script and the rendering differences of a current Chromium engine."
---

# EvoPdf Next: migrating from wkhtmltopdf, DinkToPdf and Rotativa

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## When this applies

The application calls wkhtmltopdf, as a process or through a .NET wrapper built on its library:
DinkToPdf, Rotativa, TuesPechkin or Pechkin. All of them render with the same WebKit engine, so
the option map below applies to each of them. EvoPdf Next renders with a current Chromium engine
shipped in the NuGet package; the PDF comes back from a method call as a byte array.

## From a process to a method call

```csharp
using EvoPdf.Next;

// wkhtmltopdf --page-size A4 --margin-top 10mm --margin-bottom 10mm
//             --margin-left 10mm --margin-right 10mm https://www.example.com out.pdf
var converter = new HtmlToPdfConverter();
converter.PdfDocumentOptions.LeftMargin = 28;    // 10 mm in points
converter.PdfDocumentOptions.RightMargin = 28;
converter.PdfDocumentOptions.TopMargin = 28;
converter.PdfDocumentOptions.BottomMargin = 28;

// A4 laid out 896 pixels wide and drawn at 80 percent, as wkhtmltopdf does with smart shrinking
converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 896);

byte[] pdf = converter.ConvertUrl("https://www.example.com");
File.WriteAllBytes("out.pdf", pdf);
```

## From DinkToPdf or Rotativa

The global settings become page layout calls, the object settings become converter properties and
the header settings become an HTML template with page number variables.

```csharp
var converter = new HtmlToPdfConverter();
converter.FitBrowserWindowToPage(PdfPageSize.A4);

var header = converter.PdfDocumentOptions.PdfHtmlHeader;
header.Html = "<div style=\"text-align:right\">Page {page_number} of {total_pages}</div>";

byte[] pdf = converter.ConvertHtml(html, baseUrl);
```

In an ASP.NET Core action that replaces a Rotativa `ViewAsPdf`, convert the URL of the view and
return the bytes with `File(pdf, "application/pdf")`.

## Page size, margins and smart shrinking

wkhtmltopdf produces A4 with 10 mm margins by default and smart shrinking on: the page is laid out
896 pixels wide and drawn at 80 percent. A new EvoPdf Next converter produces A4 with margins 0 and
the page laid out in a 1024 pixel browser window, scaled to the page width. Set the margins for every
conversion moved from wkhtmltopdf, then pick the layout that gives the text size wanted.

| wkhtmltopdf | EvoPdf Next | Result on A4 with 10 mm margins |
| --- | --- | --- |
| smart shrinking on (default) | `FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 896)` | laid out at 896 pixels, drawn at 80 percent |
| `--disable-smart-shrinking` | `LayoutAtPageWidth(PdfPageSize.A4)` | laid out at 718 pixels, the width between the margins, drawn 1:1 |
| `--zoom 1.3` with smart shrinking | `LayoutAtPageWidth(PdfPageSize.A4)` | the nearest equivalent of 689 pixels drawn at 104 percent |
| `--zoom 1.5 --disable-smart-shrinking` | `LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Portrait, zoom: 150)` | laid out at 479 pixels, drawn at 150 percent |
| `--page-width 200mm --page-height 300mm` | `PdfPageSize = new PdfPageSize(567, 850)` | a custom page size in points |
| `--orientation Landscape` | the orientation argument of the layout method | |

Content wider than the page is laid out at its own width and scaled to fit, so nothing is cut.

## Option map

| wkhtmltopdf | EvoPdf Next |
| --- | --- |
| `--print-media-type` | `MediaType = "print"`; screen is the default in both |
| `--no-background` | `PdfDocumentOptions.PrintBackgrounds = false` |
| `--javascript-delay 2000` | `ConversionDelay = 2`, in seconds |
| `--window-status done` | `TriggeringMode = TriggeringMode.Manual` and `evoPdfConverter_startConversion()` called by the page |
| `--run-script` | `ScriptToExecuteAfterLoad` |
| `--disable-javascript` | `JavaScriptEnabled = false` |
| `--no-stop-slow-scripts` | `NavigationTimeout`, 120 seconds by default |
| `--cookie name value` | `HttpRequestCookies.Add(name, value)` |
| `--custom-header name value` | `HttpRequestHeaders.Add(name, value)` |
| `--post name value` | `HttpPostFields.Add(name, value)` |
| `--username`, `--password` | `AuthenticationOptions.Username`, `AuthenticationOptions.Password` |
| `--enable-local-file-access` | `LocalFilesEnabled`, true by default; set it to false for HTML you do not control |
| `--user-style-sheet file.css` | `ScriptToExecuteAfterLoad` adding a style element at the end of the head |
| `--title` | `PdfDocumentInfo.Title` |
| `--outline` | `PdfDocumentOptions.GenerateDocumentOutline = true` |
| `toc` object | `PdfDocumentOptions.TableOfContents`, styled with CSS |
| `cover page.html` | convert the cover separately and merge it with `PdfMerge` |
| `--enable-forms` | `PdfDocumentOptions.GeneratePdfFormFields = true` |
| `--page-offset` | `PageNumberOffset` and `TotalPagesOffset` of the header or footer |
| `thead` repeated on every page | `PdfDocumentOptions.RepeatTableHeaderFooter = true` |
| `--dpi`, `--image-dpi`, `--image-quality` | none: text and vector graphics are exact and images keep their data |

## Headers and footers

wkhtmltopdf passes `[page]` and `[topage]` to an HTML header as query string parameters that a
script writes into the page. In EvoPdf Next the header is an HTML template in which
`{page_number}` and `{total_pages}` are written directly, with no script.

```csharp
var converter = new HtmlToPdfConverter();
var header = converter.PdfDocumentOptions.PdfHtmlHeader;
header.Html = "<table style=\"width:100%;font:10pt Arial;border-bottom:1px solid #999\"><tr>" +
              "<td>Quarterly report</td>" +
              "<td style=\"text-align:right\">Page {page_number} of {total_pages}</td></tr></table>";
header.HtmlBaseUrl = "https://www.example.com";
header.Margins.Bottom = 14;   // --header-spacing 5
```

## Rendering differences to expect

- Flexbox, grid, CSS variables, ECMAScript 6 and later and WOFF 2 fonts render as in Chrome; the
  workarounds added for wkhtmltopdf can be removed.
- Pages that use media queries see the browser window width, 1024 pixels by default.
- Lazily loaded images are loaded before printing, because `LoadLazyImages` is true by default.

## Checklist

1. Replace the process start with a converter and `ConvertUrl` or `ConvertHtml`.
2. Set the margins in points and call `FitBrowserWindowToPage` or `LayoutAtPageWidth` with the page size.
3. Move the options to the properties of the option map.
4. Rewrite the header and footer HTML with `{page_number}` and `{total_pages}`.
5. Compare a few documents side by side.

The complete guide: https://www.evopdf.com/wkhtmltopdf-alternative-dotnet

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.
