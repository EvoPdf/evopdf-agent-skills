---
name: evopdf-next-pdf-processing
description: "Create, edit and process PDF documents in .NET with EvoPdf Next: Core PDF API (PdfDocument, PdfEditor), PDF to text with search (PdfToTextConverter.FindText), PDF to image, embedded image extraction. Use for PDF manipulation tasks in C#."
---

# EvoPdf Next — PDF processing

Packages: `EvoPdf.Next.Core.*` (create/edit), `EvoPdf.Next.PdfProcessor.*` (text, images) — `.Windows` / `.Linux` / `.MacOS`. Namespace `EvoPdf.Next`.

## Create a PDF
```csharp
using EvoPdf.Next;

using var pdf = new PdfDocument(new PdfDocumentCreateSettings {
    PageSize = PdfPageSize.A4, Margins = new PdfMargins(36, 36, 36, 36) });
var font = PdfFontManager.CreateStandardFont(PdfStandardFont.Helvetica, 16f, PdfFontStyle.Bold, PdfColor.Black);
pdf.AddText(new PdfTextElement("Hello", font));
pdf.AddImage(…); pdf.AddLine(…); pdf.AddRectangle(…); pdf.AddHtmlTemplate(…); pdf.AddFileAttachment(…);
pdf.SaveToFile("created.pdf");            // or byte[] Save()
```
`PdfEditor` opens an existing PDF and exposes the same `Add…` methods plus `GetPageCount()` / `GetPdfPageInfo()` — stamp, merge (`AddPdfTemplate`), annotate, attach, sign, encrypt.

## Text, search, images
```csharp
var text = new PdfToTextConverter();
text.TextLayout = PdfToTextLayout.Original;              // or reading order
string s = text.ConvertToText(pdfBytes);
FindTextLocation[] hits = text.FindText(pdfBytes, "invoice", caseSensitive: false, wholeWord: true);
foreach (var h in hits) Console.WriteLine($"page {h.PageNumber}: {h.X},{h.Y} {h.Width}x{h.Height}");

var toImage = new PdfToImageConverter();                 // one PNG per page
var pages = toImage.ConvertToImages(pdfBytes);           // or ConvertToImageFiles(…)

var extractor = new PdfImagesExtractor();
var images = extractor.ExtractImages(pdfBytes);          // PNG, transparency preserved; ExtractImagesToFile(…)
```
- All processing methods accept password-protected PDFs and page ranges; each has an `…Async` variant.
- Positions are in PDF points, origin top-left of the page.
