---
name: evopdf-next-bookmarks-and-toc
description: "Generate a document outline and an automatic table of contents from HTML headings with EvoPdf Next: GenerateDocumentOutline, UseBrowserOutlineMode, GenerateTableOfContents, TableOfContentsOptions styling, page counting and offsets, internal links."
---

# EvoPdf Next: bookmarks and table of contents

EVO HTML to PDF Converter can be configured to automatically create a hierarchy of bookmarks in PDF document based on H1 to H6 head tags found in HTML document by simply turning on the `PdfDocumentOptions.GenerateDocumentOutline` option. An object of `PdfDocumentOptions` type is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`TableOfContentsOptions`**: This class encapsulates the options to control the table of contents automatically generated in PDF from HTML heading tags (H1 to H6) or from any elem...

1 types, 13 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Auto Create Hierarchical Bookmarks

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/auto-create-hierarchical-bookmarks.htm

## Auto Create Table of Contents

EVO HTML to PDF Converter can be configured to automatically create a table of contents in a PDF document based on H1 to H6 heading tags found in HTML document by simply turning on the `PdfDocumentOptions.GenerateTableOfContents` option. An object of the `PdfDocumentOptions` type is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property. The code below can be used to enable the table of contents generation in the PDF document.

```csharp
// Create a HTML to PDF converter object with default settings
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

// Enable or disable the automatic creation of a table of contents in the PDF document based on H1 to H6 HTML tags
htmlToPdfConverter.PdfDocumentOptions.GenerateTableOfContents = true;
```

### Table Title

```csharp
// Set the table of contents title
htmlToPdfConverter.PdfDocumentOptions.TableOfContents.Title = "Table of Contents";
```

### Inline Table of Contents

```csharp
// Set the table of contents title
htmlToPdfConverter.PdfDocumentOptions.TableOfContents.CreateInline = true;
```

All 6 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/auto-create-table-of-contents.htm

## Convert Internal Links from HTML to PDF

EVO HTML to PDF Converter automatically converts all the internal links from HTML to internal links in PDF.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-internal-links-from-html-to-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
