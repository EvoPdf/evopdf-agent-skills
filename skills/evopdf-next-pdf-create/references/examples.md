# Creating PDF documents from code: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Create PDF Documents

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents.htm

### Create a PDF Document with Default Settings

```csharp
// Create a new PDF document with the default settings
using PdfDocument pdfDocument = new PdfDocument();
```

### Create a PDF Document with Custom Settings

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

### Add a New PDF Page

```csharp
// Add a new PDF page with current settings
pdfDocument.AddPage();
```

### Add a New PDF Page with Updated Settings

```csharp
// The margins of the pages added next
pdfDocument.Margins = new PdfMargins(100, 100, 100, 100);

// A landscape A4 page; the pages added next keep this size
pdfDocument.AddPage(PdfPageSize.A4, PdfPageOrientation.Landscape);
```

### Save the PDF Document to a File

```csharp
string outputPath = Path.Combine(outputDir, "GeneratedDocument.pdf");

// Save the document to disk
pdfDocument.SaveToFile(outputPath);
```

### Save the PDF Document to Memory

```csharp
// Save the document to a memory buffer
byte[] pdfBytes = pdfDocument.Save();
```

### Asynchronously Save the PDF Document to a File

```csharp
await pdfDocument.SaveToFileAsync(outputPath);
```

### Asynchronously Save the PDF Document to Memory

```csharp
byte[] pdfBytes = await pdfDocument.SaveAsync();
```

### Save and Dispose Automatically

```csharp
// Save will dispose the document automatically
byte[] buffer = pdfDocument.Save();
```

### Manual Dispose

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
            pdfDocument.AddPage(PdfPageSize.A4, PdfPageOrientation.Landscape);
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

### Create a standard PDF font

```csharp
// Bold italic green Courier at 16 points
PdfFont fontCourier = PdfFontManager.CreateStandardFont(
    PdfStandardFont.Courier, 16f,
    PdfFontStyle.Bold | PdfFontStyle.Italic,
    PdfColor.Green);
```

### Create a Unicode TrueType PDF font (two-step)

```csharp
string fontFilePath = Path.Combine(fontsPath, "DejaVuSerif.ttf");

// Load the font file into a base font (cached by physical path)
PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);

// Derive a styled PdfFont from the base font
PdfFont trueTypeFont = PdfFontManager.CreateFont(
    baseFont, 16f,
    PdfFontStyle.Normal, PdfColor.Black);
```

### Create a text element at a specific position

```csharp
PdfTextElement pdfText = new PdfTextElement("The text element string", fontCourier)
{
    X = crtXPos,
    Y = crtYPos
};
```

### Yellow highlight band

```csharp
PdfTextElement highlighted = new PdfTextElement(text, bodyFont)
{
    X = 0, Y = crtYPos, Width = pdfDocument.ContentWidth,
    BackgroundColor = PdfColor.Yellow,
    BackgroundOpacity = 0.4f
};
pdfDocument.AddText(highlighted);
```

### Render text and update the layout cursor

```csharp
PdfTextRenderInfo textRenderInfo = pdfDocument.AddText(pdfText);

// Use the rendered geometry to position the next element
crtYPos = (int)textRenderInfo.LastPageRectangle.Bounds.Bottom + ySeparator;
```

### Continue text on the next page and draw a border per page

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

### Render right-to-left Arabic text

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

### Set the Line Spacing and Add the Page Numbers

```csharp
PdfTextElement multipage = new PdfTextElement(longText, bodyFont)
{
    X = 0,
    Y = crtYPos,
    Width = pdfDocument.ContentWidth,
    ContinueOnNextPage = true,
    Leading = 1.3f,
    OnPageRendered = (pageNumber, bounds) => Console.WriteLine("page {0}: {1}", pageNumber, bounds)
};
pdfDocument.AddText(multipage);

// The page numbers in a template repeated in all the pages
PdfTemplate footerTemplate = pdfDocument.AddPdfTemplate(0, pdfDocument.PageSize.Height - 30, pdfDocument.PageSize.Width, 20);
footerTemplate.AddText(new PdfTextElement("Page {page_number} of {total_pages}", smallFont)
{
    X = 0,
    Y = 0,
    Width = pdfDocument.PageSize.Width,
    Alignment = PdfTextAlignment.Center,
    PageNumberOffset = 0,
    TotalPagesOffset = 0
});
```

### Embed the Font

```csharp
PdfBaseFont subsetFont = PdfFontManager.CreateBaseFont(fontFilePath);
PdfBaseFont fullFont = PdfFontManager.CreateBaseFont(fontFilePath, false);
PdfBaseFont latinFont = PdfFontManager.CreateBaseFont(fontFilePath, false, new[] { PdfUnicodeRanges.BasicLatin });
```

### Code Sample - Create PDF Documents with Text Elements

```csharp
using System.IO;
using System.Collections.Generic;
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
                "This paragraph has a yellow highlight background drawn behind the text. The background " +
                "covers the full column width and the height used by the text.",
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
                "Both callbacks receive the same PdfTextPageRenderInfo type: the rectangle in " +
                "OnBeforePageRender is the area the text is about to use and in OnAfterPageRender " +
                "it is the area the text used.",
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
                BackgroundOpacity = 1f,
                // The line spacing as a multiple of the font size; 0 uses the default of 1.2
                Leading = model.Leading
            };

            // The pages and the rectangles reported by the OnPageRendered callback after each page is rendered
            List<string> renderedPages = new List<string>();
            if (model.ReportRenderedPages)
            {
                multipage.OnPageRendered = (pageNumber, bounds) =>
                    renderedPages.Add(string.Format("page {0}: ({1:F0}, {2:F0}, {3:F0}x{4:F0})", pageNumber, bounds.X, bounds.Y, bounds.Width, bounds.Height));
            }

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

            if (model.ReportRenderedPages)
            {
                crtYPos += ySeparator;
                PdfTextElement renderedPagesElement = new PdfTextElement(
                    "Pages reported by OnPageRendered for the multi-page text: " + string.Join("; ", renderedPages), smallFont)
                {
                    X = xLeft,
                    Y = crtYPos,
                    Width = pdfDocument.ContentWidth
                };
                pdfDocument.AddText(renderedPagesElement);
            }

            if (model.AddPageNumbers)
            {
                // A template repeated in all the pages with a text element whose {page_number} and {total_pages}
                // placeholders are replaced in each page, with the given offsets added to the numbers
                PdfTemplate footerTemplate = pdfDocument.AddPdfTemplate(0, pdfDocument.PageSize.Height - 30, pdfDocument.PageSize.Width, 20);
                PdfTextElement pageNumbersElement = new PdfTextElement("Page {page_number} of {total_pages}", smallFont)
                {
                    X = 0,
                    Y = 0,
                    Width = pdfDocument.PageSize.Width,
                    Alignment = PdfTextAlignment.Center,
                    PageNumberOffset = model.PageNumberOffset,
                    TotalPagesOffset = model.TotalPagesOffset
                };
                pageNumbersElement.Accessibility.StructureType = PdfStructureType.Artifact;
                footerTemplate.AddText(pageNumbersElement);
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
            return System.IO.File.ReadAllText(alphabetFilePath);
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

### Create an image element

```csharp
PdfImageElement pdfImage = new PdfImageElement(imagePath)
{
    X = crtXPos,
    Y = crtYPos
};
```

### PNG with a custom width

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

### Render the image and update the layout cursor

```csharp
PdfImageRenderInfo imageRenderInfo = pdfDocument.AddImage(pdfPngImage);

// Use the rendered geometry to position the next element
crtYPos = imageRenderInfo.BoundingBox.Bottom + ySeparator;
```

### Image continued on the next pages

```csharp
PdfImageElement tallImage = new PdfImageElement(imagePath)
{
    X = 0,
    Y = crtYPos,
    Width = pdfDocument.ContentWidth,
    // The part of the image below the bottom margin continues on the next pages
    ContinueOnNextPage = true
};

PdfImageRenderInfo info = pdfDocument.AddImage(tallImage);

// One rectangle per page where a part of the image was drawn
foreach (PdfRenderedRectangle part in info.RenderedRectangles)
    Console.WriteLine("page {0}: {1}", part.PageNumber, part.Bounds);

// The content that follows goes below the last part, on its page
crtYPos = (int)info.LastPageRectangle.Bounds.Bottom + 10;
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
            string tallImagePath = Path.Combine(imagesPath, "Tall_Report.png");

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

            if (model.AddTallImage)
            {
                // ===== Section 5: Image taller than a page =====
                pdfDocument.AddPage();
                crtYPos = 0;
                crtYPos = AddSectionLabel(pdfDocument, sectionFont,
                    "5. Image taller than a page" + (model.ContinueOnNextPage ? ", continued on the next pages" : ", cut at the bottom margin"),
                    xLeft, crtYPos, ySeparator);

                // The image is scaled to the content width. The part that does not fit above the bottom margin
                // continues on the next pages from the top margin, or it is cut when the image stays on one page
                PdfImageElement tallImage = new PdfImageElement(tallImagePath)
                {
                    X = xLeft,
                    Y = crtYPos,
                    Width = pdfDocument.ContentWidth,
                    ContinueOnNextPage = model.ContinueOnNextPage
                };
                tallImage.Accessibility.AlternateText = "Annual activity report table";

                PdfImageRenderInfo tallInfo = pdfDocument.AddImage(tallImage);

                // The rectangle of each part of the image, one per page; the content that follows the image is
                // placed below the part drawn on its last page, or below the visible part when the image is cut
                var partsText = new System.Text.StringBuilder(
                    "The image was drawn on " + tallInfo.RenderedRectangles.Count + " page(s):");
                foreach (PdfRenderedRectangle part in tallInfo.RenderedRectangles)
                {
                    partsText.AppendFormat(" page {0} ({1:F0}, {2:F0}, {3:F0}x{4:F0});", part.PageNumber,
                        part.Bounds.X, part.Bounds.Y, part.Bounds.Width, part.Bounds.Height);
                }

                PdfRectangleF lastPart = tallInfo.LastPageRectangle.VisibleBounds ?? tallInfo.LastPageRectangle.Bounds;
                PdfTextElement partsLabel = new PdfTextElement(partsText.ToString(), smallFont)
                {
                    X = xLeft,
                    Y = (int)lastPart.Bottom + ySeparator,
                    Width = pdfDocument.ContentWidth
                };
                partsLabel.Accessibility.StructureType = PdfStructureType.Artifact;
                pdfDocument.AddText(partsLabel);
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

### Draw a Translucent Line

```csharp
PdfLineElement dashedLine = new PdfLineElement(0, 100, 380, 100)
{
    LineColor = PdfColor.DarkRed,
    LineOpacity = 0.5f,
    LineStyle = new PdfLineStyle { LineWidth = 2f, CustomDashPattern = new float[] { 10f, 4f } }
};
pdfDocument.AddLine(dashedLine);
```

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

            // The corner radius of the second box comes from the demo page
            float[] radii = { 4f, model.CornerRadius, 24f, 30f };
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
                // The opacity of the line, from 0 to 1
                LineOpacity = model.LineOpacity,
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

            // The circle is given by its center and its radius; the radius of the first circle comes from the demo page
            var circle1Info = pdfDocument.AddCircle(new PdfCircleElement(xLeft + 40, crtYPos + 40, model.CircleRadius)
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
                // The arc is a part of the ellipse inscribed in the rectangle, from the start angle over the sweep angle,
                // clockwise in degrees
                var arcInfo = pdfDocument.AddArc(new PdfArcElement(arcX, crtYPos, 130, 90,
                    startAngleDegrees: model.ArcStartAngle,
                    sweepAngleDegrees: model.ArcSweepAngle)
                {
                    Closure = closures[i],
                    FillColor = PdfColor.LightSkyBlue,
                    FillOpacity = 0.4f,
                    BorderColor = PdfColor.MediumBlue,
                    BorderOpacity = model.LineOpacity,
                    Border = new PdfLineStyle { LineWidth = 1.5f }
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

### Heart shape with two cubic Beziers

```csharp
PdfPathElement heart = new PdfPathElement
{
    FillColor = PdfColor.Crimson,
    FillOpacity = 0.8f,
    BorderColor = PdfColor.DarkRed,
    Border = new PdfLineStyle { LineWidth = 1.5f, LineJoin = PdfLineJoinStyle.Round }
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

            var heartInfo = pdfDocument.AddPath(heart);
            int heartCapBottom = AddCaption(pdfDocument, labelFont,
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
                BorderColor = PdfColor.Teal,
                Border = new PdfLineStyle { LineWidth = 1.2f }
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

## Create PDF Documents with HTML Content

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-html-content.htm

### Render HTML Content in the Document

```csharp
PdfHtmlElement htmlElement = new PdfHtmlElement(htmlString, baseUrl)
{
    X = 0,
    Y = crtYPos,
    // 0 extends the content to the right margin of the page
    Width = 0
};
PdfHtmlRenderInfo htmlRenderInfo = pdfDocument.AddHtml(htmlElement);

// The elements added after the content go below it, in its last page
crtYPos = htmlRenderInfo.LastPageRectangle.Bounds.Bottom + 15;
```

### Set the Converter of the Element

```csharp
htmlElement.Converter.MediaType = "print";
htmlElement.Converter.PdfDocumentOptions.GenerateDocumentOutline = true;
```

### Table of Contents of a Document

```csharp
pdfDocument.TableOfContents.Enabled = true;
pdfDocument.TableOfContents.Title = "Contents";
pdfDocument.AddHtml(new PdfHtmlElement(firstHtml, null));
pdfDocument.AddPage();
pdfDocument.AddHtml(new PdfHtmlElement(secondHtml, null));
byte[] pdf = pdfDocument.Save();
```

### Retrieve the Positions of the HTML Elements

```csharp
htmlElement.HtmlElementsInfoSelector = "h1, h2";
PdfHtmlRenderInfo htmlRenderInfo = pdfDocument.AddHtml(htmlElement);

foreach (HtmlElementInfo elementInfo in htmlRenderInfo.HtmlElementsInfo.Elements)
{
    foreach (PdfRenderedRectangle rectangle in elementInfo.RenderedRectangles)
        Console.WriteLine("{0} on page {1} at {2}", elementInfo.TagName, rectangle.PageNumber, rectangle.Bounds);
}
```

### Code Sample - Create PDF Documents with HTML Content

```csharp
using System;
using System.IO;
using System.Text;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Creator;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Creator
{
    public class Create_PDF_Documents_with_HTMLController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_HTMLController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_HTML_ViewModel model)
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
                PdfStandard = model.PdfStandard
            };

            using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
            pdfDocument.PdfDocumentInfo.Title = "HTML Content in a PDF Document";

            string fontFilePath = Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 16f, PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont textFont = PdfFontManager.CreateFont(baseFont, 10f, PdfFontStyle.Normal, PdfColor.Black);
            PdfFont smallFont = PdfFontManager.CreateFont(baseFont, 8f, PdfFontStyle.Normal, PdfColor.DarkGray);

            // Elements created with the PDF creator before the HTML content
            PdfTextElement titleElement = new PdfTextElement("HTML Content in a PDF Document", titleFont)
            {
                X = 0,
                Y = 0,
                Width = pdfDocument.ContentWidth,
                Alignment = PdfTextAlignment.Center
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            float crtYPos = pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + 10;

            PdfTextElement introElement = new PdfTextElement(
                "The text above and this paragraph are text elements of the PDF document. The HTML content " +
                "starts below them and continues on new pages when it does not fit in this one. " +
                "The elements added after the HTML content go to its last page, below it.", textFont)
            {
                X = 0,
                Y = crtYPos,
                Width = pdfDocument.ContentWidth
            };
            crtYPos = pdfDocument.AddText(introElement).LastPageRectangle.Bounds.Bottom + 15;

            // The HTML content rendered in the PDF document, from the given position of the current page and
            // inside the page margins. The X position is kept on the next pages
            PdfHtmlElement htmlElement = model.HtmlPageSource == "Url"
                ? new PdfHtmlElement(model.Url)
                : new PdfHtmlElement(model.HtmlStringTextBox, model.BaseUrlTextBox);
            htmlElement.X = model.ElementX;
            // The content starts at the given distance below the text above it
            htmlElement.Y = crtYPos + model.ElementY;
            // 0 extends the content to the right margin of the page
            htmlElement.Width = model.ElementWidth;

            // The converter of the element takes the page size, the margins and the PDF standard from the document;
            // its other settings are set as for a HTML to PDF conversion
            htmlElement.Converter.MediaType = model.MediaType;
            // The headings of the HTML content join the outline of the PDF document
            htmlElement.Converter.PdfDocumentOptions.GenerateDocumentOutline = model.GenerateDocumentOutline;

            if (model.RetrieveElementPositions)
            {
                // The positions of the elements matching the selector are reported in the render information
                htmlElement.HtmlElementsInfoSelector = model.HtmlElementsSelector;
            }

            PdfHtmlRenderInfo htmlRenderInfo = pdfDocument.AddHtml(htmlElement);

            // The elements after the HTML content are placed below it, at the end of the content on its last page,
            // or on a new page after the content. A new page is also added when the content ends too close to the
            // bottom of its last page for the elements that follow
            const float MinimumSpace = 60;
            crtYPos = htmlRenderInfo.LastPageRectangle.Bounds.Bottom + 15;
            if (model.FollowingElementsOnNewPage || crtYPos > pdfDocument.ContentHeight - MinimumSpace)
            {
                pdfDocument.AddPage();
                crtYPos = 0;
            }

            PdfRectangleElement separator = new PdfRectangleElement(0, crtYPos, pdfDocument.ContentWidth, 1)
            {
                FillColor = PdfColor.DarkBlue,
                BorderColor = null
            };
            pdfDocument.AddRectangle(separator);

            PdfTextElement closingElement = new PdfTextElement(
                string.Format("The HTML content was rendered on {0} page(s), from page {1} to page {2}. This text was added after it.",
                    htmlRenderInfo.PagesCount, htmlRenderInfo.FirstPageNumber, htmlRenderInfo.LastPageNumber), textFont)
            {
                X = 0,
                Y = crtYPos + 8,
                Width = pdfDocument.ContentWidth,
                ContinueOnNextPage = true
            };
            crtYPos = pdfDocument.AddText(closingElement).LastPageRectangle.Bounds.Bottom + 10;

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
                    X = 0,
                    Y = crtYPos,
                    Width = pdfDocument.ContentWidth,
                    ContinueOnNextPage = true
                };
                crtYPos = pdfDocument.AddText(positionsElement).LastPageRectangle.Bounds.Bottom + 15;
            }

            if (model.AddSecondElement)
            {
                // A second HTML element continues after the first one, from the current position of the last page,
                // or from the top of a new page when the space left is too small
                if (crtYPos > pdfDocument.ContentHeight - MinimumSpace)
                {
                    pdfDocument.AddPage();
                    crtYPos = 0;
                }
                PdfHtmlElement secondElement = new PdfHtmlElement(
                    "<div style=\"font-family: Arial; font-size: 12px; border: 1px solid #1F4E79; padding: 8px; background: #EEF3F8;\">" +
                    "<b>Second HTML element</b><br/>This HTML string is rendered by another element added after the text above. " +
                    "It can have its own width and position in the page and its own converter settings.</div>", null)
                {
                    X = model.ElementX + 20,
                    Y = crtYPos,
                    Width = model.ElementWidth > 0 ? model.ElementWidth - 20 : pdfDocument.ContentWidth - model.ElementX - 40
                };
                PdfHtmlRenderInfo secondRenderInfo = pdfDocument.AddHtml(secondElement);
                crtYPos = secondRenderInfo.LastPageRectangle.Bounds.Bottom + 10;

                pdfDocument.AddText(new PdfTextElement(
                    string.Format("The second element was rendered on page {0}.", secondRenderInfo.LastPageNumber), textFont)
                {
                    X = 0,
                    Y = crtYPos,
                    Width = pdfDocument.ContentWidth,
                    ContinueOnNextPage = true
                });
            }

            if (model.AddPageNumbers)
            {
                // A template with the page number centered in the bottom margin of every page, over the pages of the HTML content too
                PdfHtmlTemplate footer = pdfDocument.AddHtmlTemplate(0, 0, pdfDocument.PageSize.Width, 20,
                    "<div style=\"font-family: Arial; font-size: 9px; color: #606060; text-align: center;\">Page {page_number} of {total_pages}</div>", null);
                footer.VerticalAlign = PdfTemplateVerticalAlign.Bottom;
            }

            byte[] outPdfBuffer = pdfDocument.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "HTML_in_Pdf_Document.pdf";
            return fileResult;
        }

        private static string Shorten(string text)
        {
            text = (text ?? string.Empty).Replace('\n', ' ').Replace('\r', ' ').Trim();
            return text.Length > 40 ? text.Substring(0, 40) + "..." : text;
        }

        private Create_PDF_Documents_with_HTML_ViewModel SetViewModel()
        {
            var model = new Create_PDF_Documents_with_HTML_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Create_PDF_Documents_with_HTML".Length);

            model.HtmlStringTextBox = System.IO.File.ReadAllText(Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/Auto_Bookmarks.html"));
            model.BaseUrlTextBox = rootUrl + "DemoAppFiles/Input/HTML_Files/";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```

## Create PDF Documents with HTML as Image

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-html-as-image.htm

### Render HTML as an Image in the Document

```csharp
PdfHtmlImageElement htmlImage = new PdfHtmlImageElement(htmlCode, baseUrl)
{
    X = 0,
    Y = crtYPos,
    // 0 draws the image at the size of the content on screen, scaled down to the right margin when it is wider
    Width = 0,
    // 0 gives no height limit
    Height = 0,
    // The image has the same size in the page at any resolution; a higher resolution gives a sharper image
    Resolution = 150
};
htmlImage.Accessibility.AlternateText = "Sales table rendered from HTML";

PdfImageRenderInfo htmlImageInfo = pdfDocument.AddHtmlImage(htmlImage);
```

### Set the Converter of the Element

```csharp
htmlImage.Converter.HtmlViewerWidth = 1024;
htmlImage.Converter.MediaType = "print";
```

### Continue the Image on the Next Pages

```csharp
htmlImage.ContinueOnNextPage = true;
PdfImageRenderInfo htmlImageInfo = pdfDocument.AddHtmlImage(htmlImage);

foreach (PdfRenderedRectangle part in htmlImageInfo.RenderedRectangles)
    Console.WriteLine("Page {0}: {1}", part.PageNumber, part.Bounds);

// The elements added after the image go below the part drawn on its last page
PdfRectangleF lastPart = htmlImageInfo.LastPageRectangle.VisibleBounds ?? htmlImageInfo.LastPageRectangle.Bounds;
crtYPos = lastPart.Bottom + 10;
```

### Code Sample - Create PDF Documents with HTML as Image

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
    public class Create_PDF_Documents_with_HTML_as_ImageController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_HTML_as_ImageController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Documents_with_HTML_as_Image_ViewModel();
            model.HtmlStringTextBox = GetSampleHtml();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Documents_with_HTML_as_Image_ViewModel model)
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
            pdfDocument.PdfDocumentInfo.Title = "HTML Rendered as an Image in a PDF Document";

            string fontFilePath = Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 16f, PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont textFont = PdfFontManager.CreateFont(baseFont, 10f, PdfFontStyle.Normal, PdfColor.Black);
            PdfFont smallFont = PdfFontManager.CreateFont(baseFont, 8f, PdfFontStyle.Normal, PdfColor.DarkGray);

            PdfTextElement titleElement = new PdfTextElement("HTML Rendered as an Image in a PDF Document", titleFont)
            {
                X = 0,
                Y = 0,
                Width = pdfDocument.ContentWidth,
                Alignment = PdfTextAlignment.Center
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            float crtYPos = pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + 10;

            PdfTextElement introElement = new PdfTextElement(
                "The HTML content below is rendered as an image at " + model.Resolution + " DPI, at the size the content has on " +
                "screen or at the size given to the element, and continues on the next pages when it is taller than the space left on the page.",
                textFont)
            {
                X = 0,
                Y = crtYPos,
                Width = pdfDocument.ContentWidth
            };
            crtYPos = pdfDocument.AddText(introElement).LastPageRectangle.Bounds.Bottom + 15;

            // The HTML content rendered as an image, from a URL or from a HTML string with a base URL. The image
            // starts at the given position in the current page, inside the margins, and the part that does not fit
            // above the bottom margin continues on the next pages from the top margin
            PdfHtmlImageElement htmlImage = model.HtmlPageSource == "Url"
                ? new PdfHtmlImageElement(model.Url)
                : new PdfHtmlImageElement(model.HtmlStringTextBox ?? string.Empty, model.BaseUrlTextBox);
            htmlImage.X = model.ElementX;
            // The image starts at the given distance below the text above it
            htmlImage.Y = crtYPos + model.ElementY;
            // 0 draws the image at the size of the content on screen, scaled down to the right margin when it is wider
            htmlImage.Width = model.ElementWidth;
            // 0 gives no height limit; a taller image is scaled down to this height, keeping its aspect ratio
            htmlImage.Height = model.ElementHeight;
            // The resolution of the image: the size in the page is the same at any resolution, a higher one gives a
            // sharper image
            htmlImage.Resolution = model.Resolution;
            // When false, the part of the image outside the page is cut
            htmlImage.ContinueOnNextPage = model.ContinueOnNextPage;
            // The width of the browser window where the HTML is laid out; the image has the width of the content
            htmlImage.Converter.HtmlViewerWidth = model.HtmlViewerWidth;
            htmlImage.Accessibility.AlternateText = "HTML content rendered as an image";

            PdfImageRenderInfo htmlImageInfo = pdfDocument.AddHtmlImage(htmlImage);

            // The rectangle of each part of the image, one for each page; the elements added after the image go
            // below the part drawn on its last page
            var partsText = new System.Text.StringBuilder(
                "The image was drawn on " + htmlImageInfo.RenderedRectangles.Count + " page(s):");
            foreach (PdfRenderedRectangle part in htmlImageInfo.RenderedRectangles)
            {
                partsText.AppendFormat(" page {0} ({1:F0}, {2:F0}, {3:F0}x{4:F0});", part.PageNumber,
                    part.Bounds.X, part.Bounds.Y, part.Bounds.Width, part.Bounds.Height);
            }

            // The part of the image drawn on its last page: the whole image, or the part that fits when it is cut
            PdfRectangleF lastPart = htmlImageInfo.LastPageRectangle.VisibleBounds ?? htmlImageInfo.LastPageRectangle.Bounds;
            PdfTextElement partsElement = new PdfTextElement(partsText.ToString(), smallFont)
            {
                X = 0,
                Y = lastPart.Bottom + 10,
                Width = pdfDocument.ContentWidth
            };
            pdfDocument.AddText(partsElement);

            byte[] outPdfBuffer = pdfDocument.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "HTML_as_Image_in_Pdf_Document.pdf";
            return fileResult;
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

## Create PDF Documents with Pages of Other PDF Documents

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-pdf-pages.htm

### Draw Pages of Other Documents

```csharp
// The letterhead page covers the whole current page, under the content added later
PdfPageElement letterhead = new PdfPageElement(letterheadPath, 1)
{
    X = -margins.Left,
    Y = -margins.Top,
    RenderUnderContent = true
};
pdfDocument.AddPdfPage(letterhead);

// A page of another document scaled to fit a rectangle, keeping its proportions
PdfPageElement pageElement = new PdfPageElement(sourcePdfBytes, 2)
{
    X = 0,
    Y = crtYPos,
    Width = 150,
    Height = 210,
    Opacity = 1,
    RotationDegrees = 0,
    IncludeAnnotations = true
};
PdfElementRenderInfo renderInfo = pdfDocument.AddPdfPage(pageElement);
PdfRectangleF bounds = renderInfo.LastPageRectangle.Bounds;
```

### Code Sample - Create PDF Documents with Pages of Other PDF Documents

```csharp
using System;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Creator;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Creator
{
    public class Create_PDF_Documents_with_PDF_PagesController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Documents_with_PDF_PagesController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> CreatePdf(Create_PDF_Documents_with_PDF_Pages_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] sourcePdfBytes = await ReadInputPdf(model.PdfFile, model.PdfFileUrl);
            string sourcePassword = string.IsNullOrEmpty(model.PdfPassword) ? null : model.PdfPassword;

            // The top and bottom margins leave room for the band and the line of the letterhead page
            PdfMargins margins = new PdfMargins(40, 40, 90, 70);
            PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
            {
                PageSize = PdfPageSize.A4,
                PageOrientation = PdfPageOrientation.Portrait,
                Margins = margins,
                PdfStandard = model.PdfStandard,
                Language = "en-US"
            };

            using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
            pdfDocument.PdfDocumentInfo.Title = "PDF Pages in a PDF Document";

            string fontFilePath = Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf");
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 16f, PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont textFont = PdfFontManager.CreateFont(baseFont, 10f, PdfFontStyle.Normal, PdfColor.Black);
            PdfFont captionFont = PdfFontManager.CreateFont(baseFont, 8f, PdfFontStyle.Normal, PdfColor.DarkGray);

            string letterheadPath = Path.Combine(GetDemoFilesPath(), "PDF_Files", "Letterhead.pdf");

            if (model.UseLetterhead)
                AddLetterhead(pdfDocument, letterheadPath, margins, model);

            PdfTextElement titleElement = new PdfTextElement("PDF Pages in a PDF Document", titleFont)
            {
                X = 0,
                Y = 0,
                Width = pdfDocument.ContentWidth
            };
            titleElement.Accessibility.StructureType = PdfStructureType.Heading1;
            float crtYPos = pdfDocument.AddText(titleElement).LastPageRectangle.Bounds.Bottom + 10;

            PdfTextElement introElement = new PdfTextElement(
                "The page of the letterhead document is drawn under the content of this page and the pages of another PDF " +
                "document are drawn below as scaled copies, with their links and form fields when the annotations are included. " +
                "A drawn page keeps its fonts, images and vector content.", textFont)
            {
                X = 0,
                Y = crtYPos,
                Width = pdfDocument.ContentWidth
            };
            crtYPos = pdfDocument.AddText(introElement).LastPageRectangle.Bounds.Bottom + 20;

            if (model.DrawPages)
            {
                int[] pageNumbers = ParsePageNumbers(model.DrawnPages);
                using PdfEditor sourceInfo = new PdfEditor(sourcePdfBytes, sourcePassword);
                int sourcePageCount = sourceInfo.PageCount;

                float cellWidth = model.PageWidth + 20;
                float cellHeight = model.PageHeight + 30;
                int columns = Math.Max(1, (int)(pdfDocument.ContentWidth / cellWidth));
                int index = 0;

                foreach (int pageNumber in pageNumbers)
                {
                    if (pageNumber < 1 || pageNumber > sourcePageCount)
                        throw new Exception("The page numbers to draw must be between 1 and " + sourcePageCount);

                    int column = index % columns;
                    if (column == 0 && index > 0)
                        crtYPos += cellHeight;
                    if (crtYPos + cellHeight > pdfDocument.ContentHeight)
                    {
                        // The next row of pages goes to a new page
                        pdfDocument.AddPage();
                        if (model.UseLetterhead)
                            AddLetterhead(pdfDocument, letterheadPath, margins, model);
                        crtYPos = 0;
                    }

                    float x = column * cellWidth;

                    // A page of the source document scaled to fit the given width and height, keeping its proportions
                    PdfPageElement pageElement = new PdfPageElement(sourcePdfBytes, pageNumber)
                    {
                        Password = sourcePassword,
                        X = x,
                        Y = crtYPos,
                        Width = model.PageWidth,
                        Height = model.PageHeight,
                        Opacity = model.PageOpacity,
                        RotationDegrees = model.RotationDegrees,
                        RotationPivot = model.RotationPivot,
                        IncludeAnnotations = model.IncludeAnnotations
                    };
                    PdfElementRenderInfo renderInfo = pdfDocument.AddPdfPage(pageElement);

                    // The rendered rectangle gives the actual size of the scaled page
                    PdfRectangleF bounds = renderInfo.LastPageRectangle.Bounds;
                    pdfDocument.AddRectangle(new PdfRectangleElement(bounds.X, bounds.Y, bounds.Width, bounds.Height)
                    {
                        BorderColor = PdfColor.Gray,
                        FillColor = null
                    });
                    pdfDocument.AddText(new PdfTextElement("Page " + pageNumber, captionFont)
                    {
                        X = x,
                        Y = crtYPos + model.PageHeight + 4,
                        Width = model.PageWidth,
                        Alignment = PdfTextAlignment.Center
                    });

                    index++;
                }
                crtYPos += cellHeight;
            }

            if (model.DrawFullSizePage)
            {
                // The first page of the source document drawn at its own size on a new page, with the margins of the document
                pdfDocument.AddPage();
                if (model.UseLetterhead)
                    AddLetterhead(pdfDocument, letterheadPath, margins, model);

                pdfDocument.AddText(new PdfTextElement("The first page of the source document at its own size, inside the margins of this page:", textFont)
                {
                    X = 0,
                    Y = 0,
                    Width = pdfDocument.ContentWidth
                });

                PdfPageElement fullPageElement = new PdfPageElement(sourcePdfBytes, 1)
                {
                    Password = sourcePassword,
                    X = 0,
                    Y = 30,
                    Opacity = model.PageOpacity,
                    IncludeAnnotations = model.IncludeAnnotations
                };
                pdfDocument.AddPdfPage(fullPageElement);
            }

            byte[] outPdfBuffer = pdfDocument.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "PDF_Pages_in_Pdf_Document.pdf";
            return fileResult;
        }

        // The letterhead page covers the whole current page, from the top left corner of the page, under the content added later
        private static void AddLetterhead(PdfDocument pdfDocument, string letterheadPath, PdfMargins margins,
            Create_PDF_Documents_with_PDF_Pages_ViewModel model)
        {
            PdfPageElement letterhead = new PdfPageElement(letterheadPath, 1)
            {
                X = -margins.Left,
                Y = -margins.Top,
                Opacity = model.LetterheadOpacity,
                RenderUnderContent = model.RenderUnderContent
            };
            pdfDocument.AddPdfPage(letterhead);
        }

        private static int[] ParsePageNumbers(string pageNumbers)
        {
            return (pageNumbers ?? string.Empty).Split(',', StringSplitOptions.RemoveEmptyEntries)
                .Select(p => int.Parse(p.Trim())).ToArray();
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

        private Create_PDF_Documents_with_PDF_Pages_ViewModel SetViewModel()
        {
            var model = new Create_PDF_Documents_with_PDF_Pages_ViewModel();

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
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Create_PDF_Documents_with_PDF_Pages".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/Multi_Page_Document.pdf";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```

## Create PDF Documents with Gradient Fills

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-gradient-fills.htm

### Rectangle with a Gradient Fill

```csharp
PdfRectangleElement rectangle = new PdfRectangleElement(0, 0, 110, 60)
{
    GradientFill = new PdfGradientFill(PdfColor.LightSkyBlue, PdfColor.DarkBlue, PdfGradientDirection.Horizontal),
    BorderColor = PdfColor.DarkBlue,
    Border = new PdfLineStyle { LineWidth = 1f }
};
pdfDocument.AddRectangle(rectangle);
```

### Badge Sized with the Text Width

```csharp
// The width of the text written with the font gives the width of its badge
float textWidth = badgeFont.GetTextWidth("GRADIENT FILL");
float badgeWidth = textWidth + 2 * badgePadding;

pdfDocument.AddRoundedRectangle(new PdfRoundedRectangleElement(badgeX, badgeY, badgeWidth, badgeHeight, badgeHeight / 2)
{
    GradientFill = new PdfGradientFill(PdfColor.LightSkyBlue, PdfColor.DarkBlue, PdfGradientDirection.Vertical),
    BorderColor = PdfColor.DarkBlue,
    Border = new PdfLineStyle { LineWidth = 0.5f }
});

// The text is centered in the badge from the measured width
PdfTextElement badgeText = new PdfTextElement("GRADIENT FILL", badgeFont)
{
    X = badgeX + badgePadding,
    Y = badgeY + (badgeHeight - badgeFont.Size) / 2 - 1,
    Width = textWidth + 2
};
pdfDocument.AddText(badgeText);
```

### Code Sample - Create PDF Documents with Gradient Fills

```csharp
using System;

// Use EVO PDF Namespace
using EvoPdf.Next;

public byte[] CreatePdfWithGradientFills(string fontFilePath, float fillOpacity)
{
    // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
    // Licensing.LicenseKey = "your-license-key";

    PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
    {
        PageSize = PdfPageSize.A4,
        PageOrientation = PdfPageOrientation.Portrait,
        Margins = new PdfMargins(36, 36, 36, 36),
        Language = "en-US"
    };

    using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
    pdfDocument.PdfDocumentInfo.Title = "PDF Gradient Fills Demo";

    PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(fontFilePath);
    PdfFont badgeFont = PdfFontManager.CreateFont(baseFont, 11f, PdfFontStyle.Bold, PdfColor.White);

    const int xLeft = 0;
    int crtYPos = 0;

    // ===== Section 1: the four gradient directions on rectangles =====
    PdfGradientDirection[] directions =
    {
        PdfGradientDirection.Vertical, PdfGradientDirection.Horizontal,
        PdfGradientDirection.ForwardDiagonal, PdfGradientDirection.BackwardDiagonal
    };
    for (int i = 0; i < directions.Length; i++)
    {
        int boxX = xLeft + i * 130;

        // A gradient fill takes the place of the solid fill color: the shape is painted
        // from the start color to the end color along the given direction
        var rectangle = new PdfRectangleElement(boxX, crtYPos, 110, 60)
        {
            GradientFill = new PdfGradientFill(PdfColor.LightSkyBlue, PdfColor.DarkBlue, directions[i]),
            FillOpacity = fillOpacity,
            BorderColor = PdfColor.DarkBlue,
            Border = new PdfLineStyle { LineWidth = 1f }
        };
        pdfDocument.AddRectangle(rectangle);
    }
    crtYPos += 80;

    // ===== Section 2: rounded rectangles, circles and ellipses =====
    pdfDocument.AddRoundedRectangle(new PdfRoundedRectangleElement(xLeft, crtYPos, 140, 70, 16f)
    {
        GradientFill = new PdfGradientFill(PdfColor.Gold, PdfColor.DarkOrange, PdfGradientDirection.ForwardDiagonal),
        FillOpacity = fillOpacity,
        BorderColor = PdfColor.DarkOrange,
        Border = new PdfLineStyle { LineWidth = 1f }
    });

    pdfDocument.AddCircle(new PdfCircleElement(xLeft + 215, crtYPos + 35, 35)
    {
        GradientFill = new PdfGradientFill(PdfColor.White, PdfColor.Crimson, PdfGradientDirection.Vertical),
        FillOpacity = fillOpacity,
        BorderColor = PdfColor.Crimson,
        Border = new PdfLineStyle { LineWidth = 1f }
    });

    pdfDocument.AddEllipse(new PdfEllipseElement(xLeft + 300, crtYPos, 150, 70)
    {
        GradientFill = new PdfGradientFill(PdfColor.LightGreen, PdfColor.DarkGreen, PdfGradientDirection.Horizontal),
        FillOpacity = fillOpacity,
        BorderColor = PdfColor.DarkGreen,
        Border = new PdfLineStyle { LineWidth = 1f },
        RotationDegrees = 20,
        RotationPivot = PdfRotationPivot.Center
    });
    crtYPos += 100;

    // ===== Section 3: arcs, polygons and paths =====

    // Only a closed arc (chord or pie) has an area to fill
    pdfDocument.AddArc(new PdfArcElement(xLeft, crtYPos, 120, 90, startAngleDegrees: 30, sweepAngleDegrees: 240)
    {
        Closure = PdfArcClosureType.Pie,
        GradientFill = new PdfGradientFill(PdfColor.Plum, PdfColor.Purple, PdfGradientDirection.BackwardDiagonal),
        FillOpacity = fillOpacity,
        BorderColor = PdfColor.Purple,
        Border = new PdfLineStyle { LineWidth = 1f }
    });

    float hexagonCenterX = xLeft + 220, hexagonCenterY = crtYPos + 45, hexagonRadius = 45;
    var hexagonPoints = new PdfPointF[6];
    for (int i = 0; i < 6; i++)
    {
        double angle = Math.PI / 3 * i;
        hexagonPoints[i] = new PdfPointF(hexagonCenterX + (float)(hexagonRadius * Math.Cos(angle)),
            hexagonCenterY + (float)(hexagonRadius * Math.Sin(angle)));
    }
    pdfDocument.AddPolygon(new PdfPolygonElement(hexagonPoints)
    {
        GradientFill = new PdfGradientFill(PdfColor.LightCyan, PdfColor.Teal, PdfGradientDirection.Vertical),
        FillOpacity = fillOpacity,
        BorderColor = PdfColor.Teal,
        Border = new PdfLineStyle { LineWidth = 1f }
    });

    // A heart drawn with two cubic curves; the gradient fills the closed path
    float cx = xLeft + 400, cy = crtYPos + 50, s = 55;
    PdfPathElement heart = new PdfPathElement
    {
        GradientFill = new PdfGradientFill(PdfColor.LightPink, PdfColor.DarkRed, PdfGradientDirection.Vertical),
        FillOpacity = fillOpacity,
        BorderColor = PdfColor.DarkRed,
        Border = new PdfLineStyle { LineWidth = 1f, LineJoin = PdfLineJoinStyle.Round }
    };
    heart.MoveTo(cx, cy + s * 0.25f)
        .CurveTo(cx - s * 0.55f, cy + s * 0.55f, cx - s * 1.10f, cy - s * 0.10f, cx, cy - s * 0.35f)
        .CurveTo(cx + s * 1.10f, cy - s * 0.10f, cx + s * 0.55f, cy + s * 0.55f, cx, cy + s * 0.25f)
        .Close();
    pdfDocument.AddPath(heart);
    crtYPos += 120;

    // ===== Section 4: badges sized with the measured text width =====
    string[] badgeTexts = { "NEW", "GRADIENT FILL", "PdfFont.GetTextWidth", "PDF" };
    PdfColor[] badgeStartColors = { PdfColor.LightGreen, PdfColor.LightSkyBlue, PdfColor.Gold, PdfColor.Plum };
    PdfColor[] badgeEndColors = { PdfColor.DarkGreen, PdfColor.DarkBlue, PdfColor.DarkOrange, PdfColor.Purple };
    float badgeX = xLeft;
    const float badgeHeight = 24f, badgePadding = 10f;
    for (int i = 0; i < badgeTexts.Length; i++)
    {
        // The width of the text written with the font gives the width of its badge
        float textWidth = badgeFont.GetTextWidth(badgeTexts[i]);
        float badgeWidth = textWidth + 2 * badgePadding;

        pdfDocument.AddRoundedRectangle(new PdfRoundedRectangleElement(badgeX, crtYPos, badgeWidth, badgeHeight, badgeHeight / 2)
        {
            GradientFill = new PdfGradientFill(badgeStartColors[i], badgeEndColors[i], PdfGradientDirection.Vertical),
            FillOpacity = fillOpacity,
            BorderColor = badgeEndColors[i],
            Border = new PdfLineStyle { LineWidth = 0.5f }
        });

        // The text is centered in the badge from the measured width
        PdfTextElement badgeText = new PdfTextElement(badgeTexts[i], badgeFont)
        {
            X = badgeX + badgePadding,
            Y = crtYPos + (badgeHeight - badgeFont.Size) / 2 - 1,
            Width = textWidth + 2
        };
        pdfDocument.AddText(badgeText);

        badgeX += badgeWidth + 12;
    }

    byte[] outPdfBuffer = pdfDocument.Save();

    return outPdfBuffer;
}
```
