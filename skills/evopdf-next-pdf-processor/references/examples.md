# Text extraction, search and page images: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Convert PDF to Text

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-pdf-to-text.htm

### Create the PDF to Text Converter

**Create a PDF to Text Converter Instance**

```csharp
// Create a new PDF to Text converter instance
PdfToTextConverter pdfToTextConverter = new PdfToTextConverter();
```

### Extracted Text Layout

**Set Extracted Text Layout**

```csharp
pdfToTextConverter.TextLayout = PdfToTextLayout.Original;
```

### Mark Page Breaks

**Mark Page Breaks in Extracted Text**

```csharp
pdfToTextConverter.MarkPageBreaks = true;
```

### Open Password Protected PDFs

**Set User and Owner Passwords**

```csharp
pdfToTextConverter.UserPassword = userPasswordString;
pdfToTextConverter.OwnerPassword = ownerPasswordString;
```

### Extracted Page Range Limit

**Unlimited Extracted PDF Page Range**

```csharp
pdfToTextConverter.MaxPageCount = 0;
```

**Limit the Extracted PDF Page Range to 10 Pages**

```csharp
pdfToTextConverter.MaxPageCount = 10;
```

### Convert PDF to Text

**Convert All Pages in a PDF from Memory to a String**

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfBytes);
```

**Convert Pages in a PDF from Memory to a String Starting at a Given Page Number**

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfBytes, startPageNumber);
```

**Convert a Range of Pages in a PDF from Memory to a String**

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfBytes, startPageNumber, endPageNumber);
```

**Convert All Pages in a PDF from a Stream or File to a String**

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfStream);
string extractedText = pdfToTextConverter.ConvertToText(inputPdfFile);
```

**Convert Pages in a PDF from a Stream or File to a String Starting at a Given Page Number**

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfStream, startPageNumber);
string extractedText = pdfToTextConverter.ConvertToText(inputPdfFile, startPageNumber);
```

**Convert a Range of Pages in a PDF from a Stream or File to a String**

```csharp
string extractedText = pdfToTextConverter.ConvertToText(inputPdfStream, startPageNumber, endPageNumber);
string extractedText = pdfToTextConverter.ConvertToText(inputPdfFile, startPageNumber, endPageNumber);
```

### Asynchronous PDF to Text Methods

**Asynchronously Convert All Pages in a PDF from Memory to a String**

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfBytes);
```

**Asynchronously Convert Pages in a PDF from Memory to a String Starting at a Given Page Number**

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfBytes, startPageNumber);
```

**Asynchronously Convert a Range of Pages in a PDF from Memory to a String**

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfBytes, startPageNumber, endPageNumber);
```

**Asynchronously Convert All Pages in a PDF from a Stream or File to a String**

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfStream);
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfFile);
```

**Asynchronously Convert Pages in a PDF from a Stream or File to a String Starting at a Given Page Number**

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfStream, startPageNumber);
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfFile, startPageNumber);
```

**Asynchronously Convert a Range of Pages in a PDF from a Stream or File to a String**

```csharp
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfStream, startPageNumber, endPageNumber);
string extractedText = await pdfToTextConverter.ConvertToTextAsync(inputPdfFile, startPageNumber, endPageNumber);
```

### Conversion Info

**Gets the Number of PDF Pages Converted to Text**

```csharp
int numberOfPagesConverted = pdfToTextConverter.ConversionInfo.PageCount;
```

### Code Sample - Convert PDF to Text

**Convert PDF to Text in ASP.NET Core**

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

**Create a PDF to Text Converter Instance**

```csharp
// Create a new PDF to Text converter instance
PdfToTextConverter pdfToTextConverter = new PdfToTextConverter();
```

### Open Password Protected PDFs

**Set User and Owner Passwords**

```csharp
pdfToTextConverter.UserPassword = userPasswordString;
pdfToTextConverter.OwnerPassword = ownerPasswordString;
```

### Searched Page Range Limit

**Unlimited Searched PDF Page Range**

```csharp
pdfToTextConverter.MaxPageCount = 0;
```

**Limit the Searched PDF Page Range to 10 Pages**

```csharp
pdfToTextConverter.MaxPageCount = 10;
```

### Find Text in PDF

**Find Text in All Pages in a PDF from Memory**

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfBytes, textToFindString, caseSensitive, wholeWord);
```

**Search Pages in a PDF from Memory Starting at a Given Page Number**

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfBytes, textToFindString, startPageNumber, caseSensitive, wholeWord);
```

**Search a Range of Pages in a PDF from Memory**

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfBytes, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
```

**Search All Pages in a PDF from a Stream or File**

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfStream, textToFindString, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfFile, textToFindString, caseSensitive, wholeWord);
```

**Search Pages in a PDF from a Stream or File Starting at a Given Page Number**

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfStream, textToFindString, startPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfFile, textToFindString, startPageNumber, caseSensitive, wholeWord);
```

**Search a Range of Pages in a PDF from a Stream or File**

```csharp
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfStream, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = pdfToTextConverter.FindText(inputPdfFile, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
```

### Asynchronous Methods to Find Text in PDF

**Asynchronously Find Text in All Pages in a PDF from Memory**

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfBytes, textToFindString, caseSensitive, wholeWord);
```

**Asynchronously Search Pages in a PDF from Memory Starting at a Given Page Number**

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfBytes, textToFindString, startPageNumber, caseSensitive, wholeWord);
```

**Asynchronously Search a Range of Pages in a PDF from Memory**

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfBytes, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
```

**Asynchronously Search All Pages in a PDF from a Stream or File**

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfStream, textToFindString, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfFile, textToFindString, caseSensitive, wholeWord);
```

**Asynchronously Search Pages in a PDF from a Stream or File Starting at a Given Page Number**

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfStream, textToFindString, startPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfFile, textToFindString, startPageNumber, caseSensitive, wholeWord);
```

**Asynchronously Search a Range of Pages in a PDF from a Stream or File**

```csharp
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfStream, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
FindTextLocation[] findTextLocations = await pdfToTextConverter.FindTextAsync(inputPdfFile, textToFindString, startPageNumber, endPageNumber, caseSensitive, wholeWord);
```

### Conversion Info

**Gets the Number of PDF Pages Searched for Text**

```csharp
int numberOfPagesSearched = pdfToTextConverter.ConversionInfo.PageCount;
```

### Code Sample - Find Text in PDF

**Find Text in PDF in ASP.NET Core**

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

**Create a PDF to Image Converter Instance**

```csharp
// Create a new PDF to Image converter instance
PdfToImageConverter pdfToImageConverter = new PdfToImageConverter();
```

### Color Space of Resulting Images

**Set Color Space of Resulting Images**

```csharp
pdfToImageConverter.ColorSpace = PdfPageImageColorSpace.RGB;
```

### Resolution of Resulting Images

**Set Resolution of Resulting Images**

```csharp
pdfToImageConverter.Resolution = 150;
```

### Background Transparency of the Resulting Images

**Enable Background Transparency of the Resulting Images**

```csharp
pdfToImageConverter.TransparencyEnabled = true;
```

### Open Password Protected PDFs

**Set User and Owner Passwords**

```csharp
pdfToImageConverter.UserPassword = userPasswordString;
pdfToImageConverter.OwnerPassword = ownerPasswordString;
```

### Converted Page Range Limit

**Unlimited Converted PDF Page Range**

```csharp
pdfToImageConverter.MaxPageCount = 0;
```

**Limit the Converted PDF Page Range to 10 Pages**

```csharp
pdfToImageConverter.MaxPageCount = 10;
```

### Convert PDF Pages to Images

**Convert All Pages in a PDF from Memory to Images**

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfBytes);
```

**Convert Pages in a PDF from Memory to Images Starting at a Given Page Number**

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfBytes, startPageNumber);
```

**Convert a Range of Pages in a PDF from Memory to Images**

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfBytes, startPageNumber, endPageNumber);
```

**Convert All Pages in a PDF from a Stream or File to Images**

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfStream);
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfFile);
```

**Convert Pages in a PDF from a Stream or File to Images Starting at a Given Page Number**

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfStream, startPageNumber);
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfFile, startPageNumber);
```

**Convert a Range of Pages in a PDF from a Stream or File to Images**

```csharp
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfStream, startPageNumber, endPageNumber);
PdfPageImage[] pdfPageImages = pdfToImageConverter.ConvertToImages(inputPdfFile, startPageNumber, endPageNumber);
```

**Convert All Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
pdfToImageConverter.ConvertToImageFiles(inputPdfBytes, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfStream, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfFile, outputDirectory, imageFileName);
```

**Convert Pages in a PDF from Memory, Stream or File to Image Files Starting at a Given Page Number**

```csharp
pdfToImageConverter.ConvertToImageFiles(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

**Convert a Range of Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
pdfToImageConverter.ConvertToImageFiles(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
pdfToImageConverter.ConvertToImageFiles(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Asynchronous PDF to Image Methods

**Asynchronously Convert All Pages in a PDF from Memory to Images**

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfBytes);
```

**Asynchronously Convert Pages in a PDF from Memory to Images Starting at a Given Page Number**

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfBytes, startPageNumber);
```

**Asynchronously Convert a Range of Pages in a PDF from Memory to Images**

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfBytes, startPageNumber, endPageNumber);
```

**Asynchronously Convert All Pages in a PDF from a Stream or File to Images**

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfStream);
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfFile);
```

**Asynchronously Convert Pages in a PDF from a Stream or File to Images Starting at a Given Page Number**

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfStream, startPageNumber);
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfFile, startPageNumber);
```

**Asynchronously Convert a Range of Pages in a PDF from a Stream or File to Images**

```csharp
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfStream, startPageNumber, endPageNumber);
PdfPageImage[] pdfPageImages = await pdfToImageConverter.ConvertToImagesAsync(inputPdfFile, startPageNumber, endPageNumber);
```

**Asynchronously Convert All Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfBytes, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfStream, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfFile, outputDirectory, imageFileName);
```

**Asynchronously Convert Pages in a PDF from Memory, Stream or File to Image Files Starting at a Given Page Number**

```csharp
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

**Asynchronously Convert a Range of Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfToImageConverter.ConvertToImageFilesAsync(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Conversion Info

**Gets the Number of PDF Pages Converted to Images**

```csharp
int numberOfPagesConverted = pdfToImageConverter.ConversionInfo.PageCount;
```

### Code Sample - Convert PDF Pages to Images

**Convert PDF Pages to Images in ASP.NET Core**

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

**Create a PDF Images Extractor Instance**

```csharp
 // Create the PDF Images Extractor instance with default options
PdfImagesExtractor pdfImagesExtractor = new PdfImagesExtractor();
```

### Open Password Protected PDFs

**Set User and Owner Passwords**

```csharp
pdfImagesExtractor.UserPassword = userPasswordString;
pdfImagesExtractor.OwnerPassword = ownerPasswordString;
```

### Extracted Page Range Limit

**Unlimited Extracted PDF Page Range**

```csharp
pdfImagesExtractor.MaxPageCount = 0;
```

**Limit the Extracted PDF Page Range to 10 Pages**

```csharp
pdfImagesExtractor.MaxPageCount = 10;
```

### Extract Images from PDF

**Extract Images from All Pages in a PDF from Memory**

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfBytes);
```

**Extract Images from a PDF from Memory Starting at a Given Page Number**

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfBytes, startPageNumber);
```

**Extract Images from a Range of Pages in a PDF from Memory**

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfBytes, startPageNumber, endPageNumber);
```

**Extract Images from All Pages in a PDF from a Stream or File**

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfStream);
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfFile);
```

**Extract Images from Pages in a PDF from a Stream or File Starting at a Given Page Number**

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfStream, startPageNumber);
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfFile, startPageNumber);
```

**Extract Images from a Range of Pages in a PDF from a Stream or File**

```csharp
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfStream, startPageNumber, endPageNumber);
ExtractedImage[][] extractedImages = pdfImagesExtractor.ExtractImages(inputPdfFile, startPageNumber, endPageNumber);
```

**Extract Images from All Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
pdfImagesExtractor.ExtractImagesToFile(inputPdfBytes, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfStream, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfFile, outputDirectory, imageFileName);
```

**Extract Images from Pages in a PDF from Memory, Stream or File to Image Files Starting at a Given Page Number**

```csharp
pdfImagesExtractor.ExtractImagesToFile(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

**Extract Images from a Range of Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
pdfImagesExtractor.ExtractImagesToFile(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
pdfImagesExtractor.ExtractImagesToFile(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Asynchronous Methods to Extract Images from PDF

**Asynchronously Extract Images from All Pages in a PDF from Memory**

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfBytes);
```

**Asynchronously Extract Images from a PDF from Memory Starting at a Given Page Number**

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfBytes, startPageNumber);
```

**Asynchronously Extract Images from a Range of Pages in a PDF from Memory**

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfBytes, startPageNumber, endPageNumber);
```

**Asynchronously Extract Images from All Pages in a PDF from a Stream or File**

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfStream);
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfFile);
```

**Asynchronously Extract Images from Pages in a PDF from a Stream or File Starting at a Given Page Number**

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfStream, startPageNumber);
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfFile, startPageNumber);
```

**Asynchronously Extract Images from a Range of Pages in a PDF from a Stream or File**

```csharp
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfStream, startPageNumber, endPageNumber);
ExtractedImage[][] extractedImages = await pdfImagesExtractor.ExtractImagesAsync(inputPdfFile, startPageNumber, endPageNumber);
```

**Asynchronously Extract Images from All Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfBytes, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfStream, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfFile, outputDirectory, imageFileName);
```

**Asynchronously Extract Images from Pages in a PDF from Memory, Stream or File to Image Files Starting at a Given Page Number**

```csharp
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfBytes, startPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfStream, startPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfFile, startPageNumber, outputDirectory, imageFileName);
```

**Asynchronously Extract Images from a Range of Pages in a PDF from Memory, Stream or File to Image Files**

```csharp
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfBytes, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfStream, startPageNumber, endPageNumber, outputDirectory, imageFileName);
await pdfImagesExtractor.ExtractImagesToFileAsync(inputPdfFile, startPageNumber, endPageNumber, outputDirectory, imageFileName);
```

### Extraction Info

**Gets the Number of PDF Pages from Which Images Were Extracted**

```csharp
int numberOfPagesExtracted = pdfImagesExtractor.ExtractionInfo.PageCount;
```

### Code Sample - Extract Images from PDF Pages

**Extract Images from PDF Pages in ASP.NET Core**

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
