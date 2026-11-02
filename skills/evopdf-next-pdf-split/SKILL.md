---
name: evopdf-next-pdf-split
description: "Extract and split pages of existing PDF documents in .NET with EvoPdf Next PdfSplit: one page, a range or page ranges such as 1,3-5,8-, split by a number of pages or by page ranges, remove pages, password protected sources, document information and security of the extracted documents."
---

# EvoPdf Next: splitting PDF documents

The `PdfSplit` class extracts pages from an existing PDF document, removes pages from it and splits it in several documents, by a number of pages or by page ranges. The extracted pages keep their content, links, bookmarks, form fields and annotations. The document is loaded from a file or from memory, with the password of a protected document and `PdfSplit.PageCount` gives its number of pages.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfSplit`**: Extracts pages from a PDF document into new PDF documents: a range of pages, the pages selected by page ranges, the document split in parts of a given...

1 types, 14 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Choosing the method

| Task | Method | Result |
|---|---|---|
| One page | `ExtractPage(pageNumber)` | `byte[]` |
| A range of pages | `ExtractPages(firstPage, lastPage)`, `ExtractPagesToFile(...)` | `byte[]` or a file |
| Pages selected by page ranges, in the order of the ranges | `ExtractPages("1,3-5,8-")` | `byte[]` |
| Everything except some pages | `RemovePages("2,4-")` | `byte[]` |
| Parts of a fixed number of pages | `SplitByPageCount(pagesPerPart)`, `SplitByPageCountToFiles(...)` | one document per part |
| One part for each page ranges text | `SplitByPageRanges(...)` | one document per part |

Page numbers are 1-based. A page ranges text is a comma separated list of page numbers and ranges; a range
without an end, such as `8-`, goes to the last page. `PdfDocumentInfo`, `PdfSecurityOptions` and
`PdfViewerPreferences` apply to every extracted document; the values of the loaded document are used when
they are not set. `PdfSplit` is disposable: create it in a `using` statement. To insert, move, rotate or
remove pages in place, use `PdfEditor` (`evopdf-next-pdf-edit`); to join documents, `PdfMerge`
(`evopdf-next-pdf-merge`).

## Split PDF Documents

### Extract and Remove Pages

```csharp
using PdfSplit pdfSplit = new PdfSplit(inputPdfBytes, null);

// One page, a range of pages and the pages of several ranges
byte[] pagePdf = pdfSplit.ExtractPage(2);
byte[] pagesPdf = pdfSplit.ExtractPages(2, 4);
byte[] rangesPdf = pdfSplit.ExtractPages("1,3-4,6-");

// The loaded document without some pages
byte[] remainingPdf = pdfSplit.RemovePages("2,5");
```

### Split in Several Documents

```csharp
// Two pages per document
List<byte[]> parts = pdfSplit.SplitByPageCount(2);

// One document for each range
List<byte[]> chapters = pdfSplit.SplitByPageRanges(new[] { "1-2", "3-5", "6-" });
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/split-pdf-documents.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
