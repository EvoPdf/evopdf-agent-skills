# Bookmarks and table of contents: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Auto Create Hierarchical Bookmarks

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/auto-create-hierarchical-bookmarks.htm

### Custom Bookmarks Using the data-heading Attribute

```html
<!DOCTYPE html>
<html>
<head>
    <title>Auto Create Bookmarks</title>
    <link href="styles/webfonts.css" type="text/css" rel="stylesheet">
    <style>
        body {
            font-family: Verdana, sans-serif;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <br />
    <br />
    <h1>Contents</h1>
    <a href="#Chapter1">Go To Chapter 1</a>
    <br />
    <a href="#Chapter2">Go To Chapter 2</a>
    <br />
    <a href="#Chapter3">Go To Chapter 3</a>
    <br />
    <a href="#CustomBookmarks">Go To Custom Bookmarks</a>
    <br />
    <h2 style="page-break-before: always" id="Chapter1">Chapter 1</h2>
    This is the chapter 1 content.
    <h2 style="page-break-before: always" id="Chapter2">Chapter 2</h2>
    This is the chapter 2 content.
    <h2 style="page-break-before: always" id="Chapter3">Chapter 3</h2>
    This is the chapter 3 content.

    <!-- Custom bookmarks created with the data-heading attribute.
     Any element (not only H1-H6) becomes a bookmark when it has a data-heading
     attribute with a valid level (1 to 6). -->
    <h2 style="page-break-before: always" id="CustomBookmarks">Custom Bookmarks Example</h2>
    These bookmarks are created from ordinary elements using the data-heading attribute.
    <div data-heading="3">Custom Bookmark - Level 3</div>
    This section was bookmarked using a custom &lt;div&gt; element with data-heading="3".
    <div data-heading="3" data-heading-text="Custom Bookmark With Explicit Title">This visible text is ignored for the bookmark</div>
    This section uses data-heading-text to set the bookmark title independently of the element text.
    <h3 data-heading="false">Excluded Heading (data-heading="false")</h3>
    This is a real H3 heading, but it is excluded from the bookmarks because of data-heading="false".

    <p><i>Note: The custom data-heading attribute is enabled only when the custom bookmark mode is used.</i></p>
</body>
</html>
```

### Code Sample - Auto Create Hierarchical Bookmarks

```csharp
using System;
using System.IO;
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
    public class Auto_Create_BookmarksController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Auto_Create_BookmarksController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public ActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Auto_Create_Bookmarks_ViewModel model)
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

            // Auto Create a hierarchy of bookmarks from H1 to H6 tags found in HTML
            if (model.GenerateDocumentOutline)
            {
                // Enable the creation of a hierarchy of bookmarks from H1 to H6 tags
                htmlToPdfConverter.PdfDocumentOptions.GenerateDocumentOutline = model.GenerateDocumentOutline;

                // Optionally, enable the outline mode to utilize browser capabilities. By default, a custom algorithm is used
                htmlToPdfConverter.PdfDocumentOptions.UseBrowserOutlineMode = model.UseBrowserOutlineMode;

                // Display the bookmarks panel in PDF viewer when the generated PDF is opened
                htmlToPdfConverter.PdfViewerPreferences.PageMode = ViewerPageMode.UseOutlines;
            }

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
                string htmlString = model.HtmlStringTextBox;
                string baseUrl = model.BaseUrlTextBox;

                // Convert a HTML string with a base URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlString, baseUrl);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Auto_Create_Hierarchical_Bookmarks.pdf";

            return fileResult;
        }

        private Auto_Create_Bookmarks_ViewModel SetViewModel()
        {
            var model = new Auto_Create_Bookmarks_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Auto_Create_Bookmarks".Length);

            model.HtmlStringTextBox = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Auto_Bookmarks.html"));
            model.BaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";

            return model;
        }
    }
}
```

## Auto Create Table of Contents

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/auto-create-table-of-contents.htm

```csharp
// Create a HTML to PDF converter object with default settings
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

// Enable or disable the automatic creation of a table of contents in the PDF document based on H1 to H6 HTML tags
htmlToPdfConverter.PdfDocumentOptions.GenerateTableOfContents = true;
```

### Custom Table of Contents Entries Using the data-heading Attribute

```html
<!DOCTYPE html>
<html>
<head>
    <title>Auto Create Table of Contents</title>
    <link href="styles/webfonts.css" type="text/css" rel="stylesheet">
</head>
<body style="font-family: Verdana, sans-serif; font-size: 16px">
    <br />
    <br />
    <h1>Contents</h1>
    <a href="#Chapter1">Go To Chapter 1</a>
    <br />
    <a href="#Chapter2">Go To Chapter 2</a>
    <br />
    <a href="#Chapter3">Go To Chapter 3</a>
    <br />
    <a href="#CustomTocItems">Go To Custom TOC Items</a>
    <br />
    <!-- This DIV is the container for inline table of contents.
    If it is not explicitly defined, a default one is created
    before the HTML content of the converted page -->
    <div id="html-to-pdf-toc"></div>
    <h2 style="page-break-before: always" id="Chapter1">Chapter 1</h2>
    This is the chapter 1 content.
    <h2 style="page-break-before: always" id="Chapter2">Chapter 2</h2>
    This is the chapter 2 content.
    <h2 style="page-break-before: always" id="Chapter3">Chapter 3</h2>
    This is the chapter 3 content.

    <!-- Custom table of contents items created with the data-heading attribute.
     Any element (not only H1-H6) becomes a table of contents entry when it has
     a data-heading attribute with a valid level (1 to 6). -->
    <h2 style="page-break-before: always" id="CustomTocItems">Custom Table of Contents Example</h2>
    These table of contents entries are created from ordinary elements using the data-heading attribute.
    <div data-heading="3">Custom TOC Item - Level 3</div>
    This entry was added to the table of contents using a custom &lt;div&gt; element with data-heading="3".
    <div data-heading="3" data-heading-text="Custom TOC Item With Explicit Title">This visible text is ignored for the table of contents</div>
    This entry uses data-heading-text to set the table of contents title independently of the element text.
    <h3 data-heading="false">Excluded Heading (data-heading="false")</h3>
    This is a real H3 heading, but it is excluded from the table of contents because of data-heading="false".

    <p><i>Note: The custom data-heading attribute is enabled only when the custom table of contents mode is used.</i></p>
</body>
</html>
```

### Table Title

```csharp
// Set the table of contents title
htmlToPdfConverter.PdfDocumentOptions.TableOfContents.Title = "Table of Contents";
```

### Inline Table of Contents

```csharp
// Set the table of contents title
htmlToPdfConverter.PdfDocumentOptions.TableOfContents.CreateInline = true;
```

### Table of Contents Style

```csharp
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.Style = """
#html-to-pdf-toc {
    font-family: Arial, sans-serif;
    margin: 0px;
    padding: 10px;
    box-sizing: border-box;
    background-color: #FFFFFF;
    width: 100%;
    position: relative;
    display : block;
    z-index: 100;
    opacity: 1;

    /* uncomment the lines below to force PDF page breaks before and after inline TOC */
    /*page-break-after : always;
    page-break-before : always;*/

    /* uncomment the line below for RTL table of contents*/
    /*direction : rtl;*/
}

/* TOC title style */
#html-to-pdf-toc .toc-title {
    font-size: 22px;
    font-weight: bold;
    margin-bottom: 15px;
    color: #444444;
    text-align: center;
}

/* Common style for all TOC entries text */
#html-to-pdf-toc li .toc-link {
    order: 1;
    text-decoration: none;
    color: inherit;
    padding: 0px;
    margin: 0px;
}

/* Level specific style for TOC entries text */

#html-to-pdf-toc li .toc-link-level-1 {
    font-weight: bold;
    font-size: 18px;
    color: #111111;
}

#html-to-pdf-toc li .toc-link-level-2 {
    font-weight: bold;
    font-size: 16px;
    color: #222222;
}

#html-to-pdf-toc li .toc-link-level-3 {
    font-weight: bold;
    font-size: 14px;
    color: #333333;
}

#html-to-pdf-toc li .toc-link-level-4 {
    font-weight: normal;
    font-size: 12px;
    color: #333333;
}

#html-to-pdf-toc li .toc-link-level-5 {
    font-weight: normal;
    font-size: 11px;
    color: #333333;
    font-style: italic;
}

#html-to-pdf-toc li .toc-link-level-6 {
    font-weight: normal;
    font-size: 10px;
    color: #333333;
    font-style: italic;
}

/* Common style for all page numbers */
#html-to-pdf-toc li .page-link {
    order: 3;
    font-family: Arial, sans-serif;
    font-size: 16px;
    font-weight: bold;
    font-style: normal;
    text-decoration: none;
    padding: 0px;
    margin: 0px;
    color: #111111;
}

/* Style for space between entry text and page number */ 
#html-to-pdf-toc li::after {
    flex-grow: 1;
    order: 2;
    content: "";
    height: 1em;
    /* comment the line below to remove the dotted line from TOC */
    border-bottom: 2px dotted lightgray;
}

#html-to-pdf-toc ul {
    list-style-type: none; 
    padding: 0px;
}

/* Common style for all TOC entries*/
#html-to-pdf-toc li {
    font-size: 14px;
    display: flex;
    padding: 0px;
    margin: 0px;
}

/* Level specific style for TOC entries */

#html-to-pdf-toc ul li.level-1 {    
    margin-bottom: 10px;
    margin-inline-start: 0px;    
}

#html-to-pdf-toc ul li.level-2 {
    margin-bottom: 8px;
    margin-inline-start: 20px;
}

#html-to-pdf-toc ul li.level-3 {
    margin-bottom: 6px;
    margin-inline-start: 40px;
}

#html-to-pdf-toc ul li.level-4 {
    margin-bottom: 4px;
    margin-inline-start: 60px;
}

#html-to-pdf-toc ul li.level-5 {
    margin-bottom: 2px;
    margin-inline-start: 80px;
}

#html-to-pdf-toc ul li.level-6 {
    margin-bottom: 2px;
    margin-inline-start: 100px;
}
""";
```

### Code Sample - Auto Create Table of Contents

```csharp
using System;
using System.IO;
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
    public class Table_of_ContentsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Table_of_ContentsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Table_of_Contents_ViewModel model)
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

            // Enable or disable the automatic creation of a table of contents in the PDF document based on H1 to H6 HTML tags
            htmlToPdfConverter.PdfDocumentOptions.GenerateTableOfContents = model.GenerateToc;

            // Optionally set the table of contents to display inline within the 'html-to-pdf-toc' DIV
            // By default, the table of contents is created at the start of the generated PDF document
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.CreateInline = model.InlineToc;

            // Optionally enable the usage of browser capabilities when creating the table of contents
            // This option is applicable only if the table of contents is not inline and it is false by default
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.UseBrowserMode = model.UseBrowserMode;

            // Set if the page numbers from table of contents are displyed
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.ShowPageNumbers = model.ShowPageNumbers;

            // Set if the TOC pages are included in the page numbers displayed in the TOC.
            // This option is not applicable to the inline table of contents
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.CountTocAndStartPages = model.CountTocPages;

            // Set an offset to be applied to all page numbers in the table of contents.
            // It can be useful when merging a PDF with a table of contents with other PDF documents
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.PageNumbersOffset = model.PageNumbersOffset;

            // Set the table of contents title
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.Title = model.TocTitle;

            // Optionally set a custom CSS style for the table of contents
            // A default style is applied by the library if this property is not set
            htmlToPdfConverter.PdfDocumentOptions.TableOfContents.Style = model.TocStyleTextBox;

            // Set the initial HTML viewer height in pixels
            if (model.HtmlViewerHeight.HasValue)
                htmlToPdfConverter.HtmlViewerHeight = model.HtmlViewerHeight.Value;

            // Set the PDF page margins in points. The default is 0
            htmlToPdfConverter.PdfDocumentOptions.LeftMargin = model.LeftMargin;
            htmlToPdfConverter.PdfDocumentOptions.RightMargin = model.RightMargin;
            htmlToPdfConverter.PdfDocumentOptions.TopMargin = model.TopMargin;
            htmlToPdfConverter.PdfDocumentOptions.BottomMargin = model.BottomMargin;

            // Set the page layout: how the width at which the HTML is laid out relates to the PDF page width
            PdfPageSize pageSize = SelectedPdfPageSize(model.PdfPageSize);
            PdfPageOrientation pageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation);

            switch (model.PageLayout)
            {
                case "FitBrowserWindowToPage":
                    // Fixed page size: the HTML is laid out as in a browser window of the given width and the result
                    // is scaled to the content width of the page, so a responsive site keeps its desktop layout.
                    // This is the default layout of the converter, with an A4 page and a 1024 pixel window
                    htmlToPdfConverter.FitBrowserWindowToPage(pageSize, pageOrientation, windowWidth: model.HtmlViewerWidth);
                    break;

                case "LayoutAtPageWidth":
                    // Fixed page size: the HTML is laid out at the content width of the page, one CSS pixel
                    // being 0.75 points. For HTML templates designed for the paper size
                    htmlToPdfConverter.LayoutAtPageWidth(pageSize, pageOrientation);
                    break;

                default:
                    // The PDF page width follows the browser window width and the HTML is drawn at the zoom, 1:1 at 100;
                    // the page height comes from the page size and the orientation
                    htmlToPdfConverter.PageWidthFromBrowserWindow(model.HtmlViewerWidth, singlePage: false, zoom: model.HtmlViewerZoom);
                    htmlToPdfConverter.PdfDocumentOptions.PdfPageSize = pageSize;
                    htmlToPdfConverter.PdfDocumentOptions.PdfPageOrientation = pageOrientation;
                    break;
            }

            // Set the maximum time in seconds to wait for HTML page to be loaded 
            // Leave it not set for a default 120 seconds maximum wait time
            htmlToPdfConverter.NavigationTimeout = model.NavigationTimeout;

            // Set an additional delay in seconds to wait for JavaScript or AJAX calls after page load completed
            // Set this property to 0 if you don't need to wait for such asynchronous operations to finish
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
                string htmlString = model.HtmlStringTextBox;
                string baseUrl = model.BaseUrlTextBox;

                // Convert a HTML string with a base URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlString, baseUrl);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Table_of_Contents.pdf";

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

        private Table_of_Contents_ViewModel SetViewModel()
        {
            var model = new Table_of_Contents_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Table_of_Contents".Length);

            model.HtmlStringTextBox = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Table_of_Contents.html"));
            model.TocStyleTextBox = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/TOC_Style.css"));
            model.BaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";

            return model;
        }
    }
}
```

## Convert Internal Links from HTML to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-internal-links-from-html-to-pdf.htm

### Code Sample - Convert Internal Links from HTML to PDF

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Convert_Internal_Links_to_PDFController : Controller
    {
        // GET: Convert_Internal_Links_to_PDF
        public ActionResult Index()
        {
            var model = new Convert_Internal_Links_to_PDF_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Convert_Internal_Links_to_PDF_ViewModel model)
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

            // Convert the HTML page to a PDF document in a memory buffer
            byte[] outPdfBuffer = htmlToPdfConverter.ConvertUrl(model.Url);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Convert_Internal_Links_to_PDF.pdf";

            return fileResult;
        }
    }
}
```
