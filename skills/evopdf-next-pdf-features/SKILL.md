---
name: evopdf-next-pdf-features
description: "PDF features of EvoPdf Next beyond plain conversion: fillable PDF forms from HTML forms, document outline (bookmarks), table of contents, internal and external links, stamps and watermarks, merging PDFs, editing existing PDFs with PdfEditor, security and digital signatures. Use when a .NET task needs one of these PDF features."
---

# EvoPdf Next — forms, bookmarks, TOC, links, stamps, merge, edit

`o` = `converter.PdfDocumentOptions` on any converter (HTML, Word, Excel, RTF, Markdown).

## Fillable PDF forms from HTML forms
```csharp
o.GeneratePdfFormFields = true;
o.PdfFormOptions.MapHtmlSubmitButtonsToPdfActions = true;   // submit/reset buttons keep working
o.PdfFormOptions.FieldNamingMode = …;                        // how field names are derived
o.PdfFormOptions.ApplyRequiredAsPdfRequiredFlag = true;
```
Other options: `SubmitBehavior`, `SubmitFormat`, `IncludeIFrameFields`, `ExcludeDisabledFieldsFromSubmit`, `FormFieldFont`/`…BoldFont`. Inputs, selects, textareas, checkboxes, radios and buttons in the HTML become PDF fields.

## Bookmarks (document outline)
```csharp
o.GenerateDocumentOutline = true;   // from the HTML headings hierarchy
o.UseBrowserOutlineMode = true;     // let Chromium build the outline
```

## Table of contents
```csharp
o.GenerateTableOfContents = true;
o.TableOfContents.Title = "Contents";
o.TableOfContents.ShowPageNumbers = true;
o.TableOfContents.Style = "…css…";        // CSS for the TOC entries
o.TableOfContents.CreateInline = false;    // true: at the position marked in the HTML
o.TableOfContents.PageNumbersOffset = 0;   // CountTocPages / CountStartPages control numbering
```

## Links
External links (`<a href="https://…">`) and internal anchors (`<a href="#section">`) in the HTML become PDF links automatically. Disable by removing/neutralising the anchors before conversion.

## Stamps, watermarks, text and images on pages
On a generated or existing PDF, through `PdfEditor`:
```csharp
using var editor = new PdfEditor(pdfBytes, password: null);   // or (path, password)
int pages = editor.GetPageCount();
PdfHtmlTemplate stamp = editor.AddHtmlTemplate(x, y, width, "<div style='opacity:.3;font:48px Arial'>DRAFT</div>", htmlBaseUrl); // repeated on the pages; overloads: (x, y, width, height, html, baseUrl), (x, y, width, htmlSourceUrl), alignment variants
stamp.ShowInFirstPage = true;                          // ShowInOddPages / ShowInEvenPages select the pages
editor.AddText(pageNumber, new PdfTextElement(…));     // page-based: AddText, AddImage, AddLine, AddRectangle, AddPath, annotations
editor.AddImage(pageNumber, new PdfImageElement(…));
byte[] result = editor.Save();                        // or SaveToFile(path)
```
`AddPage`, `AddLine`, `AddRectangle`, `AddPath`, `AddLinkAnnotation`, `AddTextAnnotation`, `AddFileAttachment` follow the same pattern. The same methods exist on `PdfDocument` for documents created from scratch.

## Merge PDFs
`editor.AddPdfTemplate(x, y, width, height, …)` places another PDF's pages as a template; for appending whole documents, add pages to a `PdfDocument`/`PdfEditor` and draw each source page with `AddPdfTemplate`, or convert several HTML documents into one PDF by concatenating the HTML.

## Security and signatures
`converter.PdfSecurityOptions` (open/permissions passwords, print/copy/edit flags); `converter.DigitalSignature`; `PdfEditor` constructors accept a `PdfDigitalSignature` to sign an existing PDF.

## PDF/UA and PDF/A
See the `evopdf-next-pdf-standards` skill (`o.PdfStandard`).

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/
