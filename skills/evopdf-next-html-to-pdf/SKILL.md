---
name: evopdf-next-html-to-pdf
description: "Convert URLs and HTML strings to PDF in .NET with EvoPdf Next HtmlToPdfConverter: the conversion methods and their async versions, the converter options, conversion delay and manual triggering, SVG, converting the current ASP.NET page. A new converter prints on an A4 page with the HTML laid out as in a 1024 pixel browser window; evopdf-next-page-setup covers the page layout. Start here for any C# HTML to PDF task."
---

# EvoPdf Next: HTML to PDF

The HTML to PDF Converter turns web pages and HTML strings into PDF documents. It renders the HTML with a Chromium engine that ships inside the NuGet package, so HTML5, CSS3, JavaScript, web fonts and SVG come out as in the Chrome browser, and there is no browser to install on the server. The same component converts HTML to images with the `HtmlToImageConverter` class.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`HtmlToPdfConverter`**: This class is the main class of the HTML to PDF Converter which offers the necessary methods to create a PDF document from a web page at given URL or ...
- **`PdfDocumentOptions`**: This class encapsulates the options to control the PDF document rendering process. The HtmlToPdfConverter class defines a reference to an object of th...
- **`PdfPadding`**: Represents padding for a PDF element.
- **`TriggeringMode`**: This enumeration represents the possible modes to trigger the conversion of a HTML document
- **`HtmlToPdfConversionInfo`**: Holds information about the result of an HTML to PDF conversion. This object is populated after the conversion completes and is exposed by the Convers...
- **`PdfDocumentCreateSettings`**: Represents configuration settings for creating a PDF document

6 types, 122 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## HTML to PDF Converter Overview

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-converter-overview.htm

## HTML To PDF Converter Options

EVO HTML to PDF Converter allows you to set various options to control the internal HTML viewer and PDF document properties. The most important options are listed below, grouped into several categories.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-converter-options.htm

## Convert the Current HTML Page to PDF

EVO HTML to PDF Converter allows you to save the current HTML page as PDF. All the values filled in HTML form will be captured in PDF page too.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-current-html-page-to-pdf.htm

## Select Conversion Triggering Mode

The HTML conversion to PDF happens in two main stages. In the first stage the HTML document is loaded in the internal HTML viewer. When the load is complete the second stage starts and the HTML content is rendered to PDF document or into an image. The moment when the HTML load is complete is not well defined for all HTML documents. There might be for example scripts running in page which continuously update the HTML document. The triggering modes help the converter to decide when the HTML page load should be considered completed and when the actual rendering to PDF can start. The triggering mode is given by the `HtmlToPdfConverter.TriggeringMode` property. There are two possible triggering modes:

```csharp
// Create the PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set the triggering mode
htmlToPdfConverter.TriggeringMode = TriggeringMode.Auto;
```

```csharp
// Create the PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set the triggering mode
htmlToPdfConverter.ConversionDelay = 3;
```

```csharp
// Create the PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set the triggering mode
htmlToPdfConverter.TriggeringMode = TriggeringMode.Manual;
```

All 5 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-conversion-triggering-mode.htm

## Convert HTML with SVG to PDF

EVO HTML to PDF Converter offers full support for SVG vector graphics. The SVG code can be embedded directly in HTML page or it can be referenced from an external file.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-with-svg-to-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
