# Merging and combining PDFs: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Convert Multiple HTML to a Single PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/merge-multiple-html-to-pdf.htm

### Code Sample - Convert Multiple HTML to a Single PDF

```csharp
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Merge_Multiple_HTML_to_PDFController : Controller
    {
        public IActionResult Index()
        {
            var model = new Merge_Multiple_HTML_to_PDF_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult MergePdf(Merge_Multiple_HTML_to_PDF_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // The PDF document that receives the HTML documents: its page size and margins apply to all of them
            PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
            {
                PageSize = SelectedPdfPageSize(model.PdfPageSize),
                PageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation),
                Margins = new PdfMargins(model.LeftMargin, model.RightMargin, model.TopMargin, model.BottomMargin)
            };

            using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);

            // Set the PDF document security options
            SetPdfSecurityOptions(pdfDocument, model);

            // One table of contents for the whole document, generated when the document is saved from the headings
            // of all the HTML documents added to it
            bool documentToc = model.TableOfContents == "Document";
            pdfDocument.TableOfContents.Enabled = documentToc;

            bool startOnNewPage = model.SecondDocumentStart == "NewPage";

            // Create the list of URLs to convert
            List<string> urlsToConvert = new List<string>() {
                model.FirstUrl,
                model.SecondUrl
            };

            // The position where the next HTML document starts in the current page
            float crtYPos = 0;

            for (int i = 0; i < urlsToConvert.Count; i++)
            {
                if (i > 0 && (startOnNewPage || crtYPos >= pdfDocument.ContentHeight))
                {
                    // The next HTML document starts at the top of a new page, as asked or because the previous
                    // one ended at the bottom of its last page
                    pdfDocument.AddPage();
                    crtYPos = 0;
                }

                // The HTML document starts at the current position and continues on as many new pages as it needs
                PdfHtmlElement htmlElement = new PdfHtmlElement(urlsToConvert[i]);
                htmlElement.Y = crtYPos;

                // Set the HTML to PDF Converter options of the element
                SetHtmlToPdfConverterOptions(htmlElement.Converter, pdfDocument, model);

                if (model.TableOfContents == "EachDocument")
                {
                    // Each HTML document has its own table of contents, inline at its start or on separate pages
                    // before it. The page numbers of the table count the pages already in the document
                    htmlElement.Converter.PdfDocumentOptions.GenerateTableOfContents = true;
                    htmlElement.Converter.PdfDocumentOptions.TableOfContents.CreateInline = !startOnNewPage || model.TocPlacement == "Inline";
                    htmlElement.Converter.PdfDocumentOptions.TableOfContents.PageNumbersOffset = pdfDocument.PageNumber - 1;
                }

                // Render the HTML document in the PDF document
                PdfHtmlRenderInfo renderInfo = pdfDocument.AddHtml(htmlElement);

                // The next HTML document starts below the rendered content of the last page
                crtYPos = renderInfo.LastPageRectangle.Bounds.Bottom;
            }

            // Save the PDF document in a memory buffer
            byte[] outPdfBuffer = pdfDocument.Save();

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Convert_Multiple_HTML.pdf";
            return fileResult;
        }

        private void SetHtmlToPdfConverterOptions(HtmlToPdfConverter htmlToPdfConverter, PdfDocument pdfDocument, Merge_Multiple_HTML_to_PDF_ViewModel model)
        {
            // Set the initial HTML viewer height in pixels
            if (model.HtmlViewerHeight.HasValue)
                htmlToPdfConverter.HtmlViewerHeight = model.HtmlViewerHeight.Value;

            // Set the page layout: how the width at which the HTML is laid out relates to the width of the document pages.
            // The pages of the element have the size, the orientation and the margins of the document
            PdfPageSize pageSize = pdfDocument.PageSize;
            switch (model.PageLayout)
            {
                case "LayoutAtPageWidth":
                    // The HTML is laid out at the content width of the page, one CSS pixel being 0.75 points.
                    // For HTML templates designed for the paper size
                    htmlToPdfConverter.LayoutAtPageWidth(pageSize);
                    break;
                default:
                    // The HTML is laid out as in a browser window of the given width and the result is scaled to the
                    // content width of the page, so a responsive site keeps its desktop layout. This is the default
                    // layout of the converter, with a 1024 pixel window
                    htmlToPdfConverter.FitBrowserWindowToPage(pageSize, windowWidth: model.HtmlViewerWidth);
                    break;
            }

            // Enable the creation of a hierarchy of bookmarks from H1 to H6 tags. The bookmarks of all the HTML documents
            // join the outline of the PDF document
            htmlToPdfConverter.PdfDocumentOptions.GenerateDocumentOutline = model.AutoBookmarks;

            // Set the maximum time in seconds to wait for HTML page to be loaded
            // Leave it not set for a default 120 seconds maximum wait time
            htmlToPdfConverter.NavigationTimeout = model.NavigationTimeout;

            // Set an adddional delay in seconds to wait for JavaScript or AJAX calls after page load completed
            // Set this property to 0 if you don't need to wait for such asynchcronous operations to finish
            if (model.ConversionDelay.HasValue)
                htmlToPdfConverter.ConversionDelay = model.ConversionDelay.Value;
        }

        private void SetPdfSecurityOptions(PdfDocument pdfDocument, Merge_Multiple_HTML_to_PDF_ViewModel model)
        {
            PdfSecurityOptions securityOptions = pdfDocument.PdfSecurityOptions;

            // Set the encryption algorithm and the encryption key size if they are not the default ones
            if (model.EncryptionKey != "Bit128" || model.EncryptionType != "RC4")
            {
                // set the encryption algorithm
                securityOptions.EncryptionAlgorithm = model.EncryptionType == "RC4" ? EncryptionAlgorithm.RC4 : EncryptionAlgorithm.AES;

                // set the encryption key size
                if (model.EncryptionKey == "Bit40")
                    securityOptions.KeySize = EncryptionKeySize.EncryptKey40Bit;
                else if (model.EncryptionKey == "Bit128")
                    securityOptions.KeySize = EncryptionKeySize.EncryptKey128Bit;
                else if (model.EncryptionKey == "Bit256")
                    securityOptions.KeySize = EncryptionKeySize.EncryptKey256Bit;
            }

            // Set user and owner passwords
            if (!string.IsNullOrEmpty(model.UserPassword))
                securityOptions.UserPassword = model.UserPassword;

            if (!string.IsNullOrEmpty(model.OwnerPassword))
                securityOptions.OwnerPassword = model.OwnerPassword;

            // Set PDF document permissions
            securityOptions.CanPrint = model.PrintEnabled;
            securityOptions.CanCopyContent = model.CopyContentEnabled;
            securityOptions.CanCopyAccessibilityContent = model.CopyAccessibilityContentEnabled;
            securityOptions.CanEditContent = model.EditContentEnabled;
            securityOptions.CanEditAnnotations = model.EditAnnotationsEnabled;
            securityOptions.CanFillFormFields = model.FillFormFieldsEnabled;

            if ((PermissionsChanged(securityOptions) || securityOptions.UserPassword.Length > 0) &&
                securityOptions.OwnerPassword.Length == 0)
            {
                // A user password is set but the owner password is not set or the permissions are not the default ones
                // Set a default owner password
                securityOptions.OwnerPassword = "owner";
            }
        }

        private bool PermissionsChanged(PdfSecurityOptions securityOptions)
        {
            return !securityOptions.CanPrint ||
                    !securityOptions.CanCopyContent || !securityOptions.CanCopyAccessibilityContent ||
                    !securityOptions.CanEditContent || !securityOptions.CanEditAnnotations ||
                    !securityOptions.CanFillFormFields;
        }

        private PdfPageSize SelectedPdfPageSize(string selectedValue)
        {
            switch (selectedValue)
            {
                case "A0":
                    return PdfPageSize.A0;
                case "A1":
                    return PdfPageSize.A1;
                case "A2":
                    return PdfPageSize.A2;
                case "A3":
                    return PdfPageSize.A3;
                case "A4":
                    return PdfPageSize.A4;
                case "A5":
                    return PdfPageSize.A5;
                case "A6":
                    return PdfPageSize.A6;
                case "A7":
                    return PdfPageSize.A7;
                case "A8":
                    return PdfPageSize.A8;
                case "A9":
                    return PdfPageSize.A9;
                case "A10":
                    return PdfPageSize.A10;
                case "ArchA":
                    return PdfPageSize.ArchA;
                case "ArchB":
                    return PdfPageSize.ArchB;
                case "ArchC":
                    return PdfPageSize.ArchC;
                case "ArchD":
                    return PdfPageSize.ArchD;
                case "ArchE":
                    return PdfPageSize.ArchE;
                case "B0":
                    return PdfPageSize.B0;
                case "B1":
                    return PdfPageSize.B1;
                case "B2":
                    return PdfPageSize.B2;
                case "B3":
                    return PdfPageSize.B3;
                case "B4":
                    return PdfPageSize.B4;
                case "B5":
                    return PdfPageSize.B5;
                case "Flsa":
                    return PdfPageSize.Flsa;
                case "HalfLetter":
                    return PdfPageSize.HalfLetter;
                case "Ledger":
                    return PdfPageSize.Ledger;
                case "Legal":
                    return PdfPageSize.Legal;
                case "Letter":
                    return PdfPageSize.Letter;
                case "Letter11x17":
                    return PdfPageSize.Letter11x17;
                case "Note":
                    return PdfPageSize.Note;
                default:
                    return PdfPageSize.A4;
            }
        }

        private PdfPageOrientation SelectedPdfPageOrientation(string selectedValue)
        {
            return selectedValue == "Portrait" ? PdfPageOrientation.Portrait : PdfPageOrientation.Landscape;
        }
    }
}
```

## Merge PDF Documents

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/merge-pdf-documents.htm

### Merge Documents and Pages

```csharp
using PdfMerge pdfMerge = new PdfMerge();

// The add methods return the number of the first added page in the merged document
int firstDocumentPage = pdfMerge.AddPdf(firstPdfBytes);

// A blank page between the documents
int blankPage = pdfMerge.AddBlankPage(PdfPageSize.A4, PdfPageOrientation.Portrait);

// Selected pages of the second document, in the order of the ranges
int secondDocumentPage = pdfMerge.AddPdfPages(secondPdfBytes, "1,3-", null);

byte[] mergedPdf = pdfMerge.Save();
```

### Keep the Fields Separate and Add Bookmarks

```csharp
pdfMerge.KeepFieldsSeparate = true;
pdfMerge.KeepSourceBookmarks = true;
pdfMerge.Bookmarks.Add("First document", firstDocumentPage, PdfLinkPageLocation.FitPage());
pdfMerge.Bookmarks.Add("Second document", secondDocumentPage, PdfLinkPageLocation.FitPage());
```

### Set the Open Action and a Footer

```csharp
pdfMerge.PdfDocumentInfo.Title = "Merged Document";
pdfMerge.OpenAction = new PdfGoToPageAction(secondDocumentPage, PdfLinkPageLocation.FitPage());

PdfHtmlTemplate footer = pdfMerge.AddHtmlTemplate(0, 0, 300, 30,
    "<div style=\"font-family: Arial; font-size: 10px;\">Page {page_number} of {total_pages}</div>", null);
footer.HorizontalAlign = PdfTemplateHorizontalAlign.Center;
footer.VerticalAlign = PdfTemplateVerticalAlign.Bottom;
```

### Code Sample - Merge PDF Documents

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Editor;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Editor
{
    public class Merge_PDF_DocumentsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Merge_PDF_DocumentsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> MergePdf(Merge_PDF_Documents_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] firstPdfBytes = await ReadInputPdf(model.FirstPdfFile, model.FirstPdfFileUrl);
            byte[] secondPdfBytes = await ReadInputPdf(model.SecondPdfFile, model.SecondPdfFileUrl);
            string firstPassword = string.IsNullOrEmpty(model.FirstPassword) ? null : model.FirstPassword;
            string secondPassword = string.IsNullOrEmpty(model.SecondPassword) ? null : model.SecondPassword;

            using PdfMerge pdfMerge = new PdfMerge();

            // The bookmarks of the merged documents are copied in the merged document, under the bookmarks added below
            pdfMerge.KeepSourceBookmarks = model.KeepSourceBookmarks;

            // The fields with the same name in several merged documents are renamed, so each copy keeps its own value;
            // otherwise they become one field with the same value in all the copies
            pdfMerge.KeepFieldsSeparate = model.KeepFieldsSeparate;

            // The form fields and annotations of the merged document become static content
            pdfMerge.Flatten = model.FlattenMergedPdf;

            pdfMerge.PdfDocumentInfo.Title = model.DocumentTitle;

            // The first document, all its pages or the pages of the given ranges. The add methods return the number
            // of the first added page in the merged document
            int firstDocumentPage = AddPdf(pdfMerge, firstPdfBytes, model.FirstPageRanges, firstPassword);
            AddBookmark(pdfMerge, model.AddDocumentBookmarks, "First document", firstDocumentPage);

            if (model.AddBlankPage)
            {
                // A blank page of the given size between the documents
                int blankPage = pdfMerge.AddBlankPage(SelectedPdfPageSize(model.BlankPageSize), SelectedPdfPageOrientation(model.BlankPageOrientation));
                AddBookmark(pdfMerge, model.AddDocumentBookmarks, "Blank page", blankPage);
            }

            if (model.AddImagePage)
            {
                // A page displaying an image: the page has the size of the image, or the image is fitted in a page of the given size
                string imagePath = Path.Combine(GetDemoFilesPath(), "Image_Files", "image.jpg");
                int imagePage = model.FitImageInPage
                    ? pdfMerge.AddImagePage(imagePath, PdfPageSize.A4, PdfPageOrientation.Portrait)
                    : pdfMerge.AddImagePage(imagePath);
                AddBookmark(pdfMerge, model.AddDocumentBookmarks, "Image page", imagePage);
            }

            int secondDocumentPage = AddPdf(pdfMerge, secondPdfBytes, model.SecondPageRanges, secondPassword);
            AddBookmark(pdfMerge, model.AddDocumentBookmarks, "Second document", secondDocumentPage);

            if (model.AddSecondPdfTwice)
            {
                int copyPage = AddPdf(pdfMerge, secondPdfBytes, model.SecondPageRanges, secondPassword);
                AddBookmark(pdfMerge, model.AddDocumentBookmarks, "Second document, copy", copyPage);
            }

            if (model.AddHeaderFooter)
            {
                // A template is drawn in all the pages of the merged document; the page number variables are replaced in each page
                PdfHtmlTemplate footer = pdfMerge.AddHtmlTemplate(0, 0, 300, 30,
                    "<div style=\"font-family: Arial; font-size: 10px; color: #606060;\">Merged document, page {page_number} of {total_pages}</div>", null);
                footer.HorizontalAlign = PdfTemplateHorizontalAlign.Center;
                footer.VerticalAlign = PdfTemplateVerticalAlign.Bottom;
            }

            switch (model.OpenAction)
            {
                case "SecondDocument":
                    // The merged document opens at the first page of the second document
                    pdfMerge.OpenAction = new PdfGoToPageAction(secondDocumentPage, PdfLinkPageLocation.FitPage());
                    break;
                case "LastPage":
                    pdfMerge.OpenAction = new PdfGoToPageAction(pdfMerge.PageCount, PdfLinkPageLocation.FitWidth(0));
                    break;
                case "JavaScript":
                    pdfMerge.OpenAction = new PdfJavaScriptAction("app.alert('The merged document has " + pdfMerge.PageCount + " pages');");
                    break;
            }

            byte[] outPdfBuffer = pdfMerge.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Merge_PDF_Documents.pdf";
            return fileResult;
        }

        // Adds all the pages of a document, or the pages of the given ranges, and returns the number of the first
        // added page in the merged document
        private static int AddPdf(PdfMerge pdfMerge, byte[] pdfBytes, string pageRanges, string password)
        {
            if (string.IsNullOrWhiteSpace(pageRanges))
                return pdfMerge.AddPdf(pdfBytes, password);

            return pdfMerge.AddPdfPages(pdfBytes, pageRanges.Trim(), password);
        }

        private static void AddBookmark(PdfMerge pdfMerge, bool addBookmarks, string title, int pageNumber)
        {
            if (addBookmarks)
                pdfMerge.Bookmarks.Add(title, pageNumber, PdfLinkPageLocation.FitPage());
        }

        private static PdfPageSize SelectedPdfPageSize(string selectedValue)
        {
            switch (selectedValue)
            {
                case "A3": return PdfPageSize.A3;
                case "A5": return PdfPageSize.A5;
                case "Letter": return PdfPageSize.Letter;
                case "Legal": return PdfPageSize.Legal;
                case "Ledger": return PdfPageSize.Ledger;
                default: return PdfPageSize.A4;
            }
        }

        private static PdfPageOrientation SelectedPdfPageOrientation(string selectedValue)
        {
            return selectedValue == "Landscape" ? PdfPageOrientation.Landscape : PdfPageOrientation.Portrait;
        }

        private async Task<byte[]> ReadInputPdf(IFormFile pdfFile, string pdfFileUrl)
        {
            // If an uploaded file exists, use it with priority
            if (pdfFile != null && pdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await pdfFile.CopyToAsync(ms);
                    return ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }
            }

            // Otherwise, fall back to the URL
            string pdfUrl = pdfFileUrl?.Trim();
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

        private Merge_PDF_Documents_ViewModel SetViewModel()
        {
            var model = new Merge_PDF_Documents_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Merge_PDF_Documents".Length);

            model.FirstPdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Multi_Page_Document.pdf";
            model.SecondPdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/PDF_Form.pdf";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
    }
}
```
