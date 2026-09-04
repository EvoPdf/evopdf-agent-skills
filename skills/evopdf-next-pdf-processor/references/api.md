# PDF Processor — API reference summary

Namespace `EvoPdf.Next`. Every method below also exists as `…Async(…, CancellationToken ct = default)`.

## PdfToTextConverter
Properties: `TextLayout` (`PdfToTextLayout.Original` | `Reading`), `MarkPageBreaks` (bool; marker `PdfToTextConverter.PAGE_BREAK_MARK`), `UserPassword`, `OwnerPassword`, `MaxPageCount` (0 = unlimited), `RunTimeoutSec`, `ConversionInfo` (`PageCount`), `PdfLoaderFilePath`.

```
string ConvertToText(byte[] pdfData)                                   string ConvertToText(byte[] pdfData, int startPageNumber)
string ConvertToText(byte[] pdfData, int startPageNumber, int endPageNumber)
string ConvertToText(Stream pdfStream [, int startPageNumber [, int endPageNumber]])
string ConvertToText(string pdfFile   [, int startPageNumber [, int endPageNumber]])

FindTextLocation[] FindText(byte[] pdfData, string textToFind, bool caseSensitive, bool wholeWord)
FindTextLocation[] FindText(byte[] pdfData, string textToFind, int startPageNumber, bool caseSensitive, bool wholeWord)
FindTextLocation[] FindText(byte[] pdfData, string textToFind, int startPageNumber, int endPageNumber, bool caseSensitive, bool wholeWord)
  … the same three shapes for Stream pdfStream and string pdfFile
```
`FindTextLocation`: `PageNumber`, `X`, `Y`, `Width`, `Height` (points, origin top-left).

## PdfToImageConverter
Properties: `Resolution` (DPI, default 150), `ColorSpace` (`PdfPageImageColorSpace.RGB` default | `Gray` | `Mono`), `TransparencyEnabled` (RGB/Gray only), `UserPassword`, `OwnerPassword`, `MaxPageCount`, `RunTimeoutSec`, `StdFontsDir`, `ConversionInfo` (`PageCount`), `PdfLoaderFilePath`.

```
PdfPageImage[] ConvertToImages(byte[] pdfData [, int startPageNumber [, int endPageNumber]])
PdfPageImage[] ConvertToImages(Stream pdfStream [, …])          PdfPageImage[] ConvertToImages(string pdfFile [, …])
void ConvertToImageFiles(byte[] pdfData [, int startPageNumber [, int endPageNumber]], string outputDirectory, string imageFileName)
  … the same for Stream and string pdfFile
```
`PdfPageImage`: `PageNumber`, `ImageData` (PNG bytes). Files are named `<imageFileName>-<page>.png` in `outputDirectory`.

## PdfImagesExtractor
Properties: `UserPassword`, `OwnerPassword`, `MaxPageCount`, `RunTimeoutSec`, `ExtractionInfo` (`PageCount`, `ImagesPerPage`), `PdfLoaderFilePath`.

```
ExtractedImage[][] ExtractImages(byte[] pdfData [, int startPageNumber [, int endPageNumber]])
ExtractedImage[][] ExtractImages(Stream pdfStream [, …])        ExtractedImage[][] ExtractImages(string pdfFile [, …])
void ExtractImagesToFile(byte[] pdfData [, int startPageNumber [, int endPageNumber]], string outputDirectory, string imageFileName)
  … the same for Stream and string pdfFile
```
`ExtractedImage`: `PageNumber`, `ImageData` (PNG bytes). The outer array has one entry per processed page.
