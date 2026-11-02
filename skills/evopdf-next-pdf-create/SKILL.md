---
name: evopdf-next-pdf-create
description: "Build PDF documents from scratch in .NET with the EvoPdf Next Core PDF API: PdfDocument, pages of any size, text and image elements, HTML content flowing over pages, HTML rendered as an image, pages of other PDFs, shapes, lines, paths and polygons with solid or gradient fills, reusable templates, colours, alignment and rotation, and the render info returned for each element."
---

# EvoPdf Next: creating PDF documents from code

You can create PDF documents programmatically by adding text, images and other types of elements. The generated PDF can be saved either in memory or as a file and can be further processed using the PdfEditor component. You can enhance the generated PDF by adding security features such as encryption, permissions or a digital signature as well as custom headers and footers or visual elements like stamps and shapes.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfDocument`**: Represents a new PDF document being constructed in memory
- **`PdfTextElement`**: Represents a block of text to be rendered in a PDF document. Supports positioning, styling and multi-page continuation
- **`PdfImageElement`**: Represents an image element that can be rendered into a PDF document. Supports positioning, scaling and alignment
- **`PdfRectangleElement`**: Defines a rectangle to be drawn in the PDF document. The coordinates are in points and relative to the top left corner of the content area of the page...
- **`PdfRoundedRectangleElement`**: Defines a rectangle with rounded corners drawn in a PDF document. The coordinates are in points and relative to the top left corner of the content are...
- **`PdfCircleElement`**: Defines a circle drawn in a PDF document, specified by its center point and radius. The coordinates are in points and relative to the top left corner ...
- **`PdfEllipseElement`**: Defines an ellipse drawn in a PDF document, inscribed within an axis-aligned bounding rectangle before any rotation is applied. The coordinates are in...
- **`PdfArcElement`**: Defines an elliptical arc drawn in a PDF document. The arc is a portion of the ellipse inscribed in the axis-aligned rectangle defined by X, Y, Width ...
- **`PdfArcClosureType`**: Specifies how an arc element is closed and whether it can be filled
- **`PdfLineElement`**: Defines a straight line segment drawn between two points in a PDF document. The coordinates are in points and relative to the top left corner of the c...
- **`PdfPathElement`**: Defines a generic path composed of move, line, cubic Bezier and close operations. Use this element when none of the dedicated shape elements is expres...
- **`PdfPathOperation`**: Represents a single drawing operation that contributes to the geometry of a PdfPathElement. Use the factory methods to construct operations rather tha...
- **`PdfPathOperationType`**: Identifies the type of operation stored in a PdfPathOperation
- **`PdfPolygonElement`**: Defines a closed polygon drawn through a sequence of points in a PDF document. The polygon can be both stroked and filled. The coordinates are in poin...
- **`PdfPolylineElement`**: Defines an open polyline drawn through a sequence of points in a PDF document. The polyline is stroked but not filled. The coordinates are in points a...
- **`PdfLineStyle`**: Defines the appearance of a stroked line, including width, dash pattern, endpoint shape and corner join style
- **`PdfLineCapStyle`**: Specifies the shape used at the endpoints of a stroked open path
- **`PdfLineDashStyle`**: Specifies the dash style for line rendering
- **`PdfLineJoinStyle`**: Specifies the shape used at corners where two stroked path segments meet
- **`PdfColor`**: Represents a color used in PDF content
- **`PdfColorModel`**: Represents the color model used by a color instance
- **`PdfElementHorizontalAlign`**: Specifies the horizontal alignment of a PDF element on a PDF page
- **`PdfElementVerticalAlign`**: Specifies the vertical alignment of a PDF element on a PDF page
- **`PdfTextAlignment`**: Specifies the horizontal alignment of text when rendering in a PDF document
- **`PdfTextDirection`**: Specifies the direction in which the text flows in the PDF
- **`PdfRotationPivot`**: Defines the pivot point used when rotating a PDF element. The pivot represents the fixed point around which the element is rotated
- **`PdfPointF`**: Represents a point in the public PDF coordinate space used by the library
- **`PdfRectangle`**: Encapsulates a rectangle coordinates
- **`PdfRectangleF`**: Represents a rectangle in the PDF coordinate space used by the library
- **`PdfQuadF`**: Represents the four transformed corners of a rectangle in page space
- **`PdfElementRenderInfo`**: Contains metadata about where an element was rendered in the document. Each entry in RenderedRectangles describes one occurrence on a specific page or...
- **`PdfTextRenderInfo`**: Contains metadata about where and how text was rendered across pages. For each page on which a portion of the text was drawn, the corresponding entry ...
- **`PdfTextPageRenderInfo`**: Carries the per-page render result for a text element: the geometric rectangle on a specific page together with the portion of text drawn on that page...
- **`PdfImageRenderInfo`**: Contains metadata about how and where an image was rendered on a PDF page. Exposes both the axis-aligned bounding box and the rotated four-corner outl...
- **`PdfRenderedRectangle`**: Represents a single occurrence of an element on a PDF page. Carries the axis-aligned bounding rectangle, the rotated four-corner outline and the page ...
- **`PdfHtmlElement`**: HTML content rendered in the pages of a document. In a document being created the content starts at a position in the current page and flows to as man...
- **`PdfHtmlImageElement`**: Represents HTML content rendered as an image in a PDF document. The HTML is converted to an image by the HTML to Image converter of the element, at th...
- **`PdfPageElement`**: A page of an existing PDF document drawn in a page of the document being created or edited, at a given position and size, optionally rotated and with ...
- **`PdfHtmlRenderInfo`**: Describes where the content of a PdfHtmlElement was rendered in the document: the pages it occupies, the area it covers on each of them and the positi...
- **`PdfContentRenderInfo`**: Describes where content that flows over one or more pages was rendered in the document: the pages it occupies and the area it covers on each of them
- **`PdfGradientFill`**: A fill that changes smoothly from one color to another across the bounds of a shape. When set on a shape it replaces its fill color
- **`PdfGradientDirection`**: The direction in which the colors of a gradient change inside a shape
- **`PdfSizeF`**: A size in points: a width and a height

43 types, 525 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

The Core PDF API alone is installed with `EvoPdf.Next.Core.Windows` (x64 and ARM64), `EvoPdf.Next.Core.Linux` (x64 and
ARM64), `EvoPdf.Next.Core.MacOS` or the `EvoPdf.Next.Core` metapackage for all three; the full packages include it.
Core Component Overview: https://www.evopdf.com/help/evopdf-next-dotnet/html/core-component.htm

`PdfDocument.DigitalSignature`, `PdfDocument.AddSignatureField` and `PdfDocument.AddElectronicInvoice` are described in
`evopdf-next-security-signatures` and `evopdf-next-electronic-invoices`; `PdfDocument.AddFormField` in
`evopdf-next-pdf-forms`; `PdfDocument.Bookmarks`, `OpenAction` and `TableOfContents` in `evopdf-next-bookmarks-and-toc`.
Word, Excel, RTF and Markdown documents are added to a `PdfDocument` with the `AddWord`, `AddExcel`, `AddRtf` and
`AddMarkdown` extension methods of the document converter packages (`evopdf-next-document-converters`).

What can be added to a page, and the method that adds it:

| Content | Element | Method |
|---|---|---|
| Text, flowing over pages | `PdfTextElement` | `AddText` |
| Image | `PdfImageElement` (`KeepAspectRatio` false stretches it to the given size) | `AddImage` |
| HTML as text and vectors, flowing over pages | `PdfHtmlElement` | `AddHtml`, `AddHtmlAsync` |
| HTML rendered as an image | `PdfHtmlImageElement` | `AddHtmlImage` |
| A page of another PDF | `PdfPageElement` | `AddPdfPage` |
| Shapes, lines, paths, polygons | `PdfRectangleElement`, `PdfCircleElement`, `PdfPathElement`, ... | `AddRectangle`, `AddCircle`, `AddPath`, ... |
| Gradient fill of a shape | `PdfGradientFill` on `GradientFill` | with the shape |

`AddPage(pageSize, orientation)` adds a page of a given size and returns its number; the pages added next keep that
size. `PdfTextElement` and `PdfImageElement` continue on the next pages when they do not fit, on pages inserted after
the current one when `InsertContinuationPages` is true, with `ContinuationTopMargin` and `ContinuationBottomMargin` on
those pages. Closed shapes and paths are outlined with `Border`, `BorderColor` and `BorderOpacity`.

## Create PDF Documents

### Create, Render Elements and Save PDF Documents

```csharp
// Create a new PDF document with the default settings
using PdfDocument pdfDocument = new PdfDocument();
```

```csharp
PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
{
  PageSize = PdfPageSize.A4,
  PageOrientation = PdfPageOrientation.Portrait,
  Margins = new PdfMargins(36, 36, 36, 36)
};

// Create a new PDF document with the specified settings
using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
```

### Add New PDF Pages

```csharp
// Add a new PDF page with current settings
pdfDocument.AddPage();
```

```csharp
// The margins of the pages added next
pdfDocument.Margins = new PdfMargins(100, 100, 100, 100);

// A landscape A4 page; the pages added next keep this size
pdfDocument.AddPage(PdfPageSize.A4, PdfPageOrientation.Landscape);
```

All 11 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents.htm

## Create PDF Documents with Text

The `PdfDocument` class lets you build a PDF from scratch and add content elements through a small object-oriented API.

### Create a PDF Font

```csharp
// Bold italic green Courier at 16 points
PdfFont fontCourier = PdfFontManager.CreateStandardFont(
    PdfStandardFont.Courier, 16f,
    PdfFontStyle.Bold | PdfFontStyle.Italic,
    PdfColor.Green);
```

```csharp
string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");

// Load the font file into a base font (cached by physical path)
PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

// Derive a styled PdfFont from the base font
PdfFont trueTypeFont = PdfFontManager.CreateFont(
    baseFont, 16f,
    PdfFontStyle.Normal, PdfColor.Black);
```

### Create the Text Element

```csharp
PdfTextElement pdfText = new PdfTextElement("The text element string", fontCourier)
{
    X = crtXPos,
    Y = crtYPos
};
```

### Background Highlight

```csharp
PdfTextElement highlighted = new PdfTextElement(text, bodyFont)
{
    X = 0, Y = crtYPos, Width = pdfDocument.ContentWidth,
    BackgroundColor = PdfColor.Yellow,
    BackgroundOpacity = 0.4f
};
pdfDocument.AddText(highlighted);
```

All 10 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-text.htm

## Create PDF Documents with Images

The `PdfDocument` class lets you build a PDF from scratch and add content elements through a small object-oriented API.

### Create the Image Element

```csharp
PdfImageElement pdfImage = new PdfImageElement(imagePath)
{
    X = crtXPos,
    Y = crtYPos
};
```

### Positioning and Scaling

```csharp
string imagesPath = GetDemoImagesPath();

// Add a transparent PNG image with a custom width
PdfImageElement pdfPngImage = new PdfImageElement(
    Path.Combine(imagesPath, "transparent.png"))
{
    X = crtXPos,
    Y = crtYPos,
    Width = 150
};
```

### Render the Image Element

```csharp
PdfImageRenderInfo imageRenderInfo = pdfDocument.AddImage(pdfPngImage);

// Use the rendered geometry to position the next element
crtYPos = imageRenderInfo.BoundingBox.Bottom + ySeparator;
```

All 5 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-images.htm

## Create PDF Documents with Shapes

The library exposes a dedicated element for each common geometric primitive. Call sites are short. The rendering pipeline is more efficient than building the same shapes with arbitrary paths.

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-shapes.htm

## Create PDF Documents with Polylines, Polygons and Paths

Three vector primitives cover most cases when the visual you need is more complex than a basic rectangle or circle. `PdfPolylineElement` renders an open piecewise-linear path. `PdfPolygonElement` renders a closed polygon described by a vertex list. `PdfPathElement` renders an arbitrary path built from move, line and Bezier curve operations.

### Arbitrary Paths

```csharp
PdfPathElement heart = new PdfPathElement
{
    FillColor = PdfColor.Crimson,
    FillOpacity = 0.8f,
    BorderColor = PdfColor.DarkRed,
    Border = new PdfLineStyle { LineWidth = 1.5f, LineJoin = PdfLineJoinStyle.Round }
};
heart
    .MoveTo(cx, cy + s * 0.25f)
    .CurveTo(cx - s * 0.55f, cy + s * 0.55f,
             cx - s * 1.10f, cy - s * 0.10f,
             cx, cy - s * 0.35f)
    .CurveTo(cx + s * 1.10f, cy - s * 0.10f,
             cx + s * 0.55f, cy + s * 0.55f,
             cx, cy + s * 0.25f)
    .Close();
pdfDocument.AddPath(heart);
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-paths-and-polygons.htm

## Create PDF Documents with HTML Content

The `PdfHtmlElement` class renders HTML content in the pages of a document being created with `PdfDocument`. The content starts at a position in the current page, after the elements added before it and flows to as many new pages as it needs, laid out inside the margins of the document. The elements added after it go to its last page, below the rendered content. The HTML is given as a string with a base URL or as the URL of a page, like for the HTML to PDF converter.

### Position and Width

```csharp
PdfHtmlElement htmlElement = new PdfHtmlElement(htmlString, baseUrl)
{
    X = 0,
    Y = crtYPos,
    // 0 extends the content to the right margin of the page
    Width = 0
};
PdfHtmlRenderInfo htmlRenderInfo = pdfDocument.AddHtml(htmlElement);

// The elements added after the content go below it, in its last page
crtYPos = htmlRenderInfo.LastPageRectangle.Bounds.Bottom + 15;
```

### Converter Settings

```csharp
htmlElement.Converter.MediaType = "print";
htmlElement.Converter.PdfDocumentOptions.GenerateDocumentOutline = true;
```

### Table of Contents

```csharp
pdfDocument.TableOfContents.Enabled = true;
pdfDocument.TableOfContents.Title = "Contents";
pdfDocument.AddHtml(new PdfHtmlElement(firstHtml, null));
pdfDocument.AddPage();
pdfDocument.AddHtml(new PdfHtmlElement(secondHtml, null));
byte[] pdf = pdfDocument.Save();
```

### Positions of the HTML Elements

```csharp
htmlElement.HtmlElementsInfoSelector = "h1, h2";
PdfHtmlRenderInfo htmlRenderInfo = pdfDocument.AddHtml(htmlElement);

foreach (HtmlElementInfo elementInfo in htmlRenderInfo.HtmlElementsInfo.Elements)
{
    foreach (PdfRenderedRectangle rectangle in elementInfo.RenderedRectangles)
        Console.WriteLine("{0} on page {1} at {2}", elementInfo.TagName, rectangle.PageNumber, rectangle.Bounds);
}
```

All 5 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-html-content.htm

## Create PDF Documents with HTML as Image

The `PdfHtmlImageElement` class renders an HTML document as an image in the pages of a document being created with `PdfDocument`. The HTML is given as the URL of a page or as a string with a base URL and it is converted by the HTML to Image converter of the element, which captures the whole page at the height of its content. The image starts at a position in the current page, after the elements added before it, and continues on the next pages when it is taller than the space left on the page. The elements added after it go below the image on its last page.

### Position, Size and Resolution

```csharp
PdfHtmlImageElement htmlImage = new PdfHtmlImageElement(htmlCode, baseUrl)
{
    X = 0,
    Y = crtYPos,
    // 0 draws the image at the size of the content on screen, scaled down to the right margin when it is wider
    Width = 0,
    // 0 gives no height limit
    Height = 0,
    // The image has the same size in the page at any resolution; a higher resolution gives a sharper image
    Resolution = 150
};
htmlImage.Accessibility.AlternateText = "Sales table rendered from HTML";

PdfImageRenderInfo htmlImageInfo = pdfDocument.AddHtmlImage(htmlImage);
```

### Converter Settings

```csharp
htmlImage.Converter.HtmlViewerWidth = 1024;
htmlImage.Converter.MediaType = "print";
```

### Image Continued on the Next Pages

```csharp
htmlImage.ContinueOnNextPage = true;
PdfImageRenderInfo htmlImageInfo = pdfDocument.AddHtmlImage(htmlImage);

foreach (PdfRenderedRectangle part in htmlImageInfo.RenderedRectangles)
    Console.WriteLine("Page {0}: {1}", part.PageNumber, part.Bounds);

// The elements added after the image go below the part drawn on its last page
PdfRectangleF lastPart = htmlImageInfo.LastPageRectangle.VisibleBounds ?? htmlImageInfo.LastPageRectangle.Bounds;
crtYPos = lastPart.Bottom + 10;
```

All 4 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-html-as-image.htm

## Create PDF Documents with Pages of Other PDF Documents

The `PdfPageElement` class draws a page of an existing PDF document in a page of the document being created, at a given position and size, optionally rotated and with a given opacity. The drawn page keeps its content, its fonts, images and vector graphics, its links and form fields when the annotations are included and its structure when both documents are tagged. The element is added with `PdfDocument.AddPdfPage(PdfPageElement)`, which returns the rectangle used by the drawn page.

### Position, Size and Rotation

```csharp
// The letterhead page covers the whole current page, under the content added later
PdfPageElement letterhead = new PdfPageElement(letterheadPath, 1)
{
    X = -margins.Left,
    Y = -margins.Top,
    RenderUnderContent = true
};
pdfDocument.AddPdfPage(letterhead);

// A page of another document scaled to fit a rectangle, keeping its proportions
PdfPageElement pageElement = new PdfPageElement(sourcePdfBytes, 2)
{
    X = 0,
    Y = crtYPos,
    Width = 150,
    Height = 210,
    Opacity = 1,
    RotationDegrees = 0,
    IncludeAnnotations = true
};
PdfElementRenderInfo renderInfo = pdfDocument.AddPdfPage(pageElement);
PdfRectangleF bounds = renderInfo.LastPageRectangle.Bounds;
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-pdf-pages.htm

## Create PDF Documents with Gradient Fills

The shapes of a PDF document can be painted with a linear gradient instead of a solid color. A `PdfGradientFill` object changes smoothly from a start color to an end color across the bounds of the shape, along one of the directions of the `PdfGradientDirection` enumeration.

### Gradient Colors and Directions

```csharp
PdfRectangleElement rectangle = new PdfRectangleElement(0, 0, 110, 60)
{
    GradientFill = new PdfGradientFill(PdfColor.LightSkyBlue, PdfColor.DarkBlue, PdfGradientDirection.Horizontal),
    BorderColor = PdfColor.DarkBlue,
    Border = new PdfLineStyle { LineWidth = 1f }
};
pdfDocument.AddRectangle(rectangle);
```

### Size Shapes with the Measured Text Width

```csharp
// The width of the text written with the font gives the width of its badge
float textWidth = badgeFont.GetTextWidth("GRADIENT FILL");
float badgeWidth = textWidth + 2 * badgePadding;

pdfDocument.AddRoundedRectangle(new PdfRoundedRectangleElement(badgeX, badgeY, badgeWidth, badgeHeight, badgeHeight / 2)
{
    GradientFill = new PdfGradientFill(PdfColor.LightSkyBlue, PdfColor.DarkBlue, PdfGradientDirection.Vertical),
    BorderColor = PdfColor.DarkBlue,
    Border = new PdfLineStyle { LineWidth = 0.5f }
});

// The text is centered in the badge from the measured width
PdfTextElement badgeText = new PdfTextElement("GRADIENT FILL", badgeFont)
{
    X = badgeX + badgePadding,
    Y = badgeY + (badgeHeight - badgeFont.Size) / 2 - 1,
    Width = textWidth + 2
};
pdfDocument.AddText(badgeText);
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-gradient-fills.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
