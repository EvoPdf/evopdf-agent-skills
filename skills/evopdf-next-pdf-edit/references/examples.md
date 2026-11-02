# Editing an existing PDF: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Add Text to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-text-to-existing-pdf.htm

### Open an existing PDF for editing

```csharp
string password = string.IsNullOrEmpty(ownerPassword) ? userPassword : ownerPassword;
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
```

### Page cursor discipline

```csharp
PdfTextElement element = new PdfTextElement(text, font) { X = 0, Y = crtYPos, Width = contentWidth };
var info = pdfEditor.AddText(currentPage, element);
currentPage = info.LastPageRectangle.PageNumber;        // follow the engine if it overflowed
crtYPos = (int)info.LastPageRectangle.Bounds.Bottom + ySeparator;
```

### Continue the Text on Inserted Pages

```csharp
PdfTextElement longText = new PdfTextElement(text, font)
{
    X = 36,
    Y = crtYPos,
    Width = contentWidth,
    ContinueOnNextPage = true,
    // The pages where the text continues are inserted after the current page
    InsertContinuationPages = true,
    ContinuationTopMargin = 36,
    ContinuationBottomMargin = 36
};
PdfTextRenderInfo info = pdfEditor.AddText(currentPage, longText);
currentPage = info.LastPageRectangle.PageNumber;
```

### Code Sample - Add Text to Existing PDF

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.Text;
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
    public class Add_Text_to_Existing_PDFController : Controller
    {
        private const int leftMargin = 36;
        private const int topMargin = 36;
        // The content area of the pages inside the demo margins, from the size of the first page of the loaded document
        private int contentWidth;
        private int contentHeight;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_Text_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_Text_to_Existing_PDF_ViewModel model)
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

            // The pages of an edited document have no margins: the demo keeps its content inside its own margins
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            PdfPageInfo firstPageInfo = pdfEditor.GetPdfPageInfo(1);
            // A rotation of 90 or 270 degrees swaps the width and the height of the page as it is displayed
            bool firstPageTurned = firstPageInfo.Rotation % 180 != 0;
            contentWidth = (int)(firstPageTurned ? firstPageInfo.PageSize.Height : firstPageInfo.PageSize.Width) - 2 * leftMargin;
            contentHeight = (int)(firstPageTurned ? firstPageInfo.PageSize.Width : firstPageInfo.PageSize.Height) - 2 * topMargin;

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont bodyFont = PdfFontManager.CreateFont(baseFont, 11f,
                PdfFontStyle.Normal, PdfColor.Black);
            PdfFont smallFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);

            const int xLeft = leftMargin;
            const int ySeparator = 10;
            int currentPage = 1;
            int crtYPos = topMargin;

            // ===== Section 1: Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Text Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = contentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            PdfTextRenderInfo titleInfo = pdfEditor.AddText(currentPage, titleElement);
            currentPage = titleInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)titleInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 2: BackgroundColor + BackgroundOpacity =====
            PdfTextElement sectionLabel1 = new PdfTextElement(
                "1. BackgroundColor + BackgroundOpacity", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel1.Accessibility.StructureType = PdfStructureType.Heading2;
            var sectionLabel1Info = pdfEditor.AddText(currentPage, sectionLabel1);
            currentPage = sectionLabel1Info.LastPageRectangle.PageNumber;
            crtYPos = (int)sectionLabel1Info.LastPageRectangle.Bounds.Bottom + ySeparator;

            PdfTextElement highlighted = new PdfTextElement(
                "This paragraph has a yellow highlight background drawn behind the text. The background " +
                "covers the full column width and the height used by the text.",
                bodyFont)
            {
                X = xLeft,
                Y = crtYPos,
                Width = contentWidth,
                BackgroundColor = PdfColor.Yellow,
                BackgroundOpacity = 0.4f
            };
            var highlightedInfo = pdfEditor.AddText(currentPage, highlighted);
            currentPage = highlightedInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)highlightedInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 3: OnBeforePageRender + OnAfterPageRender =====
            PdfTextElement sectionLabel2 = new PdfTextElement(
                "2. OnBeforePageRender (under) + OnAfterPageRender (over)", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel2.Accessibility.StructureType = PdfStructureType.Heading2;
            var sectionLabel2Info = pdfEditor.AddText(currentPage, sectionLabel2);
            currentPage = sectionLabel2Info.LastPageRectangle.PageNumber;
            crtYPos = (int)sectionLabel2Info.LastPageRectangle.Bounds.Bottom + ySeparator;

            PdfTextElement decoratedText = new PdfTextElement(
                "Below text is drawn an under-layer rectangle in the OnBeforePageRender callback, " +
                "while the OnAfterPageRender callback adds a thin border around the rendered area. " +
                "Both callbacks receive the same PdfTextPageRenderInfo type: the rectangle in " +
                "OnBeforePageRender is the area the text is about to use and in OnAfterPageRender " +
                "it is the area the text used.",
                bodyFont)
            {
                X = xLeft,
                Y = crtYPos,
                Width = contentWidth
            };

            // Under-layer painted before the text is drawn
            decoratedText.OnBeforePageRender = preInfo =>
            {
                var col = preInfo.RenderedRectangle.Bounds;
                pdfEditor.AddRectangle(preInfo.RenderedRectangle.PageNumber, new PdfRectangleElement(
                    col.X - 2, col.Y - 2, col.Width + 4, col.Height + 4)
                {
                    FillColor = PdfColor.LightBlue,
                    FillOpacity = 0.35f,
                    BorderColor = null
                });
            };

            // Post-render hook for the border that follows the actually used area
            decoratedText.OnAfterPageRender = postInfo =>
            {
                var rect = postInfo.RenderedRectangle.Bounds;
                pdfEditor.AddRectangle(postInfo.RenderedRectangle.PageNumber, new PdfRectangleElement(
                    rect.X - 2, rect.Y - 2, rect.Width + 4, rect.Height + 4)
                {
                    FillColor = null,
                    BorderColor = PdfColor.Blue,
                    Border = new PdfLineStyle { LineWidth = 1f }
                });
            };

            var decoratedTextInfo = pdfEditor.AddText(currentPage, decoratedText);
            currentPage = decoratedTextInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)decoratedTextInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 4: Rotated text + QuadPoints outline =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 180, pdfEditor, contentHeight, topMargin);

            PdfTextElement sectionLabel3 = new PdfTextElement(
                "3. Rotated text with QuadPoints-based outline", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel3.Accessibility.StructureType = PdfStructureType.Heading2;
            var sectionLabel3Info = pdfEditor.AddText(currentPage, sectionLabel3);
            currentPage = sectionLabel3Info.LastPageRectangle.PageNumber;
            crtYPos = (int)sectionLabel3Info.LastPageRectangle.Bounds.Bottom + ySeparator;

            PdfTextElement rotated = new PdfTextElement(
                "Rotated 25 degrees around the top-left corner. The polygon below follows the " +
                "rotation using the four-corner QuadPoints returned by PdfTextPageRenderInfo.",
                bodyFont)
            {
                X = xLeft + 60,
                Y = crtYPos + 10,
                Width = 350,
                RotationDegrees = -25,
                RotationPivot = PdfRotationPivot.TopLeft,
                BackgroundColor = PdfColor.LightYellow,
                BackgroundOpacity = 0.5f
            };

            // Trace a polygon along the rotated quad after rendering.
            rotated.OnAfterPageRender = postInfo =>
            {
                var quad = postInfo.RenderedRectangle.QuadPoints;
                pdfEditor.AddPolygon(postInfo.RenderedRectangle.PageNumber, new PdfPolygonElement(
                    quad.TopLeft, quad.TopRight, quad.BottomRight, quad.BottomLeft)
                {
                    FillColor = null,
                    BorderColor = PdfColor.Red,
                    Border = new PdfLineStyle { LineWidth = 1.2f, DashStyle = PdfLineDashStyle.Dashed }
                });
            };

            PdfTextRenderInfo rotatedInfo = pdfEditor.AddText(currentPage, rotated);

            currentPage = rotatedInfo.LastPageRectangle.PageNumber;
            // Advance Y past the rotated area's axis-aligned bounding box.
            crtYPos = (int)rotatedInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 4: Multi-page continuation + per-page RenderedText =====
            currentPage = pdfEditor.AddPage();
            crtYPos = topMargin;

            PdfTextElement sectionLabel4 = new PdfTextElement(
                "4. Multi-page continuation + per-page RenderedText", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel4.Accessibility.StructureType = PdfStructureType.Heading2;
            var sectionLabel4Info = pdfEditor.AddText(currentPage, sectionLabel4);
            currentPage = sectionLabel4Info.LastPageRectangle.PageNumber;
            crtYPos = (int)sectionLabel4Info.LastPageRectangle.Bounds.Bottom + ySeparator;

            // A page added after the pages of the document: the text continues over it, or the pages where the
            // text continues are inserted before it and it keeps its content, as the option asks
            int pageAfterText = pdfEditor.AddPage();
            pdfEditor.AddText(pageAfterText, new PdfTextElement("The page that followed the text", smallFont)
            {
                X = xLeft,
                Y = topMargin
            });

            // A long text that takes several pages
            string longTextSource = LoadAlphabetText();
            StringBuilder longBuilder = new StringBuilder();
            for (int i = 0; i < 8; i++) longBuilder.AppendLine(longTextSource);
            string longText = longBuilder.ToString();

            // The pages of an edited document have no margins, so the continuation margins give the area of the
            // pages where the text continues
            PdfTextElement multipage = new PdfTextElement(longText, bodyFont)
            {
                X = xLeft,
                Y = crtYPos,
                Width = contentWidth,
                Alignment = PdfTextAlignment.Left,
                ContinueOnNextPage = true,
                InsertContinuationPages = model.InsertContinuationPages,
                ContinuationTopMargin = topMargin,
                ContinuationBottomMargin = topMargin,
                BackgroundColor = PdfColor.WhiteSmoke,
                BackgroundOpacity = 1f
            };

            // Per-page hook: draw a small page-number badge using OnAfterPageRender
            multipage.OnAfterPageRender = info =>
            {
                var r = info.RenderedRectangle.Bounds;
                string badge = $"page {info.RenderedRectangle.PageNumber} - {info.RenderedText.Length} chars";

                // Place the badge to the right of the rendered text top edge
                var badgeText = new PdfTextElement(badge, smallFont)
                {
                    X = (float)r.Right - 110,
                    Y = (float)r.Y - 12,
                    Width = 110,
                    Alignment = PdfTextAlignment.Right
                };
                badgeText.Accessibility.StructureType = PdfStructureType.Artifact;
                pdfEditor.AddText(info.RenderedRectangle.PageNumber, badgeText);
            };

            PdfTextRenderInfo multipageInfo = pdfEditor.AddText(currentPage, multipage);

            currentPage = multipageInfo.LastPageRectangle.PageNumber;

            // ===== Section 5: Summary of pages rendered =====
            currentPage = pdfEditor.AddPage();
            crtYPos = topMargin;

            PdfTextElement summaryLabel = new PdfTextElement(
                "5. Summary: text rendered per page (from Pages list)", sectionFont)
            { X = xLeft, Y = crtYPos };
            summaryLabel.Accessibility.StructureType = PdfStructureType.Heading2;
            var summaryLabelInfo = pdfEditor.AddText(currentPage, summaryLabel);
            currentPage = summaryLabelInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)summaryLabelInfo.LastPageRectangle.Bounds.Bottom + ySeparator;

            // Walk the Pages list returned by the multipage render and report
            // the first 80 characters of each page
            for (int i = 0; i < multipageInfo.Pages.Count; i++)
            {
                var page = multipageInfo.Pages[i];
                string preview = page.RenderedText.Length > 80
                    ? page.RenderedText.Substring(0, 80).Replace("\n", " ").Replace("\r", "") + "..."
                    : page.RenderedText.Replace("\n", " ").Replace("\r", "");

                string entry = $"Page {page.RenderedRectangle.PageNumber}: " +
                               $"{page.RenderedText.Length} chars, " +
                               $"bounds=({page.RenderedRectangle.Bounds.X:F0}, " +
                               $"{page.RenderedRectangle.Bounds.Y:F0}, " +
                               $"{page.RenderedRectangle.Bounds.Width:F0}x" +
                               $"{page.RenderedRectangle.Bounds.Height:F0})  -  " +
                               $"\"{preview}\"";

                PdfTextElement entryElement = new PdfTextElement(entry, smallFont)
                {
                    X = xLeft,
                    Y = crtYPos,
                    Width = contentWidth
                };
                var entryElementInfo = pdfEditor.AddText(currentPage, entryElement);
                currentPage = entryElementInfo.LastPageRectangle.PageNumber;
                crtYPos = (int)entryElementInfo.LastPageRectangle.Bounds.Bottom + 4;

                EnsureSpaceOnPage(ref crtYPos, ref currentPage, 30, pdfEditor, contentHeight, topMargin);
            }

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfTextEditDemo.pdf";
            return fileResult;
        }

        private string LoadAlphabetText()
        {
            string textsPath = GetDemoTextsPath();
            string alphabetFilePath = Path.Combine(textsPath, "Alphabet.txt");
            return System.IO.File.ReadAllText(alphabetFilePath);
        }

        private void EnsureSpaceOnPage(ref int crtYPos, ref int currentPage, int requestedHeight, PdfEditor pdfEditor, int contentHeight, int topMargin)
        {
            if (crtYPos + requestedHeight > contentHeight + topMargin)
            {
                // The content continues on the following page of the document, or on a new page after the last one
                currentPage = currentPage < pdfEditor.PageCount ? currentPage + 1 : pdfEditor.AddPage();
                crtYPos = topMargin;
            }
        }

        private Add_Text_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_Text_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_Text_to_Existing_PDF".Length);

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

## Add Images to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-images-to-existing-pdf.htm

### Code Sample - Add Images to Existing PDF

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
    public class Add_Images_to_Existing_PDFController : Controller
    {
        private const int leftMargin = 36;
        private const int topMargin = 36;
        // The content area of the pages inside the demo margins, from the size of the first page of the loaded document
        private int contentWidth;
        private int contentHeight;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_Images_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_Images_to_Existing_PDF_ViewModel model)
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

            // The pages of an edited document have no margins: the demo keeps its content inside its own margins
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            PdfPageInfo firstPageInfo = pdfEditor.GetPdfPageInfo(1);
            // A rotation of 90 or 270 degrees swaps the width and the height of the page as it is displayed
            bool firstPageTurned = firstPageInfo.Rotation % 180 != 0;
            contentWidth = (int)(firstPageTurned ? firstPageInfo.PageSize.Height : firstPageInfo.PageSize.Width) - 2 * leftMargin;
            contentHeight = (int)(firstPageTurned ? firstPageInfo.PageSize.Width : firstPageInfo.PageSize.Height) - 2 * topMargin;

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont smallFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);

            const int xLeft = leftMargin;
            const int ySeparator = 10;
            int currentPage = 1;
            int crtYPos = topMargin;

            string imagesPath = GetDemoImagesPath();
            string transparentPngPath = Path.Combine(imagesPath, "transparent.png");
            string jpegPath = Path.Combine(imagesPath, "image.jpg");
            string tallImagePath = Path.Combine(imagesPath, "Tall_Report.png");

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Image Demo", titleFont)
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

            // ===== Section 1: PNG with custom width, ScaleDownToFit =====
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "1. PNG with custom Width (ScaleDownToFit, EnlargeToFit)", xLeft, crtYPos, ySeparator);

            PdfImageElement pngImage = new PdfImageElement(transparentPngPath)
            {
                X = xLeft,
                Y = crtYPos,
                Width = 150,
                ScaleDownToFit = true,
                EnlargeToFit = false,
                Opacity = 0.85f
            };
            pngImage.Accessibility.AlternateText = "Transparent glass globe";

            PdfImageRenderInfo pngInfo = pdfEditor.AddImage(currentPage, pngImage);
            crtYPos = (int)pngInfo.BoundingBox.Bottom + ySeparator * 2;

            // ===== Section 2: JPEG with alignment =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 220, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "2. JPEG aligned Center horizontally", xLeft, crtYPos, ySeparator);

            PdfImageElement jpgCentered = new PdfImageElement(jpegPath)
            {
                Y = crtYPos,
                Height = 120,
                HorizontalAlign = PdfElementHorizontalAlign.Center
            };
            jpgCentered.Accessibility.AlternateText = "Wooden house on a river";

            PdfImageRenderInfo jpgInfo = pdfEditor.AddImage(currentPage, jpgCentered);
            crtYPos = (int)jpgInfo.BoundingBox.Bottom + ySeparator * 2;

            // ===== Section 3: Rotated image with QuadPoints-based outline =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 280, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "3. Rotated 30 degrees clockwise - Bounding Box (red) vs Rotated Rectangle (blue)", xLeft, crtYPos, ySeparator);

            PdfImageElement rotatedJpg = new PdfImageElement(jpegPath)
            {
                X = xLeft + 80,
                Y = crtYPos + 10,
                Width = 200,
                Height = 150,
                RotationDegrees = -30,
                RotationPivot = PdfRotationPivot.TopLeft,
                Opacity = 0.95f
            };
            rotatedJpg.Accessibility.AlternateText = "Wooden house rotated 30 degrees clockwise";

            PdfImageRenderInfo rotatedInfo = pdfEditor.AddImage(currentPage, rotatedJpg);

            // Outline the axis-aligned Bounds (red).
            var aabb = rotatedInfo.BoundingBox;
            pdfEditor.AddRectangle(currentPage, new PdfRectangleElement(aabb.X, aabb.Y, aabb.Width, aabb.Height)
            {
                FillColor = null,
                BorderColor = PdfColor.Red,
                Border = new PdfLineStyle { LineWidth = 1f, DashStyle = PdfLineDashStyle.Dotted }
            });

            // Outline the rotated QuadPoints (blue) using a polygon through the four corners.
            var q = rotatedInfo.QuadPoints;
            pdfEditor.AddPolygon(currentPage, new PdfPolygonElement(q.TopLeft, q.TopRight, q.BottomRight, q.BottomLeft)
            {
                FillColor = null,
                BorderColor = PdfColor.Blue,
                Border = new PdfLineStyle { LineWidth = 1.5f }
            });

            // Annotate with VisibleBounds info using the actual AABB bottom.
            string visibleSummary = rotatedInfo.VisibleBounds == null
                ? "VisibleBounds: image fully outside the page"
                : $"VisibleBounds=({rotatedInfo.VisibleBounds.X:F0}, {rotatedInfo.VisibleBounds.Y:F0}, " +
                  $"{rotatedInfo.VisibleBounds.Width:F0}x{rotatedInfo.VisibleBounds.Height:F0})  on page " +
                  $"{rotatedInfo.Page} ({rotatedInfo.PageWidth:F0}x{rotatedInfo.PageHeight:F0})";

            PdfTextElement visibleLabel = new PdfTextElement(visibleSummary, smallFont)
            {
                X = xLeft,
                Y = (int)aabb.Bottom + ySeparator,
                Width = contentWidth
            };
            visibleLabel.Accessibility.StructureType = PdfStructureType.Artifact;
            var visibleLabelInfo = pdfEditor.AddText(currentPage, visibleLabel);
            currentPage = visibleLabelInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)visibleLabelInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 4: Different rotation pivots =====
            currentPage = pdfEditor.AddPage();
            crtYPos = topMargin;
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "4. Same image rotated 45 degrees around four different pivots", xLeft, crtYPos, ySeparator);

            PdfRotationPivot[] pivots = new[]
            {
                PdfRotationPivot.TopLeft, PdfRotationPivot.TopRight,
                PdfRotationPivot.BottomLeft, PdfRotationPivot.Center
            };

            const int cellW = 220;
            const int cellH = 220;
            int imgW = 100, imgH = 75;

            for (int i = 0; i < pivots.Length; i++)
            {
                int col = i % 2;
                int row = i / 2;
                int cellX = xLeft + col * cellW;
                int cellY = crtYPos + row * cellH;

                // Draw the cell as a dashed reference frame.
                pdfEditor.AddRectangle(currentPage, new PdfRectangleElement(cellX, cellY, cellW - 10, cellH - 10)
                {
                    FillColor = null,
                    BorderColor = PdfColor.LightGray,
                    Border = new PdfLineStyle { LineWidth = 0.5f, DashStyle = PdfLineDashStyle.Dashed }
                });

                // Center the image inside the cell, then rotate by 45 around the chosen pivot.
                int imgX = cellX + (cellW - 10 - imgW) / 2;
                int imgY = cellY + (cellH - 10 - imgH) / 2 + 10;

                PdfImageElement img = new PdfImageElement(jpegPath)
                {
                    X = imgX,
                    Y = imgY,
                    Width = imgW,
                    Height = imgH,
                    RotationDegrees = 45,
                    RotationPivot = pivots[i]
                };
                img.Accessibility.AlternateText = $"Sample image rotated 45 degrees around {pivots[i]}";
                PdfImageRenderInfo info = pdfEditor.AddImage(currentPage, img);

                // Trace the rotated quad.
                var rq = info.QuadPoints;
                pdfEditor.AddPolygon(currentPage, new PdfPolygonElement(rq.TopLeft, rq.TopRight, rq.BottomRight, rq.BottomLeft)
                {
                    FillColor = null,
                    BorderColor = PdfColor.Blue,
                    Border = new PdfLineStyle { LineWidth = 1f }
                });

                // Label.
                PdfTextElement pivotLabel = new PdfTextElement(pivots[i].ToString(), smallFont)
                { X = cellX + 4, Y = cellY + 2, Width = cellW - 14 };
                pivotLabel.Accessibility.StructureType = PdfStructureType.Artifact;
                pdfEditor.AddText(currentPage, pivotLabel);
            }

            if (model.AddTallImage)
            {
                // ===== Section 5: Image taller than a page =====
                currentPage = pdfEditor.AddPage();
                crtYPos = topMargin;
                crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                    "5. Image taller than a page" + (model.ContinueOnNextPage ? ", continued on the next pages" : ", cut at the bottom of the page"),
                    xLeft, crtYPos, ySeparator);

                // A page added after the pages of the document: the image continues over it, or the pages where the
                // image continues are inserted before it and it keeps its content, as the option asks
                int pageAfterImage = pdfEditor.AddPage();
                pdfEditor.AddText(pageAfterImage, new PdfTextElement("The page that followed the image", smallFont)
                {
                    X = xLeft,
                    Y = topMargin
                });

                // The image is scaled to the content width. The pages of an edited document have no margins, so the
                // continuation margins give the area of the pages where the image continues
                PdfImageElement tallImage = new PdfImageElement(tallImagePath)
                {
                    X = xLeft,
                    Y = crtYPos,
                    Width = contentWidth,
                    ContinueOnNextPage = model.ContinueOnNextPage,
                    InsertContinuationPages = model.InsertContinuationPages,
                    ContinuationTopMargin = topMargin,
                    ContinuationBottomMargin = topMargin
                };
                tallImage.Accessibility.AlternateText = "Annual activity report table";

                PdfImageRenderInfo tallInfo = pdfEditor.AddImage(currentPage, tallImage);

                // The rectangle of each part of the image, one per page; the content that follows the image is
                // placed below the part drawn on its last page, or below the visible part when the image is cut
                var partsText = new System.Text.StringBuilder(
                    "The image was drawn on " + tallInfo.RenderedRectangles.Count + " page(s):");
                foreach (PdfRenderedRectangle part in tallInfo.RenderedRectangles)
                {
                    partsText.AppendFormat(" page {0} ({1:F0}, {2:F0}, {3:F0}x{4:F0});", part.PageNumber,
                        part.Bounds.X, part.Bounds.Y, part.Bounds.Width, part.Bounds.Height);
                }

                currentPage = tallInfo.LastPage;
                PdfRectangleF lastPart = tallInfo.LastPageRectangle.VisibleBounds ?? tallInfo.LastPageRectangle.Bounds;
                PdfTextElement partsLabel = new PdfTextElement(partsText.ToString(), smallFont)
                {
                    X = xLeft,
                    Y = (int)lastPart.Bottom + ySeparator,
                    Width = contentWidth
                };
                partsLabel.Accessibility.StructureType = PdfStructureType.Artifact;
                pdfEditor.AddText(currentPage, partsLabel);
            }

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfImageEditDemo.pdf";
            return fileResult;
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

        private void EnsureSpaceOnPage(ref int crtYPos, ref int currentPage, int requestedHeight, PdfEditor pdfEditor, int contentHeight, int topMargin)
        {
            if (crtYPos + requestedHeight > contentHeight + topMargin)
            {
                // The content continues on the following page of the document, or on a new page after the last one
                currentPage = currentPage < pdfEditor.PageCount ? currentPage + 1 : pdfEditor.AddPage();
                crtYPos = topMargin;
            }
        }

        private Add_Images_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_Images_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_Images_to_Existing_PDF".Length);

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

## Add Shapes to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-shapes-to-existing-pdf.htm

### Code Sample - Add Geometric Shapes to Existing PDF

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
    public class Add_Shapes_to_Existing_PDFController : Controller
    {
        private const int leftMargin = 36;
        private const int topMargin = 36;
        // The content area of the pages inside the demo margins, from the size of the first page of the loaded document
        private int contentWidth;
        private int contentHeight;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_Shapes_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_Shapes_to_Existing_PDF_ViewModel model)
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

            // The pages of an edited document have no margins: the demo keeps its content inside its own margins
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            PdfPageInfo firstPageInfo = pdfEditor.GetPdfPageInfo(1);
            // A rotation of 90 or 270 degrees swaps the width and the height of the page as it is displayed
            bool firstPageTurned = firstPageInfo.Rotation % 180 != 0;
            contentWidth = (int)(firstPageTurned ? firstPageInfo.PageSize.Height : firstPageInfo.PageSize.Width) - 2 * leftMargin;
            contentHeight = (int)(firstPageTurned ? firstPageInfo.PageSize.Width : firstPageInfo.PageSize.Height) - 2 * topMargin;

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);

            const int xLeft = leftMargin;
            const int ySeparator = 10;
            int currentPage = 1;
            int crtYPos = topMargin;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "Geometric Shape Elements Demo", titleFont)
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

            // ===== Section 1: PdfRectangleElement =====
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "1. PdfRectangleElement (basic, dashed, rotated)", xLeft, crtYPos, ySeparator);

            // Filled basic
            var basicInfo = pdfEditor.AddRectangle(currentPage, new PdfRectangleElement(xLeft, crtYPos, 100, 60)
            {
                FillColor = PdfColor.LightSkyBlue,
                BorderColor = PdfColor.SteelBlue,
                Border = new PdfLineStyle { LineWidth = 1.5f }
            });
            int basicCaptionBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "filled + border", xLeft,
                (int)basicInfo.LastPageRectangle.Bounds.Bottom + 5, 100);

            // Dashed
            var dashedInfo = pdfEditor.AddRectangle(currentPage, new PdfRectangleElement(xLeft + 120, crtYPos, 100, 60)
            {
                FillColor = null,
                BorderColor = PdfColor.DarkOrange,
                Border = new PdfLineStyle
                {
                    LineWidth = 1.5f,
                    DashStyle = PdfLineDashStyle.Dashed
                }
            });
            int dashedCaptionBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "dashed outline", xLeft + 120,
                (int)dashedInfo.LastPageRectangle.Bounds.Bottom + 5, 100);

            // Rotated 30 degrees
            var rotatedInfo = pdfEditor.AddRectangle(currentPage, new PdfRectangleElement(xLeft + 320, crtYPos + 5, 100, 60)
            {
                FillColor = PdfColor.LightPink,
                FillOpacity = 0.6f,
                BorderColor = PdfColor.Crimson,
                Border = new PdfLineStyle { LineWidth = 1.2f },
                RotationDegrees = 30,
                RotationPivot = PdfRotationPivot.TopLeft
            });
            int rotatedCaptionBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "rotated 30 deg (TopLeft pivot)", xLeft + 320,
                (int)rotatedInfo.LastPageRectangle.Bounds.Bottom + 5, 150);

            // Advance past the deepest caption in this row
            crtYPos = Math.Max(
                Math.Max(basicCaptionBottom, dashedCaptionBottom),
                rotatedCaptionBottom) + ySeparator;

            // ===== Section 2: PdfRoundedRectangleElement =====
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "2. PdfRoundedRectangleElement (varying corner radius)", xLeft, crtYPos, ySeparator);

            float[] radii = { 4f, 12f, 24f, 30f };
            int section2MaxBottom = crtYPos;
            for (int i = 0; i < radii.Length; i++)
            {
                int boxX = xLeft + i * 130;
                var rrInfo = pdfEditor.AddRoundedRectangle(currentPage, new PdfRoundedRectangleElement(boxX, crtYPos, 110, 60, radii[i])
                {
                    FillColor = PdfColor.LightGreen,
                    FillOpacity = 0.5f,
                    BorderColor = PdfColor.DarkGreen,
                    Border = new PdfLineStyle { LineWidth = 1f }
                });
                int capBottom = AddCaption(pdfEditor, ref currentPage, labelFont, $"radius={radii[i]}", boxX,
                    (int)rrInfo.LastPageRectangle.Bounds.Bottom + 5, 110);
                section2MaxBottom = Math.Max(section2MaxBottom, capBottom);
            }
            crtYPos = section2MaxBottom + ySeparator;

            // ===== Section 3: PdfLineElement (LineCap variations) =====
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "3. PdfLineElement (LineCap: Butt, Round, ProjectingSquare)", xLeft, crtYPos, ySeparator);

            PdfLineCapStyle[] caps = { PdfLineCapStyle.Butt, PdfLineCapStyle.Round, PdfLineCapStyle.ProjectingSquare };
            string[] capLabels = { "Butt", "Round", "ProjectingSquare" };
            int section3RowTop = crtYPos;
            for (int i = 0; i < caps.Length; i++)
            {
                int lineY = section3RowTop + 8;
                var lineInfo = pdfEditor.AddLine(currentPage, new PdfLineElement(xLeft + 60, lineY, xLeft + 260, lineY)
                {
                    LineColor = PdfColor.DarkBlue,
                    LineStyle = new PdfLineStyle
                    {
                        LineWidth = 8f,
                        LineCap = caps[i]
                    }
                });
                // Thin reference line to highlight the cap extension past the endpoints
                pdfEditor.AddLine(currentPage, new PdfLineElement(xLeft + 60, lineY, xLeft + 260, lineY)
                {
                    LineColor = PdfColor.White,
                    LineStyle = new PdfLineStyle { LineWidth = 0.5f }
                });
                int capBottom = AddCaption(pdfEditor, ref currentPage, labelFont, capLabels[i], xLeft + 270, lineY - 5, 150);
                int lineBottom = (int)lineInfo.LastPageRectangle.Bounds.Bottom;
                section3RowTop = Math.Max(lineBottom, capBottom) + 5;
            }
            crtYPos = section3RowTop + ySeparator;

            // Dashed line using CustomDashPattern
            var dashLineInfo = pdfEditor.AddLine(currentPage, new PdfLineElement(xLeft, crtYPos, xLeft + 380, crtYPos)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 2f,
                    CustomDashPattern = new float[] { 10f, 4f, 2f, 4f },
                    DashPhase = 0f
                }
            });
            int dashCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "CustomDashPattern [10, 4, 2, 4]",
                xLeft, (int)dashLineInfo.LastPageRectangle.Bounds.Bottom + 5, 250);
            crtYPos = dashCapBottom + ySeparator;

            // ===== Section 4: PdfCircleElement =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 160, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "4. PdfCircleElement (filled, stroked, opacity)", xLeft, crtYPos, ySeparator);

            var circle1Info = pdfEditor.AddCircle(currentPage, new PdfCircleElement(xLeft + 40, crtYPos + 40, 30)
            {
                FillColor = PdfColor.Tomato,
                BorderColor = PdfColor.DarkRed,
                Border = new PdfLineStyle { LineWidth = 1.5f }
            });
            int circle1CapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "filled + border", xLeft + 10,
                (int)circle1Info.LastPageRectangle.Bounds.Bottom + 5, 90);

            var circle2Info = pdfEditor.AddCircle(currentPage, new PdfCircleElement(xLeft + 140, crtYPos + 40, 30)
            {
                FillColor = null,
                BorderColor = PdfColor.DarkBlue,
                Border = new PdfLineStyle { LineWidth = 2f }
            });
            int circle2CapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "stroke only", xLeft + 110,
                (int)circle2Info.LastPageRectangle.Bounds.Bottom + 5, 90);

            var circle3Info = pdfEditor.AddCircle(currentPage, new PdfCircleElement(xLeft + 240, crtYPos + 40, 30)
            {
                FillColor = PdfColor.Purple,
                FillOpacity = 0.4f,
                BorderColor = PdfColor.Purple,
                BorderOpacity = 0.7f,
                Border = new PdfLineStyle { LineWidth = 1f }
            });
            int circle3CapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "translucent", xLeft + 210,
                (int)circle3Info.LastPageRectangle.Bounds.Bottom + 5, 90);

            crtYPos = Math.Max(Math.Max(circle1CapBottom, circle2CapBottom), circle3CapBottom) + ySeparator;

            // ===== Section 5: PdfEllipseElement (rotation, axis-aligned vs QuadPoints) =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 180, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "5. PdfEllipseElement rotation - Bounds (axis-aligned) vs QuadPoints (rotated)", xLeft, crtYPos, ySeparator);

            // Axis-aligned ellipse
            var axisEllipseInfo = pdfEditor.AddEllipse(currentPage, new PdfEllipseElement(xLeft, crtYPos + 10, 120, 70)
            {
                FillColor = PdfColor.LightYellow,
                BorderColor = PdfColor.GoldenRod,
                Border = new PdfLineStyle { LineWidth = 1f }
            });
            int axisCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "axis-aligned", xLeft,
                (int)axisEllipseInfo.LastPageRectangle.Bounds.Bottom + 5, 120);

            // Rotated ellipse + outline its tight Bounds (red) and its QuadPoints (blue).
            var rotEllipse = new PdfEllipseElement(xLeft + 180, crtYPos + 25, 120, 70)
            {
                FillColor = PdfColor.LightCyan,
                BorderColor = PdfColor.Teal,
                Border = new PdfLineStyle { LineWidth = 1f },
                RotationDegrees = 35,
                RotationPivot = PdfRotationPivot.Center
            };
            var rotEllipseInfo = pdfEditor.AddEllipse(currentPage, rotEllipse);

            // axis-aligned bounding box of the rotated ellipse (red dashed)
            var eb = rotEllipseInfo.LastPageRectangle.Bounds;
            pdfEditor.AddRectangle(currentPage, new PdfRectangleElement(eb.X, eb.Y, eb.Width, eb.Height)
            {
                FillColor = null,
                BorderColor = PdfColor.Red,
                Border = new PdfLineStyle { LineWidth = 0.5f, DashStyle = PdfLineDashStyle.Dotted }
            });
            int rotCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "rotated 35 deg (red axis-aligned bounding box)",
                xLeft + 150, (int)eb.Bottom + 5, 250);

            crtYPos = Math.Max(axisCapBottom, rotCapBottom) + ySeparator;

            // ===== Section 6: PdfArcElement (3 closure types) =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 180, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "6. PdfArcElement (Open, Chord, Pie closures)", xLeft, crtYPos, ySeparator);

            PdfArcClosureType[] closures = { PdfArcClosureType.Open, PdfArcClosureType.Chord, PdfArcClosureType.Pie };
            string[] closureLabels = { "Open (stroke only)", "Chord", "Pie" };
            int arcMaxBottom = crtYPos;
            for (int i = 0; i < closures.Length; i++)
            {
                int arcX = xLeft + i * 170;
                var arcInfo = pdfEditor.AddArc(currentPage, new PdfArcElement(arcX, crtYPos, 130, 90,
                    startAngleDegrees: 20,
                    sweepAngleDegrees: 200)
                {
                    Closure = closures[i],
                    FillColor = PdfColor.LightSkyBlue,
                    FillOpacity = 0.4f,
                    BorderColor = PdfColor.MediumBlue,
                    Border = new PdfLineStyle { LineWidth = 1.5f }
                });
                int capBottom = AddCaption(pdfEditor, ref currentPage, labelFont, closureLabels[i], arcX,
                    (int)arcInfo.LastPageRectangle.Bounds.Bottom + 5, 140);
                arcMaxBottom = Math.Max(arcMaxBottom, capBottom);
            }
            crtYPos = arcMaxBottom + ySeparator;

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfShapesEditDemo.pdf";
            return fileResult;
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
                // The content continues on the following page of the document, or on a new page after the last one
                currentPage = currentPage < pdfEditor.PageCount ? currentPage + 1 : pdfEditor.AddPage();
                crtYPos = topMargin;
            }
        }

        private Add_Shapes_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_Shapes_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_Shapes_to_Existing_PDF".Length);

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

## Add Polylines, Polygons and Paths to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-paths-and-polygons-to-existing-pdf.htm

### Code Sample - Add Paths and Polygons to Existing PDF

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using System.Collections.Generic;
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
    public class Add_Paths_and_Polygons_to_Existing_PDFController : Controller
    {
        private const int leftMargin = 36;
        private const int topMargin = 36;
        // The content area of the pages inside the demo margins, from the size of the first page of the loaded document
        private int contentWidth;
        private int contentHeight;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_Paths_and_Polygons_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_Paths_and_Polygons_to_Existing_PDF_ViewModel model)
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

            // The pages of an edited document have no margins: the demo keeps its content inside its own margins
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            PdfPageInfo firstPageInfo = pdfEditor.GetPdfPageInfo(1);
            // A rotation of 90 or 270 degrees swaps the width and the height of the page as it is displayed
            bool firstPageTurned = firstPageInfo.Rotation % 180 != 0;
            contentWidth = (int)(firstPageTurned ? firstPageInfo.PageSize.Height : firstPageInfo.PageSize.Width) - 2 * leftMargin;
            contentHeight = (int)(firstPageTurned ? firstPageInfo.PageSize.Width : firstPageInfo.PageSize.Height) - 2 * topMargin;

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);

            const int xLeft = leftMargin;
            const int ySeparator = 10;
            int currentPage = 1;
            int crtYPos = topMargin;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Polyline, Polygon and Path Demo", titleFont)
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

            // ===== Section 1: PdfPolylineElement (open, with caps) =====
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "1. PdfPolylineElement (zigzag, round joins)", xLeft, crtYPos, ySeparator);

            List<PdfPointF> zigzag = new List<PdfPointF>();
            for (int i = 0; i <= 12; i++)
            {
                float x = xLeft + i * 30;
                float y = crtYPos + (i % 2 == 0 ? 0 : 40);
                zigzag.Add(new PdfPointF(x, y));
            }

            var zigzagInfo = pdfEditor.AddPolyline(currentPage, new PdfPolylineElement(zigzag)
            {
                LineColor = PdfColor.DarkOrange,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 3f,
                    LineCap = PdfLineCapStyle.Round,
                    LineJoin = PdfLineJoinStyle.Round
                }
            });
            int zigzagCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "12-segment zigzag, LineWidth=3, Round caps and joins",
                xLeft, (int)zigzagInfo.LastPageRectangle.Bounds.Bottom + 5, 400);
            crtYPos = zigzagCapBottom + ySeparator * 2;

            // Same path rotated
            List<PdfPointF> zigzag2 = new List<PdfPointF>();
            for (int i = 0; i <= 12; i++)
            {
                float x = xLeft + i * 30;
                float y = crtYPos + (i % 2 == 0 ? 0 : 30);
                zigzag2.Add(new PdfPointF(x, y));
            }
            var zigzag2Info = pdfEditor.AddPolyline(currentPage, new PdfPolylineElement(zigzag2)
            {
                LineColor = PdfColor.SteelBlue,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 2f,
                    DashStyle = PdfLineDashStyle.Dashed,
                    LineJoin = PdfLineJoinStyle.Miter
                },
                RotationDegrees = -6,
                RotationPivot = PdfRotationPivot.TopLeft
            });
            int zigzag2CapBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "Same zigzag, dashed, rotated 6 deg clockwise",
                xLeft, (int)zigzag2Info.LastPageRectangle.Bounds.Bottom + 5, 400);
            crtYPos = zigzag2CapBottom + ySeparator * 2;

            // ===== Section 2: PdfPolygonElement (triangle, hexagon, star) =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 200, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "2. PdfPolygonElement (triangle, regular hexagon, 5-point star)",
                xLeft, crtYPos, ySeparator);

            // Triangle
            var triangleInfo = pdfEditor.AddPolygon(currentPage, new PdfPolygonElement(new PdfPointF(xLeft + 60, crtYPos),
                new PdfPointF(xLeft + 110, crtYPos + 90),
                new PdfPointF(xLeft + 10, crtYPos + 90))
            {
                FillColor = PdfColor.LightSalmon,
                FillOpacity = 0.7f,
                BorderColor = PdfColor.DarkRed,
                Border = new PdfLineStyle { LineWidth = 1.5f }
            });
            int triangleCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "triangle (filled)", xLeft + 10,
                (int)triangleInfo.LastPageRectangle.Bounds.Bottom + 5, 110);

            // Regular hexagon
            var hexagonInfo = pdfEditor.AddPolygon(currentPage, new PdfPolygonElement(
                BuildRegularPolygon(xLeft + 200, crtYPos + 45, 50, 6, startAngleDeg: -90))
            {
                FillColor = PdfColor.LightGreen,
                FillOpacity = 0.6f,
                BorderColor = PdfColor.DarkGreen,
                Border = new PdfLineStyle
                {
                    LineWidth = 1.5f,
                    LineJoin = PdfLineJoinStyle.Round
                }
            });
            int hexagonCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "regular hexagon", xLeft + 150,
                (int)hexagonInfo.LastPageRectangle.Bounds.Bottom + 5, 110);

            // 5-point star
            var starInfo = pdfEditor.AddPolygon(currentPage, new PdfPolygonElement(
                BuildStar(xLeft + 340, crtYPos + 45, outerRadius: 50, innerRadius: 22, points: 5))
            {
                FillColor = PdfColor.Gold,
                FillOpacity = 0.85f,
                BorderColor = PdfColor.DarkGoldenRod,
                Border = new PdfLineStyle { LineWidth = 1.2f, LineJoin = PdfLineJoinStyle.Miter, MiterLimit = 4f }
            });
            int starCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont, "5-point star", xLeft + 295,
                (int)starInfo.LastPageRectangle.Bounds.Bottom + 5, 110);

            crtYPos = Math.Max(Math.Max(triangleCapBottom, hexagonCapBottom), starCapBottom) + ySeparator * 2;

            // ===== Section 3: PdfPathElement (heart via fluent API) =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 200, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "3. PdfPathElement (fluent MoveTo / LineTo / CurveTo / Close)",
                xLeft, crtYPos, ySeparator);

            // Heart shape built around (cx, cy) with size s.
            float cx = xLeft + 80;
            float cy = crtYPos + 80;
            float s = 60f;

            PdfPathElement heart = new PdfPathElement
            {
                FillColor = PdfColor.Crimson,
                FillOpacity = 0.8f,
                BorderColor = PdfColor.DarkRed,
                Border = new PdfLineStyle { LineWidth = 1.5f, LineJoin = PdfLineJoinStyle.Round }};

            heart
                .MoveTo(cx, cy + s * 0.25f)
                // Left (cubic Bezier upward to top center)
                .CurveTo(
                    cx - s * 0.55f, cy + s * 0.55f,
                    cx - s * 1.10f, cy - s * 0.10f,
                    cx, cy - s * 0.35f)
                // Right (cubic Bezier from top center back down)
                .CurveTo(
                    cx + s * 1.10f, cy - s * 0.10f,
                    cx + s * 0.55f, cy + s * 0.55f,
                    cx, cy + s * 0.25f)
                .Close();

            var heartInfo = pdfEditor.AddPath(currentPage, heart);
            int heartCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "heart (two cubic Beziers + Close, FillColor set)",
                xLeft, (int)heartInfo.LastPageRectangle.Bounds.Bottom + 5, 250);

            // Wave
            PdfPathElement wave = new PdfPathElement
            {
                BorderColor = PdfColor.SteelBlue,
                Border = new PdfLineStyle { LineWidth = 2f, LineCap = PdfLineCapStyle.Round }};

            float waveStartX = xLeft + 200;
            float waveY = crtYPos + 70;
            wave.MoveTo(waveStartX, waveY);
            for (int i = 0; i < 3; i++)
            {
                float x0 = waveStartX + i * 80;
                wave.CurveTo(
                    x0 + 20, waveY - 30,
                    x0 + 60, waveY + 30,
                    x0 + 80, waveY);
            }

            var waveInfo = pdfEditor.AddPath(currentPage, wave);
            int waveCapBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "stroke-only wave (3 cubic segments)",
                xLeft + 200, (int)waveInfo.LastPageRectangle.Bounds.Bottom + 5, 250);

            // Third triangle built via the params PdfPathOperation[] constructor
            float triX = xLeft + 460;
            float triY = crtYPos + 30;
            var staticTriangle = new PdfPathElement(
                PdfPathOperation.MoveTo(triX + 40, triY),
                PdfPathOperation.LineTo(triX + 80, triY + 70),
                PdfPathOperation.LineTo(triX, triY + 70),
                PdfPathOperation.Close())
            {
                FillColor = PdfColor.LightSeaGreen,
                FillOpacity = 0.6f,
                BorderColor = PdfColor.Teal,
                Border = new PdfLineStyle { LineWidth = 1.2f }
            };
            var triangleInfo3 = pdfEditor.AddPath(currentPage, staticTriangle);
            int triangle3CapBottom = AddCaption(pdfEditor, ref currentPage, labelFont,
                "static triangle (params PdfPathOperation[])",
                (int)(triX - 10), (int)triangleInfo3.LastPageRectangle.Bounds.Bottom + 5, 110);

            crtYPos = Math.Max(Math.Max(heartCapBottom, waveCapBottom), triangle3CapBottom) + ySeparator * 2;

            // ===== Section 4: Path with rotation =====
            EnsureSpaceOnPage(ref crtYPos, ref currentPage, 220, pdfEditor, contentHeight, topMargin);
            crtYPos = AddSectionLabel(pdfEditor, ref currentPage, sectionFont,
                "4. PdfPathElement (arrow shape rotated 0, 45, 90, 135 degrees)",
                xLeft, crtYPos, ySeparator);

            float[] angles = { 0, 45, 90, 135 };
            int arrowMaxBottom = crtYPos;
            for (int i = 0; i < angles.Length; i++)
            {
                int cellX = xLeft + i * 120;
                int cellY = crtYPos + 30;

                PdfPathElement arrow = new PdfPathElement
                {
                    FillColor = PdfColor.MediumPurple,
                    FillOpacity = 0.7f,
                    BorderColor = PdfColor.Indigo,
                    Border = new PdfLineStyle { LineWidth = 1f },
                    RotationDegrees = angles[i],
                    RotationPivot = PdfRotationPivot.Center
                };

                // Right-pointing arrow
                arrow
                    .MoveTo(cellX, cellY + 10)
                    .LineTo(cellX + 50, cellY + 10)
                    .LineTo(cellX + 50, cellY)
                    .LineTo(cellX + 80, cellY + 20)
                    .LineTo(cellX + 50, cellY + 40)
                    .LineTo(cellX + 50, cellY + 30)
                    .LineTo(cellX, cellY + 30)
                    .Close();

                var arrowInfo = pdfEditor.AddPath(currentPage, arrow);
                int capBottom = AddCaption(pdfEditor, ref currentPage, labelFont, $"{angles[i]} deg", cellX,
                    (int)arrowInfo.LastPageRectangle.Bounds.Bottom + 5, 80);
                arrowMaxBottom = Math.Max(arrowMaxBottom, capBottom);
            }
            crtYPos = arrowMaxBottom + ySeparator;

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfPathsPolygonsEditDemo.pdf";
            return fileResult;
        }

        // Builds a regular N-sided polygon centered at (cx, cy) with circumscribed radius r
        private static List<PdfPointF> BuildRegularPolygon(float cx, float cy, float r, int sides, float startAngleDeg)
        {
            var pts = new List<PdfPointF>(sides);
            for (int i = 0; i < sides; i++)
            {
                double a = (startAngleDeg + i * 360.0 / sides) * Math.PI / 180.0;
                pts.Add(new PdfPointF(
                    (float)(cx + r * Math.Cos(a)),
                    (float)(cy + r * Math.Sin(a))));
            }
            return pts;
        }

        // Builds an N-point star centered at (cx, cy)
        private static List<PdfPointF> BuildStar(float cx, float cy, float outerRadius, float innerRadius, int points)
        {
            int total = points * 2;
            var pts = new List<PdfPointF>(total);
            for (int i = 0; i < total; i++)
            {
                float r = (i % 2 == 0) ? outerRadius : innerRadius;
                double a = (-90 + i * 360.0 / total) * Math.PI / 180.0;
                pts.Add(new PdfPointF(
                    (float)(cx + r * Math.Cos(a)),
                    (float)(cy + r * Math.Sin(a))));
            }
            return pts;
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
                // The content continues on the following page of the document, or on a new page after the last one
                currentPage = currentPage < pdfEditor.PageCount ? currentPage + 1 : pdfEditor.AddPage();
                crtYPos = topMargin;
            }
        }

        private Add_Paths_and_Polygons_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_Paths_and_Polygons_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_Paths_and_Polygons_to_Existing_PDF".Length);

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

## Add HTML to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-html-to-existing-pdf.htm

### Render HTML Content in an Edited Document

```csharp
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, null);

PdfHtmlElement htmlElement = new PdfHtmlElement(htmlString, baseUrl)
{
    X = 36,
    Y = 120,
    Width = 523,
    // The pages where the content continues are inserted after the first page
    InsertContinuationPages = true,
    ContinuationTopMargin = 72,
    ContinuationBottomMargin = 72
};
PdfHtmlRenderInfo htmlRenderInfo = pdfEditor.AddHtml(1, htmlElement);

// The elements added after the content go below it, in its last page
int lastPage = htmlRenderInfo.LastPageNumber;
float crtYPos = htmlRenderInfo.LastPageRectangle.Bounds.Bottom + 15;
```

### Set the Converter and Retrieve the Positions of the HTML Elements

```csharp
htmlElement.Converter.MediaType = "print";
htmlElement.Converter.PdfDocumentOptions.GenerateDocumentOutline = true;
htmlElement.HtmlElementsInfoSelector = "h1, h2";

PdfHtmlRenderInfo htmlRenderInfo = pdfEditor.AddHtml(1, htmlElement);
foreach (HtmlElementInfo elementInfo in htmlRenderInfo.HtmlElementsInfo.Elements)
{
    foreach (PdfRenderedRectangle rectangle in elementInfo.RenderedRectangles)
        Console.WriteLine("{0} on page {1} at {2}", elementInfo.TagName, rectangle.PageNumber, rectangle.Bounds);
}
```

### Code Sample - Add HTML to Existing PDF

```csharp
using System;
using System.IO;
using System.Text;
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
    public class Add_HTML_to_Existing_PDFController : Controller
    {
        // The area of the letterhead pages left free by their header and footer
        private const int leftMargin = 36;
        private const int topMargin = 90;
        private const int bottomMargin = 70;
        // The content area of the pages inside the demo margins, from the size of the first page of the loaded document
        private int contentWidth;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_HTML_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_HTML_to_Existing_PDF_ViewModel model)
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

            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            PdfPageInfo firstPageInfo = pdfEditor.GetPdfPageInfo(1);
            // A rotation of 90 or 270 degrees swaps the width and the height of the page as it is displayed
            bool firstPageTurned = firstPageInfo.Rotation % 180 != 0;
            contentWidth = (int)(firstPageTurned ? firstPageInfo.PageSize.Height : firstPageInfo.PageSize.Width) - 2 * leftMargin;

            string fontFilePath = Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 16f, PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont textFont = PdfFontManager.CreateFont(baseFont, 10f, PdfFontStyle.Normal, PdfColor.Black);
            PdfFont smallFont = PdfFontManager.CreateFont(baseFont, 8f, PdfFontStyle.Normal, PdfColor.DarkGray);

            // The title and the HTML content go to the first page of the document, below its header
            int currentPage = 1;
            int crtYPos = topMargin;

            PdfTextElement titleElement = new PdfTextElement("HTML Content in an Existing PDF Document", titleFont)
            {
                X = leftMargin,
                Y = crtYPos,
                Width = contentWidth,
                Alignment = PdfTextAlignment.Center
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            var titleInfo = pdfEditor.AddText(currentPage, titleElement);
            currentPage = titleInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)titleInfo.LastPageRectangle.Bounds.Bottom + 15;

            // A page added after the pages of the document: the content continues over it and over the pages before
            // it, or the pages where the content continues are inserted before them and they keep their content, as
            // the option asks
            int pageAfterContent = pdfEditor.AddPage();
            pdfEditor.AddText(pageAfterContent, new PdfTextElement("The page that followed the HTML content", smallFont)
            {
                X = leftMargin,
                Y = topMargin
            });

            // The HTML content rendered from the given position of the page, relative to the top left corner of the
            // page. The pages of an edited document have no margins, so the continuation margins give the area of the
            // pages where the content continues
            PdfHtmlElement htmlElement = model.HtmlPageSource == "Url"
                ? new PdfHtmlElement(model.Url)
                : new PdfHtmlElement(model.HtmlStringTextBox ?? string.Empty, model.BaseUrlTextBox);
            htmlElement.X = leftMargin + model.ElementX;
            // The content starts at the given distance below the title
            htmlElement.Y = crtYPos + model.ElementY;
            // 0 extends the content to the right edge of the page
            htmlElement.Width = model.ElementWidth;
            // The pages where the content continues are inserted after the current one, so the pages that follow in
            // the document keep their content; when false, the content continues on the following pages of the document
            htmlElement.InsertContinuationPages = model.InsertContinuationPages;
            htmlElement.ContinuationTopMargin = topMargin;
            htmlElement.ContinuationBottomMargin = bottomMargin;

            // The converter of the element takes the page size and the margins from the page; its other settings are
            // set as for a HTML to PDF conversion
            htmlElement.Converter.MediaType = model.MediaType;
            // The headings of the HTML content join the bookmarks of the document
            htmlElement.Converter.PdfDocumentOptions.GenerateDocumentOutline = model.GenerateDocumentOutline;

            if (model.RetrieveElementPositions)
            {
                // The positions of the elements matching the selector are reported in the render information
                htmlElement.HtmlElementsInfoSelector = model.HtmlElementsSelector;
            }

            PdfHtmlRenderInfo htmlRenderInfo = pdfEditor.AddHtml(currentPage, htmlElement);

            // The elements after the HTML content are placed below it, at the end of the content on its last page
            currentPage = htmlRenderInfo.LastPageNumber;
            crtYPos = (int)htmlRenderInfo.LastPageRectangle.Bounds.Bottom + 15;

            PdfRectangleElement separator = new PdfRectangleElement(leftMargin, crtYPos, contentWidth, 1)
            {
                FillColor = PdfColor.DarkBlue,
                BorderColor = null
            };
            pdfEditor.AddRectangle(currentPage, separator);

            PdfTextElement closingElement = new PdfTextElement(
                string.Format("The HTML content was rendered on {0} page(s), from page {1} to page {2}. This text was added after it.",
                    htmlRenderInfo.PagesCount, htmlRenderInfo.FirstPageNumber, htmlRenderInfo.LastPageNumber), textFont)
            {
                X = leftMargin,
                Y = crtYPos + 8,
                Width = contentWidth
            };
            var closingInfo = pdfEditor.AddText(currentPage, closingElement);
            currentPage = closingInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)closingInfo.LastPageRectangle.Bounds.Bottom + 10;

            if (model.RetrieveElementPositions)
            {
                // The page and the rectangle of each matching element; an element split by a page break has a rectangle for each page
                StringBuilder positions = new StringBuilder();
                foreach (HtmlElementInfo elementInfo in htmlRenderInfo.HtmlElementsInfo.Elements)
                {
                    positions.Append(elementInfo.TagName);
                    if (!string.IsNullOrEmpty(elementInfo.Id))
                        positions.Append(" #").Append(elementInfo.Id);
                    positions.Append(" \"").Append(Shorten(elementInfo.Text)).Append("\":");
                    foreach (PdfRenderedRectangle rectangle in elementInfo.RenderedRectangles)
                    {
                        positions.Append(string.Format(" page {0} ({1:0}, {2:0}, {3:0}, {4:0})", rectangle.PageNumber,
                            rectangle.Bounds.X, rectangle.Bounds.Y, rectangle.Bounds.Width, rectangle.Bounds.Height));
                    }
                    positions.Append('\n');
                }

                PdfTextElement positionsElement = new PdfTextElement(
                    "Positions of the elements matching \"" + model.HtmlElementsSelector + "\":\n" + positions, smallFont)
                {
                    X = leftMargin,
                    Y = crtYPos,
                    Width = contentWidth,
                    ContinueOnNextPage = true,
                    ContinuationTopMargin = topMargin,
                    ContinuationBottomMargin = bottomMargin
                };
                pdfEditor.AddText(currentPage, positionsElement);
            }

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "HTML_in_Existing_Pdf_Document.pdf";
            return fileResult;
        }

        private static string Shorten(string text)
        {
            text = (text ?? string.Empty).Replace('\n', ' ').Replace('\r', ' ').Trim();
            return text.Length > 40 ? text.Substring(0, 40) + "..." : text;
        }

        private Add_HTML_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_HTML_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_HTML_to_Existing_PDF".Length);
            string contentRootPath = m_hostingEnvironment.ContentRootPath + "/wwwroot/";

            // A one-page document with a letterhead; the content goes between its header and footer
            model.PdfFileUrl = rootUrl + "DemoAppFiles/Input/PDF_Files/Letterhead.pdf";
            model.HtmlStringTextBox = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Auto_Bookmarks.html"));
            model.BaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```

## Add HTML as Image to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-html-as-image-to-existing-pdf.htm

### Render HTML as an Image in an Edited Document

```csharp
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, null);

int pageNumber = pdfEditor.AddPage();
PdfHtmlImageElement htmlImage = new PdfHtmlImageElement(htmlCode, baseUrl)
{
    X = 36,
    Y = 72,
    Resolution = 150,
    ContinueOnNextPage = true,
    // The pages where the image continues are inserted after the current page
    InsertContinuationPages = true,
    ContinuationTopMargin = 36,
    ContinuationBottomMargin = 36
};
PdfImageRenderInfo htmlImageInfo = pdfEditor.AddHtmlImage(pageNumber, htmlImage);

byte[] outPdfBuffer = pdfEditor.Save();
```

### Code Sample - Add HTML as Image to Existing PDF

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
    public class Add_HTML_as_Image_to_Existing_PDFController : Controller
    {
        // The area of the letterhead pages left free by their header and footer
        private const int leftMargin = 36;
        private const int topMargin = 90;
        private const int bottomMargin = 70;
        // The content area of the pages inside the demo margins, from the size of the first page of the loaded document
        private int contentWidth;

        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_HTML_as_Image_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_HTML_as_Image_to_Existing_PDF_ViewModel model)
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

            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            PdfPageInfo firstPageInfo = pdfEditor.GetPdfPageInfo(1);
            // A rotation of 90 or 270 degrees swaps the width and the height of the page as it is displayed
            bool firstPageTurned = firstPageInfo.Rotation % 180 != 0;
            contentWidth = (int)(firstPageTurned ? firstPageInfo.PageSize.Height : firstPageInfo.PageSize.Width) - 2 * leftMargin;

            string fontFilePath = Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 16f, PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont smallFont = PdfFontManager.CreateFont(baseFont, 8f, PdfFontStyle.Normal, PdfColor.DarkGray);

            // The title and the HTML image go to the first page of the document, below its header
            int currentPage = 1;
            int crtYPos = topMargin;

            PdfTextElement titleElement = new PdfTextElement("HTML Rendered as an Image", titleFont)
            {
                X = leftMargin,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = contentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            var titleInfo = pdfEditor.AddText(currentPage, titleElement);
            currentPage = titleInfo.LastPageRectangle.PageNumber;
            crtYPos = (int)titleInfo.LastPageRectangle.Bounds.Bottom + 15;

            // A page added after the pages of the document: the image continues over it and over the pages before it,
            // or the pages where the image continues are inserted before them and they keep their content, as the
            // option asks
            int pageAfterImage = pdfEditor.AddPage();
            pdfEditor.AddText(pageAfterImage, new PdfTextElement("The page that followed the HTML image", smallFont)
            {
                X = leftMargin,
                Y = topMargin
            });

            // The HTML content rendered as an image, from a URL or from a HTML string with a base URL. The pages
            // of an edited document have no margins, so the continuation margins give the area of the following
            // pages where the image continues
            PdfHtmlImageElement htmlImage = model.HtmlPageSource == "Url"
                ? new PdfHtmlImageElement(model.Url)
                : new PdfHtmlImageElement(model.HtmlStringTextBox ?? string.Empty, model.BaseUrlTextBox);
            htmlImage.X = leftMargin + model.ElementX;
            htmlImage.Y = crtYPos + model.ElementY;
            htmlImage.Width = model.ElementWidth;
            htmlImage.Height = model.ElementHeight;
            htmlImage.Resolution = model.Resolution;
            htmlImage.ContinueOnNextPage = model.ContinueOnNextPage;
            // The pages where the image continues are inserted after the current one, so the pages that follow in
            // the document keep their content; when false, the image continues on the following pages of the document
            htmlImage.InsertContinuationPages = model.InsertContinuationPages;
            htmlImage.ContinuationTopMargin = topMargin;
            htmlImage.ContinuationBottomMargin = bottomMargin;
            htmlImage.Converter.HtmlViewerWidth = model.HtmlViewerWidth;
            htmlImage.Accessibility.AlternateText = "HTML content rendered as an image";

            PdfImageRenderInfo htmlImageInfo = pdfEditor.AddHtmlImage(currentPage, htmlImage);

            var partsText = new System.Text.StringBuilder(
                "The image was drawn on " + htmlImageInfo.RenderedRectangles.Count + " page(s):");
            foreach (PdfRenderedRectangle part in htmlImageInfo.RenderedRectangles)
            {
                partsText.AppendFormat(" page {0} ({1:F0}, {2:F0}, {3:F0}x{4:F0});", part.PageNumber,
                    part.Bounds.X, part.Bounds.Y, part.Bounds.Width, part.Bounds.Height);
            }

            currentPage = htmlImageInfo.LastPage;
            // The part of the image drawn on its last page: the whole image, or the part that fits when it is cut
            PdfRectangleF lastPart = htmlImageInfo.LastPageRectangle.VisibleBounds ?? htmlImageInfo.LastPageRectangle.Bounds;
            PdfTextElement partsLabel = new PdfTextElement(partsText.ToString(), smallFont)
            {
                X = leftMargin,
                Y = (int)lastPart.Bottom + 10,
                Width = contentWidth
            };
            partsLabel.Accessibility.StructureType = PdfStructureType.Artifact;
            pdfEditor.AddText(currentPage, partsLabel);

            byte[] outPdfBuffer = pdfEditor.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "HTML_as_Image_Edit_Demo.pdf";
            return fileResult;
        }

        private Add_HTML_as_Image_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_HTML_as_Image_to_Existing_PDF_ViewModel();

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
                0, currentPageUrl.Length - "Add_HTML_as_Image_to_Existing_PDF".Length);

            // A one-page document with a letterhead; the image goes between its header and footer
            model.PdfFileUrl = rootUrl + "DemoAppFiles/Input/PDF_Files/Letterhead.pdf";
            model.HtmlStringTextBox = GetSampleHtml();
            return model;
        }

        // A styled table of 40 rows, taller than a page at the size it has on screen
        private static string GetSampleHtml()
        {
            var html = new System.Text.StringBuilder();
            html.Append("<html>\n<head>\n<style>\n" +
                "body { margin: 0; font-family: Arial, sans-serif; font-size: 15px; color: #1f2937; }\n" +
                ".sheet { width: 660px; padding: 14px; background: linear-gradient(180deg, #f8fafc, #ffffff); " +
                "border: 1px solid #cbd5e1; border-radius: 8px; }\n" +
                "h2 { margin: 0 0 10px 0; font-size: 22px; color: #1e3a8a; }\n" +
                "table { width: 100%; border-collapse: collapse; }\n" +
                "th { background: #1e3a8a; color: #ffffff; padding: 7px 10px; text-align: left; }\n" +
                "td { padding: 6px 10px; border-bottom: 1px solid #e2e8f0; }\n" +
                "tr:nth-child(even) td { background: #eff6ff; }\n" +
                ".num { text-align: right; font-variant-numeric: tabular-nums; }\n" +
                "</style>\n</head>\n<body>\n<div class=\"sheet\">\n<h2>Monthly Sales by Region</h2>\n<table>\n" +
                "<tr><th>Month</th><th>Region</th><th class=\"num\">Units</th><th class=\"num\">Revenue</th></tr>\n");

            string[] months = { "January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December" };
            string[] regions = { "North", "South", "East", "West" };
            int row = 0;
            foreach (string month in months)
            {
                foreach (string region in regions)
                {
                    int units = 120 + (row * 37) % 95;
                    html.AppendFormat("<tr><td>{0}</td><td>{1}</td><td class=\"num\">{2}</td><td class=\"num\">{3:N2}</td></tr>\n",
                        month, region, units, units * 24.5);
                    row++;
                }
            }

            html.Append("</table>\n</div>\n</body>\n</html>");
            return html.ToString();
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```

## Add Pages of Other PDF Documents to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-pdf-pages-to-existing-pdf.htm

### Draw a Letterhead Page under the Content

```csharp
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, null);

for (int pageNumber = 1; pageNumber <= pdfEditor.PageCount; pageNumber++)
{
    // The letterhead page under the content of each page, at its own size
    PdfPageElement pageElement = new PdfPageElement(letterheadPdfBytes, 1)
    {
        X = 0,
        Y = 0,
        Opacity = 1,
        RenderUnderContent = true,
        IncludeAnnotations = true
    };
    PdfElementRenderInfo renderInfo = pdfEditor.AddPdfPage(pageNumber, pageElement);
}

byte[] outPdfBuffer = pdfEditor.Save();
```

### Code Sample - Add Pages of Other PDF Documents to Existing PDF

```csharp
using System;
using System.Collections.Generic;
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
    public class Add_PDF_Pages_to_Existing_PDFController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Add_PDF_Pages_to_Existing_PDFController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> EditPdf(Add_PDF_Pages_to_Existing_PDF_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] inputPdfBytes = await ReadInputPdf(model.PdfFile, model.PdfFileUrl);
            byte[] sourcePdfBytes = await ReadInputPdf(model.SourcePdfFile, model.SourcePdfFileUrl);
            string password = !string.IsNullOrEmpty(model.OwnerPassword) ? model.OwnerPassword :
                !string.IsNullOrEmpty(model.UserPassword) ? model.UserPassword : null;
            string sourcePassword = string.IsNullOrEmpty(model.SourcePassword) ? null : model.SourcePassword;

            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
            int pageCount = pdfEditor.PageCount;

            List<int> targetPages = new List<int>();
            switch (model.TargetPages)
            {
                case "First":
                    targetPages.Add(1);
                    break;
                case "Last":
                    targetPages.Add(pageCount);
                    break;
                case "Odd":
                    for (int i = 1; i <= pageCount; i += 2)
                        targetPages.Add(i);
                    break;
                case "Even":
                    for (int i = 2; i <= pageCount; i += 2)
                        targetPages.Add(i);
                    break;
                default:
                    for (int i = 1; i <= pageCount; i++)
                        targetPages.Add(i);
                    break;
            }

            PdfFont captionFont = null;
            if (model.AddCaption)
                captionFont = PdfFontManager.CreateFont(Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf"), 8f, PdfFontStyle.Normal, PdfColor.DarkGray);

            foreach (int pageNumber in targetPages)
            {
                // The source page drawn in the target page. The position is relative to the top left corner of the page;
                // the page is scaled to fit the given width and height when they are set, otherwise it keeps its size
                PdfPageElement pageElement = new PdfPageElement(sourcePdfBytes, model.SourcePageNumber)
                {
                    Password = sourcePassword,
                    X = model.ElementX,
                    Y = model.ElementY,
                    Width = model.ElementWidth,
                    Height = model.ElementHeight,
                    Opacity = model.Opacity,
                    RotationDegrees = model.RotationDegrees,
                    RotationPivot = model.RotationPivot,
                    // The page goes under the existing content, as a letterhead or a watermark, or over it, as a stamp
                    RenderUnderContent = model.RenderUnderContent,
                    // The links and form fields of the source page are added to the target page
                    IncludeAnnotations = model.IncludeAnnotations
                };
                PdfElementRenderInfo renderInfo = pdfEditor.AddPdfPage(pageNumber, pageElement);

                if (model.AddCaption)
                {
                    PdfRectangleF bounds = renderInfo.LastPageRectangle.Bounds;
                    pdfEditor.AddText(pageNumber, new PdfTextElement(
                        string.Format("Page {0} of the source document drawn at ({1:0}, {2:0}), {3:0} x {4:0} points",
                            model.SourcePageNumber, bounds.X, bounds.Y, bounds.Width, bounds.Height), captionFont)
                    {
                        X = 20,
                        Y = pdfEditor.GetPdfPageInfo(pageNumber).PageSize.Height - 16
                    });
                }
            }

            byte[] outPdfBuffer = pdfEditor.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Add_PDF_Pages_to_Existing_PDF.pdf";
            return fileResult;
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

        private Add_PDF_Pages_to_Existing_PDF_ViewModel SetViewModel()
        {
            var model = new Add_PDF_Pages_to_Existing_PDF_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Add_PDF_Pages_to_Existing_PDF".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Multi_Page_Document.pdf";
            model.SourcePdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Letterhead.pdf";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```

## Add, Insert, Move, Rotate and Remove PDF Pages

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-insert-move-rotate-and-remove-pdf-pages.htm

### Insert a Cover Page and Add a Last Page

```csharp
// Insert a blank page of the chosen size before the first page. The existing pages
// are shifted and keep their content, links and bookmarks
int coverPageNumber = pdfEditor.InsertPage(1, PdfPageSize.A4, PdfPageOrientation.Portrait);

// Add a blank page at the end of the document, with the size of the last page
int lastPageNumber = pdfEditor.AddPage();
```

### Move a Page

```csharp
// Move a page to another position; the pages in between are shifted
pdfEditor.MovePage(movePageNumber, moveToPageNumber);
```

### Rotate a Page and Add Text to It

```csharp
// Rotate the page clockwise by a multiple of 90 degrees. The rotation is added to the
// current rotation of the page; SetPageRotation would set an absolute rotation instead.
// A page is rotated before content is added to it
pdfEditor.RotatePage(rotatePageNumber, 90);

// A note at the bottom of the rotated page, placed in the page as it is displayed:
// a rotation of 90 or 270 degrees swaps its width and height
PdfPageInfo rotatedPageInfo = pdfEditor.GetPdfPageInfo(rotatePageNumber);
bool turned = rotatedPageInfo.Rotation % 180 != 0;
float displayedWidth = turned ? rotatedPageInfo.PageSize.Height : rotatedPageInfo.PageSize.Width;
float displayedHeight = turned ? rotatedPageInfo.PageSize.Width : rotatedPageInfo.PageSize.Height;
PdfTextElement rotatedPageText = new PdfTextElement("This page was rotated by 90 degrees", noteFont)
{
    X = 36,
    Y = displayedHeight - 60,
    Width = displayedWidth - 72
};
pdfEditor.AddText(rotatePageNumber, rotatedPageText);
```

### Remove Pages

```csharp
// The pages are removed first, so the ranges refer to the pages of the loaded document. The
// ranges are 1-based, for example "2,4-5" or "3-" for the pages from the third one to the end
pdfEditor.RemovePages("2,4-5");
```

### Code Sample - Add, Insert, Move, Rotate and Remove PDF Pages

```csharp
using System;

// Use EVO PDF Namespace
using EvoPdf.Next;

public byte[] EditPdfPages(byte[] inputPdfBytes, string password, string fontFilePath,
    int movePageNumber, int moveToPageNumber, int rotatePageNumber, int rotateDegrees, string removePageRanges)
{
    // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
    // Licensing.LicenseKey = "your-license-key";

    byte[] outPdfBuffer;

    using (PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password))
    {
        PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
        PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 16f, PdfFontStyle.Bold, PdfColor.DarkBlue);
        PdfFont textFont = PdfFontManager.CreateFont(baseFont, 10f, PdfFontStyle.Normal, PdfColor.Black);
        PdfFont noteFont = PdfFontManager.CreateFont(baseFont, 12f, PdfFontStyle.Bold, PdfColor.DarkRed);

        // The page operations come first; the text drawn afterwards goes to the pages at their
        // final position and rotation
        if (!string.IsNullOrWhiteSpace(removePageRanges))
        {
            // The pages are removed first, so the ranges refer to the pages of the loaded document. The
            // ranges are 1-based, for example "2,4-5" or "3-" for the pages from the third one to the end
            pdfEditor.RemovePages(removePageRanges.Trim());
        }

        // Insert a blank page of the chosen size before the first page. The existing pages
        // are shifted and keep their content, links and bookmarks
        int coverPageNumber = pdfEditor.InsertPage(1, PdfPageSize.A4, PdfPageOrientation.Portrait);

        // Add a blank page at the end of the document, with the size of the last page
        int lastPageNumber = pdfEditor.AddPage();

        // Move a page to another position; the pages in between are shifted
        pdfEditor.MovePage(movePageNumber, moveToPageNumber);

        // The pages added above follow the move
        coverPageNumber = PageNumberAfterMove(coverPageNumber, movePageNumber, moveToPageNumber);
        lastPageNumber = PageNumberAfterMove(lastPageNumber, movePageNumber, moveToPageNumber);

        // Rotate the page clockwise by a multiple of 90 degrees. The rotation is added to the
        // current rotation of the page; SetPageRotation would set an absolute rotation instead.
        // A page is rotated before content is added to it
        pdfEditor.RotatePage(rotatePageNumber, rotateDegrees);

        PdfTextElement lastPageText = new PdfTextElement("This page was added at the end of the document with PdfEditor.AddPage()", textFont)
        {
            X = 36,
            Y = 36,
            Width = pdfEditor.GetPdfPageInfo(lastPageNumber).PageSize.Width - 72
        };
        pdfEditor.AddText(lastPageNumber, lastPageText);

        // A note at the bottom of the rotated page, placed in the page as it is displayed:
        // a rotation of 90 or 270 degrees swaps its width and height
        PdfPageInfo rotatedPageInfo = pdfEditor.GetPdfPageInfo(rotatePageNumber);
        bool turned = rotatedPageInfo.Rotation % 180 != 0;
        float displayedWidth = turned ? rotatedPageInfo.PageSize.Height : rotatedPageInfo.PageSize.Width;
        float displayedHeight = turned ? rotatedPageInfo.PageSize.Width : rotatedPageInfo.PageSize.Height;
        PdfTextElement rotatedPageText = new PdfTextElement(
            "This page was rotated by " + rotateDegrees + " degrees with PdfEditor.RotatePage()", noteFont)
        {
            X = 36,
            Y = displayedHeight - 60,
            Width = displayedWidth - 72
        };
        pdfEditor.AddText(rotatePageNumber, rotatedPageText);

        // The cover page lists the pages of the edited document with the size and
        // rotation reported by GetPdfPageInfo
        PdfPageInfo coverPageInfo = pdfEditor.GetPdfPageInfo(coverPageNumber);
        float textWidth = coverPageInfo.PageSize.Width - 72;

        PdfTextElement coverTitle = new PdfTextElement("Pages of the edited document", titleFont)
        {
            X = 36,
            Y = 36,
            Width = textWidth
        };
        float crtYPos = pdfEditor.AddText(coverPageNumber, coverTitle).LastPageRectangle.Bounds.Bottom + 12;

        int pageCount = pdfEditor.PageCount;
        for (int pageNumber = 1; pageNumber <= pageCount; pageNumber++)
        {
            PdfPageInfo pageInfo = pdfEditor.GetPdfPageInfo(pageNumber);
            string pageLine = string.Format("Page {0}: {1} x {2} pt, rotation {3} degrees",
                pageInfo.PageNumber, pageInfo.PageSize.Width, pageInfo.PageSize.Height, pageInfo.Rotation);

            PdfTextElement pageText = new PdfTextElement(pageLine, textFont)
            {
                X = 54,
                Y = crtYPos,
                Width = textWidth - 18
            };
            crtYPos = pdfEditor.AddText(coverPageNumber, pageText).LastPageRectangle.Bounds.Bottom + 4;
        }

        outPdfBuffer = pdfEditor.Save();
    }

    return outPdfBuffer;
}

// The number of a page after another page was moved
private static int PageNumberAfterMove(int pageNumber, int movedPage, int newPage)
{
    if (pageNumber == movedPage)
        return newPage;
    if (movedPage < pageNumber && pageNumber <= newPage)
        return pageNumber - 1;
    if (newPage <= pageNumber && pageNumber < movedPage)
        return pageNumber + 1;
    return pageNumber;
}
```
