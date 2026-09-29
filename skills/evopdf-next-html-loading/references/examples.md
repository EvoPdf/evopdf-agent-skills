# Loading the HTML page: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Convert HTML Pages with Authentication

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-pages-with-authentication.htm

### Code Sample - Explicitly Set the Authentication Options

```csharp
// Create the HTML to PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set authentication options
htmlToPdfConverter.AuthenticationOptions.Username = username;
htmlToPdfConverter.AuthenticationOptions.Password = password;            

// Create the HTML to Image converter
HtmlToImageConverter htmlToImageConverter = new HtmlToImageConverter();
// Set authentication options
htmlToImageConverter.AuthenticationOptions.Username = username;
htmlToImageConverter.AuthenticationOptions.Password = password;
```

### Code Sample - Explicitly Set the Forms Authentication Cookie

```csharp
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

// Add the authentication cookie to request
htmlToPdfConverter.HttpRequestCookies.Add(AuthCookieName, AuthCookieValue);

htmlToPdfConverter.ConvertUrl(urlToConvert);
```

## Add Cookies to HTML Page Request

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-cookies-to-html-page-request.htm

### Code Sample - Add Cookies to HTML Page Request

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Add_Cookies_to_RequestController : Controller
    {
        public IActionResult Index()
        {
            var model = new Add_Cookies_to_Request_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Add_Cookies_to_Request_ViewModel model)
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

            // Add custom HTTP cookies
            // The caller must provide a valid cookie name and RFC 6265-encoded value

            if (!string.IsNullOrEmpty(model.Cookie1Name) && !string.IsNullOrEmpty(model.Cookie1Value))
                htmlToPdfConverter.HttpRequestCookies.Add(model.Cookie1Name, model.Cookie1Value);

            if (!string.IsNullOrEmpty(model.Cookie2Name) && !string.IsNullOrEmpty(model.Cookie2Value))
                htmlToPdfConverter.HttpRequestCookies.Add(model.Cookie2Name, model.Cookie2Value);

            if (!string.IsNullOrEmpty(model.Cookie3Name) && !string.IsNullOrEmpty(model.Cookie3Value))
                htmlToPdfConverter.HttpRequestCookies.Add(model.Cookie3Name, model.Cookie3Value);

            if (!string.IsNullOrEmpty(model.Cookie4Name) && !string.IsNullOrEmpty(model.Cookie4Value))
                htmlToPdfConverter.HttpRequestCookies.Add(model.Cookie4Name, model.Cookie4Value);

            if (!string.IsNullOrEmpty(model.Cookie5Name) && !string.IsNullOrEmpty(model.Cookie5Value))
                htmlToPdfConverter.HttpRequestCookies.Add(model.Cookie5Name, model.Cookie5Value);

            // Convert the HTML page to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertUrl(model.Url);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "HTTP_Cookies.pdf";

            return fileResult;
        }
    }
}
```

## Add HTTP Headers to HTML Page Request

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-http-headers-to-html-page-request.htm

### Code Sample - Add HTTP Headers to HTML Page Request

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Add_HTTP_Headers_to_RequestController : Controller
    {
        public IActionResult Index()
        {
            var model = new Add_HTTP_Headers_to_Request_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Add_HTTP_Headers_to_Request_ViewModel model)
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

            // Set the persistent HTTP Headers option to control the inclusion of headers 
            // for each requested resource in the HTML document
            htmlToPdfConverter.PersistentHttpRequestHeaders = model.PersistentHttpHeaders;

            // Add custom HTTP headers
            // The caller must provide a valid HTTP header name and printable ASCII value

            if (!string.IsNullOrEmpty(model.Header1Name) && !string.IsNullOrEmpty(model.Header1Value))
                htmlToPdfConverter.HttpRequestHeaders.Add(model.Header1Name, model.Header1Value);

            if (!string.IsNullOrEmpty(model.Header2Name) && !string.IsNullOrEmpty(model.Header2Value))
                htmlToPdfConverter.HttpRequestHeaders.Add(model.Header2Name, model.Header2Value);

            if (!string.IsNullOrEmpty(model.Header3Name) && !string.IsNullOrEmpty(model.Header3Value))
                htmlToPdfConverter.HttpRequestHeaders.Add(model.Header3Name, model.Header3Value);

            if (!string.IsNullOrEmpty(model.Header4Name) && !string.IsNullOrEmpty(model.Header4Value))
                htmlToPdfConverter.HttpRequestHeaders.Add(model.Header4Name, model.Header4Value);

            if (!string.IsNullOrEmpty(model.Header5Name) && !string.IsNullOrEmpty(model.Header5Value))
                htmlToPdfConverter.HttpRequestHeaders.Add(model.Header5Name, model.Header5Value);

            // Convert the HTML page to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertUrl(model.Url);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "HTTP_Headers.pdf";

            return fileResult;
        }
    }
}
```

## Access a HTML Page Using GET and POST HTTP Methods

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/access-html-pages-with-get-and-post.htm

### Code Sample - Access a HTML Page Using GET and POST HTTP Methods

```csharp
using System;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models; 
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class GET_and_POST_HTTP_MethodsController : Controller
    {
        public IActionResult Index()
        {
            var model = new GET_and_POST_HTTP_Methods_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(GET_and_POST_HTTP_Methods_ViewModel model)
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

            // The POST field names and values are automatically form-urlencoded by the converter
            // Set the EncodeHttpPostFields property to false if they are already encoded by the caller
            // The parameters transmitted in query string when using the GET method must be URL-encoded by the caller

            string param1Name = !string.IsNullOrEmpty(model.Param1Name) ? model.Param1Name : "param1";
            string param1Value = !string.IsNullOrEmpty(model.Param1Value) ? model.Param1Value : "Value1";

            string param2Name = !string.IsNullOrEmpty(model.Param2Name) ? model.Param2Name : "param2";
            string param2Value = !string.IsNullOrEmpty(model.Param2Value) ? model.Param2Value : "Value2";

            string param3Name = !string.IsNullOrEmpty(model.Param3Name) ? model.Param3Name : "param3";
            string param3Value = !string.IsNullOrEmpty(model.Param3Value) ? model.Param3Value : "Value3";

            string param4Name = !string.IsNullOrEmpty(model.Param4Name) ? model.Param4Name : "param4";
            string param4Value = !string.IsNullOrEmpty(model.Param4Value) ? model.Param4Value : "Value4";

            string param5Name = !string.IsNullOrEmpty(model.Param5Name) ? model.Param5Name : "param5";
            string param5Value = !string.IsNullOrEmpty(model.Param5Value) ? model.Param5Value : "Value5";

            string urlToConvert = model.Url;

            if (model.HttpMethod == "Post")
            {
                htmlToPdfConverter.HttpPostFields.Add(param1Name, param1Value);
                htmlToPdfConverter.HttpPostFields.Add(param2Name, param2Value);
                htmlToPdfConverter.HttpPostFields.Add(param3Name, param3Value);
                htmlToPdfConverter.HttpPostFields.Add(param4Name, param4Value);
                htmlToPdfConverter.HttpPostFields.Add(param5Name, param5Value);
            }
            else
            {
                Uri getMethodUri = new Uri(model.Url);

                string query = (getMethodUri.Query.Length > 0 ? "&" : "?") + String.Format("{0}={1}", param1Name, param1Value);
                query += String.Format("&{0}={1}", param2Name, param2Value);
                query += String.Format("&{0}={1}", param3Name, param3Value);
                query += String.Format("&{0}={1}", param4Name, param4Value);
                query += String.Format("&{0}={1}", param5Name, param5Value);

                urlToConvert = model.Url + query;
            }

            // Convert the HTML page to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertUrl(urlToConvert);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "GET_and_POST.pdf";

            return fileResult;
        }
    }
}
```

## Convert a HTML Page to PDF in Same Session

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-page-to-pdf-in-same-session.htm

### Code Sample - Convert a HTML Page to PDF in Same Session

```csharp
using System;
using System.IO;
using System.ComponentModel.DataAnnotations;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc.ViewFeatures;
using Microsoft.AspNetCore.Mvc.ViewEngines;
using Microsoft.AspNetCore.Mvc.Rendering;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Convert_Page_in_Same_SessionController : Controller
    {
        private ICompositeViewEngine m_viewEngine;

        public Convert_Page_in_Same_SessionController(ICompositeViewEngine viewEngine)
        {
            m_viewEngine = viewEngine;
        }

        // GET: Convert_Page_in_Same_Session
        public ActionResult Index()
        {
            var model = new Convert_Page_in_Same_Session_ViewModel();

            return View(model);
        }

        public ActionResult Display_Session_Variables()
        {
            return View();
        }

        [HttpPost]
        public ActionResult ConvertPageInSameSessionToPdf(Convert_Page_in_Same_Session_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            ViewDataDictionary viewData = new ViewDataDictionary(ViewData);
            viewData.Clear();
            viewData.Model = model;

            // The string writer where to render the HTML code of the view
            StringWriter stringWriter = new StringWriter();

            // Render the Index view in a HTML string
            ViewEngineResult viewResult = m_viewEngine.FindView(ControllerContext, "Display_Session_Variables", false);
            ViewContext viewContext = new ViewContext(
                    ControllerContext,
                    viewResult.View,
                    viewData,
                    TempData,
                    stringWriter,
                    new HtmlHelperOptions()
                    );
            Task renderTask = viewResult.View.RenderAsync(viewContext);
            renderTask.Wait();

            // Get the view HTML string
            string htmlToConvert = stringWriter.ToString();

            // Get the base URL

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string baseUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Convert_Page_in_Same_Session/ConvertPageInSameSessionToPdf".Length);

            // Create a HTML to PDF converter object with default settings
            HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

            // Convert the HTML string to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlToConvert, baseUrl);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Convert_Page_in_Same_Session.pdf";

            return fileResult;
        }
    }
}
```

### Code Sample - Display Session Variables in Converted HTML Page

```html
@{
    Layout = null;
}

<!DOCTYPE html>

<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <base href="@Url.Content("~/")" />
    <link href="img/evo.ico" rel="shortcut icon" />
    <link href="styles/styles.css" type="text/css" rel="stylesheet">
    <title>Display Session Variables</title>
</head>
<body>
    <div>
        <b style="font-size: 16px">The session variables</b><br />
        <br />
        <b>First Name:</b>&nbsp;<span id="firstNameLabel">@(ViewData["firstName"] != null ? ViewData["firstName"].ToString() : String.Empty)</span><br />
        <b>Last Name:</b>&nbsp;<span id="lastNameLabel">@(ViewData["lastName"] != null ? ViewData["lastName"].ToString() : String.Empty)</span><br />
        <b>Gender:</b>&nbsp;<span id="genderLabel">@(ViewData["gender"] != null && ViewData["gender"].ToString() == "maleRadioButton" ? "Male" : "Female")</span><br />
        <b>I have a car:</b>&nbsp;<span id="haveCarLabel">@(ViewData["haveCar"] != null && ViewData["haveCar"].ToString() != "false" ? "Yes" : "No")</span><br />
        <div id="carTypePanel" style="display : @(ViewData["haveCar"] != null && ViewData["haveCar"].ToString() == "false" ? "none" : "inline")">
            <b>Car Type:</b>&nbsp;<span id="carTypeLabel">@(ViewData["carType"] != null ? ViewData["carType"].ToString() : String.Empty)</span><br />
        </div>
        <b>Comments:</b>&nbsp;<span id="commentsLabel">@(ViewData["comments"] != null ? ViewData["comments"].ToString() : String.Empty)</span><br />
    </div>
</body>
</html>
```

## Convert HTML with Web Fonts to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-with-web-fonts-to-pdf.htm

### Code Sample - Convert HTML with Web Fonts to PDF

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Web_Fonts_to_PDFController : Controller
    {
        // GET: Web_Fonts_to_PDF
        public ActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Web_Fonts_to_PDF_ViewModel model)
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

            // Convert the HTML page with Web Fonts to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertUrl(model.Url);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Web_Fonts_to_PDF.pdf";

            return fileResult;
        }

        private Web_Fonts_to_PDF_ViewModel SetViewModel()
        {
            var model = new Web_Fonts_to_PDF_ViewModel();
            return model;
        }
    }
}
```
