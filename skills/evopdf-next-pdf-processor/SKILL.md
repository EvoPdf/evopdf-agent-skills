---
name: evopdf-next-pdf-processor
description: "Extract content from existing PDF documents with the EvoPdf Next PDF Processor: PDF to text with layout options, text search with page positions, PDF pages to PNG images, embedded image extraction, passwords, page limits and run timeouts."
---

# EvoPdf Next: text extraction, search and page images

The PDF to Text Converter component allows you to extract the text from PDF documents. This component is distributed as part of the EvoPdf.Next.PdfProcessor.Windows NuGet package when targeting Windows x64 and as part of the EvoPdf.Next.PdfProcessor.Linux package when targeting Linux x64. The Windows x64 package is referenced by the EvoPdf.Next.Windows metapackage and the Linux x64 package is referenced by the EvoPdf.Next.Linux metapackage.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfToTextConverter`**: Provides PDF to text conversion functionality
- **`PdfToTextLayout`**: The resulted text layout
- **`PdfToTextConversionInfo`**: Holds information about the result of a PDF to Text conversion. This object is populated after the conversion completes and is exposed via the Convers...
- **`FindTextLocation`**: Represents the location of text on a PDF page
- **`PdfToImageConverter`**: Encapsulates PDF to image conversion functionality and allows converting PDF pages to PNG images
- **`PdfPageImage`**: Represents an image of a PDF page
- **`PdfPageImageColorSpace`**: Specifies the color space of a PDF page image
- **`PdfToImageConversionInfo`**: Holds information about the result of a PDF to Image conversion. This object is populated after the conversion completes and is exposed via the Conver...
- **`PdfImagesExtractor`**: Encapsulates the PDF Images Extractor functionality and allows you to extract images from a PDF document
- **`ExtractedImage`**: Encapsulates an image extracted from a PDF page
- **`PdfImagesExtractionInfo`**: Holds information about the result of extracting images from a PDF. This object is populated after the conversion completes and is exposed via the Ext...
- **`PdfProcessorImageType`**: The possible image formats for PDF processor operations
- **`PdfProcessorGlobalSettings`**: Contains global settings that configure the behavior of the library within application
- **`PdfProcessorInstallation`**: Provides information about the global installation of the PDF processor

14 types, 161 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Convert PDF to Text

### Create the PDF to Text Converter

```csharp
// Create a new PDF to Text converter instance
PdfToTextConverter pdfToTextConverter = new PdfToTextConverter();
```

### Open Password Protected PDFs

```csharp
pdfToTextConverter.UserPassword = userPasswordString;
pdfToTextConverter.OwnerPassword = ownerPasswordString;
```

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfStream);
string extractedText = pdfToTextConverter.ConvertToText(inputPdfFile);
```

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfStream, startPageNumber);
string extractedText = pdfToTextConverter.ConvertToText(inputPdfFile, startPageNumber);
```

All 9 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-pdf-to-text.htm

## Search for Text in PDF

The PDF to Text Converter component allows you to search for text in PDF documents and retrieve the text location and bounding rectangle information in PDF documents. This component is distributed as part of the EvoPdf.Next.PdfProcessor.Windows NuGet package when targeting Windows x64 and as part of the EvoPdf.Next.PdfProcessor.Linux package when targeting Linux x64. The Windows x64 package is referenced by the EvoPdf.Next.Windows metapackage and the Linux x64 package is referenced by the EvoPdf.Next.Linux metapackage.

### Create the PDF to Text Converter

```csharp
// Create a new PDF to Text converter instance
PdfToTextConverter pdfToTextConverter = new PdfToTextConverter();
```

### Open Password Protected PDFs

```csharp
pdfToTextConverter.UserPassword = userPasswordString;
pdfToTextConverter.OwnerPassword = ownerPasswordString;
```

### Find Text in PDF

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfStream, textToFindString, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfFile, textToFindString, caseSensitive, wholeWord);
```

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfStream, textToFindString, startPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfFile, textToFindString, startPageNumber, caseSensitive, wholeWord);
```

All 9 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/search-for-text-in-pdf.htm

## Convert PDF Pages to Images

The PDF to Image Converter component allows you to convert PDF pages into PNG images. This component is distributed as part of the EvoPdf.Next.PdfProcessor.Windows NuGet package when targeting Windows x64 and as part of the EvoPdf.Next.PdfProcessor.Linux package when targeting Linux x64. The Windows x64 package is referenced by the EvoPdf.Next.Windows metapackage and the Linux x64 package is referenced by the EvoPdf.Next.Linux metapackage.

### Create the PDF to Image Converter

```csharp
// Create a new PDF to Image converter instance
PdfToImageConverter pdfToImageConverter = new PdfToImageConverter();
```

### Open Password Protected PDFs

```csharp
pdfToImageConverter.UserPassword = userPasswordString;
pdfToImageConverter.OwnerPassword = ownerPasswordString;
```

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfStream);
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfFile);
```

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfStream, startPageNumber);
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfFile, startPageNumber);
```

All 15 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-pdf-pages-to-images.htm

## Extract Images from PDF

The PDF Images Extractor component of the library allows you to extract images from PDF documents in PNG format. This component is distributed as part of the EvoPdf.Next.PdfProcessor.Windows NuGet package when targeting Windows x64 and as part of the EvoPdf.Next.PdfProcessor.Linux package when targeting Linux x64. The Windows x64 package is referenced by the EvoPdf.Next.Windows metapackage and the Linux x64 package is referenced by the EvoPdf.Next.Linux metapackage.

### Create the PDF Images Extractor

```csharp
 // Create the PDF Images Extractor instance with default options
PdfImagesExtractor pdfImagesExtractor = new PdfImagesExtractor();
```

### Open Password Protected PDFs

```csharp
pdfImagesExtractor.UserPassword = userPasswordString;
pdfImagesExtractor.OwnerPassword = ownerPasswordString;
```

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfStream);
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfFile);
```

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfStream, startPageNumber);
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfFile, startPageNumber);
```

All 15 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/extract-images-from-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
