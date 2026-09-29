---
name: evopdf-next-html-element-mapping
description: "Convert or exclude parts of an HTML page with CSS selectors in EvoPdf Next and read back where each element landed in the PDF: ConvertedElementsSelector, ExcludedElementsSelector, HtmlElementsInfo, HtmlElementInfo positions, iframes and hidden elements."
---

# EvoPdf Next: selecting HTML elements and reading their PDF position

You can convert only selected parts of an HTML page to PDF by specifying a CSS selector. This allows you to choose exactly which content will be included in the PDF and to either remove or simply hide the unselected elements.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`HtmlElementInfo`**: Represents detailed style and content information about a single HTML element
- **`HtmlElementInfoCollection`**: Represents a collection of HTML elements and provides lookup functionality

2 types, 10 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Select HTML Elements to Convert to PDF

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-convert-to-pdf.htm

## Select HTML Elements to Exclude from PDF

You can exclude selected parts of an HTML page from conversion to PDF by specifying a CSS selector. This allows you to choose exactly which content will be excluded from the PDF and to either remove or simply hide the excluded elements.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-exclude-from-pdf.htm

## Retrieve HTML Element Positions in PDF

You can select the HTML elements whose position in the PDF you want to retrieve by specifying a CSS selector. Along with the position in the PDF you can also retrieve other useful information such as ID, tag name, CSS class and text content. This feature enables various possibilities for post-processing the PDF document generated from HTML and provides insight into how specific HTML elements are rendered in the final PDF. In the code sample, the retrieved elements are highlighted in the PDF with colored borders based on their tag names but other useful scenarios are also possible such as adding a PDF annotation to a specific HTML element.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/retrieve-html-element-positions-in-pdf.htm

- Select HTML Elements to Convert to PDF: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-convert-to-pdf.htm
- Select HTML Elements to Exclude from PDF: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-exclude-from-pdf.htm
- Retrieve HTML Element Positions in PDF: https://www.evopdf.com/help/evopdf-next-dotnet/html/retrieve-html-element-positions-in-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
