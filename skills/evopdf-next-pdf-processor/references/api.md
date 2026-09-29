# Text extraction, search and page images: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfToTextConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfToTextConverter.htm

Provides PDF to text conversion functionality

### Properties

- `ConversionInfo`: Gets information about the last PDF to Text conversion. Populated after the conversion completes successfully, otherwise it is null
- `MarkPageBreaks`: Flag indicating whether page breaks are marked by the PAGE_BREAK_MARK character in the resulting text document. The default is false
- `MaxPageCount`: Upper limit for the number of PDF pages to process. A value of 0 means there is no upper limit
- `OwnerPassword`: Owner password to open a password-protected PDF document
- `PAGE_BREAK_MARK`: Special character used to mark page breaks in the resulting text document
- `PdfLoaderFilePath`: Sets the full path of the PDF loader file
- `RunTimeoutSec`: The maximum time allowed for this tool to run
- `TextLayout`: Text layout of the resulting document
- `UserPassword`: User password to open a password-protected PDF document

### Methods

- `ConvertToText`: Converts a range of pages of a PDF document in a stream to text
- `ConvertToText`: Converts a range of pages of a PDF document to text
- `ConvertToText`: Converts a range of pages of a PDF file to text
- `ConvertToText`: Converts all pages in a PDF document to text
- `ConvertToText`: Converts all pages of a PDF document in a stream to text
- `ConvertToText`: Converts all pages of a PDF file to text
- `ConvertToText`: Converts the pages of a PDF document in a stream to text starting at a given page number through the end of the document
- `ConvertToText`: Converts the pages of a PDF document to text starting at a given page number through the end of the document
- `ConvertToText`: Converts the pages of a PDF file to text starting at a given page number through the end of the document
- `ConvertToTextAsync`: Asynchronously converts a range of pages of a PDF document in a stream to text
- `ConvertToTextAsync`: Asynchronously converts a range of pages of a PDF document to text
- `ConvertToTextAsync`: Asynchronously converts a range of pages of a PDF file to text
- `ConvertToTextAsync`: Asynchronously converts all pages in a PDF document to text
- `ConvertToTextAsync`: Asynchronously converts all pages of a PDF document in a stream to text
- `ConvertToTextAsync`: Asynchronously converts all pages of a PDF file to text
- `ConvertToTextAsync`: Asynchronously converts the pages of a PDF document in a stream to text starting at a given page number through the end of the document
- `ConvertToTextAsync`: Asynchronously converts the pages of a PDF document to text starting at a given page number through the end of the document
- `ConvertToTextAsync`: Asynchronously converts the pages of a PDF file to text starting at a given page number through the end of the document
- `FindText`: Searches for the specified text in a range of pages of a PDF document
- `FindText`: Searches for the specified text in a range of pages of a PDF document in a stream
- `FindText`: Searches for the specified text in a range of pages of a PDF file
- `FindText`: Searches for the specified text in all pages of a PDF document
- `FindText`: Searches for the specified text in all pages of a PDF document in a stream
- `FindText`: Searches for the specified text in all pages of a PDF file
- `FindText`: Searches for the specified text in pages of a PDF document in a stream starting at a given page number through the end of the document
- `FindText`: Searches for the specified text in pages of a PDF document starting at a given page number through the end of the document
- `FindText`: Searches for the specified text in pages of a PDF file starting at a given page number through the end of the document
- `FindTextAsync`: Asynchronously searches for the specified text in a range of pages of a PDF document
- `FindTextAsync`: Asynchronously searches for the specified text in a range of pages of a PDF document in a stream
- `FindTextAsync`: Asynchronously searches for the specified text in a range of pages of a PDF file
- `FindTextAsync`: Asynchronously searches for the specified text in all pages of a PDF document
- `FindTextAsync`: Asynchronously searches for the specified text in all pages of a PDF document in a stream
- `FindTextAsync`: Asynchronously searches for the specified text in all pages of a PDF file
- `FindTextAsync`: Asynchronously searches for the specified text in pages of a PDF document in a stream starting at a given page number through the end of the document
- `FindTextAsync`: Asynchronously searches for the specified text in pages of a PDF document starting at a given page number through the end of the document
- `FindTextAsync`: Asynchronously searches for the specified text in pages of a PDF file starting at a given page number through the end of the document

## PdfToTextLayout

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfToTextLayout.htm

The resulted text layout

### Fields and values

- `Original`: The resulting text preserves the visual layout of the text in the PDF document
- `Reading`: The resulting text preserves the reading order of the text in the PDF document

## PdfToTextConversionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfToTextConversionInfo.htm

Holds information about the result of a PDF to Text conversion. This object is populated after the conversion completes and is exposed via the ConversionInfo property of the PDF to Text converter.

### Properties

- `PageCount`: Gets the number of PDF pages within the specified range that were converted to text

## FindTextLocation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_FindTextLocation.htm

Represents the location of text on a PDF page

### Properties

- `Height`: Height of the text on the PDF page
- `PageNumber`: PDF page number
- `Width`: Width of the text on the PDF page
- `X`: X coordinate of the text on the PDF page
- `Y`: Y coordinate of the text on the PDF page

## PdfToImageConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfToImageConverter.htm

Encapsulates PDF to image conversion functionality and allows converting PDF pages to PNG images

### Properties

- `ColorSpace`: Color space of the resulting images. The default is RGB
- `ConversionInfo`: Gets information about the last PDF to Image conversion. Populated after the conversion completes successfully, otherwise it is null
- `MaxPageCount`: Upper limit for the number of PDF pages to process. A value of 0 means there is no upper limit
- `OwnerPassword`: Owner password used to open a password-protected PDF document
- `PdfLoaderFilePath`: Sets the full path of the PDF loader file
- `Resolution`: Resolution, in DPI, of the resulting images. The default is 150
- `RunTimeoutSec`: Maximum time allowed for this tool to run
- `StdFontsDir`: Directory that contains standard PDF fonts
- `TransparencyEnabled`: Enables image background transparency. The default value is false
- `UserPassword`: User password used to open a password-protected PDF document

### Methods

- `ConvertToImageFiles`: Converts a range of pages of a PDF document in a stream to image files
- `ConvertToImageFiles`: Converts a range of pages of a PDF document to image files
- `ConvertToImageFiles`: Converts a range of pages of a PDF file to image files
- `ConvertToImageFiles`: Converts all pages in a PDF document to image files
- `ConvertToImageFiles`: Converts all pages of a PDF document in a stream to image files
- `ConvertToImageFiles`: Converts all pages of a PDF file to image files
- `ConvertToImageFiles`: Converts the pages of a PDF document in a stream to image files starting at a given page number through the end of the document
- `ConvertToImageFiles`: Converts the pages of a PDF document to image files starting at a given page number through the end of the document
- `ConvertToImageFiles`: Converts the pages of a PDF file to image files starting at a given page number through the end of the document
- `ConvertToImageFilesAsync`: Asynchronously converts a range of pages of a PDF document in a stream to image files
- `ConvertToImageFilesAsync`: Asynchronously converts a range of pages of a PDF document to image files
- `ConvertToImageFilesAsync`: Asynchronously converts a range of pages of a PDF file to image files
- `ConvertToImageFilesAsync`: Asynchronously converts all pages in a PDF document to image files
- `ConvertToImageFilesAsync`: Asynchronously converts all pages of a PDF document in a stream to image files
- `ConvertToImageFilesAsync`: Asynchronously converts all pages of a PDF file to image files
- `ConvertToImageFilesAsync`: Asynchronously converts the pages of a PDF document in a stream to image files starting at a given page number through the end of the document
- `ConvertToImageFilesAsync`: Asynchronously converts the pages of a PDF document to image files starting at a given page number through the end of the document
- `ConvertToImageFilesAsync`: Asynchronously converts the pages of a PDF file to image files starting at a given page number through the end of the document
- `ConvertToImages`: Converts a range of pages of a PDF document in a stream to image objects
- `ConvertToImages`: Converts a range of pages of a PDF document to image objects
- `ConvertToImages`: Converts a range of pages of a PDF file to image objects
- `ConvertToImages`: Converts all pages in a PDF document to image objects
- `ConvertToImages`: Converts all pages of a PDF document in a stream to image objects
- `ConvertToImages`: Converts all pages of a PDF file to image objects
- `ConvertToImages`: Converts the pages of a PDF document in a stream to image objects starting at a given page number through the end of the document
- `ConvertToImages`: Converts the pages of a PDF document to image objects starting at a given page number through the end of the document
- `ConvertToImages`: Converts the pages of a PDF file to image objects starting at a given page number through the end of the document
- `ConvertToImagesAsync`: Asynchronously converts a range of pages of a PDF document in a stream to image objects
- `ConvertToImagesAsync`: Asynchronously converts a range of pages of a PDF document to image objects
- `ConvertToImagesAsync`: Asynchronously converts a range of pages of a PDF file to image objects
- `ConvertToImagesAsync`: Asynchronously converts all pages in a PDF document to image objects
- `ConvertToImagesAsync`: Asynchronously converts all pages of a PDF document in a stream to image objects
- `ConvertToImagesAsync`: Asynchronously converts all pages of a PDF file to image objects
- `ConvertToImagesAsync`: Asynchronously converts the pages of a PDF document in a stream to image objects starting at a given page number through the end of the document
- `ConvertToImagesAsync`: Asynchronously converts the pages of a PDF document to image objects starting at a given page number through the end of the document
- `ConvertToImagesAsync`: Asynchronously converts the pages of a PDF file to image objects starting at a given page number through the end of the document

## PdfPageImage

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPageImage.htm

Represents an image of a PDF page

### Properties

- `ImageData`: Underlying image data
- `PageNumber`: Number of the PDF page this image represents

## PdfPageImageColorSpace

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPageImageColorSpace.htm

Specifies the color space of a PDF page image

### Fields and values

- `Gray`: Grayscale color space
- `Mono`: Black and white color space
- `RGB`: RGB color space

## PdfToImageConversionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfToImageConversionInfo.htm

Holds information about the result of a PDF to Image conversion. This object is populated after the conversion completes and is exposed via the ConversionInfo property of the PDF to Image converter.

### Properties

- `PageCount`: Gets the number of PDF pages within the specified range that were converted to images

## PdfImagesExtractor

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfImagesExtractor.htm

Encapsulates the PDF Images Extractor functionality and allows you to extract images from a PDF document

### Properties

- `ExtractionInfo`: Gets information about the last PDF images extraction. Populated after extraction completes successfully, otherwise it is null
- `MaxPageCount`: Upper limit for the number of PDF pages to process. A value of 0 means there is no upper limit
- `OwnerPassword`: Owner password to open a password-protected PDF document
- `PdfLoaderFilePath`: Sets the full path of the PDF loader file
- `RunTimeoutSec`: The maximum time allowed for this tool to run
- `UserPassword`: User password to open a password-protected PDF document

### Methods

- `ExtractImages`: Extracts images from a range of pages of a PDF document in a stream to image objects
- `ExtractImages`: Extracts images from a range of pages of a PDF document to image objects
- `ExtractImages`: Extracts images from a range of pages of a PDF file to image objects
- `ExtractImages`: Extracts images from all pages in a PDF document to image objects
- `ExtractImages`: Extracts images from all pages of a PDF document in a stream to image objects
- `ExtractImages`: Extracts images from all pages of a PDF file to image objects
- `ExtractImages`: Extracts images from the pages of a PDF document in a stream to image objects starting at a given page number through the end of the document
- `ExtractImages`: Extracts images from the pages of a PDF document to image objects starting at a given page number through the end of the document
- `ExtractImages`: Extracts images from the pages of a PDF file to image objects starting at a given page number through the end of the document
- `ExtractImagesAsync`: Asynchronously extracts images from a range of pages of a PDF document in a stream to image objects
- `ExtractImagesAsync`: Asynchronously extracts images from a range of pages of a PDF document to image objects
- `ExtractImagesAsync`: Asynchronously extracts images from a range of pages of a PDF file to image objects
- `ExtractImagesAsync`: Asynchronously extracts images from all pages in a PDF document to image objects
- `ExtractImagesAsync`: Asynchronously extracts images from all pages of a PDF document in a stream to image objects
- `ExtractImagesAsync`: Asynchronously extracts images from all pages of a PDF file to image objects
- `ExtractImagesAsync`: Asynchronously extracts images from the pages of a PDF document in a stream to image objects starting at a given page number through the end of the document
- `ExtractImagesAsync`: Asynchronously extracts images from the pages of a PDF document to image objects starting at a given page number through the end of the document
- `ExtractImagesAsync`: Asynchronously extracts images from the pages of a PDF file to image objects starting at a given page number through the end of the document
- `ExtractImagesToFile`: Extracts images from a PDF document to image files starting at a given page number through the end of the document
- `ExtractImagesToFile`: Extracts images from a range of pages of a PDF document in a stream to image files
- `ExtractImagesToFile`: Extracts images from a range of pages of a PDF document to image files
- `ExtractImagesToFile`: Extracts images from a range of pages of a PDF file to image files
- `ExtractImagesToFile`: Extracts images from all pages in a PDF document to image files
- `ExtractImagesToFile`: Extracts images from all pages of a PDF file to image files
- `ExtractImagesToFile`: Extracts images from the pages of a PDF document in a stream to image files
- `ExtractImagesToFile`: Extracts images from the pages of a PDF document in a stream to image files starting at a given page number through the end of the document
- `ExtractImagesToFile`: Extracts images from the pages of a PDF file to image files starting at a given page number through the end of the document
- `ExtractImagesToFileAsync`: Asynchronously extracts images from a PDF document to image files starting at a given page number through the end of the document
- `ExtractImagesToFileAsync`: Asynchronously extracts images from a range of pages of a PDF document in a stream to image files
- `ExtractImagesToFileAsync`: Asynchronously extracts images from a range of pages of a PDF document to image files
- `ExtractImagesToFileAsync`: Asynchronously extracts images from a range of pages of a PDF file to image files
- `ExtractImagesToFileAsync`: Asynchronously extracts images from all pages in a PDF document to image files
- `ExtractImagesToFileAsync`: Asynchronously extracts images from all pages of a PDF file to image files
- `ExtractImagesToFileAsync`: Asynchronously extracts images from the pages of a PDF document in a stream to image files
- `ExtractImagesToFileAsync`: Asynchronously extracts images from the pages of a PDF document in a stream to image files starting at a given page number through the end of the document
- `ExtractImagesToFileAsync`: Asynchronously extracts images from the pages of a PDF file to image files starting at a given page number through the end of the document

## ExtractedImage

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ExtractedImage.htm

Encapsulates an image extracted from a PDF page

### Properties

- `ImageData`: Underlying image data
- `PageNumber`: Number of the PDF page from which this image was extracted

## PdfImagesExtractionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfImagesExtractionInfo.htm

Holds information about the result of extracting images from a PDF. This object is populated after the conversion completes and is exposed via the ExtractionInfo property of the PDF Images Extractor.

### Properties

- `ImagesPerPage`: Gets the number of images extracted from each page within the specified range. The array order corresponds to the processing order of pages.
- `PageCount`: Gets the number of PDF pages within the specified range from which images were extracted

## PdfProcessorImageType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfProcessorImageType.htm

The possible image formats for PDF processor operations

### Fields and values

- `Jpeg`: JPEG format
- `Png`: PNG format
- `Webp`: Webp format

## PdfProcessorGlobalSettings

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfProcessorGlobalSettings.htm

Contains global settings that configure the behavior of the library within application

### Properties

- `MaxParallelOperations`: Gets or sets the maximum number of parallel PDF processor operations that can run concurrently. A value of 0 allows an unlimited number of concurrent operations. If not explicitly set by the application, a default value is automatically chosen based on the number of logical processors available on the system, with enforced minimum and maximum limits to maintain good performance and system stability. This property must be set before the first PDF processor operation occurs. Changes made afterward will have no effect until the application is restarted.

## PdfProcessorInstallation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfProcessorInstallation.htm

Provides information about the global installation of the PDF processor

### Properties

- `ExecutePermissionGranted`: Indicates whether execute permission has been successfully granted
- `GrantExecutePermissionErrOutput`: Standard error captured from the execute permission grant operation
- `GrantExecutePermissionStdOutput`: Standard output captured from the execute permission grant operation
- `RuntimeCopied`: Indicates whether the runtime has been successfully copied

### Methods

- `ConfigureRuntime`: Enables copying the runtime to a different location and installing dependencies before running the PDF processor. This function is intended for environments where installed dependencies are not persisted after a restart, It must be called before the first operation, which triggers the configuration setup. Changes made afterward will have no effect until the application is restarted
- `GetRuntimeConfig`: Retrieves the current runtime configuration settings
