# Creating PDF documents from code: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Create PDF Documents

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents.htm

### Create, Render Elements and Save PDF Documents

```csharp
// Create a new PDF document with the default settings
using PdfDocument pdfDocument = new PdfDocument();
```

```csharp
PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
{
  PageSize = PdfPageSize.A4,
  PageOrientation = PdfPageOrientation.Portrait,
  Margins = new PdfMargins(36, 36, 36, 36)
};

// Create a new PDF document with the specified settings
using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
```

### Add New PDF Pages

```csharp
// Add a new PDF page with current settings
pdfDocument.AddPage();
```

```csharp
// Set the next page to landscape A4
pdfDocument.SetPageSize(PdfPageSize.A4, PdfPageOrientation.Landscape);

// Set the next page margins
pdfDocument.Margins = new PdfMargins(100, 100, 100, 100);

// Add a new PDF page with the modified page settings
pdfDocument.AddPage();
```

### Save the PDF Document

```csharp
string outputPath = Path.Combine(outputDir, "GeneratedDocument.pdf");

// Save the document to disk
pdfDocument.SaveToFile(outputPath);
```

```csharp
// Save the document to a memory buffer
byte[] pdfBytes = pdfDocument.Save();
```

### Dispose the PdfDocument

```csharp
// Save will dispose the document automatically
byte[] buffer = pdfDocument.Save();
```

```csharp
PdfDocument pdfDocument = new PdfDocument();
byte[] buffer = pdfDocument.Save();
pdfDocument.Dispose();
```

### Code Sample - Create PDF Documents with Text and Images

```csharp
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
    public class Create_PDF_DocumentsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_DocumentsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set license key received after purchase to use the library in licensed mode
            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
            {
                PageSize = PdfPageSize.A4,
                PageOrientation = PdfPageOrientation.Portrait,
                Margins = new PdfMargins(36, 36, 36, 36)
            };

            // Create a new PDF document with the specified settings
            using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);

            const int xLeft = 0;
            const int ySeparator = 15;
            int crtYPos = 0;

            string imagesPath = GetDemoImagesPath();
            string fontsPath = GetDemoFontsPath();
            string textsPath = GetDemoTextsPath();

            // Each section title in this demo uses a different standard font so
            // the showcase exercises multiple font families/styles/colors in the
            // same document
            PdfFont fontHelveticaBoldUnderlineBlack = PdfFontManager.CreateStandardFont(
                PdfStandardFont.Helvetica, 16f, PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont fontCourierBoldItalicGreen = PdfFontManager.CreateStandardFont(
                PdfStandardFont.Courier, 16f, PdfFontStyle.Bold | PdfFontStyle.Italic, PdfColor.Green);
            PdfFont fontCourierBoldBlue = PdfFontManager.CreateStandardFont(
                PdfStandardFont.Courier, 16f, PdfFontStyle.Bold, PdfColor.Blue);
            PdfFont fontCourierNormalPurple = PdfFontManager.CreateStandardFont(
                PdfStandardFont.Courier, 16f, PdfFontStyle.Normal, PdfColor.Purple);

            // ===== Section 1: Transparent PNG with custom width =====
            PdfTextElement pdfTitle1 = new PdfTextElement(
                "Transparent PNG Image with Custom Width", fontHelveticaBoldUnderlineBlack)
            {
                X = xLeft,
                Y = crtYPos
            };
            crtYPos = (int)pdfDocument.AddText(pdfTitle1).LastPageRectangle.Bounds.Bottom + ySeparator;

            PdfImageElement pdfPngImage = new PdfImageElement(Path.Combine(imagesPath, "transparent.png"))
            {
                X = xLeft,
                Y = crtYPos,
                Width = 150
            };
            crtYPos = (int)pdfDocument.AddImage(pdfPngImage).BoundingBox.Bottom + ySeparator;

            // ===== Section 2: JPEG with custom height =====
            PdfTextElement pdfTitle2 = new PdfTextElement(
                "JPEG Image with Custom Height", fontCourierBoldItalicGreen)
            {
                X = xLeft,
                Y = crtYPos
            };
            crtYPos = (int)pdfDocument.AddText(pdfTitle2).LastPageRectangle.Bounds.Bottom + ySeparator;

            // Ensure there is enough vertical space on the current page for the image.
            // Add a new page and reset Y position if needed
            EnsureSpaceOnPage(ref crtYPos, 200, pdfDocument);

            PdfImageElement pdfJpgImage = new PdfImageElement(Path.Combine(imagesPath, "image.jpg"))
            {
                X = xLeft,
                Y = crtYPos,
                Height = 150
            };
            crtYPos = (int)pdfDocument.AddImage(pdfJpgImage).BoundingBox.Bottom + ySeparator;

            // ===== Section 3: Multi-page Unicode text with per-page blue border =====
            PdfTextElement pdfTitle3 = new PdfTextElement(
                "Multi Page Unicode Text with Custom Font", fontCourierBoldBlue)
            {
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center
            };
            crtYPos = (int)pdfDocument.AddText(pdfTitle3).LastPageRectangle.Bounds.Bottom + ySeparator;

            string alphabetFilePath = Path.Combine(textsPath, "Alphabet.txt");
            string alfabetString = System.IO.File.ReadAllText(alphabetFilePath);

            // Load the Unicode TrueType font used for the long alphabet body text
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
            PdfFont trueTypeFont = PdfFontManager.CreateFont(baseFont, 16f,
                PdfFontStyle.Normal, PdfColor.Black);

            // Long Unicode text using the TrueType font, allowing continuation on next pages.
            // The OnAfterPageRender callback draws a blue rectangle around the rendered area on
            // each page the text spans
            PdfTextElement pdfText1 = new PdfTextElement(alfabetString, trueTypeFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Left,
                ContinueOnNextPage = true
            };

            pdfText1.OnAfterPageRender = info =>
            {
                var bounds = info.RenderedRectangle.Bounds;
                PdfRectangleElement border = new PdfRectangleElement(bounds.X, bounds.Y,
                    bounds.Width, bounds.Height + 5)
                {
                    BorderColor = PdfColor.Blue,
                };
                pdfDocument.AddRectangle(border);
            };

            pdfDocument.AddText(pdfText1);

            // ===== Section 4: Same text on landscape pages, centered, per-page purple border =====
            pdfDocument.SetPageSize(PdfPageSize.A4, PdfPageOrientation.Landscape);
            pdfDocument.AddPage();
            crtYPos = 0;

            PdfTextElement pdfText2 = new PdfTextElement(alfabetString, trueTypeFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                ContinueOnNextPage = true
            };

            pdfText2.OnAfterPageRender = info =>
            {
                var bounds = info.RenderedRectangle.Bounds;
                PdfRectangleElement border = new PdfRectangleElement(bounds.X, bounds.Y,
                    bounds.Width, bounds.Height + 5)
                {
                    BorderColor = PdfColor.Purple,
                };
                pdfDocument.AddRectangle(border);
            };

            pdfDocument.AddText(pdfText2);

            // ===== Section 5: Right-to-left text =====
            pdfDocument.AddPage();
            crtYPos = 0;

            PdfTextElement rtlTitle = new PdfTextElement(
                "Add Right to Left Text", fontCourierNormalPurple)
            {
                X = xLeft,
                Y = crtYPos
            };
            crtYPos = (int)pdfDocument.AddText(rtlTitle).LastPageRectangle.Bounds.Bottom + ySeparator;

            string rtlFilePath = Path.Combine(textsPath, "RightToLeft.txt");
            string rtlString = System.IO.File.ReadAllText(rtlFilePath);

            string rtlFontFilePath = Path.Combine(fontsPath, "NotoSansArabic-Regular.ttf");
            PdfFont rtlTrueTypeFont = PdfFontManager.CreateFont(rtlFontFilePath, 16f,
                PdfFontStyle.Normal, PdfColor.Black);

            PdfTextElement pdfTextRtl = new PdfTextElement(rtlString, rtlTrueTypeFont)
            {
                X = xLeft,
                Y = crtYPos,
                Direction = PdfTextDirection.RightToLeft
            };
            pdfDocument.AddText(pdfTextRtl);

            // Save to memory buffer
            byte[] outPdfBuffer = pdfDocument.Save();

            // Send PDF to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfDocument.pdf";

            return fileResult;
        }

        private void EnsureSpaceOnPage(ref int crtYPos, int requestedHeight, PdfDocument pdfDocument)
        {
            if (crtYPos + requestedHeight > pdfDocument.ContentHeight)
            {
                pdfDocument.AddPage();
                crtYPos = 0;
            }
        }

        private string GetDemoFilesPath()
        {
            return m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        }

        private string GetDemoImagesPath()
        {
            return Path.Combine(GetDemoFilesPath(), "Image_Files");
        }

        private string GetDemoFontsPath()
        {
            return Path.Combine(GetDemoFilesPath(), "Font_Files");
        }

        private string GetDemoTextsPath()
        {
            return Path.Combine(GetDemoFilesPath(), "Text_Files");
        }
    }
}
```

## Create PDF Documents with Text

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-text.htm

### Create a PDF Font

```csharp
// Bold italic green Courier at 16 points
PdfFont fontCourier = PdfFontManager.CreateStandardFont(
    PdfStandardFont.Courier, 16f,
    PdfFontStyle.Bold | PdfFontStyle.Italic,
    PdfColor.Green);
```

```csharp
string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");

// Load the font file into a base font (cached by physical path)
PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

// Derive a styled PdfFont from the base font
PdfFont trueTypeFont = PdfFontManager.CreateFont(
    baseFont, 16f,
    PdfFontStyle.Normal, PdfColor.Black);
```

### Create the Text Element

```csharp
PdfTextElement pdfText = new PdfTextElement("The text element string", fontCourier)
{
    X = crtXPos,
    Y = crtYPos
};
```

### Background Highlight

```csharp
PdfTextElement highlighted = new PdfTextElement(text, bodyFont)
{
    X = 0, Y = crtYPos, Width = pdfDocument.ContentWidth,
    BackgroundColor = PdfColor.Yellow,
    BackgroundOpacity = 0.4f
};
pdfDocument.AddText(highlighted);
```

### Render the Text Element

```csharp
PdfTextRenderInfo textRenderInfo = pdfDocument.AddText(pdfText);

// Use the rendered geometry to position the next element
crtYPos = (int)textRenderInfo.LastPageRectangle.Bounds.Bottom + ySeparator;
```

### Continue Rendering on the Next Page

```csharp
PdfTextElement pdfText1 = new PdfTextElement(alfabetString, trueTypeFont)
{
    X = crtXPos,
    Y = crtYPos,
    Alignment = PdfTextAlignment.Left,
    ContinueOnNextPage = true
};

// Draw a blue border around the text rendered on each page
pdfText1.OnAfterPageRender = info =>
{
    var bounds = info.RenderedRectangle.Bounds;
    PdfRectangleElement border = new PdfRectangleElement(
        bounds.X, bounds.Y,
        bounds.Width, bounds.Height + 5)
    {
        BorderColor = PdfColor.Blue,
    };
    pdfDocument.AddRectangle(border);
};

pdfDocument.AddText(pdfText1);
```

### Text Direction

```csharp
string rtlFontFilePath = Path.Combine(fontsPath, "NotoSansArabic-Regular.ttf");

// Two-step font creation also works for one-off use
PdfBaseFont rtlBaseFont = PdfFontManager.CreateBaseFont(rtlFontFilePath);
PdfFont rtlTrueTypeFont = PdfFontManager.CreateFont(
    rtlBaseFont, 16f,
    PdfFontStyle.Normal, PdfColor.Black);

string rtlString = System.IO.File.ReadAllText(
    Path.Combine(textsPath, "RightToLeft.txt"));

PdfTextElement pdfTextRtl = new PdfTextElement(rtlString, rtlTrueTypeFont)
{
    X = crtXPos,
    Y = crtYPos,
    Direction = PdfTextDirection.RightToLeft
};
pdfDocument.AddText(pdfTextRtl);
```

### Code Sample - Create PDF Documents with Text Elements

```csharp
using System.IO;
using System.Text;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Creator;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Creator
{
    public class Create_PDF_Documents_with_TextController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_TextController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_with_Text_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_Text_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set license key received after purchase to use the library in licensed mode
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
            pdfDocument.PdfDocumentInfo.Title = "PDF Text Demo";

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

            const int xLeft = 0;
            const int ySeparator = 10;
            int crtYPos = 0;

            // ===== Section 1: Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Text Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = pdfDocument.ContentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            PdfTextRenderInfo titleInfo = pdfDocument.AddText(titleElement);
            crtYPos = (int)titleInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 2: BackgroundColor + BackgroundOpacity =====
            PdfTextElement sectionLabel1 = new PdfTextElement(
                "1. BackgroundColor + BackgroundOpacity", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel1.Accessibility.StructureType = PdfStructureType.Heading2;
            crtYPos = (int)pdfDocument.AddText(sectionLabel1).LastPageRectangle.Bounds.Bottom + ySeparator;

            PdfTextElement highlighted = new PdfTextElement(
                "This paragraph has a yellow highlight background drawn behind the text as an " +
                "Artifact (it does not appear in the structure tree). The background covers the " +
                "full column width and the actual used height.",
                bodyFont)
            {
                X = xLeft,
                Y = crtYPos,
                Width = pdfDocument.ContentWidth,
                BackgroundColor = PdfColor.Yellow,
                BackgroundOpacity = 0.4f
            };
            crtYPos = (int)pdfDocument.AddText(highlighted).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 3: OnBeforePageRender + OnAfterPageRender =====
            PdfTextElement sectionLabel2 = new PdfTextElement(
                "2. OnBeforePageRender (under) + OnAfterPageRender (over)", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel2.Accessibility.StructureType = PdfStructureType.Heading2;
            crtYPos = (int)pdfDocument.AddText(sectionLabel2).LastPageRectangle.Bounds.Bottom + ySeparator;

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
                Width = pdfDocument.ContentWidth
            };

            // Under-layer painted before the text is drawn
            decoratedText.OnBeforePageRender = preInfo =>
            {
                var col = preInfo.RenderedRectangle.Bounds;
                pdfDocument.AddRectangle(new PdfRectangleElement(
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
                pdfDocument.AddRectangle(new PdfRectangleElement(
                    rect.X - 2, rect.Y - 2, rect.Width + 4, rect.Height + 4)
                {
                    FillColor = null,
                    BorderColor = PdfColor.Blue,
                    Border = new PdfLineStyle { LineWidth = 1f }
                });
            };

            crtYPos = (int)pdfDocument.AddText(decoratedText).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 4: Rotated text + QuadPoints outline =====
            EnsureSpaceOnPage(ref crtYPos, 180, pdfDocument);

            PdfTextElement sectionLabel3 = new PdfTextElement(
                "3. Rotated text with QuadPoints-based outline", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel3.Accessibility.StructureType = PdfStructureType.Heading2;
            crtYPos = (int)pdfDocument.AddText(sectionLabel3).LastPageRectangle.Bounds.Bottom + ySeparator;

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
                pdfDocument.AddPolygon(new PdfPolygonElement(
                    quad.TopLeft, quad.TopRight, quad.BottomRight, quad.BottomLeft)
                {
                    FillColor = null,
                    BorderColor = PdfColor.Red,
                    Border = new PdfLineStyle { LineWidth = 1.2f, DashStyle = PdfLineDashStyle.Dashed }
                });
            };

            PdfTextRenderInfo rotatedInfo = pdfDocument.AddText(rotated);
            // Advance Y past the rotated area's axis-aligned bounding box.
            crtYPos = (int)rotatedInfo.LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 5: Multi-page continuation + per-page RenderedText =====
            pdfDocument.AddPage();
            crtYPos = 0;

            PdfTextElement sectionLabel4 = new PdfTextElement(
                "4. Multi-page continuation + per-page RenderedText", sectionFont)
            { X = xLeft, Y = crtYPos };
            sectionLabel4.Accessibility.StructureType = PdfStructureType.Heading2;
            crtYPos = (int)pdfDocument.AddText(sectionLabel4).LastPageRectangle.Bounds.Bottom + ySeparator;

            // Build a long text that will overflow several pages.
            string longTextSource = LoadAlphabetText();
            StringBuilder longBuilder = new StringBuilder();
            for (int i = 0; i < 8; i++) longBuilder.AppendLine(longTextSource);
            string longText = longBuilder.ToString();

            PdfTextElement multipage = new PdfTextElement(longText, bodyFont)
            {
                X = xLeft,
                Y = crtYPos,
                Width = pdfDocument.ContentWidth,
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
                pdfDocument.AddText(badgeText);
            };

            PdfTextRenderInfo multipageInfo = pdfDocument.AddText(multipage);

            // ===== Section 6: Summary of pages rendered =====
            pdfDocument.AddPage();
            crtYPos = 0;

            PdfTextElement summaryLabel = new PdfTextElement(
                "5. Summary: text rendered per page (from Pages list)", sectionFont)
            { X = xLeft, Y = crtYPos };
            summaryLabel.Accessibility.StructureType = PdfStructureType.Heading2;
            crtYPos = (int)pdfDocument.AddText(summaryLabel).LastPageRectangle.Bounds.Bottom + ySeparator;

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
                    Width = pdfDocument.ContentWidth
                };
                crtYPos = (int)pdfDocument.AddText(entryElement).LastPageRectangle.Bounds.Bottom + 4;

                EnsureSpaceOnPage(ref crtYPos, 30, pdfDocument);
            }

            byte[] outPdfBuffer = pdfDocument.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfTextDemo.pdf";
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

## Create PDF Documents with Images

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-images.htm

### Create the Image Element

```csharp
PdfImageElement pdfImage = new PdfImageElement(imagePath)
{
    X = crtXPos,
    Y = crtYPos
};
```

### Positioning and Scaling

```csharp
string imagesPath = GetDemoImagesPath();

// Add a transparent PNG image with a custom width
PdfImageElement pdfPngImage = new PdfImageElement(
    Path.Combine(imagesPath, "transparent.png"))
{
    X = crtXPos,
    Y = crtYPos,
    Width = 150
};
```

### Render the Image Element

```csharp
PdfImageRenderInfo imageRenderInfo = pdfDocument.AddImage(pdfPngImage);

// Use the rendered geometry to position the next element
crtYPos = imageRenderInfo.BoundingBox.Bottom + ySeparator;
```

### Code Sample - Create PDF Documents with Images

```csharp
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
    public class Create_PDF_Documents_with_ImagesController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_ImagesController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_with_Images_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_Images_ViewModel model)
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
            pdfDocument.PdfDocumentInfo.Title = "PDF Image Demo";

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont smallFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);

            const int xLeft = 0;
            const int ySeparator = 10;
            int crtYPos = 0;

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
                Width = pdfDocument.ContentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            crtYPos = (int)pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 1: PNG with custom width, ScaleDownToFit =====
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
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

            PdfImageRenderInfo pngInfo = pdfDocument.AddImage(pngImage);
            crtYPos = (int)pngInfo.BoundingBox.Bottom + ySeparator * 2;

            // ===== Section 2: JPEG with alignment =====
            EnsureSpaceOnPage(ref crtYPos, 220, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "2. JPEG aligned Center horizontally", xLeft, crtYPos, ySeparator);

            PdfImageElement jpgCentered = new PdfImageElement(jpegPath)
            {
                Y = crtYPos,
                Height = 120,
                HorizontalAlign = PdfElementHorizontalAlign.Center
            };
            jpgCentered.Accessibility.AlternateText = "Wooden house on a river";

            PdfImageRenderInfo jpgInfo = pdfDocument.AddImage(jpgCentered);
            crtYPos = (int)jpgInfo.BoundingBox.Bottom + ySeparator * 2;

            // ===== Section 3: Rotated image with QuadPoints-based outline =====
            EnsureSpaceOnPage(ref crtYPos, 280, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
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

            PdfImageRenderInfo rotatedInfo = pdfDocument.AddImage(rotatedJpg);

            // Outline the axis-aligned Bounds (red).
            var aabb = rotatedInfo.BoundingBox;
            pdfDocument.AddRectangle(new PdfRectangleElement(aabb.X, aabb.Y, aabb.Width, aabb.Height)
            {
                FillColor = null,
                BorderColor = PdfColor.Red,
                Border = new PdfLineStyle { LineWidth = 1f, DashStyle = PdfLineDashStyle.Dotted }
            });

            // Outline the rotated QuadPoints (blue) using a polygon through the four corners.
            var q = rotatedInfo.QuadPoints;
            pdfDocument.AddPolygon(new PdfPolygonElement(q.TopLeft, q.TopRight, q.BottomRight, q.BottomLeft)
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
                Width = pdfDocument.ContentWidth
            };
            visibleLabel.Accessibility.StructureType = PdfStructureType.Artifact;
            crtYPos = (int)pdfDocument.AddText(visibleLabel).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 4: Different rotation pivots =====
            pdfDocument.AddPage();
            crtYPos = 0;
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
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
                pdfDocument.AddRectangle(new PdfRectangleElement(cellX, cellY, cellW - 10, cellH - 10)
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
                PdfImageRenderInfo info = pdfDocument.AddImage(img);

                // Trace the rotated quad.
                var rq = info.QuadPoints;
                pdfDocument.AddPolygon(new PdfPolygonElement(rq.TopLeft, rq.TopRight, rq.BottomRight, rq.BottomLeft)
                {
                    FillColor = null,
                    BorderColor = PdfColor.Blue,
                    Border = new PdfLineStyle { LineWidth = 1f }
                });

                // Label.
                PdfTextElement pivotLabel = new PdfTextElement(pivots[i].ToString(), smallFont)
                { X = cellX + 4, Y = cellY + 2, Width = cellW - 14 };
                pivotLabel.Accessibility.StructureType = PdfStructureType.Artifact;
                pdfDocument.AddText(pivotLabel);
            }

            byte[] outPdfBuffer = pdfDocument.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfImageDemo.pdf";
            return fileResult;
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

## Create PDF Documents with Shapes

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-shapes.htm

### Code Sample - Create PDF Documents with Geometric Shapes

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
    public class Create_PDF_Documents_with_ShapesController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_ShapesController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_with_Shapes_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_Shapes_ViewModel model)
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
            pdfDocument.PdfDocumentInfo.Title = "PDF Geometric Shapes Demo";

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);

            const int xLeft = 0;
            const int ySeparator = 10;
            int crtYPos = 0;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "Geometric Shape Elements Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = pdfDocument.ContentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            crtYPos = (int)pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 1: PdfRectangleElement =====
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "1. PdfRectangleElement (basic, dashed, rotated)", xLeft, crtYPos, ySeparator);

            // Filled basic
            var basicInfo = pdfDocument.AddRectangle(new PdfRectangleElement(xLeft, crtYPos, 100, 60)
            {
                FillColor = PdfColor.LightSkyBlue,
                BorderColor = PdfColor.SteelBlue,
                Border = new PdfLineStyle { LineWidth = 1.5f }
            });
            int basicCaptionBottom = AddCaption(pdfDocument, labelFont,
                "filled + border", xLeft,
                (int)basicInfo.LastPageRectangle.Bounds.Bottom + 5, 100);

            // Dashed
            var dashedInfo = pdfDocument.AddRectangle(new PdfRectangleElement(xLeft + 120, crtYPos, 100, 60)
            {
                FillColor = null,
                BorderColor = PdfColor.DarkOrange,
                Border = new PdfLineStyle
                {
                    LineWidth = 1.5f,
                    DashStyle = PdfLineDashStyle.Dashed
                }
            });
            int dashedCaptionBottom = AddCaption(pdfDocument, labelFont,
                "dashed outline", xLeft + 120,
                (int)dashedInfo.LastPageRectangle.Bounds.Bottom + 5, 100);

            // Rotated 30 degrees
            var rotatedInfo = pdfDocument.AddRectangle(new PdfRectangleElement(xLeft + 320, crtYPos + 5, 100, 60)
            {
                FillColor = PdfColor.LightPink,
                FillOpacity = 0.6f,
                BorderColor = PdfColor.Crimson,
                Border = new PdfLineStyle { LineWidth = 1.2f },
                RotationDegrees = 30,
                RotationPivot = PdfRotationPivot.TopLeft
            });
            int rotatedCaptionBottom = AddCaption(pdfDocument, labelFont,
                "rotated 30 deg (TopLeft pivot)", xLeft + 320,
                (int)rotatedInfo.LastPageRectangle.Bounds.Bottom + 5, 150);

            // Advance past the deepest caption in this row
            crtYPos = Math.Max(
                Math.Max(basicCaptionBottom, dashedCaptionBottom),
                rotatedCaptionBottom) + ySeparator;

            // ===== Section 2: PdfRoundedRectangleElement =====
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "2. PdfRoundedRectangleElement (varying corner radius)", xLeft, crtYPos, ySeparator);

            float[] radii = { 4f, 12f, 24f, 30f };
            int section2MaxBottom = crtYPos;
            for (int i = 0; i < radii.Length; i++)
            {
                int boxX = xLeft + i * 130;
                var rrInfo = pdfDocument.AddRoundedRectangle(new PdfRoundedRectangleElement(boxX, crtYPos, 110, 60, radii[i])
                {
                    FillColor = PdfColor.LightGreen,
                    FillOpacity = 0.5f,
                    BorderColor = PdfColor.DarkGreen,
                    Border = new PdfLineStyle { LineWidth = 1f }
                });
                int capBottom = AddCaption(pdfDocument, labelFont, $"radius={radii[i]}", boxX,
                    (int)rrInfo.LastPageRectangle.Bounds.Bottom + 5, 110);
                section2MaxBottom = Math.Max(section2MaxBottom, capBottom);
            }
            crtYPos = section2MaxBottom + ySeparator;

            // ===== Section 3: PdfLineElement (LineCap variations) =====
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "3. PdfLineElement (LineCap: Butt, Round, ProjectingSquare)", xLeft, crtYPos, ySeparator);

            PdfLineCapStyle[] caps = { PdfLineCapStyle.Butt, PdfLineCapStyle.Round, PdfLineCapStyle.ProjectingSquare };
            string[] capLabels = { "Butt", "Round", "ProjectingSquare" };
            int section3RowTop = crtYPos;
            for (int i = 0; i < caps.Length; i++)
            {
                int lineY = section3RowTop + 8;
                var lineInfo = pdfDocument.AddLine(new PdfLineElement(xLeft + 60, lineY, xLeft + 260, lineY)
                {
                    LineColor = PdfColor.DarkBlue,
                    LineStyle = new PdfLineStyle
                    {
                        LineWidth = 8f,
                        LineCap = caps[i]
                    }
                });
                // Thin reference line to highlight the cap extension past the endpoints
                pdfDocument.AddLine(new PdfLineElement(xLeft + 60, lineY, xLeft + 260, lineY)
                {
                    LineColor = PdfColor.White,
                    LineStyle = new PdfLineStyle { LineWidth = 0.5f }
                });
                int capBottom = AddCaption(pdfDocument, labelFont, capLabels[i], xLeft + 270, lineY - 5, 150);
                int lineBottom = (int)lineInfo.LastPageRectangle.Bounds.Bottom;
                section3RowTop = Math.Max(lineBottom, capBottom) + 5;
            }
            crtYPos = section3RowTop + ySeparator;

            // Dashed line using CustomDashPattern
            var dashLineInfo = pdfDocument.AddLine(new PdfLineElement(xLeft, crtYPos, xLeft + 380, crtYPos)
            {
                LineColor = PdfColor.DarkRed,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 2f,
                    CustomDashPattern = new float[] { 10f, 4f, 2f, 4f },
                    DashPhase = 0f
                }
            });
            int dashCapBottom = AddCaption(pdfDocument, labelFont, "CustomDashPattern [10, 4, 2, 4]",
                xLeft, (int)dashLineInfo.LastPageRectangle.Bounds.Bottom + 5, 250);
            crtYPos = dashCapBottom + ySeparator;

            // ===== Section 4: PdfCircleElement =====
            EnsureSpaceOnPage(ref crtYPos, 160, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "4. PdfCircleElement (filled, stroked, opacity)", xLeft, crtYPos, ySeparator);

            var circle1Info = pdfDocument.AddCircle(new PdfCircleElement(xLeft + 40, crtYPos + 40, 30)
            {
                FillColor = PdfColor.Tomato,
                BorderColor = PdfColor.DarkRed,
                Border = new PdfLineStyle { LineWidth = 1.5f }
            });
            int circle1CapBottom = AddCaption(pdfDocument, labelFont, "filled + border", xLeft + 10,
                (int)circle1Info.LastPageRectangle.Bounds.Bottom + 5, 90);

            var circle2Info = pdfDocument.AddCircle(new PdfCircleElement(xLeft + 140, crtYPos + 40, 30)
            {
                FillColor = null,
                BorderColor = PdfColor.DarkBlue,
                Border = new PdfLineStyle { LineWidth = 2f }
            });
            int circle2CapBottom = AddCaption(pdfDocument, labelFont, "stroke only", xLeft + 110,
                (int)circle2Info.LastPageRectangle.Bounds.Bottom + 5, 90);

            var circle3Info = pdfDocument.AddCircle(new PdfCircleElement(xLeft + 240, crtYPos + 40, 30)
            {
                FillColor = PdfColor.Purple,
                FillOpacity = 0.4f,
                BorderColor = PdfColor.Purple,
                BorderOpacity = 0.7f,
                Border = new PdfLineStyle { LineWidth = 1f }
            });
            int circle3CapBottom = AddCaption(pdfDocument, labelFont, "translucent", xLeft + 210,
                (int)circle3Info.LastPageRectangle.Bounds.Bottom + 5, 90);

            crtYPos = Math.Max(Math.Max(circle1CapBottom, circle2CapBottom), circle3CapBottom) + ySeparator;

            // ===== Section 5: PdfEllipseElement (rotation, axis-aligned vs QuadPoints) =====
            EnsureSpaceOnPage(ref crtYPos, 180, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "5. PdfEllipseElement rotation - Bounds (axis-aligned) vs QuadPoints (rotated)", xLeft, crtYPos, ySeparator);

            // Axis-aligned ellipse
            var axisEllipseInfo = pdfDocument.AddEllipse(new PdfEllipseElement(xLeft, crtYPos + 10, 120, 70)
            {
                FillColor = PdfColor.LightYellow,
                BorderColor = PdfColor.GoldenRod,
                Border = new PdfLineStyle { LineWidth = 1f }
            });
            int axisCapBottom = AddCaption(pdfDocument, labelFont, "axis-aligned", xLeft,
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
            var rotEllipseInfo = pdfDocument.AddEllipse(rotEllipse);

            // axis-aligned bounding box of the rotated ellipse (red dashed)
            var eb = rotEllipseInfo.LastPageRectangle.Bounds;
            pdfDocument.AddRectangle(new PdfRectangleElement(eb.X, eb.Y, eb.Width, eb.Height)
            {
                FillColor = null,
                BorderColor = PdfColor.Red,
                Border = new PdfLineStyle { LineWidth = 0.5f, DashStyle = PdfLineDashStyle.Dotted }
            });
            int rotCapBottom = AddCaption(pdfDocument, labelFont, "rotated 35 deg (red axis-aligned bounding box)",
                xLeft + 150, (int)eb.Bottom + 5, 250);

            crtYPos = Math.Max(axisCapBottom, rotCapBottom) + ySeparator;

            // ===== Section 6: PdfArcElement (3 closure types) =====
            EnsureSpaceOnPage(ref crtYPos, 180, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "6. PdfArcElement (Open, Chord, Pie closures)", xLeft, crtYPos, ySeparator);

            PdfArcClosureType[] closures = { PdfArcClosureType.Open, PdfArcClosureType.Chord, PdfArcClosureType.Pie };
            string[] closureLabels = { "Open (stroke only)", "Chord", "Pie" };
            int arcMaxBottom = crtYPos;
            for (int i = 0; i < closures.Length; i++)
            {
                int arcX = xLeft + i * 170;
                var arcInfo = pdfDocument.AddArc(new PdfArcElement(arcX, crtYPos, 130, 90,
                    startAngleDegrees: 20,
                    sweepAngleDegrees: 200)
                {
                    Closure = closures[i],
                    FillColor = PdfColor.LightSkyBlue,
                    FillOpacity = 0.4f,
                    LineColor = PdfColor.MediumBlue,
                    LineStyle = new PdfLineStyle { LineWidth = 1.5f }
                });
                int capBottom = AddCaption(pdfDocument, labelFont, closureLabels[i], arcX,
                    (int)arcInfo.LastPageRectangle.Bounds.Bottom + 5, 140);
                arcMaxBottom = Math.Max(arcMaxBottom, capBottom);
            }
            crtYPos = arcMaxBottom + ySeparator;

            byte[] outPdfBuffer = pdfDocument.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfShapesDemo.pdf";
            return fileResult;
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

## Create PDF Documents with Polylines, Polygons and Paths

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-paths-and-polygons.htm

### Arbitrary Paths

```csharp
PdfPathElement heart = new PdfPathElement
{
    FillColor = PdfColor.Crimson,
    FillOpacity = 0.8f,
    LineColor = PdfColor.DarkRed,
    LineStyle = new PdfLineStyle { LineWidth = 1.5f, LineJoin = PdfLineJoinStyle.Round }
};
heart
    .MoveTo(cx, cy + s * 0.25f)
    .CurveTo(cx - s * 0.55f, cy + s * 0.55f,
             cx - s * 1.10f, cy - s * 0.10f,
             cx, cy - s * 0.35f)
    .CurveTo(cx + s * 1.10f, cy - s * 0.10f,
             cx + s * 0.55f, cy + s * 0.55f,
             cx, cy + s * 0.25f)
    .Close();
pdfDocument.AddPath(heart);
```

### Code Sample - Create PDF Documents with Paths and Polygons

```csharp
using System;
using System.IO;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Creator;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Creator
{
    public class Create_PDF_Documents_with_Paths_and_PolygonsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_Paths_and_PolygonsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_with_Paths_and_Polygons_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_Paths_and_Polygons_ViewModel model)
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
            pdfDocument.PdfDocumentInfo.Title = "PDF Polyline, Polygon and Path Demo";

            string fontsPath = GetDemoFontsPath();
            string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f,
                PdfFontStyle.Bold | PdfFontStyle.Underline, PdfColor.Black);
            PdfFont sectionFont = PdfFontManager.CreateFont(baseFont, 14f,
                PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 9f,
                PdfFontStyle.Normal, PdfColor.DarkGray);

            const int xLeft = 0;
            const int ySeparator = 10;
            int crtYPos = 0;

            // ===== Title =====
            PdfTextElement titleElement = new PdfTextElement(
                "PDF Polyline, Polygon and Path Demo", titleFont)
            {
                X = xLeft,
                Y = crtYPos,
                Alignment = PdfTextAlignment.Center,
                Width = pdfDocument.ContentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            crtYPos = (int)pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + ySeparator * 2;

            // ===== Section 1: PdfPolylineElement (open, with caps) =====
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "1. PdfPolylineElement (zigzag, round joins)", xLeft, crtYPos, ySeparator);

            List<PdfPointF> zigzag = new List<PdfPointF>();
            for (int i = 0; i <= 12; i++)
            {
                float x = xLeft + i * 30;
                float y = crtYPos + (i % 2 == 0 ? 0 : 40);
                zigzag.Add(new PdfPointF(x, y));
            }

            var zigzagInfo = pdfDocument.AddPolyline(new PdfPolylineElement(zigzag)
            {
                LineColor = PdfColor.DarkOrange,
                LineStyle = new PdfLineStyle
                {
                    LineWidth = 3f,
                    LineCap = PdfLineCapStyle.Round,
                    LineJoin = PdfLineJoinStyle.Round
                }
            });
            int zigzagCapBottom = AddCaption(pdfDocument, labelFont,
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
            var zigzag2Info = pdfDocument.AddPolyline(new PdfPolylineElement(zigzag2)
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
            int zigzag2CapBottom = AddCaption(pdfDocument, labelFont,
                "Same zigzag, dashed, rotated 6 deg clockwise",
                xLeft, (int)zigzag2Info.LastPageRectangle.Bounds.Bottom + 5, 400);
            crtYPos = zigzag2CapBottom + ySeparator * 2;

            // ===== Section 2: PdfPolygonElement (triangle, hexagon, star) =====
            EnsureSpaceOnPage(ref crtYPos, 200, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                "2. PdfPolygonElement (triangle, regular hexagon, 5-point star)",
                xLeft, crtYPos, ySeparator);

            // Triangle
            var triangleInfo = pdfDocument.AddPolygon(new PdfPolygonElement(new PdfPointF(xLeft + 60, crtYPos),
                new PdfPointF(xLeft + 110, crtYPos + 90),
                new PdfPointF(xLeft + 10, crtYPos + 90))
            {
                FillColor = PdfColor.LightSalmon,
                FillOpacity = 0.7f,
                BorderColor = PdfColor.DarkRed,
                Border = new PdfLineStyle { LineWidth = 1.5f }
            });
            int triangleCapBottom = AddCaption(pdfDocument, labelFont, "triangle (filled)", xLeft + 10,
                (int)triangleInfo.LastPageRectangle.Bounds.Bottom + 5, 110);

            // Regular hexagon
            var hexagonInfo = pdfDocument.AddPolygon(new PdfPolygonElement(
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
            int hexagonCapBottom = AddCaption(pdfDocument, labelFont, "regular hexagon", xLeft + 150,
                (int)hexagonInfo.LastPageRectangle.Bounds.Bottom + 5, 110);

            // 5-point star
            var starInfo = pdfDocument.AddPolygon(new PdfPolygonElement(
                BuildStar(xLeft + 340, crtYPos + 45, outerRadius: 50, innerRadius: 22, points: 5))
            {
                FillColor = PdfColor.Gold,
                FillOpacity = 0.85f,
                BorderColor = PdfColor.DarkGoldenRod,
                Border = new PdfLineStyle { LineWidth = 1.2f, LineJoin = PdfLineJoinStyle.Miter, MiterLimit = 4f }
            });
            int starCapBottom = AddCaption(pdfDocument, labelFont, "5-point star", xLeft + 295,
                (int)starInfo.LastPageRectangle.Bounds.Bottom + 5, 110);

            crtYPos = Math.Max(Math.Max(triangleCapBottom, hexagonCapBottom), starCapBottom) + ySeparator * 2;

            // ===== Section 3: PdfPathElement (heart via fluent API) =====
            EnsureSpaceOnPage(ref crtYPos, 200, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
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

            var heartInfo = pdfDocument.AddPath(heart);
            int heartCapBottom = AddCaption(pdfDocument, labelFont,
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

            var waveInfo = pdfDocument.AddPath(wave);
            int waveCapBottom = AddCaption(pdfDocument, labelFont,
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
            var triangleInfo3 = pdfDocument.AddPath(staticTriangle);
            int triangle3CapBottom = AddCaption(pdfDocument, labelFont,
                "static triangle (params PdfPathOperation[])",
                (int)(triX - 10), (int)triangleInfo3.LastPageRectangle.Bounds.Bottom + 5, 110);

            crtYPos = Math.Max(Math.Max(heartCapBottom, waveCapBottom), triangle3CapBottom) + ySeparator * 2;

            // ===== Section 4: Path with rotation =====
            EnsureSpaceOnPage(ref crtYPos, 220, pdfDocument);
            crtYPos = AddSectionLabel(pdfDocument, sectionFont,
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

                var arrowInfo = pdfDocument.AddPath(arrow);
                int capBottom = AddCaption(pdfDocument, labelFont, $"{angles[i]} deg", cellX,
                    (int)arrowInfo.LastPageRectangle.Bounds.Bottom + 5, 80);
                arrowMaxBottom = Math.Max(arrowMaxBottom, capBottom);
            }
            crtYPos = arrowMaxBottom + ySeparator;

            byte[] outPdfBuffer = pdfDocument.Save();
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PdfPathsPolygonsDemo.pdf";
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
