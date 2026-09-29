# Page setup and scaling: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## HTML to PDF Page Setup and Scaling

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm

### FitBrowserWindowToPage

```csharp
// A4 portrait, the desktop layout of the page scaled to the page width: the settings of a new converter
converter.FitBrowserWindowToPage(PdfPageSize.A4);

// Letter landscape, a 1280 pixel window
converter.FitBrowserWindowToPage(PdfPageSize.Letter, PdfPageOrientation.Landscape, 1280);
```

### LayoutAtPageWidth

```csharp
// An invoice template designed for A4
converter.LayoutAtPageWidth(PdfPageSize.A4);

// A template whose @page rule sets the paper size
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Portrait, preferCssPageSize: true);

// A template with a print style sheet
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Portrait, mediaType: "print");

// A web page laid out at the width of a landscape page
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Landscape);
```

### PrintLikeChrome

```csharp
// The same as converter.PrintLikeChrome(PdfPageSize.Letter)
converter.PdfDocumentOptions.LeftMargin = 28;
converter.PdfDocumentOptions.RightMargin = 28;
converter.PdfDocumentOptions.TopMargin = 28;
converter.PdfDocumentOptions.BottomMargin = 28;
converter.PdfDocumentOptions.PrintBackgrounds = false;
converter.LayoutAtPageWidth(PdfPageSize.Letter, PdfPageOrientation.Portrait, false, "print");
```

### SinglePageOfWidth

```csharp
// A receipt for 80 mm paper, 5 mm margins
converter.SinglePageOfWidth(227, 14);
```

### PageWidthFromBrowserWindow

```csharp
// A 768 point wide page (1024 pixels), the content drawn 1:1, paginated at the A4 height
converter.PageWidthFromBrowserWindow();

// The whole page on one PDF page
converter.PageWidthFromBrowserWindow(1024, singlePage: true);
```

### One PDF Page for the Whole Content

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// The page width follows the viewer width, the page height follows the content
converter.PageWidthFromBrowserWindow(1024, singlePage: true);

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.HtmlViewerWidth = 1024;
converter.PdfDocumentOptions.AutoResizePdfPageWidth = true;
converter.PdfDocumentOptions.AutoResizePdfPageHeight = true;
```

### A Web Page on A4 or Letter with Its Desktop Layout

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// A4 portrait page, the 1024 pixel browser window scaled to it
converter.FitBrowserWindowToPage(PdfPageSize.A4);

byte[] pdf = converter.ConvertUrl("https://www.evopdf.com");

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Portrait;
// Lay the page out at 793 / 0.7747 = 1024 pixels and draw it at 77.47 percent
converter.HtmlViewerZoom = 77.47;
converter.HtmlViewerWidth = 1024;
```

### An HTML Template Designed for the Paper Size

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// Margins in points; a @page rule in the template overrides them
converter.PdfDocumentOptions.LeftMargin = 36;
converter.PdfDocumentOptions.RightMargin = 36;
converter.PdfDocumentOptions.TopMargin = 36;
converter.PdfDocumentOptions.BottomMargin = 36;

// The exact page size, the template laid out at the page width with its print style sheet;
// preferCssPageSize lets a @page { size } rule in the template decide the page size
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Portrait, preferCssPageSize: true, mediaType: "print");

byte[] pdf = converter.ConvertHtml(invoiceHtml, baseUrl);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.AutoResizePdfPageHeight = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
converter.MediaType = "print";
// Viewer width equal to the content width: (595 - 72) * 4 / 3 = 697 pixels
converter.HtmlViewerWidth = 697;
converter.PdfDocumentOptions.PreferCssPageSize = true;
```

### An HTML Page Converted at Its Exact Pixel Size

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// The page width follows a 1000 pixel window: 1000 * 0.75 = 750 points
converter.PageWidthFromBrowserWindow(1000);

// A4 height for each page; PageWidthFromBrowserWindow(1000, singlePage: true) gives one page instead
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = true;
converter.HtmlViewerWidth = 1000;
```

### A Wide Table on a Landscape Page

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// A4 landscape, the HTML laid out at its 1123 pixel content width
converter.LayoutAtPageWidth(PdfPageSize.A4, PdfPageOrientation.Landscape);

// A table built for 1280 pixels: a 1280 pixel window scaled to the page, zoom 87.73
// converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Landscape, 1280);

byte[] pdf = converter.ConvertUrl(url);
```

### The Same Output as Save as PDF in Chrome

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

converter.PrintLikeChrome(PdfPageSize.A4);

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
// Chrome prints with 1 cm margins, print media type and no backgrounds
converter.PdfDocumentOptions.LeftMargin = 28;
converter.PdfDocumentOptions.RightMargin = 28;
converter.PdfDocumentOptions.TopMargin = 28;
converter.PdfDocumentOptions.BottomMargin = 28;
converter.MediaType = "print";
converter.PdfDocumentOptions.PrintBackgrounds = false;
// Viewer width equal to the content width: (595 - 56) * 4 / 3 = 719 pixels
converter.HtmlViewerWidth = 719;
```

### A Page in Its Mobile Layout

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// A phone window of 412 pixels enlarged to an A4 page
converter.FitBrowserWindowToPage(PdfPageSize.A4, PdfPageOrientation.Portrait, 412);

byte[] pdf = converter.ConvertUrl(url);

// The same settings made one by one
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
// Lay the page out at 793 / 1.9256 = 412 pixels and draw it at 192.56 percent
converter.HtmlViewerZoom = 192.56;
converter.HtmlViewerWidth = 412;
```

### A Receipt on One Page of Fixed Width

```csharp
HtmlToPdfConverter converter = new HtmlToPdfConverter();

// One page 227 points (80 mm) wide with 14 point (5 mm) margins, as tall as the content
converter.SinglePageOfWidth(227, 14);

byte[] pdf = converter.ConvertHtml(receiptHtml, baseUrl);

// The same settings made one by one: 5 mm margins are 14 points
converter.PdfDocumentOptions.LeftMargin = 14;
converter.PdfDocumentOptions.RightMargin = 14;
converter.PdfDocumentOptions.TopMargin = 14;
converter.PdfDocumentOptions.BottomMargin = 14;
converter.PdfDocumentOptions.PdfPageSize = new PdfPageSize(227, 227);
converter.PdfDocumentOptions.AutoResizePdfPageWidth = false;
converter.PdfDocumentOptions.AutoResizePdfPageHeight = true;
// the content width in pixels: (227 - 2 * 14) * 4 / 3, rounded down
converter.HtmlViewerWidth = 265;
converter.HtmlViewerZoom = 100;
```

### Options Set After a Layout Method

```csharp
// A4 landscape in the default layout, with margins set after the call:
// the zoom is computed for the landscape page and the margins
converter.FitBrowserWindowToPage(PdfPageSize.A4);
converter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
converter.PdfDocumentOptions.LeftMargin = 36;
converter.PdfDocumentOptions.RightMargin = 36;

// Setting the zoom ends the automatic layout: the page is laid out for zoom 90, whatever its size
converter.HtmlViewerZoom = 90;
```

## Insert Page Breaks in PDF Using CSS in HTML

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/insert-page-breaks-in-pdf-using-css.htm

### Code Sample - Insert Page Breaks in PDF Using CSS in HTML

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
    public class Insert_Page_BreaksController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Insert_Page_BreaksController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        // GET: Insert_Page_Breaks
        public ActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Insert_Page_Breaks_ViewModel model)
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

            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlWithForm = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert the HTML string with page-break-before:always and page-break-after:always styles to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlWithForm, baseUrl);
            }
            else
            {
                string url = model.Url;

                // Convert the HTML page with page-break-before:always and page-break-after:always styles to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Insert_Page_Breaks.pdf";

            return fileResult;
        }

        private Insert_Page_Breaks_ViewModel SetViewModel()
        {
            var model = new Insert_Page_Breaks_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Insert_Page_Breaks".Length);

            model.HtmlString = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Page_Break_Before_and_After_Using_CSS.html"));
            model.BaseUrl = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.Url = rootUrl + "DemoAppFiles/Input/HTML_Files/Page_Break_Before_and_After_Using_CSS.html";

            return model;
        }
    }
}
```

### HTML with page-break-before:always and page-break-after:always CSS Attributes

```html
<!DOCTYPE html>
<html>
<head>
    <title>Insert Page Breaks Before and After HTML Elements Using CSS</title>
</head>
<body style="width: 1010px; font-family: 'Times New Roman'; font-size: 20px; margin: 5px">
    <div style="width: 100%; height: 500px; background-color: aliceblue; border: 2px solid gray; text-align: center">
        <div style="width: 100%; height: 200px"></div>
        A block <b>without any page break</b> style<br />
        <br />
        [ Follows a block with <i>page-break-before : always</i> and <i>page-break-after : always</i> styles ]
    </div>
    <div style="page-break-before: always; page-break-after: always; width: 100%; height: 500px; background-color: gainsboro; border: 2px solid gray; text-align: center">
        <div style="width: 100%; height: 200px"></div>
        A block with <b>page-break-before : always</b> and <b>page-break-after : always</b> styles<br />
        <br />
        <b>This block will be always rendered alone in a PDF page</b><br />
        <br />
        [ Follows a block with <i>page-break-after : always</i> style ]
    </div>
    <div style="page-break-after: always; width: 100%; height: 500px; background-color: beige; border: 2px solid gray; text-align: center">
        <div style="width: 100%; height: 200px"></div>
        A block with <b>page-break-after : always</b> style<br />
        <br />
        <b>Nothing will be rendered after this block in PDF page</b>
        <br />
        <br />
        [ Follows a block <i>without any page break</i> style ]
    </div>
    <div style="width: 100%; height: 500px; background-color: aliceblue; border: 2px solid gray; text-align: center">
        <div style="width: 100%; height: 200px"></div>
        A block <b>without any page break</b> style<br />
        <br />
        [ Follows a block with <i>page-break-before : always</i> style ]
    </div>
    <div style="page-break-before: always; width: 100%; height: 500px; background-color: lightgray; border: 2px solid gray; text-align: center">
        <div style="width: 100%; height: 200px"></div>
        A block with <b>page-break-before : always</b> style<br />
        <br />
        <b>This block will always be rendered at the top of a PDF page</b>
    </div>
</body>
</html>
```

## Avoid Page Breaks Inside HTML Elements Using CSS

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/avoid-page-break-inside-elements-uising-css.htm

### Code Sample - Avoid Page Breaks Inside HTML Elements Using CSS

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
    public class Avoid_Page_Breaks_InsideController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Avoid_Page_Breaks_InsideController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        // GET: Avoid_Page_Breaks_Inside
        public ActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Avoid_Page_Breaks_Inside_ViewModel model)
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
            fileResult.FileDownloadName = "Avoid_Page_Breaks_Inside.pdf";

            return fileResult;
        }

        private Avoid_Page_Breaks_Inside_ViewModel SetViewModel()
        {
            var model = new Avoid_Page_Breaks_Inside_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Avoid_Page_Breaks_Inside".Length);

            model.HtmlString = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Page_Breaks_Inside_Avoid_Using_CSS.html"));
            model.BaseUrl = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.Url = rootUrl + "DemoAppFiles/Input/HTML_Files/Page_Breaks_Inside_Avoid_Using_CSS.html";

            return model;
        }
    }
}
```

### HTML with page-break-inside:avoid CSS Attribute

```html
<!DOCTYPE html>
<html>
<head>
    <title>Avoid Page Breaks Inside HTML Elements Using CSS</title>
</head>
<body style="margin: 0px; font-family: 'Times New Roman'; font-size: 14px">
    <table style="width: 1024px; font-family: 'Times New Roman'; font-size: 16px">
        <!-- The automatically repeated table header -->
        <thead>
            <tr>
                <td colspan="2">
                    <table style="border-bottom: 1px solid gray; width: 100%;">
                        <tr>
                            <td style="width: 200px">
                                <img alt="Logo Image" style="float: left; width: 200px" src="img/logo.jpg" />
                            </td>
                            <td style="text-align: right; font-size: 22px; font-weight: bold; color: navy">Repeated Table Header
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
        </thead>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">1</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">2</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">3</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">4</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">5</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">6</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">7</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">8</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">9</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">10</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">11</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">12</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">13</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">14</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">15</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">16</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">17</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">18</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">19</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">20</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">21</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">22</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">23</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">24</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">25</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">26</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">27</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">28</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">29</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">30</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">31</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">32</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">33</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">34</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">35</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">36</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">37</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">38</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">39</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">40</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">41</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">42</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">43</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">44</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">45</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">46</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">47</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">48</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">49</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr style="page-break-inside: avoid">
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">50</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">This row has the <b>'page-break-inside:avoid'</b> style to avoid page breaks inside it in PDF document.<br />
                <br />
                Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <!-- The automaticaly repeated table footer -->
        <tfoot>
            <tr>
                <td colspan="2">
                    <table style="border-top: 1px solid gray; width: 100%;">
                        <tr>
                            <td style="font-size: 22px; font-weight: bold; color: green">Repeated Table Footer
                            </td>
                            <td style="width: 200px">
                                <img alt="Logo Image" style="float: right; width: 200px" src="img/logo.jpg" />
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
        </tfoot>
    </table>
</body>
</html>
```

## Select Media Type for Screen or Print

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-media-type-for-screen-or-print.htm

### Code Sample - Select Media Type When Converting HTML to PDF

```csharp
using System;
using System.IO;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Select_Screen_or_Print_Media_TypeController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Select_Screen_or_Print_Media_TypeController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        // GET: Select_Screen_or_Print_Media_Type
        public ActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Select_Screen_or_Print_Media_Type_ViewModel model)
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

            // Set the media type for which to render HTML to PDF
            htmlToPdfConverter.MediaType = model.MediaType == "Print" ? "print" : "screen";

            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlWithForm = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert a HTML string to a PDF document for the selected media type
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlWithForm, baseUrl);
            }
            else
            {
                string url = model.Url;

                // Convert the HTML page to a PDF document for the selected media type
                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Select_Screen_or_Print_Media_Type.pdf";

            return fileResult;
        }

        private Select_Screen_or_Print_Media_Type_ViewModel SetViewModel()
        {
            var model = new Select_Screen_or_Print_Media_Type_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Select_Screen_or_Print_Media_Type".Length);

            model.HtmlString = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Media_Type_Rules.html"));
            model.BaseUrl = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.Url = rootUrl + "DemoAppFiles/Input/HTML_Files/Media_Type_Rules.html";

            return model;
        }
    }
}
```

### HTML Code with @media Rules

```html
<!DOCTYPE html>
<html>
<head>
    <title>Media Type Rules</title>
    <style type="text/css">
        /* Set the body text family and font size both for screen and print*/
        body
        {
            width: 1000px;
            margin: 10px;
            font-family: 'Times New Roman';
            font-size: 18px;
        }

        /* Set the title text font size and weight both for screen and print*/
        .title
        {
            font-size: 24px;
            font-weight: bold;
        }

        @media screen
        {
            /* Set a background color only when the HTML page is displayed on screen*/
            body
            {
                background-color: aliceblue;
            }

            /* Use blue to write the text on screen*/
            p
            {
                color: darkblue;
            }
        }

        @media print
        {
            /* Hide images when priting*/
            img
            {
                display: none;
            }

            /* Use black to write the text on screen*/
            p
            {
                color: black;
            }
        }


        @media screen,print
        {
            /* Set the paragraph text family and font size both for screen and print*/
            p
            {
                font-family: 'Times New Roman';
                font-size: 20px;
            }
        }
    </style>
</head>
<body>
    <span class="title">Media Type Rules</span><br />
    <br />
    This document have different styles when it is displayed on the screen and when it is printed.
    You can instruct the converter to use the style you want setting its <i>MediaType</i> property.<br />
    <br />
    <b>The image below is visible only when the selected media type is 'screen' and is hidden when the selected media type is 'print'</b>:<br />
    <br />
    <img alt="Logo Image" src="img/logo.jpg" />
    <br />
    <br />
    <b>The text below will be dark blue when the selected media type is 'screen' and black when the selected media type is 'print':</b>
    <p>
        Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy nibh euismod tincidunt ut 
        laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, quis nostrud exerci tation 
        ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. Duis autem vel eum iriure 
        dolor in hendrerit in vulputate velit esse molestie consequat, vel illum dolore eu feugiat nulla 
        facilisis at vero eros et accumsan et iusto odio dignissim qui blandit praesent luptatum zzril delenit 
        augue duis dolore te feugait nulla facilisi.
    </p>
</body>
</html>
```

## Repeat HTML Table Header and Footer in PDF Pages

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/repeat-html-table-header-and-footer-in-pdf.htm

### Code Sample - Repeat HTML Table Header and Footer in PDF Pages

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
    public class Repeat_HTML_Table_Header_FooterController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Repeat_HTML_Table_Header_FooterController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public ActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Repeat_HTML_Table_Header_Footer_ViewModel model)
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

            // Repeat table header and footer option
            htmlToPdfConverter.PdfDocumentOptions.RepeatTableHeaderFooter = model.RepeatTableHeaderFooter;

            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlWithForm = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert the HTML string with repeated table header/footer option to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlWithForm, baseUrl);
            }
            else
            {
                string url = model.Url;

                // Convert the HTML page with repeated table header/footer option to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Repeat_HTML_Table_Header_Footer.pdf";

            return fileResult;
        }

        private Repeat_HTML_Table_Header_Footer_ViewModel SetViewModel()
        {
            var model = new Repeat_HTML_Table_Header_Footer_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Repeat_HTML_Table_Header_Footer".Length);

            model.HtmlString = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Repeat_HTML_Header_Footer.html"));
            model.BaseUrl = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.Url = rootUrl + "DemoAppFiles/Input/HTML_Files/Repeat_HTML_Header_Footer.html";

            return model;
        }
    }
}
```

### HTML Table with THEAD and TFOOT

```html
<!DOCTYPE html>
<html>
<head>
    <title>Repeat HTML Table Header and Footer in PDF Pages</title>
</head>
<body style="margin: 0px; font-family: 'Times New Roman'; font-size: 14px">
    <table style="width: 1024px; font-family: 'Times New Roman'; font-size: 16px">
        <!-- The automaticaly repeated table header -->
        <thead>
            <tr>
                <td colspan="2">
                    <table style="border-bottom: 1px solid gray; width: 100%;">
                        <tr>
                            <td style="width: 200px">
                                <img alt="Logo Image" style="float: left; width: 200px" src="img/logo.jpg" />
                            </td>
                            <td style="text-align: right; font-size: 22px; font-weight: bold; color: navy">Repeated Table Header
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
        </thead>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">1</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">2</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">3</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">4</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">5</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">6</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">7</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">8</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">9</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">10</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">11</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">12</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">13</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">14</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">15</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">16</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">17</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">18</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">19</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">20</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">21</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">22</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">23</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">24</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">25</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">26</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">27</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">28</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">29</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">30</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">31</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">32</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">33</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">34</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">35</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">36</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">37</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">38</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">39</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">40</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">41</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">42</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">43</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">44</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">45</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">46</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: gainsboro; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">47</td>
            <td style="font-weight: normal; font-style: italic; background-color: azure; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: whitesmoke; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">48</td>
            <td style="font-weight: bold; font-style: italic; background-color: beige; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima   . Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <tr>
            <td style="background-color: silver; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">49</td>
            <td style="background-color: whitesmoke; font-weight: normal; font-style: normal; padding: 30px 10px 30px 10px">Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy 
                    nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, 
                    quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. 
                    Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, 
                    vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui 
                    blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. 
            </td>
        </tr>
        <tr>
            <td style="background-color: lightgray; font-weight: bold; font-size: 24px; width: 50px; text-align: center; vertical-align: middle">50</td>
            <td style="font-weight: bold; font-style: normal; background-color: oldlace; padding: 30px 10px 30px 10px">Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim 
                    placerat facer possim assum. Typi non habent claritatem insitam; est usus legentis in iis qui facit 
                    eorum claritatem. Investigationes demonstraverunt lectores legere me lius quod ii legunt saepius. 
                    Claritas est etiam processus dynamicus, qui sequitur mutationem consuetudium lectorum. 
                    Mirum est notare quam littera gothica, quam nunc putamus parum claram, anteposuerit litterarum 
                    formas humanitatis per seacula quarta decima et quinta decima. Eodem modo typi, qui nunc 
                    nobis videntur parum clari, fiant sollemnes in futurum.
            </td>
        </tr>
        <!-- The automaticaly repeated table footer -->
        <tfoot>
            <tr>
                <td colspan="2">
                    <table style="border-top: 1px solid gray; width: 100%;">
                        <tr>
                            <td style="font-size: 22px; font-weight: bold; color: green">Repeated Table Footer
                            </td>
                            <td style="width: 200px">
                                <img alt="Logo Image" style="float: right; width: 200px" src="img/logo.jpg" />
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
        </tfoot>
    </table>
</body>
</html>
```
