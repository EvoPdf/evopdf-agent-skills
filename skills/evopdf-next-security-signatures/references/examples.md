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
using System.Security.Cryptography;
using System.Security.Cryptography.X509Certificates;
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

            if (model.KeySource == "Certificate")
            {
                // An X509Certificate2 with access to its private key: a certificate from the Windows certificate
                // store, a smartcard or a USB token. The private key stays in its store and the certificate
                // chain is built from the system stores. The sample loads the PFX as a certificate object
                X509Certificate2 certificate = LoadCertificate(certificateFilePath, "evopdf");
                htmlToPdfConverter.DigitalSignature = new PdfDigitalSignature(certificate);
            }
            else if (model.KeySource == "External")
            {
                // External signing: the library prepares the signature and asks an IExternalDigitalSignature
                // implementation for the signature value, so the private key can stay in a hardware security
                // module or a cloud key service. The signer certificate chain is given separately.
                // This sample implementation signs with the key of the PFX certificate to show the interface
                X509Certificate2 certificate = LoadCertificate(certificateFilePath, "evopdf");
                htmlToPdfConverter.DigitalSignature = new PdfDigitalSignature(new CertificateKeySignature(certificate), new[] { certificate.RawData });
            }
            else
            {
                htmlToPdfConverter.DigitalSignature = new PdfDigitalSignature(certificateFilePath, "evopdf");
            }

            // optionally set digital signature field name in PDF document
            htmlToPdfConverter.DigitalSignature.FieldName = "EvoPdf Signature Field";

            // set the digital signature information that will be displayed in the signatures panel in Adobe Reader
            // and also in the signature appearance in PDF if a custom signature text was not explicitly set
            htmlToPdfConverter.DigitalSignature.Reason = model.SignatureReason;
            htmlToPdfConverter.DigitalSignature.Location = model.SignatureLocation;
            htmlToPdfConverter.DigitalSignature.ContactInfo = model.SignatureContact;

            // The PAdES signature level. Auto produces PAdES B-T when a timestamp server is set and
            // PAdES B-B otherwise
            htmlToPdfConverter.DigitalSignature.Profile = model.Profile;

            // An approval signature by default; a certification signature declares the
            // changes the document accepts after signing: none, form filling and signing, or also annotations
            htmlToPdfConverter.DigitalSignature.CertificationLevel = model.CertificationLevel;

            // The timestamp server, for example http://timestamp.digicert.com
            if (!string.IsNullOrWhiteSpace(model.TimestampServerUrl))
                htmlToPdfConverter.DigitalSignature.TimestampServerUrl = model.TimestampServerUrl.Trim();

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

        // Loads the sample PFX as a certificate object. On Windows the private key goes to the machine key
        // store, which needs no user profile, so the code runs in IIS application pools and in Azure App Service;
        // macOS does not support in-memory keys and uses its default key storage
        private static X509Certificate2 LoadCertificate(string certificateFilePath, string password)
        {
            X509KeyStorageFlags keyStorageFlags = OperatingSystem.IsWindows() ? X509KeyStorageFlags.MachineKeySet :
                OperatingSystem.IsMacOS() ? X509KeyStorageFlags.DefaultKeySet : X509KeyStorageFlags.EphemeralKeySet;
#if NET9_0_OR_GREATER
            return X509CertificateLoader.LoadPkcs12FromFile(certificateFilePath, password, keyStorageFlags);
#else
            return new X509Certificate2(certificateFilePath, password, keyStorageFlags);
#endif
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

    // Sample IExternalDigitalSignature implementation: produces the RSA signature value with the
    // private key of an X509Certificate2. A production implementation forwards the message to the device or
    // service that holds the key and returns the signature value it produces
    public class CertificateKeySignature : IExternalDigitalSignature
    {
        private readonly X509Certificate2 m_certificate;

        public CertificateKeySignature(X509Certificate2 certificate)
        {
            m_certificate = certificate;
        }

        public string GetSignatureAlgorithmName()
        {
            return "RSA";
        }

        public byte[] Sign(byte[] message)
        {
            using (RSA rsa = m_certificate.GetRSAPrivateKey())
            {
                return rsa.SignData(message, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);
            }
        }
    }
}
```

## Digitally Sign Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-existing-pdf.htm

### Code Sample - Digitally Sign Existing PDF

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
    public class Sign_Existing_PDFController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Sign_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Sign_Existing_PDF_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] inputPdfBytes = null;

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
            }

            string certificateFilePath = m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/Certificates/evopdf.pfx";

            // The digital signature added to the loaded document when it is saved
            PdfDigitalSignature digitalSignature = new PdfDigitalSignature(certificateFilePath, "evopdf");
            digitalSignature.Reason = model.SignatureReason;
            digitalSignature.Location = model.SignatureLocation;
            digitalSignature.ContactInfo = model.SignatureContact;

            // The PAdES signature level. Auto produces PAdES B-T when a timestamp server is set and
            // PAdES B-B otherwise
            digitalSignature.Profile = model.Profile;

            // A certification signature must be the first signature of the document, so it
            // applies to documents that are not signed yet
            digitalSignature.CertificationLevel = model.CertificationLevel;

            // The timestamp server, for example http://timestamp.digicert.com
            if (!string.IsNullOrWhiteSpace(model.TimestampServerUrl))
                digitalSignature.TimestampServerUrl = model.TimestampServerUrl.Trim();

            // When the document already has an empty signature field with this name, for example a form
            // prepared with a field for each signer, the signature is placed in that field, on its page and
            // in its rectangle; otherwise a new field with this name is created
            if (!string.IsNullOrWhiteSpace(model.SignatureFieldName))
                digitalSignature.FieldName = model.SignatureFieldName.Trim();

            // Open the loaded PDF for editing with the digital signature. A document that already
            // carries signatures is saved as an incremental update, so the existing signatures stay valid
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password, digitalSignature);

            digitalSignature.AppearanceEnabled = model.EnableAppearance;
            if (digitalSignature.AppearanceEnabled)
            {
                // set the signature text in appearance or leave it null or empty to display the default signature information
                if (!string.IsNullOrEmpty(model.SignatureText))
                    digitalSignature.Appearance.Text = model.SignatureText;

                // Position of a new signature field: the bottom of the first page, to the right of the
                // signatures already there. An existing signature field keeps its own position
                PdfPageInfo pageInfo = pdfEditor.GetPdfPageInfo(1);
                int signatureCount = pdfEditor.PdfSignatureInfo.SignatureCount;
                int x = 20 + 210 * (signatureCount % 2);
                digitalSignature.Appearance.PageNumber = 1;
                digitalSignature.Appearance.BoundsRectangle = new PdfRectangle(x, (int)pageInfo.MediaBox.Height - 70, 200, 50);
            }

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Sign_Existing_PDF.pdf";
            return fileResult;
        }

        private Sign_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Sign_Existing_PDF_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(
                0, currentPageUrl.Length - "Sign_Existing_PDF".Length);

            // The default input is a document that already carries a digital signature
            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Signed_Document.pdf";

            return model;
        }
    }
}
```

## Add Signature Fields from HTML Elements

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-signature-fields-from-html-elements.htm

### Code Sample - Add Signature Fields from HTML Elements

```csharp
using System;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Signature_Fields_from_HTMLController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Signature_Fields_from_HTMLController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public ActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Signature_Fields_from_HTML_ViewModel model)
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

            // The HTML elements that mark the place of the signature fields. Their position in the
            // PDF pages is available in HtmlElementsInfo after the conversion
            htmlToPdfConverter.HtmlElementsInfoSelector = model.SignatureFieldsSelector;

            // Convert the HTML page to a PDF document
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertHtml(model.HtmlString, model.BaseUrl);

            if (htmlToPdfConverter.HtmlElementsInfo == null || htmlToPdfConverter.HtmlElementsInfo.Elements.Count == 0)
                throw new Exception("No HTML element matches the selector " + model.SignatureFieldsSelector);

            // Add an empty signature field where each placeholder was rendered
            string firstFieldName = null;
            using (PdfEditor pdfEditor = new PdfEditor(outPdfBuffer))
            {
                int fieldIndex = 1;
                foreach (HtmlElementInfo element in htmlToPdfConverter.HtmlElementsInfo.Elements)
                {
                    // The id of the placeholder is the name of the field. A digital signature with this
                    // field name is placed in the field
                    string fieldName = !string.IsNullOrEmpty(element.Id) ? element.Id : "Signature" + fieldIndex;
                    fieldIndex++;
                    if (firstFieldName == null)
                        firstFieldName = fieldName;

                    // The page and the rectangle where the placeholder was rendered
                    PdfRenderedRectangle renderedRectangle = element.RenderedRectangles[0];

                    pdfEditor.AddSignatureField(new PdfSignatureField(fieldName, renderedRectangle.PageNumber, renderedRectangle.Bounds));
                }

                outPdfBuffer = pdfEditor.Save();
            }

            // Sign the first field. The other fields stay empty, each waiting for its signer; the
            // Digitally Sign Existing PDF demo signs them by field name
            if (model.SignFirstField)
            {
                string certificateFilePath = m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/Certificates/evopdf.pfx";

                PdfDigitalSignature digitalSignature = new PdfDigitalSignature(certificateFilePath, "evopdf");
                digitalSignature.FieldName = firstFieldName;
                digitalSignature.Reason = "Approved";
                digitalSignature.Location = "My Signature Location";
                digitalSignature.ContactInfo = "My Contact Information";

                using PdfEditor signingEditor = new PdfEditor(outPdfBuffer, null, digitalSignature);

                // The appearance fills the rectangle of the field
                digitalSignature.AppearanceEnabled = true;
                digitalSignature.Appearance.Text = "Approved by EVO PDF Software";

                outPdfBuffer = signingEditor.Save();
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Signature_Fields_from_HTML.pdf";

            return fileResult;
        }

        private Signature_Fields_from_HTML_ViewModel SetViewModel()
        {
            var model = new Signature_Fields_from_HTML_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Signature_Fields_from_HTML".Length);

            model.HtmlString = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Signature_Fields.html"));
            model.BaseUrl = rootUrl + "DemoAppFiles/Input/HTML_Files/";

            return model;
        }
    }
}
```
