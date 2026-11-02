---
name: evopdf-next-pdf-standards
description: "Produce standards-compliant PDF with EvoPdf Next: all PdfStandard values including PDF/UA-1, PDF/UA-2, Well-Tagged PDF (WTPDF 1.0), PDF/A-2, PDF/A-3, PDF/A-4 and the combined profiles, the document structure taken from the HTML, and per-element tagging with structure roles, alternate text, actual text, language and role maps."
---

# EvoPdf Next: PDF/UA and PDF/A compliance

EVO HTML to PDF Converter can be configured to automatically generate PDF documents compliant with accessibility and archival standards by setting the `PdfDocumentOptions.PdfStandard` property. An object of `PdfDocumentOptions` type is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfStandard`**: Specifies the PDF standards compliance level for the generated PDF document
- **`PdfAccessibilityProperties`**: Accessibility metadata controlling how an element is inserted into the structure tree of a tagged PDF
- **`PdfStructureType`**: Identifies the PDF logical structure role attached to a content element when the document is tagged

3 types, 49 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## PDF/UA and PDF/A from HTML

Set the standard on the converter before the conversion; the rest of the code does not change.
The document title comes from the HTML `title` element when `PdfDocumentInfo.Title` is not set,
and `PdfDocumentInfo.Language` is written as the document language.

```csharp
var converter = new HtmlToPdfConverter();
converter.PdfDocumentOptions.PdfStandard = PdfStandard.PdfUa2;   // PdfUa1, PdfA2b, PdfUa1PdfA2b, PdfUa2PdfA4, ...
converter.PdfDocumentInfo.Language = "en-US";
byte[] pdf = converter.ConvertUrl(url);
```

`PdfStandard.None`, the default, produces a plain PDF without a structure tree. With a tagged
standard selected, the structure of the document comes from the HTML: the headings, paragraphs,
lists, tables with their header cells, links and the alternate text of the images carry over to the
PDF, together with the document language and title. Write the HTML with that structure (`th` cells,
`alt` attributes, a `lang` attribute) to get a complete accessible document.

`PdfStandard.Wtpdf1`, `Wtpdf1PdfA4` and `Wtpdf1PdfA4f` produce Well-Tagged PDF 1.0 documents, also identified as
PDF/UA-2 and written as PDF 2.0. `PdfA4f`, `PdfUa2PdfA4f` and `Wtpdf1PdfA4f` require at least one embedded file.
Electronic invoices (Factur-X, ZUGFeRD) are PDF/A-3 documents: see the `evopdf-next-electronic-invoices` skill.

## Create PDF/UA and PDF/A Compliant Documents

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdfua-and-pdfa-documents.htm

## Create PDF/UA and PDF/A Documents

A PDF created from scratch with `PdfDocument` can target an accessibility (PDF/UA) or archival (PDF/A) standard simply by setting the `PdfDocumentCreateSettings.PdfStandard` property on the `PdfDocumentCreateSettings` passed to the constructor. The library produces a document that conforms to the selected standard.

### Configure Standard, Language and Page Setup

```csharp
PdfDocumentCreateSettings settings = new PdfDocumentCreateSettings
{
    PageSize = PdfPageSize.A4,
    PageOrientation = PdfPageOrientation.Portrait,
    Margins = new PdfMargins(36, 36, 36, 36),
    PdfStandard = PdfStandard.PdfUa2PdfA4,
    Language = "en-US"
};
using PdfDocument pdfDocument = new PdfDocument(settings);
pdfDocument.PdfDocumentInfo.Title = "Accessibility and Archival Demo";
pdfDocument.PdfViewerPreferences.DisplayDocTitle = true;
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-standards.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
