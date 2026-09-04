---
name: evopdf-next-core-pdf-api
description: "Create PDF documents from scratch and edit existing ones in .NET with the EvoPdf Next Core PDF API: PdfDocument, PdfEditor, text, images, shapes, HTML templates, attachments, annotations, links, merge with AddPdfTemplate, PDF/UA and PDF/A settings. Use for PDF creation, editing, stamping and merging tasks in C#; for text extraction, search, page images or image extraction use the evopdf-next-pdf-processor skill."
---

# EvoPdf Next — Core PDF API (create and edit)

Package: `EvoPdf.Next.Core.Windows` / `.Linux` / `.MacOS` (or the all-components package). Namespace `EvoPdf.Next`. Text extraction, search, page images and image extraction are a different component — see `evopdf-next-pdf-processor`.

## Create a PDF
```csharp
using EvoPdf.Next;

using var pdf = new PdfDocument(new PdfDocumentCreateSettings {
    PageSize = PdfPageSize.A4, Margins = new PdfMargins(36, 36, 36, 36) });
var font = PdfFontManager.CreateStandardFont(PdfStandardFont.Helvetica, 16f, PdfFontStyle.Bold, PdfColor.Black);
pdf.AddText(new PdfTextElement("Hello", font));
pdf.AddImage(…); pdf.AddLine(…); pdf.AddRectangle(…); pdf.AddHtmlTemplate(…); pdf.AddFileAttachment(…);
pdf.SaveToFile("created.pdf");            // or byte[] Save()
```
`PdfEditor` opens an existing PDF and exposes the same `Add…` methods plus `GetPageCount()` / `GetPdfPageInfo()` — stamp, merge (`AddPdfTemplate`), annotate, attach, sign, encrypt.

## Merge and stamp (PdfEditor)
See `evopdf-next-pdf-features` for `AddHtmlTemplate`, `AddPdfTemplate`, security and signatures on existing documents.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents.htm · Runnable: `quickstarts/Samples/PdfEditor.Stamp.cs` in https://github.com/EvoPdf/evopdf-next-samples
