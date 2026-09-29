---
name: evopdf-next-html-to-image
description: "Render a URL or HTML string to PNG, JPEG or WebP in .NET with EvoPdf Next HtmlToImageConverter: viewport size, entire page capture, image type and quality, element selectors."
---

# EvoPdf Next: HTML to image

EvoPdf Next for .NET also allows you to easily convert HTML pages and HTML strings to raster images in PNG, JPG or WebP format with just a few lines of code. In this section you can learn about the basic settings of the converter.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`HtmlToImageConverter`**: This class offers the necessary methods to create a raster image from a web page at given URL or from a HTML string. The generated image can be saved ...
- **`ImageType`**: The possible image formats
- **`CaptureEntirePageMode`**: This enumeration defines the possible options for capturing the image of the entire HTML page, not just the visible viewport

3 types, 52 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## HTML to Image Converter Overview

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-image-converter-overview.htm

## Select HTML Elements to Convert to Image

You can convert only selected parts of an HTML page to an image by specifying a CSS selector. This allows you to choose exactly which content will be included in the image and to either remove or simply hide the unselected elements.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-convert-to-image.htm

## Select HTML Elements to Exclude from Image

You can exclude selected parts of an HTML page from conversion to an image by specifying a CSS selector. This allows you to choose exactly which content will be excluded from the image and to either remove or simply hide the excluded elements.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-exclude-from-image.htm

- HTML to Image Converter Overview: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-image-converter-overview.htm
- Select HTML Elements to Convert to Image: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-convert-to-image.htm
- Select HTML Elements to Exclude from Image: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-exclude-from-image.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
