---
name: evopdf-next-pdf-merge
description: "Combine PDF documents in .NET with EvoPdf Next: the PdfMerge class which preserves bookmarks, annotations and internal links, AddStartPdf and AddEndPdf to wrap a generated PDF with existing documents, and AddPdfTemplate to draw one PDF onto another for stamping."
---

# EvoPdf Next: merging and combining PDFs

EvoPdf Next can convert multiple HTML documents to a single PDF using the `PdfMerge` class. Each HTML is converted to PDF and the result is added to PdfMerge using the `PdfMerge.AddPdf` method. The PdfMerge class allows you to set security options, configure PDF viewer preferences and apply a digital signature to the resulting PDF.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfMerge`**: The PdfMerge class provides functionality to merge multiple PDF files or streams into a single PDF document. It supports adding password protected PDF...
- **`PdfMergeInfo`**: An object of this class is exposed by the PdfMerge class to provide details such as the total number of pages produced and the number of pages from ea...

2 types, 28 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Merge Multiple HTML to PDF

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/merge-multiple-html-to-pdf.htm

- Merge Multiple HTML to PDF: https://www.evopdf.com/help/evopdf-next-dotnet/html/merge-multiple-html-to-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
