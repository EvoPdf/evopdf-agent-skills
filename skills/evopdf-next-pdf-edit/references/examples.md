# Editing an existing PDF: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Add Text to Existing PDF

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-text-to-existing-pdf.htm

### Open the Source PDF

```csharp
string password = string.IsNullOrEmpty(ownerPassword) ? userPassword : ownerPassword;
using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
```

### AddText, Page Cursor and Multi-Page Flow

```csharp
PdfTextElement element = new PdfTextElement(text, font) { X = 0, Y = crtYPos, Width = contentWidth };
var info = pdfEditor.AddText(currentPage, element);
currentPage = info.LastPageRectangle.PageNumber;        // follow the engine if it overflowed
crtYPos = (int)info.LastPageRectangle.Bounds.Bottom + ySeparator;
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
        private const int contentWidth = 595 - 72;
        private const int contentHeight = 842 - 72;

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

            // Set license key received after purchase to use the library in licensed mode
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
            pdfEditor.PdfDocumentInfo.Title = "PDF Text Demo";

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
                "This paragraph has a yellow highlight background drawn behind the text as an " +
                "Artifact (it does not appear in the structure tree). The background covers the " +
                "full column width and the actual used height.",
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
                "Both callbacks receive the same PdfTextPageRenderInfo type but the rectangle in " +
                "OnBeforePageRender is the predicted column area while in OnAfterPageRender it is " +
                "the actual rendered area.",
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

            // ===== Section 5: Multi-page continuation + per-page RenderedText =====
            currentPage = pdfEditor.AddPage();
            crtYPos = topMargin;

            PdfTextElement sectionLabel4 = new PdfTextElement(
                "4. Multi-page continuation + per-page RenderedText", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel4.Accessibility.StructureType = PdfStructureType.Heading2;
            var sectionLabel4Info = pdfEditor.AddText(currentPage, sectionLabel4);
            currentPage = sectionLabel4Info.LastPageRectangle.PageNumber;
            crtYPos = (int)sectionLabel4Info.LastPageRectangle.Bounds.Bottom + ySeparator;

            // Build a long text that will overflow several pages.
            string longTextSource = LoadAlphabetText();
            StringBuilder longBuilder = new StringBuilder();
            for (int i = 0; i < 8; i++) longBuilder.AppendLine(longTextSource);
            string longText = longBuilder.ToString();

            PdfTextElement multipage = new PdfTextElement(longText, bodyFont)
            {
                X = xLeft,
                Y = crtYPos,
                Width = contentWidth,
                Alignment = PdfTextAlignment.Left,
                ContinueOnNextPage = true,
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

            // ===== Section 6: Summary of pages rendered =====
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
            if (System.IO.File.Exists(alphabetFilePath))
                return System.IO.File.ReadAllText(alphabetFilePath);

            // Fallback so the demo runs even without the alphabet file.
            return "The quick brown fox jumps over the lazy dog. " +
                   "Pack my box with five dozen liquor jugs. " +
                   "Sphinx of black quartz, judge my vow. " +
                   "How vexingly quick daft zebras jump. ";
        }

        private void EnsureSpaceOnPage(ref int crtYPos, ref int currentPage, int requestedHeight, PdfEditor pdfEditor, int contentHeight, int topMargin)
        {
            if (crtYPos + requestedHeight > contentHeight + topMargin)
            {
                currentPage = pdfEditor.AddPage();
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
        private const int contentWidth = 595 - 72;
        private const int contentHeight = 842 - 72;

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

            // Open the loaded PDF for editing.  PdfEditor inherits the
            // standard (PDF/A, PDF/UA etc.) and Language from the source
            // document, so no PdfDocumentCreateSettings is needed.  Page
            // size and margins are also taken from the existing pages;
            // we apply our own margins manually below via leftMargin /
            // topMargin since PdfEditor draws at absolute page coordinates
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
            pdfEditor.PdfDocumentInfo.Title = "PDF Image Demo";

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
                currentPage = pdfEditor.AddPage();
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
        private const int contentWidth = 595 - 72;
        private const int contentHeight = 842 - 72;

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

            // Open the loaded PDF for editing.  PdfEditor inherits the
            // standard (PDF/A, PDF/UA etc.) and Language from the source
            // document, so no PdfDocumentCreateSettings is needed.  Page
            // size and margins are also taken from the existing pages;
            // we apply our own margins manually below via leftMargin /
            // topMargin since PdfEditor draws at absolute page coordinates
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
            pdfEditor.PdfDocumentInfo.Title = "PDF Geometric Shapes Demo";

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
                    LineColor = PdfColor.MediumBlue,
                    LineStyle = new PdfLineStyle { LineWidth = 1.5f }
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
                currentPage = pdfEditor.AddPage();
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
        private const int contentWidth = 595 - 72;
        private const int contentHeight = 842 - 72;

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

            // Open the loaded PDF for editing.  PdfEditor inherits the
            // standard (PDF/A, PDF/UA etc.) and Language from the source
            // document, so no PdfDocumentCreateSettings is needed.  Page
            // size and margins are also taken from the existing pages;
            // we apply our own margins manually below via leftMargin /
            // topMargin since PdfEditor draws at absolute page coordinates
            string password = string.IsNullOrEmpty(model.OwnerPassword) ? model.UserPassword : model.OwnerPassword;
            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);
            pdfEditor.PdfDocumentInfo.Title = "PDF Polyline, Polygon and Path Demo";

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
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle { LineWidth = 1.5f, LineJoin = PdfLineJoinStyle.Round }};

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
                LineColor = PdfColor.SteelBlue,
                LineStyle = new PdfLineStyle { LineWidth = 2f, LineCap = PdfLineCapStyle.Round }};

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
                LineColor = PdfColor.Teal,
                LineStyle = new PdfLineStyle { LineWidth = 1.2f }
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
                    LineColor = PdfColor.Indigo,
                    LineStyle = new PdfLineStyle { LineWidth = 1f },
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
                currentPage = pdfEditor.AddPage();
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
