---
name: evopdf-next-pdf-processor
description: "Process existing PDF documents in .NET with the EvoPdf Next PDF Processor: extract text (PdfToTextConverter, layout or reading order), search text and get the position of every match (FindText, FindTextLocation), render pages to PNG images (PdfToImageConverter, DPI, color space, transparency), extract embedded images (PdfImagesExtractor). Use for PDF to text, PDF search, PDF to image, PDF thumbnails and image extraction tasks in C#."
---

# EvoPdf Next — PDF Processor

Package: `EvoPdf.Next.PdfProcessor.Windows` / `.Linux` / `.MacOS` (or the all-components package). Namespace `EvoPdf.Next`. Three classes: `PdfToTextConverter`, `PdfToImageConverter`, `PdfImagesExtractor`. Read `references/api.md` for every overload.

## Rules shared by the three classes
- **Instances are not reusable** — a new instance for every conversion, search or extraction; a second call throws.
- Input as file path, `byte[]` or `Stream`. Page ranges in every method: `(source)` = all pages, `(source, startPage)` = to the end, `(source, startPage, endPage)` = inclusive; pages are 1-based.
- Protected PDFs: set `UserPassword` or `OwnerPassword` before the call.
- Guards: `MaxPageCount` (0 = unlimited, default) caps the pages processed; `RunTimeoutSec` caps the run time.
- After a call, `ConversionInfo` / `ExtractionInfo` report `PageCount` (and `ImagesPerPage` for extraction).
- Every method has an `…Async(…, CancellationToken)` twin.
- On Linux the native runtime needs execute permission; on Azure Functions Linux call `PdfProcessorInstallation.ConfigureRuntime(true, null)` first — see the linux and azure skills.

## PDF to text
```csharp
using EvoPdf.Next;

var converter = new PdfToTextConverter();
converter.TextLayout = PdfToTextLayout.Original;   // default: keep the visual layout; Reading = reading order
converter.MarkPageBreaks = true;                    // insert PdfToTextConverter.PAGE_BREAK_MARK between pages (default false)
converter.UserPassword = "…";                       // only for protected PDFs
string all = converter.ConvertToText("input.pdf");
string pages2to5 = new PdfToTextConverter().ConvertToText(pdfBytes, 2, 5);
```
Use `Original` for invoices, tables and forms where position matters; `Reading` for articles and multi-column text that will be indexed or fed to an LLM.

## Find text with positions
```csharp
var search = new PdfToTextConverter();
FindTextLocation[] hits = search.FindText(pdfBytes, "Total", caseSensitive: false, wholeWord: true);
foreach (FindTextLocation h in hits)
    Console.WriteLine($"page {h.PageNumber}: x={h.X} y={h.Y} w={h.Width} h={h.Height}");   // points, origin top-left of the page
```
Results come in document order (top to bottom, left to right). Typical uses: highlight (draw a rectangle at the location with `PdfEditor.AddRectangle`), redact, locate a signature field, verify that generated PDFs contain expected text.

## PDF pages to images
```csharp
var toImage = new PdfToImageConverter();
toImage.Resolution = 150;                              // DPI, default 150
toImage.ColorSpace = PdfPageImageColorSpace.RGB;       // RGB (default), Gray, Mono
toImage.TransparencyEnabled = false;                   // true only with RGB or Gray
PdfPageImage[] pages = toImage.ConvertToImages("input.pdf");     // PNG per page: ImageData, PageNumber
foreach (PdfPageImage p in pages) File.WriteAllBytes($"page-{p.PageNumber}.png", p.ImageData);

new PdfToImageConverter().ConvertToImageFiles("input.pdf", 1, 3, "out", "page");   // writes out/page-1.png …
```
Thumbnails: lower `Resolution` (e.g. 40–72 DPI) instead of resizing full-size images. `StdFontsDir` points at standard fonts for PDFs that do not embed them.

## Extract embedded images
```csharp
var extractor = new PdfImagesExtractor();
ExtractedImage[][] perPage = extractor.ExtractImages("input.pdf");   // one array per page; ExtractedImage.ImageData (PNG), PageNumber
int n = 0;
foreach (ExtractedImage[] page in perPage)
    foreach (ExtractedImage img in page) File.WriteAllBytes($"img-{img.PageNumber}-{++n}.png", img.ImageData);

new PdfImagesExtractor().ExtractImagesToFile("input.pdf", "out", "img");   // files out/img-… ; extractor.ExtractionInfo.ImagesPerPage
```
Images are returned as PNG with transparency preserved; vector graphics are not images and are not extracted (render the page instead).

## Choosing between them
| Need | Use |
|---|---|
| Index, search, feed text to an LLM | `ConvertToText` with `Reading` layout |
| Exact position of a string | `FindText` |
| Preview, thumbnail, OCR input, print-like snapshot | `ConvertToImages` |
| The photos/logos inside the PDF, at original resolution | `ExtractImages` |

Runnable versions: `quickstarts/Samples/PdfProcessor.*.cs` in https://github.com/EvoPdf/evopdf-next-samples · Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-pdf-to-text.htm · search-for-text-in-pdf.htm · convert-pdf-pages-to-images.htm · extract-images-from-pdf.htm
