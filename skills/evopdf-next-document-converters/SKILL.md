---
name: evopdf-next-document-converters
description: "Convert DOCX, XLSX, RTF and Markdown documents to PDF in .NET with EvoPdf Next: WordToPdfConverter, ExcelToPdfConverter, RtfToPdfConverter, MarkdownToPdfConverter and their document options."
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

8 types, 185 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

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

All 3 samples of this topic, complete: `references/examples.md`.

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

All 5 samples of this topic, complete: `references/examples.md`.

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
rtfToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
rtfToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
rtfToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
rtfToPdfConverter.PdfDocumentOptions.RightMargin = 20;
rtfToPdfConverter.PdfDocumentOptions.TopMargin = 30;
rtfToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

All 3 samples of this topic, complete: `references/examples.md`.

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

All 6 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-markdown-to-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
