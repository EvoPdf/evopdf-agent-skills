# Migrating from EvoPdf Classic: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Migrate HTML to PDF Page Setup and Headers from EvoPdf Classic

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/migrate-html-to-pdf-from-classic.htm

### The Classic Default: 1024 Pixel Layout Fitted to an A4 Page

```csharp
using EvoPdf;

HtmlToPdfConverter converter = new HtmlToPdfConverter();
converter.LicenseKey = "...";

converter.HtmlViewerWidth = 1024;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.PdfDocumentOptions.FitWidth = true;

byte[] pdf = converter.ConvertUrl(url);
```

```csharp
using EvoPdf.Next;

Licensing.LicenseKey = "...";
HtmlToPdfConverter converter = new HtmlToPdfConverter();

byte[] pdf = converter.ConvertUrl(url);

// The same settings made explicitly
converter.FitBrowserWindowToPage(PdfPageSize.A4);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.HtmlViewerZoom = 77.47;
converter.HtmlViewerWidth = 1024;
```

### A Page with a Fixed Width Larger Than the Viewer

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// Same as Classic: A4 page, a 1600 pixel window scaled to it, drawn at 49.58 percent
converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 1600);

// Or a 1200 point wide page with the content at 1:1
converter.PageWidthFromBrowserWindow(1600);
```

### AutoSizePdfPage and SinglePage

```csharp
// Classic
converter.PdfDocumentOptions.FitWidth = false;
converter.PdfDocumentOptions.AutoSizePdfPage = true;
converter.PdfDocumentOptions.SinglePage = true;

// Next
converter.PageWidthFromBrowserWindow(1024, singlePage: true);
```

### StretchToFit

```csharp
// Classic
converter.PdfDocumentOptions.FitWidth = true;
converter.PdfDocumentOptions.StretchToFit = true;

// Next: a 600 pixel window enlarged to the A4 page, drawn at 132 percent
converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 600);
```

### A Header with HTML and Page Numbers

```csharp
using EvoPdf;
using System.Drawing;

converter.PdfDocumentOptions.ShowHeader = true;
converter.PdfHeaderOptions.HeaderHeight = 60;
converter.PdfHeaderOptions.HeaderBackColor = Color.White;

HtmlToPdfElement headerHtml = new HtmlToPdfElement(headerHtmlUrl);
headerHtml.FitHeight = true;
converter.PdfHeaderOptions.AddElement(headerHtml);

float headerWidth = converter.PdfDocumentOptions.PdfPageSize.Width -
    converter.PdfDocumentOptions.LeftMargin - converter.PdfDocumentOptions.RightMargin;
LineElement headerLine = new LineElement(0, 59, headerWidth, 59);
headerLine.ForeColor = Color.Gray;
converter.PdfHeaderOptions.AddElement(headerLine);

converter.PdfDocumentOptions.ShowFooter = true;
converter.PdfFooterOptions.FooterHeight = 40;
TextElement footerText = new TextElement(0, 15, "Page &p; of &P;",
    new Font(new FontFamily("Times New Roman"), 10, GraphicsUnit.Point));
footerText.TextAlign = HorizontalTextAlign.Right;
converter.PdfFooterOptions.AddElement(footerText);
```

```csharp
using EvoPdf.Next;

PdfHtmlHeaderFooter header = converter.PdfDocumentOptions.PdfHtmlHeader;
header.HtmlSourceUrl = headerHtmlUrl;
header.Height = 60;
header.FitHeight = true;

PdfHtmlHeaderFooter footer = converter.PdfDocumentOptions.PdfHtmlFooter;
footer.Html = "<div style=\"font-family: 'Times New Roman'; font-size: 10pt; text-align: right\">" +
    "Page {page_number} of {total_pages}</div>";
footer.HtmlBaseUrl = baseUrl;
footer.Height = 40;
```

```html
<!DOCTYPE html>
<html>
<body style="margin: 0; background: white; border-bottom: 1px solid gray; font-family: Arial; font-size: 12pt">
    <div style="padding: 8px">Quarterly report</div>
</body>
</html>
```

### Header Hidden on the First Page

```csharp
converter.PrepareRenderPdfPageEvent += (eventParams) =>
{
    if (eventParams.PageNumber == 1)
        eventParams.Page.ShowHeader = false;
};
```

```csharp
converter.PdfDocumentOptions.PdfHtmlHeader.ShowInFirstPage = false;

// Give the header space back to the content on the first page
converter.PdfDocumentOptions.PdfHtmlHeader.ReserveSpaceAlways = false;
```

```csharp
converter.PdfDocumentOptions.PdfHtmlHeader.OnPageRendering = (placement) =>
{
    return placement.DocumentPageNumber >= 3 && placement.DocumentPageNumber <= 10;
};
```

### A Different Header on the First Page

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// A fixed page size, so that the page width is known for the template below
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;

// The document header, hidden on the first page, with its space kept on every page
PdfHtmlHeaderFooter header = converter.PdfDocumentOptions.PdfHtmlHeader;
header.HtmlSourceUrl = headerHtmlUrl;
header.Height = 60;
header.ShowInFirstPage = false;
header.ReserveSpaceAlways = true;

// The alternative header: an HTML template of the same height, drawn at the top of the first page only
int pageWidth = converter.PdfDocumentOptions.PdfPageSize.Width;
PdfHtmlTemplate firstPageHeader = converter.PdfDocumentOptions.AddHtmlTemplate(0, 0, pageWidth, 60,
    firstPageHeaderHtml, baseUrl);
firstPageHeader.ShowInFirstPage = true;
firstPageHeader.ShowInOddPages = false;
firstPageHeader.ShowInEvenPages = false;

byte[] pdf = converter.ConvertUrl(url);
```

### A Different Header and Height for Each Section of a Document

```csharp
using PdfMerge pdfMerge = new PdfMerge();

// Section 1: a tall header with the report title
HtmlToPdfConverter section1 = new HtmlToPdfConverter();
section1.PdfDocumentOptions.PdfHtmlHeader.Html = titleHeaderHtml;
section1.PdfDocumentOptions.PdfHtmlHeader.HtmlBaseUrl = baseUrl;
section1.PdfDocumentOptions.PdfHtmlHeader.Height = 120;
section1.PdfDocumentOptions.PdfHtmlFooter.Html = "<div>Page {page_number}</div>";
section1.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = baseUrl;
int section1Pages = pdfMerge.AddPdf(section1.ConvertHtml(section1Html, baseUrl));

// Section 2: a one line header, page numbers continue after section 1
HtmlToPdfConverter section2 = new HtmlToPdfConverter();
section2.PdfDocumentOptions.PdfHtmlHeader.Html = lineHeaderHtml;
section2.PdfDocumentOptions.PdfHtmlHeader.HtmlBaseUrl = baseUrl;
section2.PdfDocumentOptions.PdfHtmlHeader.Height = 30;
section2.PdfDocumentOptions.PdfHtmlFooter.Html = "<div>Page {page_number}</div>";
section2.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = baseUrl;
section2.PdfDocumentOptions.PdfHtmlFooter.PageNumberOffset = section1Pages;
pdfMerge.AddPdf(section2.ConvertHtml(section2Html, baseUrl));

byte[] pdf = pdfMerge.Save();
```
