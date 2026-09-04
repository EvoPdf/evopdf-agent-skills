---
name: evopdf-next-document-converters
description: "Convert Word DOCX, Excel XLSX, RTF and Markdown documents to PDF in .NET with EvoPdf Next (WordToPdfConverter, ExcelToPdfConverter, RtfToPdfConverter, MarkdownToPdfConverter). Use for Office/RTF/Markdown to PDF tasks in C#."
---

# EvoPdf Next — Word, Excel, RTF and Markdown to PDF

Packages: `EvoPdf.Next.WordToPdf.*`, `EvoPdf.Next.ExcelToPdf.*`, `EvoPdf.Next.RtfToPdf.*`, `EvoPdf.Next.MarkdownToPdf.*` (`.Windows` / `.Linux` / `.MacOS`). Namespace `EvoPdf.Next`. No Microsoft Office required.

```csharp
using EvoPdf.Next;

var word = new WordToPdfConverter();
word.PdfDocumentOptions.GenerateTableOfContents = true;
byte[] pdf = word.ConvertToPdf(docxBytes);          // or ConvertToPdf(string path), ConvertToPdfFile(…)

var excel = new ExcelToPdfConverter();
excel.PdfDocumentOptions.UsePageSettingsFromExcel = true;   // ExcelToPdfDocumentOptions: also PageBreakBetweenWorksheets, ConvertOnlyFirstWorksheet, Zoom
pdf = excel.ConvertToPdf(xlsxBytes);

var rtf = new RtfToPdfConverter();
pdf = rtf.ConvertStringToPdf(rtfString);            // also ConvertToPdf(bytes), ConvertFileToPdf, ConvertStreamToPdf

var md = new MarkdownToPdfConverter();
pdf = md.ConvertStringToPdf(markdown, baseUrl);     // baseUrl resolves relative images and links; also ConvertFileToPdf
```
- Every converter has `…Async` methods and `…ToPdfFile` variants that write directly to disk.
- Converter instances are not reusable: create a new one per document.
- All share the `PdfDocumentOptions` model: page size/orientation/margins, headers and footers (`PdfHtmlHeader` / `PdfHtmlFooter`), table of contents, `PdfStandard`, security and stamps — see the headers-footers and pdf-standards skills.
- Excel: `UsePageSettingsFromExcel = true` keeps the workbook's page setup; `false` reflows into `PdfPageSize` / orientation / margins you set.
- Word: keeps the document's page settings by default; set your own on `PdfDocumentOptions` to reflow.
