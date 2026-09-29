---
name: evopdf-next-pdf-create
description: "Build PDF documents from scratch in .NET with the EvoPdf Next Core PDF API: PdfDocument, pages, text and image elements, shapes, lines, paths and polygons, reusable templates, colours, alignment and rotation, and the render info returned for each element."
---

# EvoPdf Next: creating PDF documents from code

You can create PDF documents programmatically by adding text, images and other types of elements. The generated PDF can be saved either in memory or as a file and can be further processed using the PdfEditor component. You can enhance the generated PDF by adding security features such as encryption, permissions or a digital signature as well as custom headers and footers or visual elements like stamps and shapes.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfDocument`**: Represents a new PDF document being constructed in memory
- **`PdfTextElement`**: Represents a block of text to be rendered in a PDF document. Supports positioning, styling and multi-page continuation
- **`PdfImageElement`**: Represents an image element that can be rendered into a PDF document. Supports positioning, scaling and alignment
- **`PdfRectangleElement`**: Defines a rectangle to be drawn in the PDF document. Coordinates are relative to the top-left corner of the page or template
- **`PdfRoundedRectangleElement`**: Defines a rectangle with rounded corners drawn in a PDF document. Coordinates are relative to the top-left corner of the page or template
- **`PdfCircleElement`**: Defines a circle drawn in a PDF document, specified by its center point and radius. Coordinates are relative to the top-left corner of the page or tem...
- **`PdfEllipseElement`**: Defines an ellipse drawn in a PDF document, inscribed within an axis-aligned bounding rectangle before any rotation is applied. Coordinates are relati...
- **`PdfArcElement`**: Defines an elliptical arc drawn in a PDF document. The arc is a portion of the ellipse inscribed in the axis-aligned rectangle defined by X, Y, Width ...
- **`PdfArcClosureType`**: Specifies how an arc element is closed and whether it can be filled
- **`PdfLineElement`**: Defines a straight line segment drawn between two points in a PDF document. Coordinates are relative to the top-left corner of the page or template
- **`PdfPathElement`**: Defines a generic path composed of move, line, cubic Bezier and close operations. Use this element when none of the dedicated shape elements is expres...
- **`PdfPathOperation`**: Represents a single drawing operation that contributes to the geometry of a PdfPathElement. Use the factory methods to construct operations rather tha...
- **`PdfPathOperationType`**: Identifies the type of operation stored in a PdfPathOperation
- **`PdfPolygonElement`**: Defines a closed polygon drawn through a sequence of points in a PDF document. The polygon can be both stroked and filled. Coordinates are relative to...
- **`PdfPolylineElement`**: Defines an open polyline drawn through a sequence of points in a PDF document. The polyline is stroked but not filled. Coordinates are relative to the...
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

35 types, 446 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

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
// Set the next page to landscape A4
pdfDocument.SetPageSize(PdfPageSize.A4, PdfPageOrientation.Landscape);

// Set the next page margins
pdfDocument.Margins = new PdfMargins(100, 100, 100, 100);

// Add a new PDF page with the modified page settings
pdfDocument.AddPage();
```

All 9 samples of this topic, complete: `references/examples.md`.

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

All 8 samples of this topic, complete: `references/examples.md`.

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

All 4 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-images.htm

## Create PDF Documents with Shapes

The library exposes a dedicated element for each common geometric primitive. Call sites are short. The rendering pipeline is more efficient than building the same shapes with arbitrary paths.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-shapes.htm

## Create PDF Documents with Polylines, Polygons and Paths

Three vector primitives cover most cases when the visual you need is more complex than a basic rectangle or circle. `PdfPolylineElement` renders an open piecewise-linear path. `PdfPolygonElement` renders a closed polygon described by a vertex list. `PdfPathElement` renders an arbitrary path built from move, line and Bezier curve operations.

### Arbitrary Paths

```csharp
PdfPathElement heart = new PdfPathElement
{
    FillColor = PdfColor.Crimson,
    FillOpacity = 0.8f,
    LineColor = PdfColor.DarkRed,
    LineStyle = new PdfLineStyle { LineWidth = 1.5f, LineJoin = PdfLineJoinStyle.Round }
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

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
