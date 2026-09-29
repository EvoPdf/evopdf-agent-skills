# Link and text annotations: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Create PDF Documents with Link Annotations

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-link-annotations.htm

### External URL Links

```csharp
PdfTextElement t = new PdfTextElement("Visit evopdf.com", linkFont) { X = 0, Y = crtYPos };
var info = pdfDocument.AddText(t);
var b = info.LastPageRectangle.Bounds;

PdfLinkAnnotation link = PdfLinkAnnotation.FromUrl(
    url: "https://www.evopdf.com",
    pageNumber: 1,
    x: b.X, y: b.Y, width: b.Width, height: b.Height);
link.Description = "EvoPdf homepage";
link.BorderStyle = PdfLinkBorderStyle.None;

pdfDocument.AddLinkAnnotation(link);
```

### Code Sample - Create PDF Documents with Link Annotations

```csharp
using System;
using System.IO;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Creator;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Creator
{
    public class Create_PDF_Documents_with_Link_AnnotationsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_Link_AnnotationsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_with_Link_Annotations_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_Link_Annotations_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
            {
                PageSize = PdfPageSize.A4,
                PageOrientation = PdfPageOrientation.Portrait,
                Margins = new PdfMargins(36, 36, 36, 36),
                PdfStandard = model.PdfStandard,
                Language = "en-US"
            };

            using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
            pdfDocument.PdfDocumentInfo.Title = "PDF Link Annotations Demo";

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont linkFont = PdfFontManager.CreateFont(baseFont, 11f,
                PdfFontStyle.Underline, PdfColor.MediumBlue);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);
            PdfFont anchorFont = PdfFontManager.CreateFont(baseFont, 11f,
                PdfFontStyle.Bold, PdfColor.DarkRed);

            const int xLeft = 0;
            const int ySeparator = 10;
            const int sourcePageNumber = 1;
            const int targetPageNumber = 2;
            int crtYPos = 0;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Link Annotations Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = pdfDocument.ContentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            crtYPos = (int)pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 1: PdfLinkAnnotation.FromUrl =====
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "1. PdfLinkAnnotation.FromUrl (external URLs with border styles)",
                xLeft, crtYPos, ySeparator);

            // Text-style hyperlink - no border, the underlying underlined
            // blue text indicates the clickable area to the reader
            crtYPos = AddUrlLink(pdfDocument, linkFont, labelFont,
                visibleText: "Visit evopdf.com",
                url: "https://www.evopdf.com",
                description: "EvoPdf homepage",
                borderStyle: PdfLinkBorderStyle.None,
                captionText: "BorderStyle = None (typical text hyperlink)",
                x: xLeft, y: crtYPos, pageNumber: sourcePageNumber, separator: ySeparator);

            // Solid border - viewer renders a solid rectangle outline
            // around the hotspot (button-like appearance)
            crtYPos = AddUrlLink(pdfDocument, linkFont, labelFont,
                visibleText: "Visit github.com",
                url: "https://github.com",
                description: "GitHub homepage",
                borderStyle: PdfLinkBorderStyle.Solid,
                captionText: "BorderStyle = Solid, BorderWidth = 1",
                x: xLeft, y: crtYPos, pageNumber: sourcePageNumber, separator: ySeparator);

            // Dashed border - viewer renders a dashed rectangle outline
            crtYPos = AddUrlLink(pdfDocument, linkFont, labelFont,
                visibleText: "Visit wikipedia.org",
                url: "https://www.wikipedia.org",
                description: "Wikipedia main page",
                borderStyle: PdfLinkBorderStyle.Dashed,
                captionText: "BorderStyle = Dashed, BorderWidth = 1",
                x: xLeft, y: crtYPos, pageNumber: sourcePageNumber, separator: ySeparator);

            crtYPos += ySeparator;

            // ===== Section 2: PdfLinkAnnotation.ToPage =====
            EnsureSpaceOnPage(ref crtYPos, 80, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "2. PdfLinkAnnotation.ToPage (jump to whole page)",
                xLeft, crtYPos, ySeparator);

            // Bordered link box pointing at page 2 with the implicit /Fit
            // destination produced by ToPage
            crtYPos = AddIntraDocLink(pdfDocument, linkFont, labelFont,
                visibleText: $"Jump to page {targetPageNumber} (whole page Fit)",
                description: $"Navigate to page {targetPageNumber}",
                linkFactory: (lx, ly, lw, lh) => PdfLinkAnnotation.ToPage(
                    targetPageNumber: targetPageNumber,
                    pageNumber: sourcePageNumber,
                    x: lx, y: ly, width: lw, height: lh),
                captionText: "Navigates to page 2 with the whole page fitted in the view",
                x: xLeft, y: crtYPos, separator: ySeparator);

            crtYPos += ySeparator;

            // ===== Section 3: PdfLinkAnnotation.ToPageLocation =====
            EnsureSpaceOnPage(ref crtYPos, 220, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "3. PdfLinkAnnotation.ToPageLocation (Fit modes and explicit zoom)",
                xLeft, crtYPos, ySeparator);

            // FitPage() -- same effect as ToPage(), exposed through
            // PdfLinkPageLocation so the call site is symmetric with
            // the other fit variants
            crtYPos = AddPageLocationLink(pdfDocument, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.FitPage()",
                description: "Fit whole page in viewport (/Fit destination)",
                location: PdfLinkPageLocation.FitPage(),
                captionText: "Fits the whole target page in the viewport (equivalent to ToPage)",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // FitWidth(top=400) -- fits the page width in the viewport
            // and scrolls vertically so y=400 (from page top) is at the
            // top of the viewport
            crtYPos = AddPageLocationLink(pdfDocument, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.FitWidth(top = 400)",
                description: "Fit page width, scroll so y=400 is at top (/FitH destination)",
                location: PdfLinkPageLocation.FitWidth(top: 400f),
                captionText: "Fits the page WIDTH, scrolled so y=400 from page top is at the top of the viewport",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // FitHeight(left=200) -- fits the page height in the viewport
            // and scrolls horizontally so x=200 (from page left) is at
            // the left of the viewport
            crtYPos = AddPageLocationLink(pdfDocument, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.FitHeight(left = 200)",
                description: "Fit page height, scroll so x=200 is at left (/FitV destination)",
                location: PdfLinkPageLocation.FitHeight(left: 200f),
                captionText: "Fits the page HEIGHT, scrolled so x=200 from page left is at the left of the viewport",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // AtCoordinates(50, 300, zoom=1.5) -- explicit position with
            // custom zoom factor.  The (50, 300) point on the target
            // page is placed at the top-left of the viewport and the
            // viewer applies a 150% zoom
            crtYPos = AddPageLocationLink(pdfDocument, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.AtCoordinates(50, 300, zoom = 1.5)",
                description: "Position (50,300) at viewport top-left with 150% zoom (/XYZ destination)",
                location: PdfLinkPageLocation.AtCoordinates(left: 50f, top: 300f, zoom: 1.5f),
                captionText: "Positions (50,300) at the top-left of the viewport with explicit zoom = 1.5 (150%)",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // ===== TARGET PAGE -- landing points for intra-document links =====
            pdfDocument.AddPage();

            PdfTextElement targetTitle = new PdfTextElement(
                "Target Page — landing points for intra-document links", titleFont)
            {
                X = xLeft, Y = 0,
                Alignment = PdfTextAlignment.Center,
                Width = pdfDocument.ContentWidth
            };
            targetTitle.Accessibility.StructureType = PdfStructureType.Heading1;
            pdfDocument.AddText(targetTitle);

            // Top marker: landing point for FitPage() and ToPage()
            PdfTextElement topMarker = new PdfTextElement(
                "\u2191 Top of page \u2014 landing point for ToPage() and FitPage()", anchorFont)
            {
                X = xLeft, Y = 50, Width = pdfDocument.ContentWidth
            };
            pdfDocument.AddText(topMarker);

            // Horizontal reference line at y=400 for FitWidth(top=400)
            pdfDocument.AddLine(new PdfLineElement(
                xLeft, 400, xLeft + pdfDocument.ContentWidth, 400)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 0.8f,
                    DashStyle = PdfLineDashStyle.Dashed
                }
            });
            PdfTextElement fitWidthMarker = new PdfTextElement(
                "\u2192 y=400 \u2014 top of viewport when FitWidth(top = 400) is followed", anchorFont)
            {
                X = xLeft, Y = 405, Width = pdfDocument.ContentWidth
            };
            pdfDocument.AddText(fitWidthMarker);

            // Vertical reference line at x=200 for FitHeight(left=200)
            pdfDocument.AddLine(new PdfLineElement(
                200, 90, 200, pdfDocument.ContentHeight - 30)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 0.8f,
                    DashStyle = PdfLineDashStyle.Dashed
                }
            });
            PdfTextElement fitHeightMarker = new PdfTextElement(
                "\u2193 x=200 \u2014 left of viewport when FitHeight(left = 200) is followed", anchorFont)
            {
                X = 205, Y = 95, Width = 350
            };
            pdfDocument.AddText(fitHeightMarker);

            // Crosshair at (50, 300) for AtCoordinates(50, 300, zoom=1.5)
            pdfDocument.AddLine(new PdfLineElement(40, 300, 60, 300)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle { LineWidth = 1.2f }
            });
            pdfDocument.AddLine(new PdfLineElement(50, 290, 50, 310)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle { LineWidth = 1.2f }
            });
            PdfTextElement xyzMarker = new PdfTextElement(
                "+ (50, 300) \u2014 top-left of viewport when AtCoordinates(50, 300, zoom = 1.5) is followed",
                anchorFont)
            {
                X = 65, Y = 293, Width = 470
            };
            pdfDocument.AddText(xyzMarker);

            byte[] outPdfBuffer = pdfDocument.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfLinkAnnotationsDemo.pdf";
            return fileResult;
        }

        // === Helpers ===

        // Renders an external-URL link as underlined-blue text and attaches a
        // PdfLinkAnnotation that covers the rendered text bounds
        private int AddUrlLink(
            PdfDocument doc, PdfFont linkFont, PdfFont labelFont,
            string visibleText, string url, string description,
            PdfLinkBorderStyle borderStyle, string captionText,
            int x, int y, int pageNumber, int separator)
        {
            PdfTextElement t = new PdfTextElement(visibleText, linkFont) { X = x, Y = y };
            var info = doc.AddText(t);
            var tb = info.LastPageRectangle.Bounds;

            PdfLinkAnnotation link = PdfLinkAnnotation.FromUrl(
                url: url, pageNumber: pageNumber,
                x: tb.X, y: tb.Y, width: tb.Width, height: tb.Height);
            link.Description = description;
            link.BorderStyle = borderStyle;
            doc.AddLinkAnnotation(link);

            int capBottom = AddCaption(doc, labelFont, captionText,
                x, (int)tb.Bottom + 2, 450);
            return capBottom + separator;
        }

        // Renders an intra-document link as a bordered button with
        // underlined-blue text inside
        private int AddIntraDocLink(
            PdfDocument doc, PdfFont linkFont, PdfFont labelFont,
            string visibleText, string description,
            Func<float, float, float, float, PdfLinkAnnotation> linkFactory,
            string captionText,
            int x, int y, int separator)
        {
            const int linkW = 360;
            const int linkH = 22;

            // Visible bordered hotspot drawn explicitly so the click region
            // is obvious regardless of how the viewer renders /Border.
            doc.AddRectangle(new PdfRectangleElement(x, y, linkW, linkH)
            {
                BorderColor = PdfColor.SteelBlue,
                Border = new PdfLineStyle { LineWidth = 0.6f }
            });
            PdfTextElement t = new PdfTextElement(visibleText, linkFont)
            {
                X = x + 4, Y = y + 4, Width = linkW - 8
            };
            doc.AddText(t);

            PdfLinkAnnotation link = linkFactory(x, y, linkW, linkH);
            link.Description = description;
            doc.AddLinkAnnotation(link);

            int capBottom = AddCaption(doc, labelFont, captionText,
                x, y + linkH + 3, 540);
            return capBottom + separator;
        }

        // Convenience wrapper for ToPageLocation links -- builds the link
        // annotation from the supplied target page and PdfLinkPageLocation
        // then delegates to AddIntraDocLink for layout
        private int AddPageLocationLink(
            PdfDocument doc, PdfFont linkFont, PdfFont labelFont,
            string visibleText, string description,
            PdfLinkPageLocation location, string captionText,
            int x, int y, int pageNumber, int targetPageNumber, int separator)
        {
            return AddIntraDocLink(doc, linkFont, labelFont,
                visibleText: visibleText, description: description,
                linkFactory: (lx, ly, lw, lh) => PdfLinkAnnotation.ToPageLocation(
                    targetPageNumber: targetPageNumber, location: location,
                    pageNumber: pageNumber,
                    x: lx, y: ly, width: lw, height: lh),
                captionText: captionText,
                x: x, y: y, separator: separator);
        }

        private int AddSectionLabel(PdfDocument doc, PdfFont sectionFont,
            string label, int x, int y, int separator)
        {
            PdfTextElement section = new PdfTextElement(label, sectionFont)
            { X = x, Y = y };
            section.Accessibility.StructureType = PdfStructureType.Heading2;
            var info = doc.AddText(section);
            return (int)info.LastPageRectangle.Bounds.Bottom + separator;
        }

        private int AddCaption(PdfDocument doc, PdfFont labelFont,
            string caption, int x, int y, int width)
        {
            PdfTextElement t = new PdfTextElement(caption, labelFont)
            { X = x, Y = y, Width = width };
            t.Accessibility.StructureType = PdfStructureType.Artifact;
            var info = doc.AddText(t);
            return (int)info.LastPageRectangle.Bounds.Bottom;
        }

        private void EnsureSpaceOnPage(ref int crtYPos, int requestedHeight, PdfDocument pdfDocument)
        {
            if (crtYPos + requestedHeight > pdfDocument.ContentHeight)
            {
                pdfDocument.AddPage();
                crtYPos = 0;
            }
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoImagesPath() => Path.Combine(GetDemoFilesPath(), "Image_Files");
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
        private string GetDemoTextsPath() => Path.Combine(GetDemoFilesPath(), "Text_Files");
    }
}
```

## Add Link Annotations to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-link-annotations-to-existing-pdf.htm

### External URL Links

```csharp
PdfTextElement t = new PdfTextElement("Visit evopdf.com", linkFont) { X = 0, Y = crtYPos };
var info = pdfEditor.AddText(currentPage, t);
var b = info.LastPageRectangle.Bounds;

PdfLinkAnnotation link = PdfLinkAnnotation.FromUrl(
    url: "https://www.evopdf.com",
    pageNumber: currentPage,
    x: b.X, y: b.Y, width: b.Width, height: b.Height);
link.Description = "EvoPdf homepage";
link.BorderStyle = PdfLinkBorderStyle.None;
pdfEditor.AddLinkAnnotation(link);
```

### Code Sample - Add Link Annotations to Existing PDF

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
    public class Add_Link_Annotations_to_Existing_PDFController : Controller
    {
        private const int leftMargin = 36;
        private const int topMargin = 36;
        private const int contentWidth = 595 - 72;
        private const int contentHeight = 842 - 72;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_Link_Annotations_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_Link_Annotations_to_Existing_PDF_ViewModel model)
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

            // Open the loaded PDF for editing.  PdfEditor inherits the
            // standard (PDF/A, PDF/UA etc.) and Language from the source
            // document, so no PdfDocumentCreateSettings is needed.  Page
            // size and margins are also taken from the existing pages;
            // we apply our own margins manually below via leftMargin /
            // topMargin since PdfEditor draws at absolute page coordinates
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
            pdfEditor.PdfDocumentInfo.Title = "PDF Link Annotations Demo";

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont linkFont = PdfFontManager.CreateFont(baseFont, 11f,
                PdfFontStyle.Underline, PdfColor.MediumBlue);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);
            PdfFont anchorFont = PdfFontManager.CreateFont(baseFont, 11f,
                PdfFontStyle.Bold, PdfColor.DarkRed);

            const int xLeft = leftMargin;
            const int ySeparator = 10;
            const int sourcePageNumber = 1;
            const int targetPageNumber = 2;
            int currentPage = 1;
            int crtYPos = topMargin;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Link Annotations Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = contentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            var titleElementInfo = pdfEditor.AddText(currentPage, titleElement);
            currentPage = titleElementInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)titleElementInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 1: PdfLinkAnnotation.FromUrl =====
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "1. PdfLinkAnnotation.FromUrl (external URLs with border styles)",
                xLeft, crtYPos, ySeparator);

            // Text-style hyperlink - no border, the underlying underlined
            // blue text indicates the clickable area to the reader
            crtYPos = AddUrlLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: "Visit evopdf.com",
                url: "https://www.evopdf.com",
                description: "EvoPdf homepage",
                borderStyle: PdfLinkBorderStyle.None,
                captionText: "BorderStyle = None (typical text hyperlink)",
                x: xLeft, y: crtYPos, pageNumber: sourcePageNumber, separator: ySeparator);

            // Solid border - viewer renders a solid rectangle outline
            // around the hotspot (button-like appearance)
            crtYPos = AddUrlLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: "Visit github.com",
                url: "https://github.com",
                description: "GitHub homepage",
                borderStyle: PdfLinkBorderStyle.Solid,
                captionText: "BorderStyle = Solid, BorderWidth = 1",
                x: xLeft, y: crtYPos, pageNumber: sourcePageNumber, separator: ySeparator);

            // Dashed border - viewer renders a dashed rectangle outline
            crtYPos = AddUrlLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: "Visit wikipedia.org",
                url: "https://www.wikipedia.org",
                description: "Wikipedia main page",
                borderStyle: PdfLinkBorderStyle.Dashed,
                captionText: "BorderStyle = Dashed, BorderWidth = 1",
                x: xLeft, y: crtYPos, pageNumber: sourcePageNumber, separator: ySeparator);

            crtYPos += ySeparator;

            // ===== Section 2: PdfLinkAnnotation.ToPage =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 80, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "2. PdfLinkAnnotation.ToPage (jump to whole page)",
                xLeft, crtYPos, ySeparator);

            // Bordered link box pointing at page 2 with the implicit /Fit
            // destination produced by ToPage
            crtYPos = AddIntraDocLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: $"Jump to page {targetPageNumber} (whole page Fit)",
                description: $"Navigate to page {targetPageNumber}",
                linkFactory: (lx, ly, lw, lh) => PdfLinkAnnotation.ToPage(
                    targetPageNumber: targetPageNumber,
                    pageNumber: sourcePageNumber,
                    x: lx, y: ly, width: lw, height: lh),
                captionText: "Navigates to page 2 with the whole page fitted in the view",
                x: xLeft, y: crtYPos, separator: ySeparator);

            crtYPos += ySeparator;

            // ===== Section 3: PdfLinkAnnotation.ToPageLocation =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 220, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "3. PdfLinkAnnotation.ToPageLocation (Fit modes and explicit zoom)",
                xLeft, crtYPos, ySeparator);

            // FitPage() -- same effect as ToPage(), exposed through
            // PdfLinkPageLocation so the call site is symmetric with
            // the other fit variants
            crtYPos = AddPageLocationLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.FitPage()",
                description: "Fit whole page in viewport (/Fit destination)",
                location: PdfLinkPageLocation.FitPage(),
                captionText: "Fits the whole target page in the viewport (equivalent to ToPage)",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // FitWidth(top=400) -- fits the page width in the viewport
            // and scrolls vertically so y=400 (from page top) is at the
            // top of the viewport
            crtYPos = AddPageLocationLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.FitWidth(top = 400)",
                description: "Fit page width, scroll so y=400 is at top (/FitH destination)",
                location: PdfLinkPageLocation.FitWidth(top: 400f),
                captionText: "Fits the page WIDTH, scrolled so y=400 from page top is at the top of the viewport",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // FitHeight(left=200) -- fits the page height in the viewport
            // and scrolls horizontally so x=200 (from page left) is at
            // the left of the viewport
            crtYPos = AddPageLocationLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.FitHeight(left = 200)",
                description: "Fit page height, scroll so x=200 is at left (/FitV destination)",
                location: PdfLinkPageLocation.FitHeight(left: 200f),
                captionText: "Fits the page HEIGHT, scrolled so x=200 from page left is at the left of the viewport",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // AtCoordinates(50, 300, zoom=1.5) -- explicit position with
            // custom zoom factor.  The (50, 300) point on the target
            // page is placed at the top-left of the viewport and the
            // viewer applies a 150% zoom
            crtYPos = AddPageLocationLink(pdfEditor, ref currentPage, linkFont, labelFont,
                visibleText: "PdfLinkPageLocation.AtCoordinates(50, 300, zoom = 1.5)",
                description: "Position (50,300) at viewport top-left with 150% zoom (/XYZ destination)",
                location: PdfLinkPageLocation.AtCoordinates(left: 50f, top: 300f, zoom: 1.5f),
                captionText: "Positions (50,300) at the top-left of the viewport with explicit zoom = 1.5 (150%)",
                x: xLeft, y: crtYPos,
                pageNumber: sourcePageNumber, targetPageNumber: targetPageNumber,
                separator: ySeparator);

            // ===== TARGET PAGE -- landing points for intra-document links =====
            currentPage = pdfEditor.AddPage();

            PdfTextElement targetTitle = new PdfTextElement(
                "Target Page — landing points for intra-document links", titleFont)
            {
                X = xLeft, Y = 0,
                Alignment = PdfTextAlignment.Center,
                Width = contentWidth
            };
            targetTitle.Accessibility.StructureType = PdfStructureType.Heading1;
            pdfEditor.AddText(currentPage, targetTitle);

            // Top marker: landing point for FitPage() and ToPage()
            PdfTextElement topMarker = new PdfTextElement(
                "\u2191 Top of page \u2014 landing point for ToPage() and FitPage()", anchorFont)
            {
                X = xLeft, Y = 50, Width = contentWidth
            };
            pdfEditor.AddText(currentPage, topMarker);

            // Horizontal reference line at y=400 for FitWidth(top=400)
            pdfEditor.AddLine(currentPage, new PdfLineElement(
                xLeft, 400, xLeft + contentWidth, 400)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 0.8f,
                    DashStyle = PdfLineDashStyle.Dashed
                }
            });
            PdfTextElement fitWidthMarker = new PdfTextElement(
                "\u2192 y=400 \u2014 top of viewport when FitWidth(top = 400) is followed", anchorFont)
            {
                X = xLeft, Y = 405, Width = contentWidth
            };
            pdfEditor.AddText(currentPage, fitWidthMarker);

            // Vertical reference line at x=200 for FitHeight(left=200)
            pdfEditor.AddLine(currentPage, new PdfLineElement(
                200, 90, 200, contentHeight - 30)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 0.8f,
                    DashStyle = PdfLineDashStyle.Dashed
                }
            });
            PdfTextElement fitHeightMarker = new PdfTextElement(
                "\u2193 x=200 \u2014 left of viewport when FitHeight(left = 200) is followed", anchorFont)
            {
                X = 205, Y = 95, Width = 350
            };
            pdfEditor.AddText(currentPage, fitHeightMarker);

            // Crosshair at (50, 300) for AtCoordinates(50, 300, zoom=1.5)
            pdfEditor.AddLine(currentPage, new PdfLineElement(40, 300, 60, 300)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle { LineWidth = 1.2f }
            });
            pdfEditor.AddLine(currentPage, new PdfLineElement(50, 290, 50, 310)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle { LineWidth = 1.2f }
            });
            PdfTextElement xyzMarker = new PdfTextElement(
                "+ (50, 300) \u2014 top-left of viewport when AtCoordinates(50, 300, zoom = 1.5) is followed",
                anchorFont)
            {
                X = 65, Y = 293, Width = 470
            };
            pdfEditor.AddText(currentPage, xyzMarker);

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfLinkAnnotationsEditDemo.pdf";
            return fileResult;
        }

        // === Helpers ===

        // Renders an external-URL link as underlined-blue text and attaches a
        // PdfLinkAnnotation that covers the rendered text bounds
        private int AddUrlLink(PdfEditor editor, ref int currentPage, PdfFont linkFont, PdfFont labelFont,
            string visibleText, string url, string description,
            PdfLinkBorderStyle borderStyle, string captionText,
            int x, int y, int pageNumber, int separator) {
            PdfTextElement t = new PdfTextElement(visibleText, linkFont) { X = x, Y = y };
            var info = editor.AddText(currentPage, t);
            currentPage = info.LastPageRectangle.PageNumber;
            var tb = info.LastPageRectangle.Bounds;

            PdfLinkAnnotation link = PdfLinkAnnotation.FromUrl(
                url: url, pageNumber: pageNumber,
                x: tb.X, y: tb.Y, width: tb.Width, height: tb.Height);
            link.Description = description;
            link.BorderStyle = borderStyle;
            editor.AddLinkAnnotation(link);

            int capBottom = AddCaption(editor, ref currentPage, labelFont, captionText,
                x, (int)tb.Bottom + 2, 450);
            return capBottom + separator;
        }

        // Renders an intra-document link as a bordered button with
        // underlined-blue text inside
        private int AddIntraDocLink(PdfEditor editor, ref int currentPage, PdfFont linkFont, PdfFont labelFont,
            string visibleText, string description,
            Func<float, float, float, float, PdfLinkAnnotation> linkFactory,
            string captionText,
            int x, int y, int separator) {
            const int linkW = 360;
            const int linkH = 22;

            // Visible bordered hotspot drawn explicitly so the click region
            // is obvious regardless of how the viewer renders /Border.
            editor.AddRectangle(currentPage, new PdfRectangleElement(x, y, linkW, linkH)
            {
                BorderColor = PdfColor.SteelBlue,
                Border = new PdfLineStyle { LineWidth = 0.6f }
            });
            PdfTextElement t = new PdfTextElement(visibleText, linkFont)
            {
                X = x + 4, Y = y + 4, Width = linkW - 8
            };
            editor.AddText(currentPage, t);

            PdfLinkAnnotation link = linkFactory(x, y, linkW, linkH);
            link.Description = description;
            editor.AddLinkAnnotation(link);

            int capBottom = AddCaption(editor, ref currentPage, labelFont, captionText,
                x, y + linkH + 3, contentWidth);
            return capBottom + separator;
        }

        // Convenience wrapper for ToPageLocation links -- builds the link
        // annotation from the supplied target page and PdfLinkPageLocation
        // then delegates to AddIntraDocLink for layout
        private int AddPageLocationLink(PdfEditor editor, ref int currentPage, PdfFont linkFont, PdfFont labelFont,
            string visibleText, string description,
            PdfLinkPageLocation location, string captionText,
            int x, int y, int pageNumber, int targetPageNumber, int separator) {
            return AddIntraDocLink(editor, ref currentPage, linkFont, labelFont,
                visibleText: visibleText, description: description,
                linkFactory: (lx, ly, lw, lh) => PdfLinkAnnotation.ToPageLocation(
                    targetPageNumber: targetPageNumber, location: location,
                    pageNumber: pageNumber,
                    x: lx, y: ly, width: lw, height: lh),
                captionText: captionText,
                x: x, y: y, separator: separator);
        }

        private int AddSectionLabel(PdfEditor editor, ref int currentPage, PdfFont sectionFont,
            string label, int x, int y, int separator) {
            PdfTextElement section = new PdfTextElement(label, sectionFont)
            { X = x, Y = y };
            section.Accessibility.StructureType = PdfStructureType.Heading2;
            var info = editor.AddText(currentPage, section);
            currentPage = info.LastPageRectangle.PageNumber;
            return (int)info.LastPageRectangle.Bounds.Bottom + separator;
        }

        private int AddCaption(PdfEditor editor, ref int currentPage, PdfFont labelFont,
            string caption, int x, int y, int width) {
            PdfTextElement t = new PdfTextElement(caption, labelFont)
            { X = x, Y = y, Width = width };
            t.Accessibility.StructureType = PdfStructureType.Artifact;
            var info = editor.AddText(currentPage, t);
            currentPage = info.LastPageRectangle.PageNumber;
            return (int)info.LastPageRectangle.Bounds.Bottom;
        }

        private void EnsureSpaceOnPage(ref int crtYPos, ref int currentPage, int requestedHeight, PdfEditor pdfEditor, int contentHeight, int topMargin)
        {
            if (crtYPos + requestedHeight > contentHeight + topMargin)
            {
                currentPage = pdfEditor.AddPage();
                crtYPos = topMargin;
            }
        }

        private Add_Link_Annotations_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_Link_Annotations_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_Link_Annotations_to_Existing_PDF".Length);

            // Default input is empty.pdf so this demo edits a fresh
            // blank A4 page.  The user can upload another PDF or paste
            // a different URL
            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/empty.pdf";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoImagesPath() => Path.Combine(GetDemoFilesPath(), "Image_Files");
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
        private string GetDemoTextsPath() => Path.Combine(GetDemoFilesPath(), "Text_Files");
    }
}
```

## Create PDF Documents with Text Annotations

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-text-annotations.htm

### Author Identification and Initial Popup State

```csharp
PdfTextAnnotation ann = PdfTextAnnotation.Create(
    contents: "Please confirm the issue date before mailing.",
    pageNumber: 1, x: 30, y: 200);
ann.Icon = PdfTextAnnotationIcon.Comment;
ann.Author = "Jane Reviewer";
ann.Open = true;
pdfDocument.AddTextAnnotation(ann);
```

### Code Sample - Create PDF Documents with Sticky Note Text Annotations

```csharp
using System;
using System.IO;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Creator;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Creator
{
    public class Create_PDF_Documents_with_Text_AnnotationsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_Text_AnnotationsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_with_Text_Annotations_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_Text_Annotations_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
            {
                PageSize = PdfPageSize.A4,
                PageOrientation = PdfPageOrientation.Portrait,
                Margins = new PdfMargins(36, 36, 36, 36),
                PdfStandard = model.PdfStandard,
                Language = "en-US"
            };

            using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
            pdfDocument.PdfDocumentInfo.Title = "PDF Text Annotations Demo";

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont bodyFont = PdfFontManager.CreateFont(baseFont, 11f,
                PdfFontStyle.Normal, PdfColor.Black);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);
            PdfFont iconCaptionFont = PdfFontManager.CreateFont(baseFont, 10f,
                PdfFontStyle.Bold, PdfColor.DarkSlateGray);

            const int xLeft = 0;
            const int ySeparator = 10;
            const int pageNumber = 1;
            int crtYPos = 0;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Text Annotations Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = pdfDocument.ContentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            crtYPos = (int)pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 1: Icon variants =====
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "1. Icon variants (PdfTextAnnotationIcon)",
                xLeft, crtYPos, ySeparator);

            crtYPos = AddCaption(pdfDocument, bodyFont,
                "PdfTextAnnotation.Icon selects the marker drawn on the page. Click any marker to open its popup",
                xLeft, crtYPos, 540) + ySeparator;

            // Four icons in a row, equally spaced across the content width.
            // Each icon is the default 24x24 sticky-note size; the caption
            // beneath identifies the PdfTextAnnotationIcon enum value.
            const int iconSize = 24;
            int[] iconXs = { 30, 160, 290, 420 };

            AddIconSample(pdfDocument, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Note — the default sticky-note marker, used for general comments",
                icon: PdfTextAnnotationIcon.Note,
                caption: "Note (default)",
                x: iconXs[0], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            AddIconSample(pdfDocument, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Comment — used for review feedback in collaborative workflows",
                icon: PdfTextAnnotationIcon.Comment,
                caption: "Comment",
                x: iconXs[1], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            AddIconSample(pdfDocument, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Help — used to flag content that needs clarification",
                icon: PdfTextAnnotationIcon.Help,
                caption: "Help",
                x: iconXs[2], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            AddIconSample(pdfDocument, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Insert — used to suggest text insertions during editing",
                icon: PdfTextAnnotationIcon.Insert,
                caption: "Insert",
                x: iconXs[3], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            crtYPos += iconSize + 25 + ySeparator;

            // ===== Section 2: Author identification =====
            EnsureSpaceOnPage(ref crtYPos, 120, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "2. Author identification",
                xLeft, crtYPos, ySeparator);

            crtYPos = AddCaption(pdfDocument, bodyFont,
                "Author appears in the popup title and in the viewer's Comments panel, useful for tracking review feedback",
                xLeft, crtYPos, 540) + ySeparator;

            // A body paragraph with a sticky note anchored at the end of a
            // specific phrase that the reviewer is commenting on.  The note
            // popup carries the reviewer's name so multiple reviewers can
            // be told apart in the Comments panel.
            const int paragraphX = 0;
            const int paragraphWidth = 500;
            PdfTextElement paragraph = new PdfTextElement(
                "The PdfTextAnnotation API supports an optional Author property " +
                "that is written to the annotation dictionary as /T. PDF viewers " +
                "show this value as the popup title and group comments by author " +
                "in the Comments panel.",
                bodyFont)
            {
                X = paragraphX, Y = crtYPos, Width = paragraphWidth
            };
            var paraInfo = pdfDocument.AddText(paragraph);
            float paraBottom = paraInfo.LastPageRectangle.Bounds.Bottom;

            // Place the sticky note at the right edge of the paragraph,
            // vertically aligned with its first line.
            AddTextAnnotationFull(pdfDocument,
                contents: "Consider also setting CreationDate when persisting reviewer history. " +
                          "Some viewers sort the Comments panel chronologically.",
                author: "Jane Reviewer",
                icon: PdfTextAnnotationIcon.Comment,
                x: paragraphWidth + 10, y: crtYPos,
                pageNumber: pageNumber);

            crtYPos = (int)paraBottom + ySeparator * 2;

            // ===== Section 3: Initially expanded popup =====
            EnsureSpaceOnPage(ref crtYPos, 80, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "3. Initially expanded popup (Open = true)",
                xLeft, crtYPos, ySeparator);

            crtYPos = AddCaption(pdfDocument, bodyFont,
                "When Open is true the viewer shows the popup expanded as soon as the page is rendered, " +
                "without requiring the reader to click the icon",
                xLeft, crtYPos, 540) + ySeparator;

            // Anchor the open-by-default note so the popup naturally
            // expands into empty space below.
            AddTextAnnotationFull(pdfDocument,
                contents: "This popup is shown expanded on page load because Open = true was set on the annotation. " +
                          "Click the icon to collapse it",
                author: "EVO PDF Demo",
                icon: PdfTextAnnotationIcon.Note,
                x: 30, y: crtYPos,
                pageNumber: pageNumber, open: true);

            AddCaption(pdfDocument, labelFont,
                "Marker on the left — its popup is visible immediately when the PDF is opened",
                60, crtYPos + 6, 480);

            byte[] outPdfBuffer = pdfDocument.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfTextAnnotationsDemo.pdf";
            return fileResult;
        }

        // === Helpers ===

        // Adds a sticky-note annotation with the supplied icon at (x, y)
        // plus a bold caption beneath it identifying the icon name.
        // Used by Section 1 to show all four PdfTextAnnotationIcon values
        // in a horizontal row
        private void AddIconSample(
            PdfDocument doc, PdfFont captionFont,
            string contents, PdfTextAnnotationIcon icon, string caption,
            int x, int y, int pageNumber, int iconSize)
        {
            PdfTextAnnotation ann = PdfTextAnnotation.Create(contents, pageNumber, x, y);
            ann.Icon = icon;
            ann.Width = iconSize;
            ann.Height = iconSize;
            doc.AddTextAnnotation(ann);

            PdfTextElement label = new PdfTextElement(caption, captionFont)
            {
                X = x - 20, Y = y + iconSize + 4, Width = 100,
                Alignment = PdfTextAlignment.Left
            };
            doc.AddText(label);
        }

        // Adds a sticky-note annotation with the full set of properties.
        // Used by sections 2 and 3 where Author and Open matter
        private void AddTextAnnotationFull(
            PdfDocument doc,
            string contents, string author, PdfTextAnnotationIcon icon,
            int x, int y, int pageNumber, bool open = false)
        {
            PdfTextAnnotation ann = PdfTextAnnotation.Create(contents, pageNumber, x, y);
            ann.Icon = icon;
            ann.Author = author;
            ann.Open = open;
            doc.AddTextAnnotation(ann);
        }

        private int AddSectionLabel(PdfDocument doc, PdfFont sectionFont,
            string label, int x, int y, int separator)
        {
            PdfTextElement section = new PdfTextElement(label, sectionFont)
            { X = x, Y = y };
            section.Accessibility.StructureType = PdfStructureType.Heading2;
            var info = doc.AddText(section);
            return (int)info.LastPageRectangle.Bounds.Bottom + separator;
        }

        private int AddCaption(PdfDocument doc, PdfFont labelFont,
            string caption, int x, int y, int width)
        {
            PdfTextElement t = new PdfTextElement(caption, labelFont)
            { X = x, Y = y, Width = width };
            t.Accessibility.StructureType = PdfStructureType.Artifact;
            var info = doc.AddText(t);
            return (int)info.LastPageRectangle.Bounds.Bottom;
        }

        private void EnsureSpaceOnPage(ref int crtYPos, int requestedHeight, PdfDocument pdfDocument)
        {
            if (crtYPos + requestedHeight > pdfDocument.ContentHeight)
            {
                pdfDocument.AddPage();
                crtYPos = 0;
            }
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```

## Add Text Annotations to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-text-annotations-to-existing-pdf.htm

### Author Identification and Initial Popup State

```csharp
PdfTextAnnotation ann = PdfTextAnnotation.Create(
    contents: "Please confirm the issue date before mailing.",
    pageNumber: 1, x: 30, y: 200);
ann.Icon = PdfTextAnnotationIcon.Comment;
ann.Author = "Jane Reviewer";
ann.Open = true;
pdfEditor.AddTextAnnotation(ann);
```

### Code Sample - Add Sticky Note Text Annotations to Existing PDF

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
    public class Add_Text_Annotations_to_Existing_PDFController : Controller
    {
        private const int leftMargin = 36;
        private const int topMargin = 36;
        private const int contentWidth = 595 - 72;
        private const int contentHeight = 842 - 72;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_Text_Annotations_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_Text_Annotations_to_Existing_PDF_ViewModel model)
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

            // Open the loaded PDF for editing.  PdfEditor inherits the
            // standard (PDF/A, PDF/UA etc.) and Language from the source
            // document, so no PdfDocumentCreateSettings is needed.  Page
            // size and margins are also taken from the existing pages;
            // we apply our own margins manually below via leftMargin /
            // topMargin since PdfEditor draws at absolute page coordinates
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
            pdfEditor.PdfDocumentInfo.Title = "PDF Text Annotations Demo";

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont bodyFont = PdfFontManager.CreateFont(baseFont, 11f,
                PdfFontStyle.Normal, PdfColor.Black);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);
            PdfFont iconCaptionFont = PdfFontManager.CreateFont(baseFont, 10f,
                PdfFontStyle.Bold, PdfColor.DarkSlateGray);

            const int xLeft = leftMargin;
            const int ySeparator = 10;
            const int pageNumber = 1;
            int currentPage = 1;
            int crtYPos = topMargin;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Text Annotations Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = contentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            var titleElementInfo = pdfEditor.AddText(currentPage, titleElement);
            currentPage = titleElementInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)titleElementInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 1: Icon variants =====
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "1. Icon variants (PdfTextAnnotationIcon)",
                xLeft, crtYPos, ySeparator);

            crtYPos = AddCaption(pdfEditor, ref currentPage, bodyFont,
                "PdfTextAnnotation.Icon selects the marker drawn on the page. Click any marker to open its popup",
                xLeft, crtYPos, contentWidth) + ySeparator;

            // Four icons in a row, equally spaced across the content width.
            // Each icon is the default 24x24 sticky-note size; the caption
            // beneath identifies the PdfTextAnnotationIcon enum value.
            const int iconSize = 24;
            int[] iconXs = { leftMargin + 30, leftMargin + 160, leftMargin + 290, leftMargin + 420 };

            AddIconSample(pdfEditor, ref currentPage, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Note — the default sticky-note marker, used for general comments",
                icon: PdfTextAnnotationIcon.Note,
                caption: "Note (default)",
                x: iconXs[0], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            AddIconSample(pdfEditor, ref currentPage, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Comment — used for review feedback in collaborative workflows",
                icon: PdfTextAnnotationIcon.Comment,
                caption: "Comment",
                x: iconXs[1], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            AddIconSample(pdfEditor, ref currentPage, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Help — used to flag content that needs clarification",
                icon: PdfTextAnnotationIcon.Help,
                caption: "Help",
                x: iconXs[2], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            AddIconSample(pdfEditor, ref currentPage, iconCaptionFont,
                contents: "PdfTextAnnotationIcon.Insert — used to suggest text insertions during editing",
                icon: PdfTextAnnotationIcon.Insert,
                caption: "Insert",
                x: iconXs[3], y: crtYPos, pageNumber: pageNumber, iconSize: iconSize);

            crtYPos += iconSize + 25 + ySeparator;

            // ===== Section 2: Custom dimensions =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 110, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "2. Custom marker dimensions",
                xLeft, crtYPos, ySeparator);

            crtYPos = AddCaption(pdfEditor, ref currentPage, bodyFont,
                "Width and Height in points override the default 24x24 marker size",
                xLeft, crtYPos, contentWidth) + ySeparator;

            // Three sizes laid out side by side: small, default, large.
            // The Y of each icon is adjusted so the bottoms align, which
            // makes the size difference easier to compare visually.
            const int baselineY = 0;
            const int sizeSmall = 16;
            const int sizeDefault = 24;
            const int sizeLarge = 48;

            AddSizedSample(pdfEditor, ref currentPage, iconCaptionFont,
                contents: "Small marker — Width = 16, Height = 16",
                size: sizeSmall,
                caption: "16 x 16",
                x: leftMargin + 30, baseY: crtYPos + sizeLarge - sizeSmall + baselineY,
                pageNumber: pageNumber, captionY: crtYPos + sizeLarge + 5);

            AddSizedSample(pdfEditor, ref currentPage, iconCaptionFont,
                contents: "Default marker — Width = 24, Height = 24",
                size: sizeDefault,
                caption: "24 x 24 (default)",
                x: leftMargin + 130, baseY: crtYPos + sizeLarge - sizeDefault + baselineY,
                pageNumber: pageNumber, captionY: crtYPos + sizeLarge + 5);

            AddSizedSample(pdfEditor, ref currentPage, iconCaptionFont,
                contents: "Large marker — Width = 48, Height = 48",
                size: sizeLarge,
                caption: "48 x 48",
                x: leftMargin + 270, baseY: crtYPos + baselineY,
                pageNumber: pageNumber, captionY: crtYPos + sizeLarge + 5);

            crtYPos += sizeLarge + 25 + ySeparator;

            // ===== Section 3: Author identification =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 120, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "3. Author identification",
                xLeft, crtYPos, ySeparator);

            crtYPos = AddCaption(pdfEditor, ref currentPage, bodyFont,
                "Author appears in the popup title and in the viewer's Comments panel, useful for tracking review feedback",
                xLeft, crtYPos, contentWidth) + ySeparator;

            // A body paragraph with a sticky note anchored at the end of a
            // specific phrase that the reviewer is commenting on.  The note
            // popup carries the reviewer's name so multiple reviewers can
            // be told apart in the Comments panel.
            const int paragraphX = leftMargin;
            const int paragraphWidth = 500;
            PdfTextElement paragraph = new PdfTextElement(
                "The PdfTextAnnotation API supports an optional Author property " +
                "that is written to the annotation dictionary as /T. PDF viewers " +
                "show this value as the popup title and group comments by author " +
                "in the Comments panel.",
                bodyFont)
            {
                X = paragraphX, Y = crtYPos, Width = paragraphWidth
            };
            var paraInfo = pdfEditor.AddText(currentPage, paragraph);
            currentPage = paraInfo.LastPageRectangle.PageNumber;
            float paraBottom = paraInfo.LastPageRectangle.Bounds.Bottom;

            // Place the sticky note at the right edge of the paragraph,
            // vertically aligned with its first line.
            AddTextAnnotationFull(pdfEditor, ref currentPage,
                contents: "Consider also setting CreationDate when persisting reviewer history. " +
                          "Some viewers sort the Comments panel chronologically.",
                author: "Jane Reviewer",
                icon: PdfTextAnnotationIcon.Comment,
                x: paragraphX + paragraphWidth + 10, y: crtYPos,
                pageNumber: pageNumber);

            crtYPos = (int)paraBottom + ySeparator * 2;

            // ===== Section 4: Initially expanded popup =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 80, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "4. Initially expanded popup (Open = true)",
                xLeft, crtYPos, ySeparator);

            crtYPos = AddCaption(pdfEditor, ref currentPage, bodyFont,
                "When Open is true the viewer shows the popup expanded as soon as the page is rendered, " +
                "without requiring the reader to click the icon",
                xLeft, crtYPos, contentWidth) + ySeparator;

            // Anchor the open-by-default note so the popup naturally
            // expands into empty space below.
            AddTextAnnotationFull(pdfEditor, ref currentPage,
                contents: "This popup is shown expanded on page load because Open = true was set on the annotation. " +
                          "Click the icon to collapse it",
                author: "EVO PDF Demo",
                icon: PdfTextAnnotationIcon.Note,
                x: leftMargin + 30, y: crtYPos,
                pageNumber: pageNumber, open: true);

            AddCaption(pdfEditor, ref currentPage, labelFont,
                "Marker on the left — its popup is visible immediately when the PDF is opened",
                leftMargin + 60, crtYPos + 6, 480);

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfTextAnnotationsEditDemo.pdf";
            return fileResult;
        }

        // === Helpers ===

        // Adds a sticky-note annotation with the supplied icon at (x, y)
        // plus a bold caption beneath it identifying the icon name.
        // Used by Section 1 to show all four PdfTextAnnotationIcon values
        // in a horizontal row
        private void AddIconSample(PdfEditor editor, ref int currentPage, PdfFont captionFont,
            string contents, PdfTextAnnotationIcon icon, string caption,
            int x, int y, int pageNumber, int iconSize) {
            PdfTextAnnotation ann = PdfTextAnnotation.Create(contents, pageNumber, x, y);
            ann.Icon = icon;
            ann.Width = iconSize;
            ann.Height = iconSize;
            editor.AddTextAnnotation(ann);

            PdfTextElement label = new PdfTextElement(caption, captionFont)
            {
                X = x - 20, Y = y + iconSize + 4, Width = 100,
                Alignment = PdfTextAlignment.Left
            };
            editor.AddText(currentPage, label);
        }

        // Adds a sticky-note annotation sized via Width/Height plus a caption
        // beneath it identifying the dimensions.  baseY positions the icon's
        // top-left so the bottoms of differently sized icons can be aligned
        private void AddSizedSample(PdfEditor editor, ref int currentPage, PdfFont captionFont,
            string contents, int size, string caption,
            int x, int baseY, int pageNumber, int captionY) {
            PdfTextAnnotation ann = PdfTextAnnotation.Create(contents, pageNumber, x, baseY);
            ann.Icon = PdfTextAnnotationIcon.Note;
            ann.Width = size;
            ann.Height = size;
            editor.AddTextAnnotation(ann);

            PdfTextElement label = new PdfTextElement(caption, captionFont)
            {
                X = x - 10, Y = captionY, Width = 120
            };
            editor.AddText(currentPage, label);
        }

        // Adds a sticky-note annotation with the full set of properties.
        // Used by sections 3 and 4 where Author and Open matter
        private void AddTextAnnotationFull(PdfEditor editor, ref int currentPage,
            string contents, string author, PdfTextAnnotationIcon icon,
            int x, int y, int pageNumber, bool open = false) {
            PdfTextAnnotation ann = PdfTextAnnotation.Create(contents, pageNumber, x, y);
            ann.Icon = icon;
            ann.Author = author;
            ann.Open = open;
            editor.AddTextAnnotation(ann);
        }

        private int AddSectionLabel(PdfEditor editor, ref int currentPage, PdfFont sectionFont,
            string label, int x, int y, int separator) {
            PdfTextElement section = new PdfTextElement(label, sectionFont)
            { X = x, Y = y };
            section.Accessibility.StructureType = PdfStructureType.Heading2;
            var info = editor.AddText(currentPage, section);
            currentPage = info.LastPageRectangle.PageNumber;
            return (int)info.LastPageRectangle.Bounds.Bottom + separator;
        }

        private int AddCaption(PdfEditor editor, ref int currentPage, PdfFont labelFont,
            string caption, int x, int y, int width) {
            PdfTextElement t = new PdfTextElement(caption, labelFont)
            { X = x, Y = y, Width = width };
            t.Accessibility.StructureType = PdfStructureType.Artifact;
            var info = editor.AddText(currentPage, t);
            currentPage = info.LastPageRectangle.PageNumber;
            return (int)info.LastPageRectangle.Bounds.Bottom;
        }

        private void EnsureSpaceOnPage(ref int crtYPos, ref int currentPage, int requestedHeight, PdfEditor pdfEditor, int contentHeight, int topMargin)
        {
            if (crtYPos + requestedHeight > contentHeight + topMargin)
            {
                currentPage = pdfEditor.AddPage();
                crtYPos = topMargin;
            }
        }

        private Add_Text_Annotations_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_Text_Annotations_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_Text_Annotations_to_Existing_PDF".Length);

            // Default input is empty.pdf so this demo edits a fresh
            // blank A4 page.  The user can upload another PDF or paste
            // a different URL
            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/empty.pdf";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```
