---
name: evopdf-next-document-converters
description: "Convert DOCX, XLSX, RTF and Markdown documents to PDF in .NET with EvoPdf Next: WordToPdfConverter, ExcelToPdfConverter, RtfToPdfConverter, MarkdownToPdfConverter and their document options, the fonts embedded in the document or given as files, the document headers and footers, bookmarks from headings, PDF/UA and PDF/A, attachments and electronic invoices, and adding Word, Excel, RTF and Markdown content to a PdfDocument with AddWord, AddExcel, AddRtf and AddMarkdown."
---

# EvoPdf Next: Word, Excel, RTF and Markdown to PDF

The Word to PDF Converter component allows you to convert DOCX Word documents to PDF.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`WordToPdfConverter`**: Provides functionality for converting Word (.docx) documents to PDF format with support for customization options such as metadata, security and viewe...
- **`WordToPdfDocumentOptions`**: This class encapsulates the options to control the PDF document rendering process. The WordToPdfConverter class defines a reference to an object of th...
- **`ExcelToPdfConverter`**: Provides functionality for converting Excel (.xlsx) documents to PDF format with support for customization options such as metadata, security and view...
- **`ExcelToPdfDocumentOptions`**: This class encapsulates the options to control the PDF document rendering process. The ExcelToPdfConverter class defines a reference to an object of t...
- **`RtfToPdfConverter`**: Provides functionality for converting RTF documents to PDF format with support for customization options such as metadata, security and viewer prefere...
- **`RtfToPdfDocumentOptions`**: This class encapsulates the options to control the PDF document rendering process. The RtfToPdfConverter class defines a reference to an object of thi...
- **`MarkdownToPdfConverter`**: Provides functionality for converting Markdown documents to PDF format with support for customization options such as metadata, security and viewer pr...
- **`MarkdownToPdfDocumentOptions`**: This class encapsulates the options to control the PDF document rendering process. The MarkdownToPdfConverter class defines a reference to an object o...
- **`WordToPdfConversionInfo`**: Information about the result of a Word to PDF conversion: the pages produced and the pages inserted before and after them
- **`PdfWordElement`**: A Word document rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many new p...
- **`WordPdfDocumentExtensions`**: Renders Word documents in the pages of a PdfDocument
- **`ExcelToPdfConversionInfo`**: Information about the result of a Excel to PDF conversion: the pages produced and the pages inserted before and after them
- **`PdfExcelElement`**: An Excel workbook rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many new...
- **`ExcelPdfDocumentExtensions`**: Renders Excel workbooks in the pages of a PdfDocument
- **`RtfToPdfConversionInfo`**: Information about the result of a RTF to PDF conversion: the pages produced and the pages inserted before and after them
- **`PdfRtfElement`**: An RTF document rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many new p...
- **`RtfPdfDocumentExtensions`**: Renders RTF documents in the pages of a PdfDocument
- **`MarkdownToPdfConversionInfo`**: Information about the result of a Markdown to PDF conversion: the pages produced and the pages inserted before and after them
- **`PdfMarkdownElement`**: A Markdown document rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many n...
- **`MarkdownPdfDocumentExtensions`**: Renders Markdown documents in the pages of a PdfDocument
- **`PdfContentRenderInfo`**: Describes where content that flows over one or more pages was rendered in the document: the pages it occupies and the area it covers on each of them

21 types, 305 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

| Task | API |
|---|---|
| Convert a whole document | `WordToPdfConverter`, `ExcelToPdfConverter`, `RtfToPdfConverter`, `MarkdownToPdfConverter` |
| Add a document to a `PdfDocument` built from code, from a position, flowing over pages | `pdfDocument.AddWord(new PdfWordElement(...))`, `AddExcel(PdfExcelElement)`, `AddRtf(PdfRtfElement)`, `AddMarkdown(PdfMarkdownElement)` |
| Fonts of the document | `FontFiles`, `FallbackFontFiles` on each converter; `WordToPdfConverter.UseEmbeddedFonts` for the fonts embedded in the DOCX |
| Header and footer of the Word or RTF document | `WordToPdfDocumentOptions.UseHeaderFooterFromWord`, `RtfToPdfDocumentOptions.UseHeaderFooterFromRtf` |
| Bookmarks from the document headings | `GenerateDocumentOutline` on the Word, RTF and Markdown document options |
| Wide tables and pictures | `FitWideContentToPage` on the document options |
| PDF/UA or PDF/A, attachments, bookmarks, open action, electronic invoice | `PdfStandard`, `AddFileAttachment`, `Bookmarks`, `OpenAction`, `AddElectronicInvoice` on the document options (`converter.PdfDocumentOptions`) |
| Page counts after the conversion | `ConversionInfo` on each converter |

Each converter package (`EvoPdf.Next.WordToPdf.<Platform>` and the others) also brings the extension method for
`PdfDocument`; the full platform packages (`EvoPdf.Next.Windows`, ...) include all of them.

## Convert Word DOCX to PDF

### Create the Word to PDF Converter

```csharp
// Create a new Word to PDF converter instance
WordToPdfConverter wordToPdfConverter = new WordToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
wordToPdfConverter.PdfDocumentOptions.UsePageSettingsFromWord = false;
wordToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
wordToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
wordToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
wordToPdfConverter.PdfDocumentOptions.RightMargin = 20;
wordToPdfConverter.PdfDocumentOptions.TopMargin = 30;
wordToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

All 17 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-word-docx-to-pdf.htm

## Convert Excel XLSX to PDF

The Excel to PDF Converter component allows you to convert XLSX Excel documents to PDF.

### Create the Excel to PDF Converter

```csharp
// Create a new Excel to PDF converter instance
ExcelToPdfConverter excelToPdfConverter = new ExcelToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
excelToPdfConverter.PdfDocumentOptions.UsePageSettingsFromExcel = true;
excelToPdfConverter.PdfDocumentOptions.UseFirstWorksheetPageSettings = true;
```

```csharp
excelToPdfConverter.PdfDocumentOptions.UsePageSettingsFromExcel = false;
excelToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
excelToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
excelToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
excelToPdfConverter.PdfDocumentOptions.RightMargin = 20;
excelToPdfConverter.PdfDocumentOptions.TopMargin = 30;
excelToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

### Convert Only the First Worksheet or All Worksheets

```csharp
excelToPdfConverter.PdfDocumentOptions.ConvertOnlyFirstWorksheet = false;
excelToPdfConverter.PdfDocumentOptions.PageBreakBetweenWorksheets = true;
```

All 19 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-excel-xlsx-to-pdf.htm

## Convert RTF to PDF

The RTF to PDF Converter component allows you to convert RTF documents to PDF.

### Create the RTF to PDF Converter

```csharp
// Create a new RTF to PDF converter instance
RtfToPdfConverter rtfToPdfConverter = new RtfToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
rtfToPdfConverter.PdfDocumentOptions.UsePageSettingsFromRtf = false;
rtfToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
rtfToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
rtfToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
rtfToPdfConverter.PdfDocumentOptions.RightMargin = 20;
rtfToPdfConverter.PdfDocumentOptions.TopMargin = 30;
rtfToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

All 24 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-rtf-to-pdf.htm

## Convert Markdown to PDF

The Markdown to PDF Converter component allows you to convert Markdown documents to PDF.

### Create the Markdown to PDF Converter

```csharp
// Create a new Markdown to PDF converter instance
MarkdownToPdfConverter markdownToPdfConverter = new MarkdownToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
markdownToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
markdownToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
markdownToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
markdownToPdfConverter.PdfDocumentOptions.RightMargin = 20;
markdownToPdfConverter.PdfDocumentOptions.TopMargin = 30;
markdownToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

### Apply a Custom Style Sheet

```csharp
markdownToPdfConverter.PdfDocumentOptions.StyleSheet = @"
h1 {
    font-size: 24pt;
    color: #0b57d0;
}
";
```

```csharp
byte[] inputMarkdownBytes = System.IO.File.ReadAllBytes(markdownFilePath);
string markdownString = Encoding.UTF8.GetString(inputMarkdownBytes);
string baseUrl = "file://" + markdownFilePath;
```

All 14 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-markdown-to-pdf.htm

## Add Word Documents to PDF Documents

A Word document can be rendered in the pages of a PDF document created with the `PdfDocument` class, between the text, images and other elements added from code. The content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the PDF document. The document keeps its structure, headings, links and images.

### Position, Width and Zoom of the Content

```csharp
PdfWordElement wordElement = new PdfWordElement(inputWordBytes)
{
    X = 0,
    Y = crtYPos,
    // 0 extends the content to the right margin of the page
    Width = 0,
    Zoom = 100,
    GenerateDocumentOutline = true
};

PdfContentRenderInfo wordRenderInfo = pdfDocument.AddWord(wordElement);
```

### The Rendering Result

```csharp
// The rendered area reported for the last page places the elements after the content
crtYPos = wordRenderInfo.LastPageRectangle.Bounds.Bottom + 15;

PdfTextElement closingElement = new PdfTextElement("This text was added after the Word content", textFont)
{
    X = 0,
    Y = crtYPos,
    Width = pdfDocument.ContentWidth
};
pdfDocument.AddText(closingElement);
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-word-documents-to-pdf-documents.htm

## Add Excel Workbooks to PDF Documents

An Excel workbook can be rendered in the pages of a PDF document created with the `PdfDocument` class, between the text, images and other elements added from code. The worksheets start at a position in the current page and flow to as many new pages as they need, laid out inside the margins of the PDF document. The worksheets keep the formatting, the images and the shapes of the workbook.

### Position, Width and Zoom of the Content

```csharp
PdfExcelElement excelElement = new PdfExcelElement(inputExcelBytes)
{
    X = 0,
    Y = crtYPos,
    // 0 extends the content to the right margin of the page
    Width = 0,
    Zoom = 100
};

PdfContentRenderInfo excelRenderInfo = pdfDocument.AddExcel(excelElement);
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-excel-workbooks-to-pdf-documents.htm

## Add RTF Documents to PDF Documents

An RTF document can be rendered in the pages of a PDF document created with the `PdfDocument` class, between the text, images and other elements added from code. The content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the PDF document. The document keeps its formatting, headings, links and images.

### The RTF Content

```csharp
// A RTF string is given to the element as it is
PdfRtfElement rtfElement = new PdfRtfElement(rtfString);

// The content of a RTF file
PdfRtfElement rtfFileElement = new PdfRtfElement(inputRtfBytes);
```

### Position, Width and Zoom of the Content

```csharp
rtfElement.X = 0;
rtfElement.Y = crtYPos;
// 0 extends the content to the right margin of the page
rtfElement.Width = 0;
rtfElement.Zoom = 100;
// The headings of the RTF document join the outline of the PDF document
rtfElement.GenerateDocumentOutline = true;

PdfContentRenderInfo rtfRenderInfo = pdfDocument.AddRtf(rtfElement);
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-rtf-documents-to-pdf-documents.htm

## Add Markdown Content to PDF Documents

Markdown content can be rendered in the pages of a PDF document created with the `PdfDocument` class, between the text, images and other elements added from code. The content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the PDF document. The content keeps its structure, headings, links, code blocks and images.

### The Markdown Content

```csharp
// A Markdown string with a base URL for its relative links and images
PdfMarkdownElement markdownElement = new PdfMarkdownElement(markdownString, baseUrl);

// A Markdown file
PdfMarkdownElement markdownFileElement = new PdfMarkdownElement(markdownFilePath);
```

### Position, Width and Zoom of the Content

```csharp
markdownElement.X = 0;
markdownElement.Y = crtYPos;
// 0 extends the content to the right margin of the page
markdownElement.Width = 0;
markdownElement.Zoom = 100;
// The CSS applied to the rendered Markdown content
markdownElement.StyleSheet = styleSheet;
// The headings of the Markdown content join the outline of the PDF document
markdownElement.GenerateDocumentOutline = true;

PdfContentRenderInfo markdownRenderInfo = pdfDocument.AddMarkdown(markdownElement);
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-markdown-content-to-pdf-documents.htm

## Convert Word to PDF with Document Fonts

The Word to PDF Converter renders the text of a document with the fonts installed on the machine, with the fonts stored in the Word document by its author and with the font files you give to the converter. This way the text keeps its look on a server where the fonts of the document are not installed.

### Use the Fonts Stored in the Word Document

```csharp
wordToPdfConverter.UseEmbeddedFonts = true;
```

### Add Font Files

```csharp
wordToPdfConverter.FontFiles.Add(Path.Combine(fontsPath, "DejaVuSerif.ttf"));
```

### Add Fallback Font Files

```csharp
wordToPdfConverter.FallbackFontFiles.Add(Path.Combine(fontsPath, "NotoSansArabic-Regular.ttf"));
```

All 4 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-word-to-pdf-with-document-fonts.htm

## Convert Word to PDF with Document Headers and Footers

The Word to PDF Converter renders the running header and footer of the Word document on the PDF pages, with the page number and page count fields they contain and with a different first page when the document has one. The feature is controlled by the `WordToPdfDocumentOptions.UseHeaderFooterFromWord` property, which is true by default.

### Use the Header and Footer of the Word Document

```csharp
wordToPdfConverter.PdfDocumentOptions.UseHeaderFooterFromWord = true;
```

### Replace the Document Header or Footer

```csharp
wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Html = footerHtml;
wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = footerHtmlBaseUrl;
wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoSizeContentHeight = true;
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-word-to-pdf-with-document-headers-and-footers.htm

## Convert Word to PDF with Outline, Open Action and Attachments

The Word to PDF Converter can create the outline of the generated PDF document from the headings of the Word document, add your own bookmarks to it, set the action executed when the document is opened and attach files to the document. The options are exposed by the `WordToPdfDocumentOptions` object of the `WordToPdfConverter.PdfDocumentOptions` property. The bookmarks, open action and attachment options are also available in the Excel, RTF and Markdown to PDF converters and the outline created from the headings in the RTF and Markdown to PDF converters.

### Create the Outline from the Document Headings

```csharp
wordToPdfConverter.PdfDocumentOptions.GenerateDocumentOutline = true;
```

### Add Custom Bookmarks

```csharp
// Bookmarks added to the outline after the ones created from the headings: a bookmark to
// a page location with a child bookmark and a bookmark opening a web address
PdfBookmark contentBookmark = wordToPdfConverter.PdfDocumentOptions.Bookmarks.Add(
    "Converted Word document", firstContentPage, PdfLinkPageLocation.FitWidth(0));
contentBookmark.Bold = true;
contentBookmark.Color = PdfColor.DarkBlue;
contentBookmark.Children.Add("Top of the first page, zoomed", firstContentPage, PdfLinkPageLocation.AtCoordinates(0, 0, 1.5f));

wordToPdfConverter.PdfDocumentOptions.Bookmarks.AddUrl("Library web site", "https://www.evopdf.com");
```

### Set the Open Action

```csharp
// The action executed when the document is opened: go to the first page of the
// converted content, fit to the page width
wordToPdfConverter.PdfDocumentOptions.OpenAction = new PdfGoToPageAction(firstContentPage, PdfLinkPageLocation.FitWidth(0));
```

### Attach Files to the Document

```csharp
// Attach the source Word document to the generated PDF. The attachment appears in the
// Attachments panel of the PDF viewer
PdfFileAttachment attachment = PdfFileAttachment.FromBytes(inputWordBytes, "Source_Document.docx");
attachment.Description = "The Word document the PDF was generated from";
wordToPdfConverter.PdfDocumentOptions.AddFileAttachment(attachment);

// The panel shown by the viewer when the document is opened
wordToPdfConverter.PdfViewerPreferences.PageMode = ViewerPageMode.UseAttachments;
```

All 5 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-word-to-pdf-with-outline-open-action-and-attachments.htm

## Convert RTF to PDF with Document Headers and Footers

The RTF to PDF Converter renders the running header and footer of the RTF document on the PDF pages, with the page number and page count fields they contain and with a different first page when the document has one. The feature is controlled by the `RtfToPdfDocumentOptions.UseHeaderFooterFromRtf` property, which is true by default.

### Use the Header and Footer of the RTF Document

```csharp
rtfToPdfConverter.PdfDocumentOptions.UseHeaderFooterFromRtf = true;
```

### Replace the Document Header or Footer

```csharp
rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Html = footerHtml;
rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = footerHtmlBaseUrl;
rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoSizeContentHeight = true;
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-rtf-to-pdf-with-document-headers-and-footers.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
