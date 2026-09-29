# HTML to PDF: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## HTML to PDF Converter Overview

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-converter-overview.htm

### Code Sample - Convert HTML to PDF with HtmlToPdfConverter Class

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
    public class HTML_to_PDF_Getting_StartedController : Controller
    {
        // GET: Getting_Started
        public ActionResult Index()
        {
            var model = new HTML_to_PDF_Getting_Started_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(HTML_to_PDF_Getting_Started_ViewModel model)
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

            // Set the initial HTML viewer height in pixels
            if (model.HtmlViewerHeight.HasValue)
                htmlToPdfConverter.HtmlViewerHeight = model.HtmlViewerHeight.Value;

            // Optionally load the lazy images
            htmlToPdfConverter.LoadLazyImages = model.LoadLazyImages;

            // Set the lazy images load mode
            htmlToPdfConverter.LazyImagesLoadMode = model.LazyImagesLoadMode == "Browser" ?
                LazyImagesLoadMode.Browser : LazyImagesLoadMode.Custom;

            // JavaScript in the converted page; some options of the converter turn it on when they need it

            htmlToPdfConverter.JavaScriptEnabled = model.JavaScriptEnabled;

            // Set the page layout: how the width at which the HTML is laid out relates to the PDF page width
            PdfPageSize pageSize = SelectedPdfPageSize(model.PdfPageSize);
            PdfPageOrientation pageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation);

            switch (model.PageLayout)
            {
                case "FitBrowserWindowToPage":
                    // Fixed page size: the HTML is laid out as in a browser window of the given width and the result
                    // is scaled to the content width of the page, so a responsive site keeps its desktop layout.
                    // This is the default layout of the converter, with an A4 page and a 1024 pixel window
                    htmlToPdfConverter.FitBrowserWindowToPage(pageSize, pageOrientation, windowWidth: model.HtmlViewerWidth, singlePage: model.SinglePage);
                    break;

                case "LayoutAtPageWidth":
                    // Fixed page size: the HTML is laid out at the content width of the page, one CSS pixel
                    // being 0.75 points. For HTML templates designed for the paper size
                    htmlToPdfConverter.LayoutAtPageWidth(pageSize, pageOrientation, zoom: model.HtmlViewerZoom, singlePage: model.SinglePage);
                    break;

                case "PrintLikeChrome":
                    // The output of the Save as PDF command of Chrome: the print media type, 1 cm margins, no
                    // background colors or images, drawn at the zoom; the margins and the backgrounds set below
                    // replace the ones of Chrome when they were changed in the form
                    htmlToPdfConverter.PrintLikeChrome(pageSize, pageOrientation, zoom: model.HtmlViewerZoom, singlePage: model.SinglePage);
                    break;

                default:
                    // The PDF page width follows the browser window width and the HTML is drawn at the zoom, 1:1 at 100;
                    // the page height comes from the page size and the orientation
                    htmlToPdfConverter.PageWidthFromBrowserWindow(model.HtmlViewerWidth, singlePage: model.SinglePage, zoom: model.HtmlViewerZoom);
                    htmlToPdfConverter.PdfDocumentOptions.PdfPageSize = pageSize;
                    htmlToPdfConverter.PdfDocumentOptions.PdfPageOrientation = pageOrientation;
                    break;
            }

            // The page margins in points, after the layout, so that they replace the ones a layout sets. The default is 0

            htmlToPdfConverter.PdfDocumentOptions.LeftMargin = model.LeftMargin;

            htmlToPdfConverter.PdfDocumentOptions.RightMargin = model.RightMargin;

            htmlToPdfConverter.PdfDocumentOptions.TopMargin = model.TopMargin;

            htmlToPdfConverter.PdfDocumentOptions.BottomMargin = model.BottomMargin;

            // The background colors and images of the HTML, printed or not
            htmlToPdfConverter.PdfDocumentOptions.PrintBackgrounds = model.PrintBackgrounds;

            // The media type used in @media rules, after the layout, so that it replaces the one a layout sets
            htmlToPdfConverter.MediaType = model.MediaType == "Print" ? "print" : "screen";

            // Sets the PDF standard for the generated document
            // Leave as None to generate a plain PDF without an accessibility structure tree or archival metadata
            htmlToPdfConverter.PdfDocumentOptions.PdfStandard = model.PdfStandard;

            // Set the maximum time, in seconds, to wait for the HTML page to load
            // The default value is 120 seconds
            htmlToPdfConverter.NavigationTimeout = model.NavigationTimeout;

            // Set an additional delay, in seconds, to wait for asynchronous content after the initial load
            // The default value is 0
            if (model.ConversionDelay.HasValue)
                htmlToPdfConverter.ConversionDelay = model.ConversionDelay.Value;

            // The buffer to receive the generated PDF document
            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Url")
            {
                string url = model.Url;

                // Convert the HTML page given by an URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }
            else
            {
                string htmlString = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert a HTML string with a base URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlString, baseUrl);
            }

            // Send the PDF file to browser
            // The zoom the HTML was drawn at, read from the PDF: the zoom of the layout, lower when the browser window
            // grew to the content; in the name of the file
            string printZoom = htmlToPdfConverter.ConversionInfo.PrintZoom.ToString("0.#", System.Globalization.CultureInfo.InvariantCulture);

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            if (!model.OpenInline)
            {
                // send as attachment
                fileResult.FileDownloadName = "HTML_to_PDF_Getting_Started_zoom_" + printZoom + ".pdf";
            }

            return fileResult;
        }

        private PdfPageSize SelectedPdfPageSize(string selectedValue)
        {
            switch (selectedValue)
            {
                case "A0":
                    return PdfPageSize.A0;
                case "A1":
                    return PdfPageSize.A1;
                case "A10":
                    return PdfPageSize.A10;
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

## HTML To PDF Converter Options

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-converter-options.htm

### Code Sample - HTML Content Destination and Scaling in PDF

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
    public class HTML_to_PDF_Getting_StartedController : Controller
    {
        // GET: Getting_Started
        public ActionResult Index()
        {
            var model = new HTML_to_PDF_Getting_Started_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(HTML_to_PDF_Getting_Started_ViewModel model)
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

            // Set the initial HTML viewer height in pixels
            if (model.HtmlViewerHeight.HasValue)
                htmlToPdfConverter.HtmlViewerHeight = model.HtmlViewerHeight.Value;

            // Optionally load the lazy images
            htmlToPdfConverter.LoadLazyImages = model.LoadLazyImages;

            // Set the lazy images load mode
            htmlToPdfConverter.LazyImagesLoadMode = model.LazyImagesLoadMode == "Browser" ?
                LazyImagesLoadMode.Browser : LazyImagesLoadMode.Custom;

            // JavaScript in the converted page; some options of the converter turn it on when they need it

            htmlToPdfConverter.JavaScriptEnabled = model.JavaScriptEnabled;

            // Set the page layout: how the width at which the HTML is laid out relates to the PDF page width
            PdfPageSize pageSize = SelectedPdfPageSize(model.PdfPageSize);
            PdfPageOrientation pageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation);

            switch (model.PageLayout)
            {
                case "FitBrowserWindowToPage":
                    // Fixed page size: the HTML is laid out as in a browser window of the given width and the result
                    // is scaled to the content width of the page, so a responsive site keeps its desktop layout.
                    // This is the default layout of the converter, with an A4 page and a 1024 pixel window
                    htmlToPdfConverter.FitBrowserWindowToPage(pageSize, pageOrientation, windowWidth: model.HtmlViewerWidth, singlePage: model.SinglePage);
                    break;

                case "LayoutAtPageWidth":
                    // Fixed page size: the HTML is laid out at the content width of the page, one CSS pixel
                    // being 0.75 points. For HTML templates designed for the paper size
                    htmlToPdfConverter.LayoutAtPageWidth(pageSize, pageOrientation, zoom: model.HtmlViewerZoom, singlePage: model.SinglePage);
                    break;

                case "PrintLikeChrome":
                    // The output of the Save as PDF command of Chrome: the print media type, 1 cm margins, no
                    // background colors or images, drawn at the zoom; the margins and the backgrounds set below
                    // replace the ones of Chrome when they were changed in the form
                    htmlToPdfConverter.PrintLikeChrome(pageSize, pageOrientation, zoom: model.HtmlViewerZoom, singlePage: model.SinglePage);
                    break;

                default:
                    // The PDF page width follows the browser window width and the HTML is drawn at the zoom, 1:1 at 100;
                    // the page height comes from the page size and the orientation
                    htmlToPdfConverter.PageWidthFromBrowserWindow(model.HtmlViewerWidth, singlePage: model.SinglePage, zoom: model.HtmlViewerZoom);
                    htmlToPdfConverter.PdfDocumentOptions.PdfPageSize = pageSize;
                    htmlToPdfConverter.PdfDocumentOptions.PdfPageOrientation = pageOrientation;
                    break;
            }

            // The page margins in points, after the layout, so that they replace the ones a layout sets. The default is 0

            htmlToPdfConverter.PdfDocumentOptions.LeftMargin = model.LeftMargin;

            htmlToPdfConverter.PdfDocumentOptions.RightMargin = model.RightMargin;

            htmlToPdfConverter.PdfDocumentOptions.TopMargin = model.TopMargin;

            htmlToPdfConverter.PdfDocumentOptions.BottomMargin = model.BottomMargin;

            // The background colors and images of the HTML, printed or not
            htmlToPdfConverter.PdfDocumentOptions.PrintBackgrounds = model.PrintBackgrounds;

            // The media type used in @media rules, after the layout, so that it replaces the one a layout sets
            htmlToPdfConverter.MediaType = model.MediaType == "Print" ? "print" : "screen";

            // Sets the PDF standard for the generated document
            // Leave as None to generate a plain PDF without an accessibility structure tree or archival metadata
            htmlToPdfConverter.PdfDocumentOptions.PdfStandard = model.PdfStandard;

            // Set the maximum time, in seconds, to wait for the HTML page to load
            // The default value is 120 seconds
            htmlToPdfConverter.NavigationTimeout = model.NavigationTimeout;

            // Set an additional delay, in seconds, to wait for asynchronous content after the initial load
            // The default value is 0
            if (model.ConversionDelay.HasValue)
                htmlToPdfConverter.ConversionDelay = model.ConversionDelay.Value;

            // The buffer to receive the generated PDF document
            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Url")
            {
                string url = model.Url;

                // Convert the HTML page given by an URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }
            else
            {
                string htmlString = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert a HTML string with a base URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlString, baseUrl);
            }

            // Send the PDF file to browser
            // The zoom the HTML was drawn at, read from the PDF: the zoom of the layout, lower when the browser window
            // grew to the content; in the name of the file
            string printZoom = htmlToPdfConverter.ConversionInfo.PrintZoom.ToString("0.#", System.Globalization.CultureInfo.InvariantCulture);

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            if (!model.OpenInline)
            {
                // send as attachment
                fileResult.FileDownloadName = "HTML_to_PDF_Getting_Started_zoom_" + printZoom + ".pdf";
            }

            return fileResult;
        }

        private PdfPageSize SelectedPdfPageSize(string selectedValue)
        {
            switch (selectedValue)
            {
                case "A0":
                    return PdfPageSize.A0;
                case "A1":
                    return PdfPageSize.A1;
                case "A10":
                    return PdfPageSize.A10;
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

## Convert the Current HTML Page to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-current-html-page-to-pdf.htm

### Code Sample - Convert the Current HTML Page to PDF

```csharp
using System;
using System.Threading.Tasks;
using System.IO;
using System.ComponentModel.DataAnnotations;
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
    public class Convert_Current_PageController : Controller
    {
        private ICompositeViewEngine m_viewEngine;

        public Convert_Current_PageController(ICompositeViewEngine viewEngine)
        {
            m_viewEngine = viewEngine;
        }

        // GET: Convert_Current_Page
        public ActionResult Index()
        {
            var model = new Convert_Current_Page_ViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertCurrentPageToPdf(Convert_Current_Page_ViewModel model)
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
            ViewEngineResult viewResult = m_viewEngine.FindView(ControllerContext, "Index", true);
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
            string baseUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Convert_Current_Page/ConvertCurrentPageToPdf".Length);

            // Create a HTML to PDF converter object with default settings
            HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

            // Convert the HTML string to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlToConvert, baseUrl);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Convert_Current_Page.pdf";

            return fileResult;
        }
    }
}
```

## Select Conversion Triggering Mode

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-conversion-triggering-mode.htm

```csharp
// Create the PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set the triggering mode
htmlToPdfConverter.TriggeringMode = TriggeringMode.Auto;
```

```csharp
// Create the PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set the triggering mode
htmlToPdfConverter.ConversionDelay = 3;
```

```csharp
// Create the PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set the triggering mode
htmlToPdfConverter.TriggeringMode = TriggeringMode.Manual;
```

### Code Sample - Select Conversion Triggering Mode

```csharp
using System;
using System.IO;
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
    public class Conversion_Triggering_ModesController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Conversion_Triggering_ModesController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        // GET: Conversion_Triggering_Modes
        public ActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Conversion_Triggering_Modes_ViewModel model)
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

            // Set the conversion triggering mode
            if (model.TriggeringMode == "Auto")
            {
                // Set Auto triggering mode
                htmlToPdfConverter.TriggeringMode = TriggeringMode.Auto;

                // Optionally set a delay
                htmlToPdfConverter.ConversionDelay = model.ConversionDelay;
            }
            else if (model.TriggeringMode == "Manual")
            {
                // Set manual triggering mode
                // The conversion starts when the evoPdfConverter_startConversion() function is called 
                // in JavaScript code of the converted HTML page
                htmlToPdfConverter.TriggeringMode = TriggeringMode.Manual;
            }

            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlWithForm = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert the HTML string with page-break-inside:avoid styles to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlWithForm, baseUrl);
            }
            else
            {
                string url = model.Url;

                // Convert the HTML page with page-break-inside:avoid styles to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Conversion_Triggering_Modes.pdf";

            return fileResult;
        }

        private Conversion_Triggering_Modes_ViewModel SetViewModel()
        {
            var model = new Conversion_Triggering_Modes_ViewModel();

            var contentRootPath = Path.Combine(m_hostingEnvironment.ContentRootPath, "wwwroot");

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Conversion_Triggering_Modes".Length);

            model.HtmlString = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Triggering_Modes.html"));
            model.BaseUrl = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.Url = rootUrl + "DemoAppFiles/Input/HTML_Files/Triggering_Modes.html";

            return model;
        }
    }
}
```

### HTML Code with Manual Triggering

```html
<!DOCTYPE html>
<html>
<head>
    <title>Conversion Triggering Modes</title>
    <script type="text/javascript">
        var counter = 0;
        function count() {
            // Display the current counter value
            document.getElementById("counterDiv").innerHTML = counter;

            if (typeof evoPdfConverter_startConversion == 'function' && counter == 5) {
                // Trigger conversion when counter reached a value
                counterDiv.style.color = "green";

                evoPdfConverter_startConversion();
            }
            else {
                // Increment counter
                counter++;

                // After 1 second and call count() again
                setTimeout("count()", 1000);
            }
        }
    </script>
</head>
<body onload="count()" style="font-family: 'Times New Roman'; font-size: 14px">
    The <span style="color: green"><b>count()</b></span> function is called immediately after the HTML page was loaded as a handler of the <span style="color: blue"><b>onload event</b></span> of the body element. The
    <span style="color: green"><b>count()</b></span> function increments the counter and schedules a new call to the same function to occur after 1 second if the counter did not reach the value 5 yet.<br />
    <ul>
        <li>
            If the selected triggering mode is <span style="background-color: aliceblue"><b>Auto</b></span> then the <span style="color: green">count()</span> will be invoked once a second during the given delay interval and after that the conversion to PDF will start
        </li>
        <li>
            If the selected triggering mode is <span style="background-color: aliceblue"><b>Manual</b></span> then the <span style="color: green">count()</span> will be invoked once a second until the counter reached the value 5. When the counter reached this value the
            <span style="background-color: whitesmoke"><b>evoPdfConverter_startConversion()</b></span> function is called to trigger the conversion to PDF
        </li>
    </ul>
    <br />
    <b style="font-size: 30px">Counter value:</b>
    <span style="font-size: 30px; color: red" id="counterDiv">0</span>
    <br />
    <br />
    <br />
    <span style="color: navy">
        <script type="text/javascript">
            if (typeof evoPdfInfo != "undefined") {
                document.write("Created by " + evoPdfInfo.Version);
            }
        </script>
    </span>
</body>
</html>
```

## Convert HTML with SVG to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-with-svg-to-pdf.htm

### Code Sample - Convert HTML with SVG to PDF

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class SVG_to_PDFController : Controller
    {
        // GET: SVG_to_PDF
        public ActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(SVG_to_PDF_ViewModel model)
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

            // Convert the HTML page with SVG to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertUrl(model.Url);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "SVG_to_PDF.pdf";

            return fileResult;
        }

        private SVG_to_PDF_ViewModel SetViewModel()
        {
            var model = new SVG_to_PDF_ViewModel();
            return model;
        }
    }
}
```
