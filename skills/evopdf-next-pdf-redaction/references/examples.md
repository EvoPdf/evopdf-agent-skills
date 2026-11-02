# PDF redaction: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Redact Text and Personal Data in PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/redact-text-and-personal-data-in-pdf.htm

### Redact Texts

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

// Every occurrence of a name, as a whole word
pdfRedactor.RedactText("Jane Smith", false, true);

// The texts that match a regular expression, for example the customer numbers
pdfRedactor.RedactText(new Regex(@"C-\d{4}-\d{5}"));

byte[] redactedPdf = pdfRedactor.Save();
```

### Redact Personal Data

```csharp
// All the email addresses, phone numbers and bank accounts
pdfRedactor.RedactPersonalData(PdfPersonalDataTypes.EmailAddress | PdfPersonalDataTypes.PhoneNumber |
    PdfPersonalDataTypes.Iban);

// The payment card numbers found, reviewed one by one
foreach (PdfTextMatch match in pdfRedactor.FindPersonalData(PdfPersonalDataTypes.PaymentCardNumber))
{
    if (match.PageNumber == 1)
        pdfRedactor.AddRedaction(match);
}
```

### Redaction Marks for a Review

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);
pdfRedactor.RedactText("Jane Smith", false, true);
pdfRedactor.RedactPersonalData(PdfPersonalDataTypes.All);
// The document with the redaction marks, to review before the content is removed
byte[] markedPdf = pdfRedactor.SaveRedactionMarks();
```

### Code Sample - Redact Text and Personal Data

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Text.RegularExpressions;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.Redact_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.Redact_PDF
{
    public class Redact_Text_and_Personal_DataController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Redact_Text_and_Personal_DataController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> RedactText(Redact_Text_and_Personal_Data_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] inputPdfBytes = await ReadInputPdf(model);
            string password = !string.IsNullOrEmpty(model.OwnerPassword) ? model.OwnerPassword :
                !string.IsNullOrEmpty(model.UserPassword) ? model.UserPassword : null;

            using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

            // The appearance of the redacted areas: a fill color and a text written over each area
            pdfRedactor.FillColor = GetFillColor(model.FillColor);
            if (!string.IsNullOrWhiteSpace(model.OverlayText))
            {
                pdfRedactor.OverlayText = model.OverlayText.Trim();
                pdfRedactor.OverlayTextColor = model.FillColor == "White" ? PdfColor.Black : PdfColor.White;
                pdfRedactor.RepeatOverlayText = model.RepeatOverlayText;
            }

            // Each text is searched in all the pages and every occurrence is marked for redaction. The same texts
            // are also replaced in the document information, bookmarks, annotations and form field values
            if (!string.IsNullOrWhiteSpace(model.TextsToRedact))
            {
                string[] texts = model.TextsToRedact.Split(new[] { '\r', '\n' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string text in texts)
                {
                    if (!string.IsNullOrWhiteSpace(text))
                        pdfRedactor.RedactText(text.Trim(), model.MatchCase, model.WholeWord);
                }
            }

            // The texts that match a regular expression, for example the customer numbers
            if (model.UseRegularExpression && !string.IsNullOrWhiteSpace(model.RegularExpression))
            {
                Regex pattern;
                try
                {
                    pattern = new Regex(model.RegularExpression, RegexOptions.CultureInvariant, TimeSpan.FromSeconds(5));
                }
                catch (ArgumentException ex)
                {
                    throw new Exception("The regular expression is not valid", ex);
                }

                pdfRedactor.RedactText(pattern);
            }

            // The personal data is found by its structure and validated, for example by the check digits of
            // bank account and payment card numbers
            PdfPersonalDataTypes personalDataTypes = GetPersonalDataTypes(model);
            if (personalDataTypes != PdfPersonalDataTypes.None)
                pdfRedactor.RedactPersonalData(personalDataTypes);

            // Remove the information that can remain in the document outside the redacted areas
            pdfRedactor.RemoveMetadata = model.RemoveMetadata;
            pdfRedactor.RemoveJavaScript = model.RemoveJavaScript;
            pdfRedactor.RemoveEmbeddedFiles = model.RemoveEmbeddedFiles;
            pdfRedactor.RemoveHiddenContent = model.RemoveHiddenContent;

            byte[] outPdfBuffer;
            string fileName;
            if (model.SaveRedactionMarks)
            {
                // The marked areas are saved as redaction annotations, for a review in a PDF viewer before the
                // content is removed by applying them
                outPdfBuffer = pdfRedactor.SaveRedactionMarks();
                fileName = "Redaction_Marks.pdf";
            }
            else
            {
                // Apply the redactions. The removed content cannot be recovered from the saved document and
                // pdfRedactor.Result gives the number of removed characters, images, annotations and form fields
                outPdfBuffer = pdfRedactor.Save();
                fileName = "Redact_Text_and_Personal_Data.pdf";
            }

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = fileName;
            return fileResult;
        }

        private static PdfColor GetFillColor(string fillColor)
        {
            switch (fillColor)
            {
                case "Black":
                    return PdfColor.Black;
                case "DarkGray":
                    return PdfColor.DarkGray;
                case "White":
                    return PdfColor.White;
                case "None":
                    // The content is removed and the areas are left empty
                    return null;
                default:
                    return PdfColor.Black;
            }
        }

        private static PdfPersonalDataTypes GetPersonalDataTypes(Redact_Text_and_Personal_Data_ViewModel model)
        {
            PdfPersonalDataTypes types = PdfPersonalDataTypes.None;
            if (model.RedactEmailAddresses)
                types |= PdfPersonalDataTypes.EmailAddress;
            if (model.RedactPhoneNumbers)
                types |= PdfPersonalDataTypes.PhoneNumber;
            if (model.RedactIbans)
                types |= PdfPersonalDataTypes.Iban;
            if (model.RedactPaymentCardNumbers)
                types |= PdfPersonalDataTypes.PaymentCardNumber;
            if (model.RedactIpAddresses)
                types |= PdfPersonalDataTypes.IpAddress;
            if (model.RedactSocialSecurityNumbers)
                types |= PdfPersonalDataTypes.UsSocialSecurityNumber;
            if (model.RedactPersonalNumericCodes)
                types |= PdfPersonalDataTypes.RomanianPersonalNumericCode;
            return types;
        }

        private async Task<byte[]> ReadInputPdf(Redact_Text_and_Personal_Data_ViewModel model)
        {
            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    return ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }
            }

            // Otherwise, fall back to the URL
            string pdfUrl = model.PdfFileUrl?.Trim();
            if (string.IsNullOrWhiteSpace(pdfUrl))
                throw new Exception("No PDF file provided: upload a file or specify a URL");

            try
            {
                if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                {
                    string localPath = new Uri(pdfUrl).LocalPath;
                    return await System.IO.File.ReadAllBytesAsync(localPath);
                }

                using var httpClient = new System.Net.Http.HttpClient();
                return await httpClient.GetByteArrayAsync(pdfUrl);
            }
            catch (Exception ex)
            {
                throw new Exception("Could not download the PDF file from URL", ex);
            }
        }

        private Redact_Text_and_Personal_Data_ViewModel SetViewModel()
        {
            var model = new Redact_Text_and_Personal_Data_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder
            {
                Scheme = request.Scheme,
                Host = request.Host.Host,
                Path = request.PathBase.ToString() + request.Path.ToString(),
                Query = request.QueryString.ToString()
            };
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Redact_Text_and_Personal_Data".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Personal_Data_Document.pdf";

            return model;
        }
    }
}
```

## Redact Areas and Pages of PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/redact-areas-and-pages-in-pdf.htm

### Redact Areas

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

// The payment details table of the first page
pdfRedactor.AddRedaction(1, new PdfRectangleF(36, 288, 525, 62));

// The signature, covered with an image that keeps its aspect ratio
PdfRedactionArea signatureArea = pdfRedactor.AddRedaction(1, new PdfRectangleF(50, 505, 200, 60));
signatureArea.SetOverlayImage(System.IO.File.ReadAllBytes("Redacted_Stamp.png"), true);

byte[] redactedPdf = pdfRedactor.Save();
```

### Redact Pages

```csharp
// The second page loses all its content and receives its own overlay text
PdfRedactionArea pageArea = pdfRedactor.RedactPage(2);
pageArea.OverlayText = "PAGE REMOVED";
pageArea.OverlayFontSize = 24;
```

### Code Sample - Redact Areas and Pages

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.Redact_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.Redact_PDF
{
    public class Redact_Areas_and_PagesController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Redact_Areas_and_PagesController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> RedactAreas(Redact_Areas_and_Pages_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] inputPdfBytes = await ReadInputPdf(model);
            string password = !string.IsNullOrEmpty(model.OwnerPassword) ? model.OwnerPassword :
                !string.IsNullOrEmpty(model.UserPassword) ? model.UserPassword : null;

            using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);
            int pageCount = pdfRedactor.PageCount;

            // The color that covers the areas after their content is removed
            pdfRedactor.FillColor = GetFillColor(model.FillColor);

            // The annotations, links and form fields that intersect an area are removed with the content
            pdfRedactor.RemoveIntersectingAnnotations = model.RemoveIntersectingAnnotations;

            byte[] overlayImage = null;
            if (model.DrawOverlayImage)
                overlayImage = System.IO.File.ReadAllBytes(Path.Combine(GetDemoImagesPath(), "Redacted_Stamp.png"));

            // Each area is given by the page number and by its position and size in points, measured from the
            // top left corner of the page. The text, images and graphics inside the area are removed, while the
            // content partially inside the area is cut at the area boundary
            if (!string.IsNullOrWhiteSpace(model.Areas))
            {
                string[] lines = model.Areas.Split(new[] { '\r', '\n' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string line in lines)
                {
                    if (string.IsNullOrWhiteSpace(line))
                        continue;

                    float[] values = ParseArea(line);
                    int pageNumber = (int)values[0];
                    CheckPage(pageNumber, pageCount);

                    PdfRedactionArea area = pdfRedactor.AddRedaction(pageNumber,
                        new PdfRectangleF(values[1], values[2], values[3], values[4]));

                    // An image drawn over the area, resized to fit the area while preserving its aspect ratio
                    if (overlayImage != null)
                        area.SetOverlayImage(overlayImage, true);
                }
            }

            // The whole pages lose all their content, including the content outside the visible area of the page,
            // and receive their own overlay text
            if (!string.IsNullOrWhiteSpace(model.PagesToRedact))
            {
                foreach (int pageNumber in ParsePageRanges(model.PagesToRedact, pageCount))
                {
                    PdfRedactionArea pageArea = pdfRedactor.RedactPage(pageNumber);
                    if (!string.IsNullOrWhiteSpace(model.PageOverlayText))
                    {
                        pageArea.OverlayText = model.PageOverlayText.Trim();
                        pageArea.OverlayTextColor = model.FillColor == "White" ? PdfColor.Black : PdfColor.White;
                        pageArea.OverlayFontSize = 24;
                    }
                }
            }

            pdfRedactor.RemoveMetadata = model.RemoveMetadata;

            // Apply the redactions. The removed content cannot be recovered from the saved document
            byte[] outPdfBuffer = pdfRedactor.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Redact_Areas_and_Pages.pdf";
            return fileResult;
        }

        // An area is written as page, x, y, width, height
        private static float[] ParseArea(string line)
        {
            string[] parts = line.Split(',');
            if (parts.Length != 5)
                throw new Exception("Each area must be written as page, x, y, width, height");

            float[] values = new float[5];
            for (int i = 0; i < 5; i++)
            {
                if (!float.TryParse(parts[i].Trim(), NumberStyles.Float, CultureInfo.InvariantCulture, out values[i]))
                    throw new Exception("The area '" + line.Trim() + "' contains an invalid number");
            }

            if (values[3] <= 0 || values[4] <= 0)
                throw new Exception("The width and height of the area '" + line.Trim() + "' must be positive");

            return values;
        }

        // Page ranges are written as 2,4-5 or 3- (from page 3 to the end)
        private static List<int> ParsePageRanges(string pageRanges, int pageCount)
        {
            var pages = new List<int>();
            foreach (string range in pageRanges.Split(',', StringSplitOptions.RemoveEmptyEntries))
            {
                string[] bounds = range.Trim().Split('-');
                if (bounds.Length > 2 || !int.TryParse(bounds[0], out int first))
                    throw new Exception("The page range '" + range.Trim() + "' is not valid");

                int last = first;
                if (bounds.Length == 2)
                {
                    if (bounds[1].Trim().Length == 0)
                        last = pageCount;
                    else if (!int.TryParse(bounds[1], out last))
                        throw new Exception("The page range '" + range.Trim() + "' is not valid");
                }

                CheckPage(first, pageCount);
                CheckPage(last, pageCount);
                for (int page = first; page <= last; page++)
                {
                    if (!pages.Contains(page))
                        pages.Add(page);
                }
            }

            return pages;
        }

        private static void CheckPage(int pageNumber, int pageCount)
        {
            if (pageNumber < 1 || pageNumber > pageCount)
                throw new Exception("The page number must be between 1 and " + pageCount);
        }

        private static PdfColor GetFillColor(string fillColor)
        {
            switch (fillColor)
            {
                case "Black":
                    return PdfColor.Black;
                case "DarkGray":
                    return PdfColor.DarkGray;
                case "White":
                    return PdfColor.White;
                case "None":
                    // The content is removed and the areas are left empty
                    return null;
                default:
                    return PdfColor.Black;
            }
        }

        private async Task<byte[]> ReadInputPdf(Redact_Areas_and_Pages_ViewModel model)
        {
            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    return ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }
            }

            // Otherwise, fall back to the URL
            string pdfUrl = model.PdfFileUrl?.Trim();
            if (string.IsNullOrWhiteSpace(pdfUrl))
                throw new Exception("No PDF file provided: upload a file or specify a URL");

            try
            {
                if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                {
                    string localPath = new Uri(pdfUrl).LocalPath;
                    return await System.IO.File.ReadAllBytesAsync(localPath);
                }

                using var httpClient = new System.Net.Http.HttpClient();
                return await httpClient.GetByteArrayAsync(pdfUrl);
            }
            catch (Exception ex)
            {
                throw new Exception("Could not download the PDF file from URL", ex);
            }
        }

        private Redact_Areas_and_Pages_ViewModel SetViewModel()
        {
            var model = new Redact_Areas_and_Pages_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder
            {
                Scheme = request.Scheme,
                Host = request.Host.Host,
                Path = request.PathBase.ToString() + request.Path.ToString(),
                Query = request.QueryString.ToString()
            };
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Redact_Areas_and_Pages".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Personal_Data_Document.pdf";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoImagesPath() => Path.Combine(GetDemoFilesPath(), "Image_Files");
    }
}
```

## Apply Redaction Annotations in PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/apply-redaction-annotations-in-pdf.htm

### Apply Redaction Annotations

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

// The areas marked in a PDF viewer
int markedCount = pdfRedactor.AddRedactionAnnotations();

// Other redactions can be added to the same operation
pdfRedactor.RedactPersonalData(PdfPersonalDataTypes.All);

byte[] redactedPdf = pdfRedactor.Save();
```

### Create Redaction Marks

```csharp
// First step: the marks, saved for a review
using (PdfRedactor marker = new PdfRedactor(inputPdfBytes, password))
{
    marker.OverlayText = "REDACTED";
    marker.RedactPersonalData(PdfPersonalDataTypes.All);
    marker.SaveRedactionMarksToFile("Document_with_Redaction_Marks.pdf");
}

// Second step: the reviewed marks, applied
using (PdfRedactor pdfRedactor = new PdfRedactor("Document_with_Redaction_Marks.pdf"))
{
    pdfRedactor.AddRedactionAnnotations();
    pdfRedactor.SaveToFile("Redacted_Document.pdf");
}
```

### Code Sample - Apply Redaction Annotations

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.Redact_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.Redact_PDF
{
    public class Apply_Redaction_AnnotationsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Apply_Redaction_AnnotationsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ApplyRedactions(Apply_Redaction_Annotations_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] inputPdfBytes = await ReadInputPdf(model);
            string password = !string.IsNullOrEmpty(model.OwnerPassword) ? model.OwnerPassword :
                !string.IsNullOrEmpty(model.UserPassword) ? model.UserPassword : null;

            using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

            // The areas marked with redaction annotations in a PDF viewer are redacted with the fill color and
            // the overlay text set in each annotation, then the annotations are removed from the document
            int markedCount = pdfRedactor.AddRedactionAnnotations();
            if (markedCount == 0 && !model.RedactPersonalData)
                throw new Exception("The PDF document does not contain redaction annotations");

            // The personal data found in the document can be redacted in the same operation
            if (model.RedactPersonalData)
                pdfRedactor.RedactPersonalData(PdfPersonalDataTypes.All);

            pdfRedactor.RemoveMetadata = model.RemoveMetadata;
            pdfRedactor.RemoveJavaScript = model.RemoveJavaScript;

            // Apply the redactions. The removed content cannot be recovered from the saved document
            byte[] outPdfBuffer = pdfRedactor.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Apply_Redaction_Annotations.pdf";
            return fileResult;
        }

        private async Task<byte[]> ReadInputPdf(Apply_Redaction_Annotations_ViewModel model)
        {
            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    return ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }
            }

            // Otherwise, fall back to the URL
            string pdfUrl = model.PdfFileUrl?.Trim();
            if (string.IsNullOrWhiteSpace(pdfUrl))
                throw new Exception("No PDF file provided: upload a file or specify a URL");

            try
            {
                if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                {
                    string localPath = new Uri(pdfUrl).LocalPath;
                    return await System.IO.File.ReadAllBytesAsync(localPath);
                }

                using var httpClient = new System.Net.Http.HttpClient();
                return await httpClient.GetByteArrayAsync(pdfUrl);
            }
            catch (Exception ex)
            {
                throw new Exception("Could not download the PDF file from URL", ex);
            }
        }

        private Apply_Redaction_Annotations_ViewModel SetViewModel()
        {
            var model = new Apply_Redaction_Annotations_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder
            {
                Scheme = request.Scheme,
                Host = request.Host.Host,
                Path = request.PathBase.ToString() + request.Path.ToString(),
                Query = request.QueryString.ToString()
            };
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Apply_Redaction_Annotations".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Document_with_Redaction_Marks.pdf";

            return model;
        }
    }
}
```
