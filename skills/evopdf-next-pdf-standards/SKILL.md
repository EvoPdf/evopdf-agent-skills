---
name: evopdf-next-pdf-standards
description: "Produce standards-compliant PDF with EvoPdf Next: all PdfStandard values including PDF/UA-1, PDF/UA-2, PDF/A-2, PDF/A-3, PDF/A-4 and the combined profiles, automatic accessibility fixes, and per-element tagging with structure roles, alternate text, actual text, language and role maps."
---

# EvoPdf Next: PDF/UA and PDF/A compliance

EVO HTML to PDF Converter can be configured to automatically generate PDF documents compliant with accessibility and archival standards by setting the `PdfDocumentOptions.PdfStandard` property. An object of `PdfDocumentOptions` type is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfStandard`**: Specifies the PDF standards compliance level for the generated PDF document
- **`PdfAccessibilityOptions`**: Controls the accessibility settings of the generated PDF document. These settings take effect only when a PDF/UA or PDF/A standard requiring a tagged ...
- **`PdfAccessibilityProperties`**: Accessibility metadata controlling how an element is inserted into the structure tree of a tagged PDF
- **`PdfStructureType`**: Identifies the PDF logical structure role attached to a content element when the document is tagged

4 types, 48 public members. The complete member list with the shipped
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
standard selected, `PdfDocumentOptions.AccessibilityOptions` adds a default alternate text to images
that have none (`AddMissingImageAlternateText`) and a header row to tables without header cells
(`InsertMissingTableHeaders`); both are on by default.

## Create PDF/UA and PDF/A Compliant Documents

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdfua-and-pdfa-documents.htm

## Create PDF/UA and PDF/A Documents

A PDF created from scratch with `PdfDocument` can target an accessibility (PDF/UA) or archival (PDF/A) standard simply by setting the `PdfDocumentCreateSettings.PdfStandard` property on the `PdfDocumentCreateSettings` passed to the constructor. The library then handles the standard-mandated artefacts. These include embedded fonts, color profiles, the structure tree, namespaces, document metadata and viewer preferences.

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
