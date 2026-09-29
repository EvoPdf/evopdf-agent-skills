# Encryption, permissions and digital signatures: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Set Permissions and Password of the Generated PDF Document

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/set-pdf-permissions-and-password.htm

### Code Sample - Set Permissions and Password of the Generated PDF Document

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class PDF_SecurityController : Controller
    {
        public ActionResult Index()
        {
            var model = new PDF_Security_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(PDF_Security_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a HTML to PDF converter object with default settings
            HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

            // Set the encryption algorithm and the encryption key size if they are not the default ones
            if (model.EncryptionKey != "Bit128" || model.EncryptionType != "RC4")
            {
                // set the encryption algorithm
                htmlToPdfConverter.PdfSecurityOptions.EncryptionAlgorithm = model.EncryptionType == "RC4" ? EncryptionAlgorithm.RC4 : EncryptionAlgorithm.AES;

                // set the encryption key size
                if (model.EncryptionKey == "Bit40")
                    htmlToPdfConverter.PdfSecurityOptions.KeySize = EncryptionKeySize.EncryptKey40Bit;
                else if (model.EncryptionKey == "Bit128")
                    htmlToPdfConverter.PdfSecurityOptions.KeySize = EncryptionKeySize.EncryptKey128Bit;
                else if (model.EncryptionKey == "Bit256")
                    htmlToPdfConverter.PdfSecurityOptions.KeySize = EncryptionKeySize.EncryptKey256Bit;
            }

            // Set user and owner passwords
            if (!string.IsNullOrEmpty(model.UserPassword))
                htmlToPdfConverter.PdfSecurityOptions.UserPassword = model.UserPassword;

            if (!string.IsNullOrEmpty(model.OwnerPassword))
                htmlToPdfConverter.PdfSecurityOptions.OwnerPassword = model.OwnerPassword;

            // Set PDF document permissions
            htmlToPdfConverter.PdfSecurityOptions.CanPrint = model.PrintEnabled;
            htmlToPdfConverter.PdfSecurityOptions.CanCopyContent = model.CopyContentEnabled;
            htmlToPdfConverter.PdfSecurityOptions.CanCopyAccessibilityContent = model.CopyAccessibilityContentEnabled;
            htmlToPdfConverter.PdfSecurityOptions.CanEditContent = model.EditContentEnabled;
            htmlToPdfConverter.PdfSecurityOptions.CanEditAnnotations = model.EditAnnotationsEnabled;
            htmlToPdfConverter.PdfSecurityOptions.CanFillFormFields = model.FillFormFieldsEnabled;

            if ((PermissionsChanged(htmlToPdfConverter) || htmlToPdfConverter.PdfSecurityOptions.UserPassword.Length > 0) &&
                htmlToPdfConverter.PdfSecurityOptions.OwnerPassword.Length == 0)
            {
                // A user password is set but the owner password is not set or the permissions are not the default ones
                // Set a default owner password
                htmlToPdfConverter.PdfSecurityOptions.OwnerPassword = "owner";
            }

            // Convert the HTML page to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertUrl(model.Url);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Set_Permissions_Password.pdf";

            return fileResult;
        }

        private bool PermissionsChanged(HtmlToPdfConverter htmlToPdfConverter)
        {
            return !htmlToPdfConverter.PdfSecurityOptions.CanPrint ||
                    !htmlToPdfConverter.PdfSecurityOptions.CanCopyContent || !htmlToPdfConverter.PdfSecurityOptions.CanCopyAccessibilityContent ||
                    !htmlToPdfConverter.PdfSecurityOptions.CanEditContent || !htmlToPdfConverter.PdfSecurityOptions.CanEditAnnotations ||
                    !htmlToPdfConverter.PdfSecurityOptions.CanFillFormFields;
        }
    }
}
```

## Add a Digital Signature to Generated PDF Document

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-the-generated-pdf.htm

### Code Sample - Add A Digital Signature to Generated PDF Document

```csharp
using System;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Hosting;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class PDF_Digital_SignaturesController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public PDF_Digital_SignaturesController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public ActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(PDF_Digital_Signatures_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a HTML to PDF converter object with default settings
            HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

            string certificateFilePath = m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/Certificates/evopdf.pfx";
            htmlToPdfConverter.DigitalSignature = new PdfDigitalSignature(certificateFilePath, "evopdf");

            // optionally set digital signature field name in PDF document
            htmlToPdfConverter.DigitalSignature.FieldName = "EvoPdf Signature Field";

            // set the digital signature information that will be displayed in the signatures panel in Adobe Reader
            // and also in the signature appearance in PDF if a custom signature text was not explicitly set
            htmlToPdfConverter.DigitalSignature.Reason = model.SignatureReason;
            htmlToPdfConverter.DigitalSignature.Location = model.SignatureLocation;
            htmlToPdfConverter.DigitalSignature.ContactInfo = model.SignatureContact;

            // Uncomment the line below to optionally set a timestamp from a certified timestamp server
            //htmlToPdfConverter.DigitalSignature.TimestampServerUrl = "http://tsa.belgium.be/connect";

            // enable signature appearance in PDF page
            htmlToPdfConverter.DigitalSignature.AppearanceEnabled = model.EnableAppearance;

            if (htmlToPdfConverter.DigitalSignature.AppearanceEnabled)
            {
                // set the digital signature appearance position in PDF document
                if (model.DisplayOnLastPage)
                {
                    // set the appearance to be displayed at the bottom of the last page
                    htmlToPdfConverter.DigitalSignature.Appearance.DisplayOnLastPage = true;
                    htmlToPdfConverter.DigitalSignature.Appearance.BoundsRectangle =
                            new PdfRectangle(0, htmlToPdfConverter.PdfDocumentOptions.PdfPageSize.Height - 50, 200, 50);

                    // optionally reserve space for signature appearance at the bottom of the PDF page
                    htmlToPdfConverter.PdfDocumentOptions.BottomMargin = 50;
                }
                else
                {
                    // set the appearance to be displayed at the top of the first page
                    htmlToPdfConverter.DigitalSignature.Appearance.PageNumber = 1;
                    htmlToPdfConverter.DigitalSignature.Appearance.BoundsRectangle = new PdfRectangle(0, 0, 200, 50);

                    // optionally reserve space for signature appearance at the top of the PDF page
                    htmlToPdfConverter.PdfDocumentOptions.TopMargin = 50;
                }

                // set the signature text in appearance or leave it null or empty to display the default signature information
                if (model.AddSignatureText && !string.IsNullOrEmpty(model.SignatureText))
                    htmlToPdfConverter.DigitalSignature.Appearance.Text = model.SignatureText;

                // set the signature image in appearance
                if (model.AddSignatureImage)
                {
                    string imageFilePath = m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/Images/evologo.png";
                    htmlToPdfConverter.DigitalSignature.Appearance.SetImage(imageFilePath, true);
                }
            }

            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlWithForm = model.HtmlString;
                string baseUrl = model.BaseUrl;

                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlWithForm, baseUrl);
            }
            else
            {
                string url = model.Url;

                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Digital_Signatures.pdf";

            return fileResult;
        }

        private PDF_Digital_Signatures_ViewModel SetViewModel()
        {
            var model = new PDF_Digital_Signatures_ViewModel();

            var contentRootPath = m_hostingEnvironment.ContentRootPath + "/wwwroot";

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "PDF_Digital_Signatures".Length);

            model.Url = "http://www.evopdf.com";
            model.HtmlString = "Enter the <b>HTML String to Convert</b> and optionally set a <b>Base URL</b> if the HTML string references external resources by relative URLs";
            model.BaseUrl = rootUrl;

            model.SignatureReason = "My Signature Reason";
            model.SignatureLocation = "My Signature Location";
            model.SignatureContact = "My Contact Information";
            model.EnableAppearance = true;
            model.DisplayOnLastPage = false;
            model.AddSignatureText = true;
            model.SignatureText = "Signed by EVO PDF Software";
            model.AddSignatureImage = true;

            return model;
        }
    }
}
```
