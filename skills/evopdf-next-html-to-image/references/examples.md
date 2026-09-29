# HTML to image: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## HTML to Image Converter Overview

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-image-converter-overview.htm

### Code Sample - Convert HTML to Image

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_Image;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_Image
{
    public class Convert_HTML_to_ImageController : Controller
    {
        [HttpPost]
        public ActionResult ConvertHtmlToImage(Convert_HTML_to_Image_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a HTML to Image converter object with default settings
            HtmlToImageConverter htmlToImageConverter = new HtmlToImageConverter();

            // Set HTML Viewer width in pixels which is the equivalent in converter of the browser window width
            htmlToImageConverter.HtmlViewerWidth = model.HtmlViewerWidth;

            // Set HTML viewer height in pixels to convert the top part of a HTML page 
            // Leave it not set to convert the entire HTML
            if (model.HtmlViewerHeight.HasValue)
                htmlToImageConverter.HtmlViewerHeight = model.HtmlViewerHeight.Value;

            // enable the conversion of the entire page, not only the viewport defined by HtmlViewerWidth and HtmlViewerHeight
            htmlToImageConverter.CaptureEntirePage = model.CaptureEntirePage;

            // Set the loading mode used to capture the entire page content
            htmlToImageConverter.CaptureEntirePageMode = model.CaptureEntirePageMode == "Browser" ?
                CaptureEntirePageMode.Browser : CaptureEntirePageMode.Custom;

            // Optionally auto resize HTML viewer height at the HTML content size determined after the initial loading
            htmlToImageConverter.AutoResizeHtmlViewerHeight = model.AutoResizeViewerHeight;

            // Set the maximum time in seconds to wait for HTML page to be loaded 
            // Leave it not set for a default 120 seconds maximum wait time
            htmlToImageConverter.NavigationTimeout = model.NavigationTimeout;

            // Set an adddional delay in seconds to wait for JavaScript or AJAX calls after page load completed
            // Set this property to 0 if you don't need to wait for such asynchcronous operations to finish
            if (model.ConversionDelay.HasValue)
                htmlToImageConverter.ConversionDelay = model.ConversionDelay.Value;

            byte[] outImageBuffer = null;
            if (model.HtmlPageSource == "Url")
            {
                string url = model.Url;

                // Convert the HTML page given by an URL to an image into a memory buffer
                outImageBuffer = htmlToImageConverter.ConvertUrl(url, SelectedImageFormat(model.ImageFormat));
            }
            else
            {
                string htmlString = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert a HTML string with a base URL to an image into a memory buffer
                outImageBuffer = htmlToImageConverter.ConvertHtml(htmlString ?? string.Empty, baseUrl, SelectedImageFormat(model.ImageFormat));
            }

            string imageFormatName = model.ImageFormat.ToLower();

            // Send the image file to browser
            FileResult fileResult = new FileContentResult(outImageBuffer, "image/" + (imageFormatName == "jpg" ? "jpeg" : imageFormatName));
            fileResult.FileDownloadName = "HTML_to_Image." + imageFormatName;

            return fileResult;
        }

        private ImageType SelectedImageFormat(string selectedValue)
        {
            switch (selectedValue)
            {
                case "Png":
                    return ImageType.Png;
                case "Jpg":
                    return ImageType.Jpeg;
                case "Webp":
                    return ImageType.Webp;
                default:
                    return ImageType.Png;
            }
        }
    }
}
```

## Select HTML Elements to Convert to Image

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-convert-to-image.htm

### Code Sample - Select HTML Elements to Convert to Image

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_Image;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_Image
{
    public class Select_HTML_Elements_to_Convert_to_ImageController : Controller
    {
        [HttpPost]
        public ActionResult ConvertHtmlToImage(Select_HTML_Elements_to_Convert_to_Image_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a HTML to Image converter object with default settings    
            HtmlToImageConverter htmlToImageConverter = new HtmlToImageConverter();

            bool enableElementsSelector = model.EnableElementsSelector;
            if (enableElementsSelector)
            {
                // The CSS selector used to identify the elements to include in the image
                htmlToImageConverter.ConvertedElementsSelector = model.ConvertedElementsSelector;

                // Specify whether elements that are not matched by ConvertedElementsSelector
                // should be completely removed from the layout rather than just hidden
                htmlToImageConverter.RemoveUnselectedElements = model.RemoveUnselectedElements;

                // Allow image to auto-resize at minimum height
                htmlToImageConverter.HtmlViewerHeight = 1;
            }

            byte[] outImageBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlWithForm = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert a HTML string to a PNG image
                outImageBuffer = htmlToImageConverter.ConvertHtml(htmlWithForm ?? string.Empty, baseUrl, ImageType.Png);
            }
            else
            {
                string url = model.Url;

                // Convert the HTML page to a PNG image
                outImageBuffer = htmlToImageConverter.ConvertUrl(url, ImageType.Png);
            }

            // Send the image file to browser
            FileResult fileResult = new FileContentResult(outImageBuffer, "image/png");
            fileResult.FileDownloadName = "Select_HTML_Elements_to_Convert_to_Image.png";

            return fileResult;
        }
    }
}
```

## Select HTML Elements to Exclude from Image

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/select-html-elements-to-exclude-from-image.htm

### Code Sample - Select HTML Elements to Convert to Image

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_Image;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_Image
{
    public class Select_HTML_Elements_to_Exclude_from_ImageController : Controller
    {
        [HttpPost]
        public ActionResult ConvertHtmlToImage(Select_HTML_Elements_to_Exclude_from_Image_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a HTML to Image converter object with default settings
            HtmlToImageConverter htmlToImageConverter = new HtmlToImageConverter();

            bool enableExcludedElementsSelector = model.EnableExcludedElementsSelector;
            if (enableExcludedElementsSelector)
            {
                // The CSS selector used to identify the elements to exclude from conversion to image
                htmlToImageConverter.ExcludedElementsSelector = model.ExcludedElementsSelector;

                // Specify whether elements that are not matched by ExcludedElementsSelector
                // should be completely removed from the layout rather than just hidden
                htmlToImageConverter.RemoveExcludedElements = model.RemoveExcludedElements;
            }

            byte[] outImageBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlWithForm = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert a HTML string to a PNG image
                outImageBuffer = htmlToImageConverter.ConvertHtml(htmlWithForm ?? string.Empty, baseUrl, ImageType.Png);
            }
            else
            {
                string url = model.Url;

                // Convert the HTML page to a PNG image
                outImageBuffer = htmlToImageConverter.ConvertUrl(url, ImageType.Png);
            }

            // Send the image file to browser
            FileResult fileResult = new FileContentResult(outImageBuffer, "image/png");
            fileResult.FileDownloadName = "Select_HTML_Elements_to_Exclude_from_Image.png";

            return fileResult;
        }
    }
}
```
