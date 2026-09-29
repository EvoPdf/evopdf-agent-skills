# Text extraction, search and page images: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Convert PDF to Text

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-pdf-to-text.htm

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

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfStream, startPageNumber, endPageNumber);
string extractedText = pdfToTextConverter.ConvertToText(inputPdfFile, startPageNumber, endPageNumber);
```

### Asynchronous PDF to Text Methods

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfStream);
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfFile);
```

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfStream, startPageNumber);
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfFile, startPageNumber);
```

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfStream, startPageNumber, endPageNumber);
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfFile, startPageNumber, endPageNumber);
```

### Code Sample - Convert PDF to Text

```csharp
using System;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_to_Text;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_to_Text
{
    public class PDF_to_TextController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public PDF_to_TextController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ConvertPdfToText(PDF_to_Text_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create the PDF to Text converter instance with default options
            PdfToTextConverter pdfToTextConverter = new PdfToTextConverter();

            // Optionally set the user password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.UserPassword))
                pdfToTextConverter.UserPassword = model.UserPassword;

            // Optionally set the owner password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.OwnerPassword))
                pdfToTextConverter.OwnerPassword = model.OwnerPassword;

            // Configure the output text layout
            pdfToTextConverter.TextLayout = model.TextLayout == "Original" ? PdfToTextLayout.Original : PdfToTextLayout.Reading;

            // Mark PDF page breaks with the PdfToTextConverter.PAGE_BREAK_MARK special character
            pdfToTextConverter.MarkPageBreaks = model.MarkPageBreaks;

            // PDF page number to start text extraction from
            int startPageNumber = model.StartPageNumber;

            // PDF page number to end text extraction at
            // If 0, extraction continues to the end of the document
            int endPageNumber = 0;
            if (model.EndPageNumber.HasValue)
                endPageNumber = model.EndPageNumber.Value;

            byte[] inputPdfBytes = null;
            string outputFileName = null;

            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    inputPdfBytes = ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFile.FileName) + ".txt";
            }
            else
            {
                // Otherwise, fall back to the URL
                string pdfUrl = model.PdfFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(pdfUrl))
                    throw new Exception("No PDF file provided: upload a file or specify a URL");

                try
                {
                    if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(pdfUrl).LocalPath;
                        inputPdfBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputPdfBytes = await httpClient.GetByteArrayAsync(pdfUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the PDF file from URL", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFileUrl) + ".txt";
            }

            // Extract text from the specified PDF page range
            string extractedText = pdfToTextConverter.ConvertToText(inputPdfBytes, startPageNumber, endPageNumber);

            // Encode the extracted text as UTF-8 bytes
            byte[] outputTextBytes = Encoding.UTF8.GetBytes(extractedText);

            // Return the text as a downloadable file
            return File(outputTextBytes, "text/plain; charset=utf-8", outputFileName);
        }

        private PDF_to_Text_ViewModel SetViewModel()
        {
            var model = new PDF_to_Text_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "PDF_to_Text".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PdfProcessor_Files/PDF_Document.pdf";

            return model;
        }
    }
}
```

## Search for Text in PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/search-for-text-in-pdf.htm

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

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfStream, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfFile, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
```

### Asynchronous Methods to Find Text in PDF

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfStream, textToFindString, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfFile, textToFindString, caseSensitive, wholeWord);
```

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfStream, textToFindString, startPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfFile, textToFindString, startPageNumber, caseSensitive, wholeWord);
```

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfStream, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfFile, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
```

### Code Sample - Find Text in PDF

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_to_Text;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_to_Text
{
    public class Find_PDF_TextController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Find_PDF_TextController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> FindPdfText(Find_PDF_Text_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create the PDF to Text converter instance with default options
            PdfToTextConverter pdfToTextConverter = new PdfToTextConverter();

            // Optionally set the user password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.UserPassword))
                pdfToTextConverter.UserPassword = model.UserPassword;

            // Optionally set the owner password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.OwnerPassword))
                pdfToTextConverter.OwnerPassword = model.OwnerPassword;

            // PDF page number to start text search from
            int startPageNumber = model.StartPageNumber;

            // PDF page number to end text search at
            // If 0, search continues to the end of the document
            int endPageNumber = 0;
            if (model.EndPageNumber.HasValue)
                endPageNumber = model.EndPageNumber.Value;

            byte[] inputPdfBytes = null;
            string outputFileName = null;

            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    inputPdfBytes = ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFile.FileName) + "_Highlighted.pdf";
            }
            else
            {
                // Otherwise, fall back to the URL
                string pdfUrl = model.PdfFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(pdfUrl))
                    throw new Exception("No PDF file provided: upload a file or specify a URL");

                try
                {
                    if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(pdfUrl).LocalPath;
                        inputPdfBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputPdfBytes = await httpClient.GetByteArrayAsync(pdfUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the PDF file from URL", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFileUrl) + "_Highlighted.pdf";
            }

            // Search text in PDF
            FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfBytes, model.TextToFind,
                        startPageNumber, endPageNumber, model.CaseSensitive, model.WholeWord);

            // Open the PDF in editor
            string password = string.IsNullOrEmpty(model.OwnerPassword)? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            // Highlight the found text in PDF
            foreach (FindTextLocation findTextLocation in findTextLocations)
            {
                PdfRectangleElement highlightRectangle = new PdfRectangleElement(findTextLocation.X, findTextLocation.Y,
                    findTextLocation.Width, findTextLocation.Height);
                highlightRectangle.BorderColor = PdfColor.Yellow;

                pdfEditor.AddRectangle(findTextLocation.PageNumber, highlightRectangle);
            }

            // Save the highlighted PDF in a memory buffer
            byte[] outPdfBuffer = pdfEditor.Save();

            // Return the highlighted PDF as a downloadable file
            return File(outPdfBuffer, "application/pdf", outputFileName);
        }

        private Find_PDF_Text_ViewModel SetViewModel()
        {
            var model = new Find_PDF_Text_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Find_PDF_Text".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PdfProcessor_Files/PDF_Document.pdf";

            return model;
        }
    }
}
```

## Convert PDF Pages to Images

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-pdf-pages-to-images.htm

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

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfStream, startPageNumber, endPageNumber);
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfFile, startPageNumber, endPageNumber);
```

```csharp
pdfToImageConverter.ConvertToImageFiles(inputPdfBytes, outputDirectory, imageFileName);          
pdfToImageConverter.ConvertToImageFiles(inputPdfStream, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfFile, outputDirectory, imageFileName);
```

```csharp
pdfToImageConverter.ConvertToImageFiles(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);          
pdfToImageConverter.ConvertToImageFiles(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

```csharp
pdfToImageConverter.ConvertToImageFiles(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);          
pdfToImageConverter.ConvertToImageFiles(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Asynchronous PDF to Image Methods

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfStream);
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfFile);
```

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfStream, startPageNumber);
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfFile, startPageNumber);
```

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfStream, startPageNumber, endPageNumber);
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfFile, startPageNumber, endPageNumber);
```

```csharp
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfBytes, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfStream, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfFile, outputDirectory, imageFileName);
```

```csharp
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

```csharp
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Code Sample - Convert PDF Pages to Images

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_to_Image;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_to_Image
{
    public class PDF_to_ImageController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public PDF_to_ImageController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ConvertPdfToImage(PDF_to_Image_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create the PDF to Image converter instance with default options
            PdfToImageConverter pdfToImageConverter = new PdfToImageConverter();

            // Optionally set the user password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.UserPassword))
                pdfToImageConverter.UserPassword = model.UserPassword;

            // Optionally set the owner password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.OwnerPassword))
                pdfToImageConverter.OwnerPassword = model.OwnerPassword;

            // Set the color space of the resulting images
            pdfToImageConverter.ColorSpace = SelectedColorSpace(model.ColorSpace);

            // Set the resolution of the resulting images
            pdfToImageConverter.Resolution = model.Resolution;

            // Set whether image background transparency is enabled
            pdfToImageConverter.TransparencyEnabled = model.TransparencyEnabled;

            // PDF page number to start conversion from
            int startPageNumber = model.StartPageNumber;

            // PDF page number to end conversion at
            // If 0, conversion continues to the end of the document
            int endPageNumber = 0;
            if (model.EndPageNumber.HasValue)
                endPageNumber = model.EndPageNumber.Value;

            byte[] inputPdfBytes = null;
            string outputFileName = null;

            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    inputPdfBytes = ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFile.FileName);
            }
            else
            {
                // Otherwise, fall back to the URL
                string pdfUrl = model.PdfFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(pdfUrl))
                    throw new Exception("No PDF file provided: upload a file or specify a URL");

                try
                {
                    if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(pdfUrl).LocalPath;
                        inputPdfBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputPdfBytes = await httpClient.GetByteArrayAsync(pdfUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the PDF file from URL", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFileUrl);
            }

            // Convert to images the specified PDF page range
            PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfBytes, startPageNumber, endPageNumber);

            if (pdfPageImages.Length == 1)
            {
                // Return the single image as a downloadable file
                outputFileName += ".png";
                return File(pdfPageImages[0].ImageData, "image/png", outputFileName);
            }
            else
            {
                // Build an in-memory ZIP with all page images
                using var zipMs = new MemoryStream();
                using (var zip = new System.IO.Compression.ZipArchive(zipMs, System.IO.Compression.ZipArchiveMode.Create, leaveOpen: true))
                {
                    foreach (var pdfPageImage in pdfPageImages)
                    {
                        var entry = zip.CreateEntry($"page-{pdfPageImage.PageNumber:000000}.png", System.IO.Compression.CompressionLevel.Fastest);

                        // Write the image bytes into the ZIP entry
                        using var entryStream = entry.Open();
                        entryStream.Write(pdfPageImage.ImageData, 0, pdfPageImage.ImageData.Length);
                    }
                }

                outputFileName += ".zip";

                // Copy ZIP memory stream to a byte array
                byte[] outputZipBytes = zipMs.ToArray();

                // Return the ZIP as a downloadable file                
                return File(outputZipBytes, "application/zip", outputFileName);
            }
        }

        private PdfPageImageColorSpace SelectedColorSpace(string colorSpace)
        {
            switch (colorSpace)
            {
                case "RGB":
                    return PdfPageImageColorSpace.RGB;
                case "Mono":
                    return PdfPageImageColorSpace.Mono;
                case "Gray":
                    return PdfPageImageColorSpace.Gray;
                default:
                    return PdfPageImageColorSpace.RGB;
            }
        }

        private PDF_to_Image_ViewModel SetViewModel()
        {
            var model = new PDF_to_Image_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "PDF_to_Image".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PdfProcessor_Files/PDF_Document.pdf";

            return model;
        }
    }
}
```

## Extract Images from PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/extract-images-from-pdf.htm

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

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfStream, startPageNumber, endPageNumber);
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfFile, startPageNumber, endPageNumber);
```

```csharp
pdfImagesExtractor.ExtractImagesToFile(inputPdfBytes, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfStream, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfFile, outputDirectory, imageFileName);
```

```csharp
pdfImagesExtractor.ExtractImagesToFile(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

```csharp
pdfImagesExtractor.ExtractImagesToFile(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Asynchronous Methods to Extract Images from PDF

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfStream);
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfFile);
```

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfStream, startPageNumber);
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfFile, startPageNumber);
```

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfStream, startPageNumber, endPageNumber);
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfFile, startPageNumber, endPageNumber);
```

```csharp
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfBytes, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfStream, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfFile, outputDirectory, imageFileName);
```

```csharp
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

```csharp
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Code Sample - Extract Images from PDF Pages

```csharp
using System;
using System.IO;
using System.ComponentModel.DataAnnotations;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Images_Extractor;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Images_Extractor
{
    public class Extract_PDF_ImagesController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Extract_PDF_ImagesController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ExtractPdfImages(Extract_PDF_Images_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set license key received after purchase to use the extractor in licensed mode
            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create the PDF Images Extractor instance with default options
            PdfImagesExtractor pdfImagesExtractor = new PdfImagesExtractor();

            // Optionally set the user password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.UserPassword))
                pdfImagesExtractor.UserPassword = model.UserPassword;

            // Optionally set the owner password to open a password-protected PDF
            if (!string.IsNullOrEmpty(model.OwnerPassword))
                pdfImagesExtractor.OwnerPassword = model.OwnerPassword;

            // PDF page number to start extraction from
            int startPageNumber = model.StartPageNumber;

            // PDF page number to end extraction at
            // If 0, extraction continues to the end of the document
            int endPageNumber = 0;
            if (model.EndPageNumber.HasValue)
                endPageNumber = model.EndPageNumber.Value;

            byte[] inputPdfBytes = null;
            string outputFileName = null;

            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    inputPdfBytes = ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFile.FileName);
            }
            else
            {
                // Otherwise, fall back to the URL
                string pdfUrl = model.PdfFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(pdfUrl))
                    throw new Exception("No PDF file provided: upload a file or specify a URL");

                try
                {
                    if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(pdfUrl).LocalPath;
                        inputPdfBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputPdfBytes = await httpClient.GetByteArrayAsync(pdfUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the PDF file from URL", ex);
                }

                outputFileName = Path.GetFileNameWithoutExtension(model.PdfFileUrl);
            }

            // Extract the images from the specified PDF page range, grouped by page
            ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfBytes, startPageNumber, endPageNumber);

            int nPdfPages = extractedImages.Length;
            if (nPdfPages == 1 && extractedImages[0].Length > 0 && model.ExtractLargest)
            {
                // If only one page was processed and only the largest image is requested, return that image directly
                // Return the largest image as a downloadable file
                outputFileName += "-largest.png";
                ExtractedImage largestImage = GetLargestImage(extractedImages[0]);
                return File(largestImage.ImageData, "image/png", outputFileName);
            }
            else
            {
                // Build an in-memory ZIP with all page images and return it
                using var zipMs = new MemoryStream();
                using (var zip = new System.IO.Compression.ZipArchive(zipMs, System.IO.Compression.ZipArchiveMode.Create, leaveOpen: true))
                {
                    for (int pageIdx = 0; pageIdx < extractedImages.Length; pageIdx++)
                    {
                        var pageImages = extractedImages[pageIdx];
                        if (model.ExtractLargest)
                        {
                            // Add only the largest image from the page to the ZIP
                            ExtractedImage largestImage = GetLargestImage(pageImages);
                            if (largestImage != null)
                            {
                                var entry = zip.CreateEntry($"page-{largestImage.PageNumber:000000}-largest.png", System.IO.Compression.CompressionLevel.Fastest);
                                // Write the image bytes into the ZIP entry
                                using var entryStream = entry.Open();
                                entryStream.Write(largestImage.ImageData, 0, largestImage.ImageData.Length);
                            }
                        }
                        else
                        {
                            // Add all images from the PDF page to the ZIP
                            for (int imgIdx = 0; imgIdx < pageImages.Length; imgIdx++)
                            {
                                ExtractedImage extractedImage = pageImages[imgIdx];
                                var entry = zip.CreateEntry($"page-{extractedImage.PageNumber:000000}-{imgIdx:000000}.png", System.IO.Compression.CompressionLevel.Fastest);

                                // Write the image bytes into the ZIP entry
                                using var entryStream = entry.Open();
                                entryStream.Write(extractedImage.ImageData, 0, extractedImage.ImageData.Length);
                            }
                        }
                    }
                }

                outputFileName += ".zip";

                // Copy ZIP memory stream to a byte array
                byte[] outputZipBytes = zipMs.ToArray();

                // Return the ZIP as a downloadable file
                return File(outputZipBytes, "application/zip", outputFileName);
            }
        }

        private ExtractedImage GetLargestImage(ExtractedImage[] extractedImages)
        {
            ExtractedImage largestImage = null;
            int largestSize = 0;
            foreach (var image in extractedImages)
            {
                if (image.ImageData.Length > largestSize)
                {
                    largestImage = image;
                    largestSize = image.ImageData.Length;
                }
            }
            return largestImage;
        }

        private Extract_PDF_Images_ViewModel SetViewModel()
        {
            var model = new Extract_PDF_Images_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Extract_PDF_Images".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PdfProcessor_Files/PDF_Document.pdf";

            return model;
        }
    }
}
```
