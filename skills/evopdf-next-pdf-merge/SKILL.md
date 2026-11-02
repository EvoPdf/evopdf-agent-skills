---
name: evopdf-next-pdf-merge
description: "Combine PDF documents in .NET with EvoPdf Next PdfMerge: whole documents or selected page ranges, blank and image pages, the form fields, bookmarks, annotations and internal links kept, new bookmarks and open action, footers on every page, security and signature of the merged PDF, several HTML documents rendered in one PDF with PdfDocument.AddHtml, and AddStartPdf and AddEndPdf to wrap a generated PDF with existing documents."
---

# EvoPdf Next: merging and combining PDFs

The `PdfMerge` class merges PDF documents, or selected pages of them, blank pages and images into a single PDF document. The form fields, bookmarks, annotations, internal links and scripts of the merged documents are kept. Several HTML documents can also be rendered one after the other in a single PDF with `PdfDocument.AddHtml`, without converting each of them to a separate PDF first.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfMerge`**: Merges PDF documents, or selected pages of them, blank pages and images into a single PDF document. The form fields, bookmarks, annotations, internal ...
- **`PdfMergeInfo`**: An object of this class is exposed by the PdfMerge class to provide details such as the total number of pages produced and the number of pages from ea...

2 types, 39 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

| Task | Member |
|---|---|
| Add a whole document, from memory or a file, with its password | `AddPdf` |
| Add selected pages, by range or by page ranges such as `"1,3-5,8-"` | `AddPdfPages` |
| Add a blank page or a page with an image | `AddBlankPage`, `AddImagePage` |
| Keep the form fields of the merged documents apart when their names repeat | `KeepFieldsSeparate = true` |
| Keep or drop the bookmarks of the merged documents; add new ones | `KeepSourceBookmarks`, `Bookmarks` |
| Open action, document information, security, viewer preferences, signature | `OpenAction`, `PdfDocumentInfo`, `PdfSecurityOptions`, `PdfViewerPreferences`, `DigitalSignature` |
| Footer or stamp on every page | `AddHtmlTemplate` |
| Static form fields | `Flatten = true` |

The add methods return the 1-based number of the first added page in the merged document, where a bookmark or the
open action can point; `PageCount` gives the number of pages of the merged document.
To extract or split pages, use `PdfSplit` (`evopdf-next-pdf-split`).

## Merge Multiple HTML to PDF

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/merge-multiple-html-to-pdf.htm

- Merge Multiple HTML to PDF: https://www.evopdf.com/help/evopdf-next-dotnet/html/merge-multiple-html-to-pdf.htm

## Merge PDF Documents

The `PdfMerge` class merges PDF documents, or selected pages of them, blank pages and images into a single PDF document. The form fields, bookmarks, annotations, internal links and scripts of the merged documents are kept. The merged documents can be password protected, the merged document can have its own document information, security options, viewer preferences, bookmarks, open action and digital signature and HTML templates can be drawn in all its pages, for example a footer with the page numbers.

### Add Documents and Pages

```csharp
using PdfMerge pdfMerge = new PdfMerge();

// The add methods return the number of the first added page in the merged document
int firstDocumentPage = pdfMerge.AddPdf(firstPdfBytes);

// A blank page between the documents
int blankPage = pdfMerge.AddBlankPage(PdfPageSize.A4, PdfPageOrientation.Portrait);

// Selected pages of the second document, in the order of the ranges
int secondDocumentPage = pdfMerge.AddPdfPages(secondPdfBytes, "1,3-", null);

byte[] mergedPdf = pdfMerge.Save();
```

### Form Fields and Bookmarks

```csharp
pdfMerge.KeepFieldsSeparate = true;
pdfMerge.KeepSourceBookmarks = true;
pdfMerge.Bookmarks.Add("First document", firstDocumentPage, PdfLinkPageLocation.FitPage());
pdfMerge.Bookmarks.Add("Second document", secondDocumentPage, PdfLinkPageLocation.FitPage());
```

### Properties of the Merged Document

```csharp
pdfMerge.PdfDocumentInfo.Title = "Merged Document";
pdfMerge.OpenAction = new PdfGoToPageAction(secondDocumentPage, PdfLinkPageLocation.FitPage());

PdfHtmlTemplate footer = pdfMerge.AddHtmlTemplate(0, 0, 300, 30,
    "<div style=\"font-family: Arial; font-size: 10px;\">Page {page_number} of {total_pages}</div>", null);
footer.HorizontalAlign = PdfTemplateHorizontalAlign.Center;
footer.VerticalAlign = PdfTemplateVerticalAlign.Bottom;
```

All 4 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/merge-pdf-documents.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
