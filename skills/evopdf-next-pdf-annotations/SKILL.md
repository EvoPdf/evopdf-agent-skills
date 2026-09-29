---
name: evopdf-next-pdf-annotations
description: "Add clickable link annotations and sticky-note text annotations to generated or existing PDFs with EvoPdf Next: internal page destinations, external URLs, border and highlight styles, note icons, colour and opacity."
---

# EvoPdf Next: link and text annotations

Link annotations turn an arbitrary rectangle on a page into a clickable hotspot. The hotspot opens an external URL or jumps to a destination inside the same document. Link annotations only describe the click target. The visible content (typically underlined blue text or a bordered button) is rendered separately with AddText or AddRectangle on the same area.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfLinkAnnotation`**: A clickable hyperlink annotation placed on a PDF page. Use one of the static factory methods (FromUrl, ToPage, ToPageLocation to build the link, then ...
- **`PdfLinkBorderStyle`**: Border style drawn around a PdfLinkAnnotation hotspot. Default is None -- typical for text hyperlinks where the clickable area is implied by the under...
- **`PdfLinkHighlightMode`**: Visual feedback shown when the user activates a PdfLinkAnnotation hotspot
- **`PdfLinkPageFit`**: Page-fit type used by PdfLinkPageLocation to describe how the target page is displayed when the link is followed
- **`PdfLinkPageLocation`**: Describes how the target page is positioned and zoomed when a PdfLinkAnnotation created with ToPageLocation is followed. Use one of the static factory...
- **`PdfTextAnnotation`**: Represents a sticky-note style annotation displayed as a clickable icon on a specific page of the PDF. Clicking the icon opens a popup with the note t...
- **`PdfTextAnnotationIcon`**: Icon displayed on the page surface for a sticky-note text annotation

7 types, 45 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Create PDF Documents with Link Annotations

### External URL Links

```csharp
PdfTextElement t = new PdfTextElement("Visit evopdf.com", linkFont) { X = 0, Y = crtYPos };
var info = pdfDocument.AddText(t);
var b = info.LastPageRectangle.Bounds;

PdfLinkAnnotation link = PdfLinkAnnotation.FromUrl(
    url: "https://www.evopdf.com",
    pageNumber: 1,
    x: b.X, y: b.Y, width: b.Width, height: b.Height);
link.Description = "EvoPdf homepage";
link.BorderStyle = PdfLinkBorderStyle.None;

pdfDocument.AddLinkAnnotation(link);
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-link-annotations.htm

## Add Link Annotations to Existing PDF

The `PdfEditor` class lets you add new content to an existing PDF without rewriting the rest of the document. The editor is instantiated with the source PDF bytes or file path and an optional password. The standard claimed by the source document and its existing page geometry are inherited by the editor. Content is added at absolute page coordinates using the dedicated Add methods. Every Add method takes the target page number as the first argument. If the rendered content overflows the available space a new page is created automatically by the engine. The resulting modified PDF is produced with `PdfEditor.Save`.

### External URL Links

```csharp
PdfTextElement t = new PdfTextElement("Visit evopdf.com", linkFont) { X = 0, Y = crtYPos };
var info = pdfEditor.AddText(currentPage, t);
var b = info.LastPageRectangle.Bounds;

PdfLinkAnnotation link = PdfLinkAnnotation.FromUrl(
    url: "https://www.evopdf.com",
    pageNumber: currentPage,
    x: b.X, y: b.Y, width: b.Width, height: b.Height);
link.Description = "EvoPdf homepage";
link.BorderStyle = PdfLinkBorderStyle.None;
pdfEditor.AddLinkAnnotation(link);
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-link-annotations-to-existing-pdf.htm

## Create PDF Documents with Text Annotations

Text annotations (also known as sticky notes) are small markers placed on a PDF page. When clicked they open a popup containing a text comment. They are the standard mechanism for review feedback, in-document comments and collaborative annotations.

### Author Identification and Initial Popup State

```csharp
PdfTextAnnotation ann = PdfTextAnnotation.Create(
    contents: "Please confirm the issue date before mailing.",
    pageNumber: 1, x: 30, y: 200);
ann.Icon = PdfTextAnnotationIcon.Comment;
ann.Author = "Jane Reviewer";
ann.Open = true;
pdfDocument.AddTextAnnotation(ann);
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-text-annotations.htm

## Add Text Annotations to Existing PDF

The `PdfEditor` class lets you add new content to an existing PDF without rewriting the rest of the document. The editor is instantiated with the source PDF bytes or file path and an optional password. The standard claimed by the source document and its existing page geometry are inherited by the editor. Content is added at absolute page coordinates using the dedicated Add methods. Every Add method takes the target page number as the first argument. If the rendered content overflows the available space a new page is created automatically by the engine. The resulting modified PDF is produced with `PdfEditor.Save`.

### Author Identification and Initial Popup State

```csharp
PdfTextAnnotation ann = PdfTextAnnotation.Create(
    contents: "Please confirm the issue date before mailing.",
    pageNumber: 1, x: 30, y: 200);
ann.Icon = PdfTextAnnotationIcon.Comment;
ann.Author = "Jane Reviewer";
ann.Open = true;
pdfEditor.AddTextAnnotation(ann);
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-text-annotations-to-existing-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
