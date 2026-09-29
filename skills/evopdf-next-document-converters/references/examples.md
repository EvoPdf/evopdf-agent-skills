# Word, Excel, RTF and Markdown to PDF: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Convert Word DOCX to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-word-docx-to-pdf.htm

### Create the Word to PDF Converter

```csharp
// Create a new Word to PDF converter instance
WordToPdfConverter wordToPdfConverter = new WordToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
wordToPdfConverter.PdfDocumentOptions.UsePageSettingsFromWord = false;
wordToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
wordToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
wordToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
wordToPdfConverter.PdfDocumentOptions.RightMargin = 20;
wordToPdfConverter.PdfDocumentOptions.TopMargin = 30;
wordToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

### Code Sample - Convert Word to PDF

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Hosting;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.Word_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.Word_to_PDF
{
    public class Word_to_PDFController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Word_to_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ConvertWordToPdf(Word_to_PDF_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a Word to PDF converter object with default settings
            WordToPdfConverter wordToPdfConverter = new WordToPdfConverter();

            // Set whether a table of contents is automatically generated from headings
            wordToPdfConverter.PdfDocumentOptions.GenerateTableOfContents = model.GenerateToc;

            //  Set whether to use page settings (size, margins) from the Word document or the custom settings
            wordToPdfConverter.PdfDocumentOptions.UsePageSettingsFromWord = model.PdfPageSettingsMode == "FromWordDocument";

            // Set whether page break marks from Word documents should be processed
            wordToPdfConverter.ProcessPageBreakMarks = model.ProcessWordPageBreakMarks;

            if (!wordToPdfConverter.PdfDocumentOptions.UsePageSettingsFromWord)
            {
                // Set PDF page size which can be a predefined size like A4 or a custom size in points 
                // Leave it not set to have a default A4 PDF page
                wordToPdfConverter.PdfDocumentOptions.PdfPageSize = SelectedPdfPageSize(model.PdfPageSize);

                // Set PDF page orientation to Portrait or Landscape
                // Leave it not set to have a default Portrait orientation for PDF page
                wordToPdfConverter.PdfDocumentOptions.PdfPageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation);

                // Set PDF page margins in points or leave them not set to have a PDF page without margins
                wordToPdfConverter.PdfDocumentOptions.LeftMargin = model.LeftMargin;
                wordToPdfConverter.PdfDocumentOptions.RightMargin = model.RightMargin;
                wordToPdfConverter.PdfDocumentOptions.TopMargin = model.TopMargin;
                wordToPdfConverter.PdfDocumentOptions.BottomMargin = model.BottomMargin;

                // Set the Word viewer zoom percentage
                wordToPdfConverter.PdfDocumentOptions.Zoom = model.WordViewerZoom;
            }

            // Set PDF header and footer
            SetHeader(wordToPdfConverter, model);
            SetFooter(wordToPdfConverter, model);

            byte[] inputWordBytes = null;

            // If an uploaded file exists, use it with priority
            if (model.WordFile != null && model.WordFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.WordFile.CopyToAsync(ms);
                    inputWordBytes = ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded Word file", ex);
                }
            }
            else
            {
                // Otherwise, fall back to the URL
                string wordUrl = model.WordFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(wordUrl))
                    throw new Exception("No Word file provided: upload a file or specify a URL");

                try
                {
                    if (wordUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(wordUrl).LocalPath;
                        inputWordBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputWordBytes = await httpClient.GetByteArrayAsync(wordUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the Word file from URL", ex);
                }
            }

            // The buffer to receive the generated PDF document
            byte[] outPdfBuffer = wordToPdfConverter.ConvertToPdf(inputWordBytes);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            // send as attachment
            fileResult.FileDownloadName = "Word_to_Pdf.pdf";

            return fileResult;
        }

        private void SetHeader(WordToPdfConverter wordToPdfConverter, Word_to_PDF_ViewModel model)
        {
            bool headerEnabled = model.HeaderEnabled;
            if (!headerEnabled)
                return;

            // Set the header HTML from a URL or from an HTML string
            bool headerHtmlFromUrl = model.HeaderHtmlSource == "Url";
            if (headerHtmlFromUrl)
            {
                string headerUrl = model.HeaderUrlTextBox;

                wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlSourceUrl = headerUrl;
            }
            else
            {
                string headerHtml = model.HeaderHtmlTextBox;
                string headerHtmlBaseUrl = model.HeaderHtmlBaseUrlTextBox;

                wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Html = headerHtml;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlBaseUrl = headerHtmlBaseUrl;
            }

            // Enable automatic height adjustment based on header HTML content
            bool autoSizeHeaderContentHeight = model.AutoSizeHeaderContentHeight;
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoSizeContentHeight = autoSizeHeaderContentHeight;

            // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MinContentHeight = model.HeaderMinContentHeight;
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MaxContentHeight = model.HeaderMaxContentHeight;

            // Set a fixed height for the header if AutoResizeHeight is disabled
            if (model.HeaderHeight.HasValue)
            {
                int headerHeight = model.HeaderHeight.Value;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Height = headerHeight;
            }

            // If AutoResizeHeight is enabled and both Height and FitHeight are set,
            // the content may be scaled down to fit the specified height
            bool fitHeaderHeight = model.FitHeaderHeight;
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.FitHeight = fitHeaderHeight;

            // Enable automatic top margin adjustment in the PDF based on the header
            bool autoResizeTopMargin = model.AutoResizeTopMargin;
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoResizePdfMargins = autoResizeTopMargin;

            // Set header visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInFirstPage = model.ShowHeaderInFirstPage;
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInOddPages = model.ShowHeaderInOddPages;
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInEvenPages = model.ShowHeaderInEvenPages;

            // Reserve space for the header on all pages, regardless of visibility
            // If false, the document will be rendered using print styles instead of screen styles
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ReserveSpaceAlways = model.ReserveHeaderSpace;

            // Optimize the header rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
            wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.SkipVariablesParsing = model.SkipHeaderVariablesParsing;

            // Optionally set additional time to wait for the asynchronous header HTML content before rendering
            if (model.HeaderConversionDelay.HasValue)
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ConversionDelay = model.HeaderConversionDelay.Value;
        }

        private void SetFooter(WordToPdfConverter wordToPdfConverter, Word_to_PDF_ViewModel model)
        {
            bool footerEnabled = model.FooterEnabled;
            if (footerEnabled)
            {
                // Set the footer HTML from a URL or from an HTML string
                bool footerHtmlFromUrl = model.FooterHtmlSource == "Url";
                if (footerHtmlFromUrl)
                {
                    string footerUrl = model.FooterUrlTextBox;

                    wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlSourceUrl = footerUrl;
                }
                else
                {
                    string footerHtml = model.FooterHtmlTextBox;
                    string footerHtmlBaseUrl = model.FooterHtmlBaseUrlTextBox;

                    wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Html = footerHtml;
                    wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = footerHtmlBaseUrl;
                }

                // Enable automatic height adjustment based on footer HTML content
                bool autoSizeFooterContentHeight = model.AutoSizeFooterContentHeight;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoSizeContentHeight = autoSizeFooterContentHeight;

                // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MinContentHeight = model.FooterMinContentHeight;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MaxContentHeight = model.FooterMaxContentHeight;

                // Set a fixed height for the footer if AutoResizeHeight is disabled
                if (model.FooterHeight.HasValue)
                {
                    int footerHeight = model.FooterHeight.Value;
                    wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Height = footerHeight;
                }

                // If AutoResizeHeight is enabled and both Height and FitHeight are set,
                // the content may be scaled down to fit the specified height
                bool fitFooterHeight = model.FitFooterHeight;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.FitHeight = fitFooterHeight;

                // Enable automatic bottom margin adjustment in the PDF based on the footer
                bool autoResizeBottomMargin = model.AutoResizeBottomMargin;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoResizePdfMargins = autoResizeBottomMargin;

                // Set footer visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInFirstPage = model.ShowFooterInFirstPage;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInOddPages = model.ShowFooterInOddPages;
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInEvenPages = model.ShowFooterInEvenPages;

                // Reserve space for the footer on all pages, regardless of visibility
                // If false, the document will be rendered using print styles instead of screen styles
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ReserveSpaceAlways = model.ReserveFooterSpace;

                // Optimize the footer rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
                wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.SkipVariablesParsing = model.SkipFooterVariablesParsing;

                // Optionally set additional time to wait for the asynchronous footer HTML content before rendering
                if (model.FooterConversionDelay.HasValue)
                    wordToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ConversionDelay = model.FooterConversionDelay.Value;
            }
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

        private Word_to_PDF_ViewModel SetViewModel()
        {
            var model = new Word_to_PDF_ViewModel();

            var contentRootPath = System.IO.Path.Combine(m_hostingEnvironment.ContentRootPath, "wwwroot");

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Word_To_PDF".Length);

            model.WordFileUrl = rootUrl + "/DemoAppFiles/Input/Word_Files/Word_Document.docx";
            model.HeaderHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Header_HTML.html"));
            model.FooterHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Footer_HTML.html"));
            model.HeaderHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.HeaderUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Header_HTML.html";
            model.FooterHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.FooterUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Footer_HTML.html";

            return model;
        }
    }
}
```

## Convert Excel XLSX to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-excel-xlsx-to-pdf.htm

### Create the Excel to PDF Converter

```csharp
// Create a new Excel to PDF converter instance
ExcelToPdfConverter excelToPdfConverter = new ExcelToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
excelToPdfConverter.PdfDocumentOptions.UsePageSettingsFromExcel = true;
excelToPdfConverter.PdfDocumentOptions.UseFirstWorksheetPageSettings = true;
```

```csharp
excelToPdfConverter.PdfDocumentOptions.UsePageSettingsFromExcel = false;
excelToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
excelToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
excelToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
excelToPdfConverter.PdfDocumentOptions.RightMargin = 20;
excelToPdfConverter.PdfDocumentOptions.TopMargin = 30;
excelToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

### Convert Only the First Worksheet or All Worksheets

```csharp
excelToPdfConverter.PdfDocumentOptions.ConvertOnlyFirstWorksheet = false;
excelToPdfConverter.PdfDocumentOptions.PageBreakBetweenWorksheets = true;
```

### Code Sample - Convert Excel to PDF

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Hosting;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.Excel_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.Excel_to_PDF
{
    public class Excel_to_PDFController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Excel_to_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ConvertExcelToPdf(Excel_to_PDF_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create an Excel to PDF converter object with default settings
            ExcelToPdfConverter excelToPdfConverter = new ExcelToPdfConverter();

            // Set whether to keep the original Excel column widths percentage in the generated PDF document
            excelToPdfConverter.KeepColumnWidths = model.KeepColumnWidths;

            // Set whether to keep the original Excel row heights in the generated PDF document
            excelToPdfConverter.KeepRowHeights = model.KeepRowHeights;

            // Set whether to use page settings (size, margins) from the Excel document or the custom settings
            excelToPdfConverter.PdfDocumentOptions.UsePageSettingsFromExcel = model.PdfPageSettingsMode == "FromExcelDocument";

            // Set whether page settings from the first worksheet or from active worksheet are used for all worksheets
            // Has no effect when UsePageSettingsFromExcel is false
            excelToPdfConverter.PdfDocumentOptions.UseFirstWorksheetPageSettings = model.UseFirstWorksheetPageSettings;

            // Set whether to convert only the first worksheet of the workbook or all worksheets
            excelToPdfConverter.PdfDocumentOptions.ConvertOnlyFirstWorksheet = model.ConvertOnlyFirstWorksheet;

            // Set whether each worksheet starts on a new PDF page
            excelToPdfConverter.PdfDocumentOptions.PageBreakBetweenWorksheets = model.PageBreakBetweenWorksheets;

            // Set whether the worksheet title is displayed at the top of each worksheet in the PDF document
            excelToPdfConverter.PdfDocumentOptions.DisplayWorksheetTitle = model.DisplayWorksheetTitle;

            if (!excelToPdfConverter.PdfDocumentOptions.UsePageSettingsFromExcel)
            {
                // Set PDF page size which can be a predefined size like A4 or a custom size in points 
                // Leave it not set to have a default A4 PDF page
                excelToPdfConverter.PdfDocumentOptions.PdfPageSize = SelectedPdfPageSize(model.PdfPageSize);

                // Set PDF page orientation to Portrait or Landscape
                // Leave it not set to have a default Portrait orientation for PDF page
                excelToPdfConverter.PdfDocumentOptions.PdfPageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation);

                // Set PDF page margins in points or leave them not set to have a PDF page without margins
                excelToPdfConverter.PdfDocumentOptions.LeftMargin = model.LeftMargin;
                excelToPdfConverter.PdfDocumentOptions.RightMargin = model.RightMargin;
                excelToPdfConverter.PdfDocumentOptions.TopMargin = model.TopMargin;
                excelToPdfConverter.PdfDocumentOptions.BottomMargin = model.BottomMargin;

                // Set the excel viewer zoom percentage
                excelToPdfConverter.PdfDocumentOptions.Zoom = model.ExcelViewerZoom;
            }

            // Set PDF header and footer
            SetHeader(excelToPdfConverter, model);
            SetFooter(excelToPdfConverter, model);

            byte[] inputExcelBytes = null;

            // If an uploaded file exists, use it with priority
            if (model.ExcelFile != null && model.ExcelFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.ExcelFile.CopyToAsync(ms);
                    inputExcelBytes = ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded Excel file", ex);
                }
            }
            else
            {
                // Otherwise, fall back to the URL
                string excelUrl = model.ExcelFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(excelUrl))
                    throw new Exception("No Excel file provided: upload a file or specify a URL");

                try
                {
                    if (excelUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(excelUrl).LocalPath;
                        inputExcelBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputExcelBytes = await httpClient.GetByteArrayAsync(excelUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the Excel file from URL", ex);
                }
            }

            // The buffer to receive the generated PDF document
            byte[] outPdfBuffer = excelToPdfConverter.ConvertToPdf(inputExcelBytes);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            // send as attachment
            fileResult.FileDownloadName = "Excel_to_Pdf.pdf";

            return fileResult;
        }

        private void SetHeader(ExcelToPdfConverter excelToPdfConverter, Excel_to_PDF_ViewModel model)
        {
            bool headerEnabled = model.HeaderEnabled;
            if (!headerEnabled)
                return;

            // Set the header HTML from a URL or from an HTML string
            bool headerHtmlFromUrl = model.HeaderHtmlSource == "Url";
            if (headerHtmlFromUrl)
            {
                string headerUrl = model.HeaderUrlTextBox;

                excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlSourceUrl = headerUrl;
            }
            else
            {
                string headerHtml = model.HeaderHtmlTextBox;
                string headerHtmlBaseUrl = model.HeaderHtmlBaseUrlTextBox;

                excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Html = headerHtml;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlBaseUrl = headerHtmlBaseUrl;
            }

            // Enable automatic height adjustment based on header HTML content
            bool autoSizeHeaderContentHeight = model.AutoSizeHeaderContentHeight;
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoSizeContentHeight = autoSizeHeaderContentHeight;

            // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MinContentHeight = model.HeaderMinContentHeight;
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MaxContentHeight = model.HeaderMaxContentHeight;

            // Set a fixed height for the header if AutoResizeHeight is disabled
            if (model.HeaderHeight.HasValue)
            {
                int headerHeight = model.HeaderHeight.Value;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Height = headerHeight;
            }

            // If AutoResizeHeight is enabled and both Height and FitHeight are set,
            // the content may be scaled down to fit the specified height
            bool fitHeaderHeight = model.FitHeaderHeight;
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.FitHeight = fitHeaderHeight;

            // Enable automatic top margin adjustment in the PDF based on the header
            bool autoResizeTopMargin = model.AutoResizeTopMargin;
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoResizePdfMargins = autoResizeTopMargin;

            // Set header visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInFirstPage = model.ShowHeaderInFirstPage;
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInOddPages = model.ShowHeaderInOddPages;
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInEvenPages = model.ShowHeaderInEvenPages;

            // Reserve space for the header on all pages, regardless of visibility
            // If false, the document will be rendered using print styles instead of screen styles
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ReserveSpaceAlways = model.ReserveHeaderSpace;

            // Optimize the header rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
            excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.SkipVariablesParsing = model.SkipHeaderVariablesParsing;

            // Optionally set additional time to wait for the asynchronous header HTML content before rendering
            if (model.HeaderConversionDelay.HasValue)
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ConversionDelay = model.HeaderConversionDelay.Value;
        }

        private void SetFooter(ExcelToPdfConverter excelToPdfConverter, Excel_to_PDF_ViewModel model)
        {
            bool footerEnabled = model.FooterEnabled;
            if (footerEnabled)
            {
                // Set the footer HTML from a URL or from an HTML string
                bool footerHtmlFromUrl = model.FooterHtmlSource == "Url";
                if (footerHtmlFromUrl)
                {
                    string footerUrl = model.FooterUrlTextBox;

                    excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlSourceUrl = footerUrl;
                }
                else
                {
                    string footerHtml = model.FooterHtmlTextBox;
                    string footerHtmlBaseUrl = model.FooterHtmlBaseUrlTextBox;

                    excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Html = footerHtml;
                    excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = footerHtmlBaseUrl;
                }

                // Enable automatic height adjustment based on footer HTML content
                bool autoSizeFooterContentHeight = model.AutoSizeFooterContentHeight;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoSizeContentHeight = autoSizeFooterContentHeight;

                // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MinContentHeight = model.FooterMinContentHeight;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MaxContentHeight = model.FooterMaxContentHeight;

                // Set a fixed height for the footer if AutoResizeHeight is disabled
                if (model.FooterHeight.HasValue)
                {
                    int footerHeight = model.FooterHeight.Value;
                    excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Height = footerHeight;
                }

                // If AutoResizeHeight is enabled and both Height and FitHeight are set,
                // the content may be scaled down to fit the specified height
                bool fitFooterHeight = model.FitFooterHeight;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.FitHeight = fitFooterHeight;

                // Enable automatic bottom margin adjustment in the PDF based on the footer
                bool autoResizeBottomMargin = model.AutoResizeBottomMargin;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoResizePdfMargins = autoResizeBottomMargin;

                // Set footer visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInFirstPage = model.ShowFooterInFirstPage;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInOddPages = model.ShowFooterInOddPages;
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInEvenPages = model.ShowFooterInEvenPages;

                // Reserve space for the footer on all pages, regardless of visibility
                // If false, the document will be rendered using print styles instead of screen styles
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ReserveSpaceAlways = model.ReserveFooterSpace;

                // Optimize the footer rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
                excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.SkipVariablesParsing = model.SkipFooterVariablesParsing;

                // Optionally set additional time to wait for the asynchronous footer HTML content before rendering
                if (model.FooterConversionDelay.HasValue)
                    excelToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ConversionDelay = model.FooterConversionDelay.Value;
            }
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

        private Excel_to_PDF_ViewModel SetViewModel()
        {
            var model = new Excel_to_PDF_ViewModel();

            var contentRootPath = System.IO.Path.Combine(m_hostingEnvironment.ContentRootPath, "wwwroot");

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Excel_To_PDF".Length);

            model.ExcelFileUrl = rootUrl + "/DemoAppFiles/Input/Excel_Files/Excel_Document.xlsx";
            model.HeaderHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Header_HTML.html"));
            model.FooterHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Footer_HTML.html"));
            model.HeaderHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.HeaderUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Header_HTML.html";
            model.FooterHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.FooterUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Footer_HTML.html";

            return model;
        }
    }
}
```

## Convert RTF to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-rtf-to-pdf.htm

### Create the RTF to PDF Converter

```csharp
// Create a new RTF to PDF converter instance
RtfToPdfConverter rtfToPdfConverter = new RtfToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
rtfToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
rtfToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
rtfToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
rtfToPdfConverter.PdfDocumentOptions.RightMargin = 20;
rtfToPdfConverter.PdfDocumentOptions.TopMargin = 30;
rtfToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

### Code Sample - Convert RTF to PDF

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Hosting;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.RTF_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.RTF_to_PDF
{
    public class RTF_to_PDFController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public RTF_to_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ConvertRtfToPdf(RTF_to_PDF_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create an RTF to PDF converter object with default settings
            RtfToPdfConverter rtfToPdfConverter = new RtfToPdfConverter();

            // Set whether a table of contents is automatically generated from headings
            rtfToPdfConverter.PdfDocumentOptions.GenerateTableOfContents = model.GenerateToc;

            // Set PDF page size which can be a predefined size like A4 or a custom size in points 
            // Leave it not set to have a default A4 PDF page
            rtfToPdfConverter.PdfDocumentOptions.PdfPageSize = SelectedPdfPageSize(model.PdfPageSize);

            // Set PDF page orientation to Portrait or Landscape
            // Leave it not set to have a default Portrait orientation for PDF page
            rtfToPdfConverter.PdfDocumentOptions.PdfPageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation);

            // Set PDF page margins in points or leave them not set to have a PDF page without margins
            rtfToPdfConverter.PdfDocumentOptions.LeftMargin = model.LeftMargin;
            rtfToPdfConverter.PdfDocumentOptions.RightMargin = model.RightMargin;
            rtfToPdfConverter.PdfDocumentOptions.TopMargin = model.TopMargin;
            rtfToPdfConverter.PdfDocumentOptions.BottomMargin = model.BottomMargin;

            // Set the RTF viewer zoom percentage
            rtfToPdfConverter.PdfDocumentOptions.Zoom = model.RtfViewerZoom;

            // Set PDF header and footer
            SetHeader(rtfToPdfConverter, model);
            SetFooter(rtfToPdfConverter, model);

            byte[] inputRtfBytes = null;

            // If an uploaded file exists, use it with priority
            if (model.RtfFile != null && model.RtfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.RtfFile.CopyToAsync(ms);
                    inputRtfBytes = ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded RTF file", ex);
                }
            }
            else
            {
                // Otherwise, fall back to the URL
                string rtfUrl = model.RtfFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(rtfUrl))
                    throw new Exception("No RTF file provided: upload a file or specify a URL");

                try
                {
                    if (rtfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(rtfUrl).LocalPath;
                        inputRtfBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputRtfBytes = await httpClient.GetByteArrayAsync(rtfUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the RTF file from URL", ex);
                }
            }

            // The buffer to receive the generated PDF document
            byte[] outPdfBuffer = rtfToPdfConverter.ConvertToPdf(inputRtfBytes);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            // send as attachment
            fileResult.FileDownloadName = "RTF_to_Pdf.pdf";

            return fileResult;
        }

        private void SetHeader(RtfToPdfConverter rtfToPdfConverter, RTF_to_PDF_ViewModel model)
        {
            bool headerEnabled = model.HeaderEnabled;
            if (!headerEnabled)
                return;

            // Set the header HTML from a URL or from an HTML string
            bool headerHtmlFromUrl = model.HeaderHtmlSource == "Url";
            if (headerHtmlFromUrl)
            {
                string headerUrl = model.HeaderUrlTextBox;

                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlSourceUrl = headerUrl;
            }
            else
            {
                string headerHtml = model.HeaderHtmlTextBox;
                string headerHtmlBaseUrl = model.HeaderHtmlBaseUrlTextBox;

                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Html = headerHtml;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlBaseUrl = headerHtmlBaseUrl;
            }

            // Enable automatic height adjustment based on header HTML content
            bool autoSizeHeaderContentHeight = model.AutoSizeHeaderContentHeight;
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoSizeContentHeight = autoSizeHeaderContentHeight;

            // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MinContentHeight = model.HeaderMinContentHeight;
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MaxContentHeight = model.HeaderMaxContentHeight;

            // Set a fixed height for the header if AutoResizeHeight is disabled
            if (model.HeaderHeight.HasValue)
            {
                int headerHeight = model.HeaderHeight.Value;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Height = headerHeight;
            }

            // If AutoResizeHeight is enabled and both Height and FitHeight are set,
            // the content may be scaled down to fit the specified height
            bool fitHeaderHeight = model.FitHeaderHeight;
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.FitHeight = fitHeaderHeight;

            // Enable automatic top margin adjustment in the PDF based on the header
            bool autoResizeTopMargin = model.AutoResizeTopMargin;
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoResizePdfMargins = autoResizeTopMargin;

            // Set header visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInFirstPage = model.ShowHeaderInFirstPage;
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInOddPages = model.ShowHeaderInOddPages;
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInEvenPages = model.ShowHeaderInEvenPages;

            // Reserve space for the header on all pages, regardless of visibility
            // If false, the document will be rendered using print styles instead of screen styles
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ReserveSpaceAlways = model.ReserveHeaderSpace;

            // Optimize the header rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
            rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.SkipVariablesParsing = model.SkipHeaderVariablesParsing;

            // Optionally set additional time to wait for the asynchronous header HTML content before rendering
            if (model.HeaderConversionDelay.HasValue)
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ConversionDelay = model.HeaderConversionDelay.Value;
        }

        private void SetFooter(RtfToPdfConverter rtfToPdfConverter, RTF_to_PDF_ViewModel model)
        {
            bool footerEnabled = model.FooterEnabled;
            if (footerEnabled)
            {
                // Set the footer HTML from a URL or from an HTML string
                bool footerHtmlFromUrl = model.FooterHtmlSource == "Url";
                if (footerHtmlFromUrl)
                {
                    string footerUrl = model.FooterUrlTextBox;

                    rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlSourceUrl = footerUrl;
                }
                else
                {
                    string footerHtml = model.FooterHtmlTextBox;
                    string footerHtmlBaseUrl = model.FooterHtmlBaseUrlTextBox;

                    rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Html = footerHtml;
                    rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = footerHtmlBaseUrl;
                }

                // Enable automatic height adjustment based on footer HTML content
                bool autoSizeFooterContentHeight = model.AutoSizeFooterContentHeight;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoSizeContentHeight = autoSizeFooterContentHeight;

                // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MinContentHeight = model.FooterMinContentHeight;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MaxContentHeight = model.FooterMaxContentHeight;

                // Set a fixed height for the footer if AutoResizeHeight is disabled
                if (model.FooterHeight.HasValue)
                {
                    int footerHeight = model.FooterHeight.Value;
                    rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Height = footerHeight;
                }

                // If AutoResizeHeight is enabled and both Height and FitHeight are set,
                // the content may be scaled down to fit the specified height
                bool fitFooterHeight = model.FitFooterHeight;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.FitHeight = fitFooterHeight;

                // Enable automatic bottom margin adjustment in the PDF based on the footer
                bool autoResizeBottomMargin = model.AutoResizeBottomMargin;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoResizePdfMargins = autoResizeBottomMargin;

                // Set footer visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInFirstPage = model.ShowFooterInFirstPage;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInOddPages = model.ShowFooterInOddPages;
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInEvenPages = model.ShowFooterInEvenPages;

                // Reserve space for the footer on all pages, regardless of visibility
                // If false, the document will be rendered using print styles instead of screen styles
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ReserveSpaceAlways = model.ReserveFooterSpace;

                // Optimize the footer rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
                rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.SkipVariablesParsing = model.SkipFooterVariablesParsing;

                // Optionally set additional time to wait for the asynchronous footer HTML content before rendering
                if (model.FooterConversionDelay.HasValue)
                    rtfToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ConversionDelay = model.FooterConversionDelay.Value;
            }
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

        private RTF_to_PDF_ViewModel SetViewModel()
        {
            var model = new RTF_to_PDF_ViewModel();

            var contentRootPath = System.IO.Path.Combine(m_hostingEnvironment.ContentRootPath, "wwwroot");

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "RTF_To_PDF".Length);

            model.RtfFileUrl = rootUrl + "/DemoAppFiles/Input/RTF_Files/RTF_Document.rtf";
            model.HeaderHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Header_HTML.html"));
            model.FooterHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Footer_HTML.html"));
            model.HeaderHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.HeaderUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Header_HTML.html";
            model.FooterHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.FooterUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Footer_HTML.html";

            return model;
        }
    }
}
```

## Convert Markdown to PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-markdown-to-pdf.htm

### Create the Markdown to PDF Converter

```csharp
// Create a new Markdown to PDF converter instance
MarkdownToPdfConverter markdownToPdfConverter = new MarkdownToPdfConverter();
```

### Configure the PDF Page Settings

```csharp
markdownToPdfConverter.PdfDocumentOptions.PdfPageSize = PdfPageSize.A4;
markdownToPdfConverter.PdfDocumentOptions.PdfPageOrientation = PdfPageOrientation.Landscape;
markdownToPdfConverter.PdfDocumentOptions.LeftMargin = 20;
markdownToPdfConverter.PdfDocumentOptions.RightMargin = 20;
markdownToPdfConverter.PdfDocumentOptions.TopMargin = 30;
markdownToPdfConverter.PdfDocumentOptions.BottomMargin = 30;
```

### Apply a Custom Style Sheet

```csharp
markdownToPdfConverter.PdfDocumentOptions.StyleSheet = @"
h1 { 
    font-size: 24pt;
    color: #0b57d0;
}
";
```

```csharp
byte[] inputMarkdownBytes = System.IO.File.ReadAllBytes(markdownFilePath);
string markdownString = Encoding.UTF8.GetString(inputMarkdownBytes);
string baseUrl = "file://" + markdownFilePath;
```

```csharp
string markdownString = System.IO.File.ReadAllText(markdownFilePath, Encoding.UTF8);
string baseUrl = "file://" + markdownFilePath;
```

### Code Sample - Convert Markdown to PDF

```csharp
using System;
using System.ComponentModel.DataAnnotations;
using System.Text;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.Markdown_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.Markdown_to_PDF
{
    public class Markdown_to_PDFController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Markdown_to_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> ConvertMarkdownToPdf(Markdown_to_PDF_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a Markdown to PDF converter object with default settings
            MarkdownToPdfConverter markdownToPdfConverter = new MarkdownToPdfConverter();

            // Set custom styling rules using CSS syntax
            markdownToPdfConverter.PdfDocumentOptions.StyleSheet = model.StyleSheet;

            // Set whether a table of contents is automatically generated from headings
            markdownToPdfConverter.PdfDocumentOptions.GenerateTableOfContents = model.GenerateToc;

            // Set PDF page size which can be a predefined size like A4 or a custom size in points 
            // Leave it not set to have a default A4 PDF page
            markdownToPdfConverter.PdfDocumentOptions.PdfPageSize = SelectedPdfPageSize(model.PdfPageSize);

            // Set PDF page orientation to Portrait or Landscape
            // Leave it not set to have a default Portrait orientation for PDF page
            markdownToPdfConverter.PdfDocumentOptions.PdfPageOrientation = SelectedPdfPageOrientation(model.PdfPageOrientation);

            // Set PDF page margins in points or leave them not set to have a PDF page without margins
            markdownToPdfConverter.PdfDocumentOptions.LeftMargin = model.LeftMargin;
            markdownToPdfConverter.PdfDocumentOptions.RightMargin = model.RightMargin;
            markdownToPdfConverter.PdfDocumentOptions.TopMargin = model.TopMargin;
            markdownToPdfConverter.PdfDocumentOptions.BottomMargin = model.BottomMargin;

            // Set the Markdown viewer zoom percentage
            markdownToPdfConverter.PdfDocumentOptions.Zoom = model.MarkdownViewerZoom;

            // Set PDF header and footer
            SetHeader(markdownToPdfConverter, model);
            SetFooter(markdownToPdfConverter, model);

            string markdownString = null;
            string baseUrl = null;

            if (model.MarkdownSource == "Url")
            {
                // Obtain the Markdown string and base URL from a Markdown file using UTF-8 encoding
                string markdownUrl = model.MarkdownFileUrl?.Trim();
                if (string.IsNullOrWhiteSpace(markdownUrl))
                    throw new Exception("No Markdown file provided: upload a file or specify a URL");

                byte[] inputMarkdownBytes = null;

                try
                {
                    if (markdownUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                    {
                        string localPath = new Uri(markdownUrl).LocalPath;
                        inputMarkdownBytes = await System.IO.File.ReadAllBytesAsync(localPath);
                    }
                    else
                    {
                        using var httpClient = new System.Net.Http.HttpClient();
                        inputMarkdownBytes = await httpClient.GetByteArrayAsync(markdownUrl);
                    }
                }
                catch (Exception ex)
                {
                    throw new Exception("Could not download the Markdown file from URL", ex);
                }

                markdownString = Encoding.UTF8.GetString(inputMarkdownBytes);
                baseUrl = markdownUrl;
            }
            else
            {
                // The Markdown string and base URL are provided directly
                markdownString = model.MarkdownString;
                baseUrl = model.BaseUrl;
            }

            // Convert the Markdown string to a PDF document in a memory buffer
            byte[] outPdfBuffer = markdownToPdfConverter.ConvertStringToPdf(markdownString, baseUrl);

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            // send as attachment
            fileResult.FileDownloadName = "Markdown_to_Pdf.pdf";

            return fileResult;
        }

        private void SetHeader(MarkdownToPdfConverter markdownToPdfConverter, Markdown_to_PDF_ViewModel model)
        {
            bool headerEnabled = model.HeaderEnabled;
            if (!headerEnabled)
                return;

            // Set the header HTML from a URL or from an HTML string
            bool headerHtmlFromUrl = model.HeaderHtmlSource == "Url";
            if (headerHtmlFromUrl)
            {
                string headerUrl = model.HeaderUrlTextBox;

                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlSourceUrl = headerUrl;
            }
            else
            {
                string headerHtml = model.HeaderHtmlTextBox;
                string headerHtmlBaseUrl = model.HeaderHtmlBaseUrlTextBox;

                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Html = headerHtml;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.HtmlBaseUrl = headerHtmlBaseUrl;
            }

            // Enable automatic height adjustment based on header HTML content
            bool autoSizeHeaderContentHeight = model.AutoSizeHeaderContentHeight;
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoSizeContentHeight = autoSizeHeaderContentHeight;

            // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MinContentHeight = model.HeaderMinContentHeight;
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.MaxContentHeight = model.HeaderMaxContentHeight;

            // Set a fixed height for the header if AutoResizeHeight is disabled
            if (model.HeaderHeight.HasValue)
            {
                int headerHeight = model.HeaderHeight.Value;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.Height = headerHeight;
            }

            // If AutoResizeHeight is enabled and both Height and FitHeight are set,
            // the content may be scaled down to fit the specified height
            bool fitHeaderHeight = model.FitHeaderHeight;
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.FitHeight = fitHeaderHeight;

            // Enable automatic top margin adjustment in the PDF based on the header
            bool autoResizeTopMargin = model.AutoResizeTopMargin;
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.AutoResizePdfMargins = autoResizeTopMargin;

            // Set header visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInFirstPage = model.ShowHeaderInFirstPage;
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInOddPages = model.ShowHeaderInOddPages;
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ShowInEvenPages = model.ShowHeaderInEvenPages;

            // Reserve space for the header on all pages, regardless of visibility
            // If false, the document will be rendered using print styles instead of screen styles
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ReserveSpaceAlways = model.ReserveHeaderSpace;

            // Optimize the header rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
            markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.SkipVariablesParsing = model.SkipHeaderVariablesParsing;

            // Optionally set additional time to wait for the asynchronous header HTML content before rendering
            if (model.HeaderConversionDelay.HasValue)
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlHeader.ConversionDelay = model.HeaderConversionDelay.Value;
        }

        private void SetFooter(MarkdownToPdfConverter markdownToPdfConverter, Markdown_to_PDF_ViewModel model)
        {
            bool footerEnabled = model.FooterEnabled;
            if (footerEnabled)
            {
                // Set the footer HTML from a URL or from an HTML string
                bool footerHtmlFromUrl = model.FooterHtmlSource == "Url";
                if (footerHtmlFromUrl)
                {
                    string footerUrl = model.FooterUrlTextBox;

                    markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlSourceUrl = footerUrl;
                }
                else
                {
                    string footerHtml = model.FooterHtmlTextBox;
                    string footerHtmlBaseUrl = model.FooterHtmlBaseUrlTextBox;

                    markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Html = footerHtml;
                    markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.HtmlBaseUrl = footerHtmlBaseUrl;
                }

                // Enable automatic height adjustment based on footer HTML content
                bool autoSizeFooterContentHeight = model.AutoSizeFooterContentHeight;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoSizeContentHeight = autoSizeFooterContentHeight;

                // Set the minimum and maximum content height used when AutoSizeContentHeight is enabled
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MinContentHeight = model.FooterMinContentHeight;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.MaxContentHeight = model.FooterMaxContentHeight;

                // Set a fixed height for the footer if AutoResizeHeight is disabled
                if (model.FooterHeight.HasValue)
                {
                    int footerHeight = model.FooterHeight.Value;
                    markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.Height = footerHeight;
                }

                // If AutoResizeHeight is enabled and both Height and FitHeight are set,
                // the content may be scaled down to fit the specified height
                bool fitFooterHeight = model.FitFooterHeight;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.FitHeight = fitFooterHeight;

                // Enable automatic bottom margin adjustment in the PDF based on the footer
                bool autoResizeBottomMargin = model.AutoResizeBottomMargin;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.AutoResizePdfMargins = autoResizeBottomMargin;

                // Set footer visibility on specific PDF pages: first page, odd-numbered pages and even-numbered pages
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInFirstPage = model.ShowFooterInFirstPage;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInOddPages = model.ShowFooterInOddPages;
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ShowInEvenPages = model.ShowFooterInEvenPages;

                // Reserve space for the footer on all pages, regardless of visibility
                // If false, the document will be rendered using print styles instead of screen styles
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ReserveSpaceAlways = model.ReserveFooterSpace;

                // Optimize the footer rendering time by providing a hint if the HTML template contains variables such as { page_number} or { total_pages}
                markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.SkipVariablesParsing = model.SkipFooterVariablesParsing;

                // Optionally set additional time to wait for the asynchronous footer HTML content before rendering
                if (model.FooterConversionDelay.HasValue)
                    markdownToPdfConverter.PdfDocumentOptions.PdfHtmlFooter.ConversionDelay = model.FooterConversionDelay.Value;
            }
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

        private Markdown_to_PDF_ViewModel SetViewModel()
        {
            var model = new Markdown_to_PDF_ViewModel();

            var contentRootPath = System.IO.Path.Combine(m_hostingEnvironment.ContentRootPath, "wwwroot");

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Markdown_To_PDF".Length);

            model.MarkdownFileUrl = rootUrl + "/DemoAppFiles/Input/Markdown_Files/Markdown_Document.md";
            model.MarkdownString = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/Markdown_Files/Markdown_Document.md"));
            model.BaseUrl = rootUrl + "/DemoAppFiles/Input/Markdown_Files/";
            model.StyleSheet = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/Markdown_Files/Markdown_Style.css"));
            model.HeaderHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Header_HTML.html"));
            model.FooterHtmlTextBox = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Footer_HTML.html"));
            model.HeaderHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.HeaderUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Header_HTML.html";
            model.FooterHtmlBaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.FooterUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/Footer_HTML.html";

            return model;
        }
    }
}
```
