---
name: evopdf-next-page-setup
description: "Choose the PDF page size and how the HTML is laid out and scaled on it with the EvoPdf Next layout methods: FitBrowserWindowToPage (a new converter's layout, A4 with a 1024 pixel window), LayoutAtPageWidth, PrintLikeChrome, SinglePageOfWidth and PageWidthFromBrowserWindow; the zoom and the browser window width, the window that follows content wider than the page and the zoom drawn, one page for the whole content, margins, @page rules, page breaks, media type and repeated table headers."
---

# EvoPdf Next: page setup and scaling

The layout methods of the `HtmlToPdfConverter` class decide three things: the size of the PDF page, the width at which the HTML is laid out and the scale at which the layout is drawn on the page. Each method covers one kind of document; you call it, convert, and the converter computes the layout width and the scale from the page size, the orientation and the margins in use when the PDF is generated.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PageLayoutMethod`**: The layout method that sets how the HTML is laid out on the PDF page
- **`PdfPageSize`**: This class represents a PDF page size
- **`PdfPageOrientation`**: This enumeration represents the possible orientations of the PDF pages of a PDF document
- **`PdfMargins`**: The PDF document margins class used by PDF pages

4 types, 45 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## HTML to PDF Page Setup and Scaling

### FitBrowserWindowToPage

```csharp
// A4 portrait, the desktop layout of the page scaled to the page width: the settings of a new converter
converter.FitBrowserWindowToPage(PdfPageSize.A4);

// Letter landscape, a 1280 pixel window
converter.FitBrowserWindowToPage(PdfPageSize.Letter, PdfPageOrientation.Landscape, 1280);
```

### LayoutAtPageWidth

```csharp
// An invoice template designed for A4
converter.LayoutAtPageWidth(PdfPageSize.A4);

// A template whose @page rule sets the paper size
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Portrait, preferCssPageSize: true);

// A template with a print style sheet
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Portrait, mediaType: "print");

// A web page laid out at the width of a landscape page
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Landscape);
```

### PrintLikeChrome

```csharp
// The same as converter.PrintLikeChrome(PdfPageSize.Letter)
converter.PdfDocumentOptions.LeftMargin = 28;
converter.PdfDocumentOptions.RightMargin = 28;
converter.PdfDocumentOptions.TopMargin = 28;
converter.PdfDocumentOptions.BottomMargin = 28;
converter.PdfDocumentOptions.PrintBackgrounds = false;
converter.LayoutAtPageWidth(PdfPageSize.Letter, PdfPageOrientation.Portrait, false, "print");
```

### SinglePageOfWidth

```csharp
// A receipt for 80 mm paper, 5 mm margins
converter.SinglePageOfWidth(227, 14);
```

### PageWidthFromBrowserWindow

```csharp
// A 768 point wide page (1024 pixels), the content drawn 1:1, paginated at the A4 height
converter.PageWidthFromBrowserWindow();

// The whole page on one PDF page
converter.PageWidthFromBrowserWindow(1024, singlePage: true);
```

### One PDF Page for the Whole Content

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// The page width follows the viewer width, the page height follows the content
converter.PageWidthFromBrowserWindow(1024, singlePage: true);

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.HtmlViewerWidth = 1024;
converter.PdfDocumentOptions.AutoResizePdfPageWidth = true;
converter.PdfDocumentOptions.AutoResizePdfPageHeight = true;
```

### A Web Page on A4 or Letter with Its Desktop Layout

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// A4 portrait page, the 1024 pixel browser window scaled to it
converter.FitBrowserWindowToPage(PdfPageSize.A4);

byte[] pdf = converter.ConvertUrl("https://www.evopdf.com");

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Portrait;
// Lay the page out at 793 / 0.7747 = 1024 pixels and draw it at 77.47 percent
converter.HtmlViewerZoom = 77.47;
converter.HtmlViewerWidth = 1024;
```

### An HTML Template Designed for the Paper Size

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// Margins in points; a @page rule in the template overrides them
converter.PdfDocumentOptions.LeftMargin = 36;
converter.PdfDocumentOptions.RightMargin = 36;
converter.PdfDocumentOptions.TopMargin = 36;
converter.PdfDocumentOptions.BottomMargin = 36;

// The exact page size, the template laid out at the page width with its print style sheet;
// preferCssPageSize lets a @page { size } rule in the template decide the page size
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Portrait, preferCssPageSize: true, mediaType: "print");

byte[] pdf = converter.ConvertHtml(invoiceHtml, baseUrl);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.AutoResizePdfPageHeight = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.MediaType = "print";
// Viewer width equal to the content width: (595 - 72) * 4 / 3 = 697 pixels
converter.HtmlViewerWidth = 697;
converter.PdfDocumentOptions.PreferCssPageSize = true;
```

### An HTML Page Converted at Its Exact Pixel Size

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// The page width follows a 1000 pixel window: 1000 * 0.75 = 750 points
converter.PageWidthFromBrowserWindow(1000);

// A4 height for each page; PageWidthFromBrowserWindow(1000, singlePage: true) gives one page instead
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = true;
converter.HtmlViewerWidth = 1000;
```

### A Wide Table on a Landscape Page

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// A4 landscape, the HTML laid out at its 1123 pixel content width
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Landscape);

// A table built for 1280 pixels: a 1280 pixel window scaled to the page, zoom 87.73
// converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Landscape, 1280);

byte[] pdf = converter.ConvertUrl(url);
```

### The Same Output as Save as PDF in Chrome

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

converter.PrintLikeChrome(PdfPageSize.A4);

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
// Chrome prints with 1 cm margins, print media type and no backgrounds
converter.PdfDocumentOptions.LeftMargin = 28;
converter.PdfDocumentOptions.RightMargin = 28;
converter.PdfDocumentOptions.TopMargin = 28;
converter.PdfDocumentOptions.BottomMargin = 28;
converter.MediaType = "print";
converter.PdfDocumentOptions.PrintBackgrounds = false;
// Viewer width equal to the content width: (595 - 56) * 4 / 3 = 719 pixels
converter.HtmlViewerWidth = 719;
```

### A Page in Its Mobile Layout

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// A phone window of 412 pixels enlarged to an A4 page
converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 412);

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
// Lay the page out at 793 / 1.9256 = 412 pixels and draw it at 192.56 percent
converter.HtmlViewerZoom = 192.56;
converter.HtmlViewerWidth = 412;
```

### A Receipt on One Page of Fixed Width

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// One page 227 points (80 mm) wide with 14 point (5 mm) margins, as tall as the content
converter.SinglePageOfWidth(227, 14);

byte[] pdf = converter.ConvertHtml(receiptHtml, baseUrl);

// The same settings made one by one: 5 mm margins are 14 points
converter.PdfDocumentOptions.LeftMargin = 14;
converter.PdfDocumentOptions.RightMargin = 14;
converter.PdfDocumentOptions.TopMargin = 14;
converter.PdfDocumentOptions.BottomMargin = 14;
converter.PdfDocumentOptions.PdfPageSize = new PdfPageSize(227, 227);
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.AutoResizePdfPageHeight = true;
// the content width in pixels: (227 - 2 * 14) * 4 / 3, rounded down
converter.HtmlViewerWidth = 265;
converter.HtmlViewerZoom = 100;
```

### Options Set After a Layout Method

```csharp
// A4 landscape in the default layout, with margins set after the call:
// the zoom is computed for the landscape page and the margins
converter.FitBrowserWindowToPage(PdfPageSize.A4);
converter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
converter.PdfDocumentOptions.LeftMargin = 36;
converter.PdfDocumentOptions.RightMargin = 36;

// Setting the zoom ends the automatic layout: the page is laid out for zoom 90, whatever its size
converter.HtmlViewerZoom = 90;
```

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm

## Insert Page Breaks in PDF Using CSS in HTML

You can insert page breaks in PDF using the page-break-before:always and page-break-after:always CSS attributes in HTML elements styles. The page-break-before:always attribute will force a page break right before the position in PDF page where the element would be normally rendered and the page-break-after:always will force a page break right after the position in PDF page where the element would be normally rendered.

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/insert-page-breaks-in-pdf-using-css.htm

## Avoid Page Breaks Inside HTML Elements Using CSS

You can avoid page breaks inside the HTML elements rendered to PDF using the page-break-inside:avoid CSS attribute in HTML elements styles. If a HTML element has this CSS attribute and the HTML element would be normally rendered on two PDF pages then converter will try to move the rendering of the element to the beginning of the next PDF page.

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/avoid-page-break-inside-elements-uising-css.htm

## Select Media Type for Screen or Print

In the HTML document you can define different styles for different media types using @media rules. For example you can have a style for screen with background colors and images and a style for printing without background colors or images to save ink. EVO HTML to PDF Converter allows you to select the media type for which you want to render the HTML document using the `HtmlToPdfConverter.MediaType` property.

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-media-type-for-screen-or-print.htm

## Repeat HTML Table Header and Footer in PDF Pages

The EvoPdf Next HTML to PDF Converter provides the `PdfDocumentOptions.RepeatTableHeaderFooter` property, which enables the automatic repetition of HTML table header (thead) and footer (tfoot) sections on each page when converting HTML to PDF. This is particularly useful for long tables that span multiple pages, ensuring that headers and footers remain visible throughout the document. The default value is false. An object of type `PdfDocumentOptions` is accessible through the `HtmlToPdfConverter.PdfDocumentOptions` property.

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/repeat-html-table-header-and-footer-in-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
