---
name: evopdf-next-pdf-edit
description: "Open and modify an existing PDF in .NET with EvoPdf Next PdfEditor: add text, images, shapes, HTML content, HTML as an image, pages of other PDFs and HTML templates to loaded pages, insert, move, rotate and remove pages, append blank pages, flatten, and save. Use for stamping, watermarking and post-processing tasks."
---

# EvoPdf Next: editing an existing PDF

The `PdfEditor` class lets you add new content to an existing PDF without rewriting the rest of the document. The editor is instantiated with the source PDF bytes or file path and an optional password. The standard claimed by the source document and its existing page geometry are inherited by the editor. Content is added at absolute page coordinates using the dedicated Add methods. Every Add method takes the target page number as the first argument. If the rendered content overflows the available space a new page is created automatically by the engine. The resulting modified PDF is produced with `PdfEditor.Save`.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfEditor`**: Allows editing of existing PDF documents by adding HTML content, text, images and shapes, form fields and annotations, filling the existing form field...

1 types, 67 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

`PdfEditor` also signs existing documents, adds signature fields and long-term validation data (see
`evopdf-next-security-signatures`), embeds electronic invoices in PDF/A-3 documents (see `evopdf-next-electronic-invoices`),
fills, flattens and adds form fields (`Form`, `AddFormField`, see `evopdf-next-pdf-forms`) and edits the bookmarks and the
open action (`Bookmarks`, `OpenAction`, see `evopdf-next-bookmarks-and-toc`).

| Task | Member |
|---|---|
| Number of pages | `PageCount` |
| Add a blank page at the end, of the size of the last page or of a given size | `AddPage()`, `AddPage(pageSize, orientation)` |
| Insert, move, remove pages | `InsertPage`, `MovePage`, `RemovePage`, `RemovePages("2,4-")` |
| Rotate a page | `RotatePage(page, degrees)` adds to the rotation, `SetPageRotation(page, rotation)` sets it |
| HTML as text and vectors from a position, flowing over the next pages | `AddHtml(pageNumber, PdfHtmlElement)` |
| HTML as an image | `AddHtmlImage(pageNumber, PdfHtmlImageElement)` |
| A page of another PDF drawn on a page | `AddPdfPage(pageNumber, PdfPageElement)` |
| Static form fields and no form | `Flatten = true` |
| Document information, viewer preferences, security of the saved document | `PdfDocumentInfo`, `PdfViewerPreferences`, `PdfSecurityOptions` |
| The same, read from the loaded document | `DocumentInfo`, `ViewerPreferencesInfo`, `SecurityInfo`, `SignatureInfo` |

Page numbers are 1-based. Page operations are done before content is added to the pages concerned; the bookmarks and
the open action set in the editor keep pointing to the same pages.

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

All 4 samples of this topic, complete: `references/examples.md`.

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

## Add HTML to Existing PDF

The `PdfEditor` class renders HTML content in the pages of a loaded document with `PdfEditor.AddHtml(Int32, PdfHtmlElement)`, or asynchronously with `PdfEditor.AddHtmlAsync(Int32, PdfHtmlElement, CancellationToken)`, which take the number of the page where the content starts and a `PdfHtmlElement`, the element described in the topic. The content starts at the position given by the element, relative to the top left corner of the page, and continues on the next pages when it does not fit, between the continuation margins of the element. The links and the form fields of the content are kept and the outline generated from the HTML headings joins the bookmarks of the document. In a tagged document, like a PDF/UA document, the content is rendered with the standard of the document and its structure joins the structure tree of the document.

### Position, Width and Continuation

```csharp
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, null);

PdfHtmlElement htmlElement = new PdfHtmlElement(htmlString, baseUrl)
{
    X = 36,
    Y = 120,
    Width = 523,
    // The pages where the content continues are inserted after the first page
    InsertContinuationPages = true,
    ContinuationTopMargin = 72,
    ContinuationBottomMargin = 72
};
PdfHtmlRenderInfo htmlRenderInfo = pdfEditor.AddHtml(1, htmlElement);

// The elements added after the content go below it, in its last page
int lastPage = htmlRenderInfo.LastPageNumber;
float crtYPos = htmlRenderInfo.LastPageRectangle.Bounds.Bottom + 15;
```

### Converter Settings and Element Positions

```csharp
htmlElement.Converter.MediaType = "print";
htmlElement.Converter.PdfDocumentOptions.GenerateDocumentOutline = true;
htmlElement.HtmlElementsInfoSelector = "h1, h2";

PdfHtmlRenderInfo htmlRenderInfo = pdfEditor.AddHtml(1, htmlElement);
foreach (HtmlElementInfo elementInfo in htmlRenderInfo.HtmlElementsInfo.Elements)
{
    foreach (PdfRenderedRectangle rectangle in elementInfo.RenderedRectangles)
        Console.WriteLine("{0} on page {1} at {2}", elementInfo.TagName, rectangle.PageNumber, rectangle.Bounds);
}
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-html-to-existing-pdf.htm

## Add HTML as Image to Existing PDF

The `PdfEditor` class renders an HTML document as an image in a page of a loaded document with `PdfEditor.AddHtmlImage(Int32, PdfHtmlImageElement)`, which takes the number of the page to draw in and a `PdfHtmlImageElement`, the element described in the topic. In an edited document the position of the element is given from the top left corner of the page and the image is scaled down to the right edge of the page when it is wider.

### Image Continued on the Next Pages

```csharp
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, null);

int pageNumber = pdfEditor.AddPage();
PdfHtmlImageElement htmlImage = new PdfHtmlImageElement(htmlCode, baseUrl)
{
    X = 36,
    Y = 72,
    Resolution = 150,
    ContinueOnNextPage = true,
    // The pages where the image continues are inserted after the current page
    InsertContinuationPages = true,
    ContinuationTopMargin = 36,
    ContinuationBottomMargin = 36
};
PdfImageRenderInfo htmlImageInfo = pdfEditor.AddHtmlImage(pageNumber, htmlImage);

byte[] outPdfBuffer = pdfEditor.Save();
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-html-as-image-to-existing-pdf.htm

## Add Pages of Other PDF Documents to Existing PDF

The `PdfEditor` class draws a page of another PDF document in the pages of a loaded document with `PdfEditor.AddPdfPage(Int32, PdfPageElement)`, which takes the number of the page to draw in and a `PdfPageElement`, the element described in the topic. The drawn page keeps its size or is scaled to fit the given width and height, it can be rotated around a pivot point and it can have an opacity. The links and form fields of the drawn page are added to the edited page when the annotations are included.

### Draw a Page in the Edited Pages

```csharp
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, null);

for (int pageNumber = 1; pageNumber <= pdfEditor.PageCount; pageNumber++)
{
    // The letterhead page under the content of each page, at its own size
    PdfPageElement pageElement = new PdfPageElement(letterheadPdfBytes, 1)
    {
        X = 0,
        Y = 0,
        Opacity = 1,
        RenderUnderContent = true,
        IncludeAnnotations = true
    };
    PdfElementRenderInfo renderInfo = pdfEditor.AddPdfPage(pageNumber, pageElement);
}

byte[] outPdfBuffer = pdfEditor.Save();
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-pdf-pages-to-existing-pdf.htm

## Add, Insert, Move, Rotate and Remove PDF Pages

The `PdfEditor` class can change the pages of an existing PDF document: it adds blank pages at the end, inserts blank pages at a position, moves pages to another position, rotates pages and removes pages. The content, the links and the bookmarks of the existing pages are not changed by these operations.

### Add and Insert Pages

```csharp
// Insert a blank page of the chosen size before the first page. The existing pages
// are shifted and keep their content, links and bookmarks
int coverPageNumber = pdfEditor.InsertPage(1, PdfPageSize.A4, PdfPageOrientation.Portrait);

// Add a blank page at the end of the document, with the size of the last page
int lastPageNumber = pdfEditor.AddPage();
```

### Move Pages

```csharp
// Move a page to another position; the pages in between are shifted
pdfEditor.MovePage(movePageNumber, moveToPageNumber);
```

### Rotate Pages

```csharp
// Rotate the page clockwise by a multiple of 90 degrees. The rotation is added to the
// current rotation of the page; SetPageRotation would set an absolute rotation instead.
// A page is rotated before content is added to it
pdfEditor.RotatePage(rotatePageNumber, 90);

// A note at the bottom of the rotated page, placed in the page as it is displayed:
// a rotation of 90 or 270 degrees swaps its width and height
PdfPageInfo rotatedPageInfo = pdfEditor.GetPdfPageInfo(rotatePageNumber);
bool turned = rotatedPageInfo.Rotation % 180 != 0;
float displayedWidth = turned ? rotatedPageInfo.PageSize.Height : rotatedPageInfo.PageSize.Width;
float displayedHeight = turned ? rotatedPageInfo.PageSize.Width : rotatedPageInfo.PageSize.Height;
PdfTextElement rotatedPageText = new PdfTextElement("This page was rotated by 90 degrees", noteFont)
{
    X = 36,
    Y = displayedHeight - 60,
    Width = displayedWidth - 72
};
pdfEditor.AddText(rotatePageNumber, rotatedPageText);
```

### Remove Pages

```csharp
// The pages are removed first, so the ranges refer to the pages of the loaded document. The
// ranges are 1-based, for example "2,4-5" or "3-" for the pages from the third one to the end
pdfEditor.RemovePages("2,4-5");
```

All 5 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-insert-move-rotate-and-remove-pdf-pages.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
