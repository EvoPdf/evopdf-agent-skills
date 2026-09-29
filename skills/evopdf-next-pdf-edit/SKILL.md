---
name: evopdf-next-pdf-edit
description: "Open and modify an existing PDF in .NET with EvoPdf Next PdfEditor: add text, images, shapes and HTML templates to loaded pages, append blank pages, flatten, and save. Use for stamping, watermarking and post-processing tasks."
---

# EvoPdf Next: editing an existing PDF

The `PdfEditor` class lets you add new content to an existing PDF without rewriting the rest of the document. The editor is instantiated with the source PDF bytes or file path and an optional password. The standard claimed by the source document and its existing page geometry are inherited by the editor. Content is added at absolute page coordinates using the dedicated Add methods. Every Add method takes the target page number as the first argument. If the rendered content overflows the available space a new page is created automatically by the engine. The resulting modified PDF is produced with `PdfEditor.Save`.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfEditor`**: Allows editing of existing PDF documents by adding HTML content, setting metadata, applying security options, and digital signatures. Use Save or Save...

1 types, 49 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Add Text to Existing PDF

### Open the Source PDF

```csharp
string password = string.IsNullOrEmpty(ownerPassword) ? userPassword : ownerPassword;
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
```

### AddText, Page Cursor and Multi-Page Flow

```csharp
PdfTextElement element = new PdfTextElement(text, font) { X = 0, Y = crtYPos, Width = contentWidth };
var info = pdfEditor.AddText(currentPage, element);
currentPage = info.LastPageRectangle.PageNumber;        // follow the engine if it overflowed
crtYPos = (int)info.LastPageRectangle.Bounds.Bottom + ySeparator;
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-text-to-existing-pdf.htm

## Add Images to Existing PDF

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-images-to-existing-pdf.htm

## Add Shapes to Existing PDF

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-shapes-to-existing-pdf.htm

## Add Polylines, Polygons and Paths to Existing PDF

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-paths-and-polygons-to-existing-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
