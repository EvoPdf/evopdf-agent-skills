# Headers, footers and stamps: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfHtmlHeaderFooter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfHtmlHeaderFooter.htm

A PDF Template can be used to repeat an HTML content in many PDF pages. The HTML can contain placeholders such as {page-number} and {total-pages} to be replaced when the PDF is generated. The HTML to PDF conversion features require installing the corresponding core runtime in addition to the core library. It is recommended to use at least the HTML to PDF Converter NuGet package (or a meta-package depending on it) which provides the correct dependencies for all the HTML conversion features in the core library.

### Properties

- `AutoResizePdfMargins`: Allows the top and bottom margins of the PDF document to be automatically resized based on the header and footer HTML content. The default value is true
- `DelayContentRendering`: When set to true, the header or footer will reserve layout space on applicable pages as it would be rendered, but will not render any visible content. This is useful when the actual content is intended to be inserted in a later processing stage. The default value is false.
- `ReserveSpaceAlways`: If true, space is reserved on all pages regardless of header and footer visibility. This is the default behavior. If false, space for the header or footer is reserved only on pages where they are actually rendered, and styling rules intended for print layout will be applied in this case. It is ignored if ShowOnlyInHtmlToPdfPages is explicitly set to false. The default value is true

## PdfHtmlTemplate

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfHtmlTemplate.htm

A PDF Template can be used to repeat HTML content on multiple PDF pages. The HTML content can contain placeholders such as {page_number} and {total_pages} to be replaced when generating the final PDF. The HTML to PDF conversion features require installing the corresponding core runtime in addition to the core library. It is recommended to use at least the HTML to PDF Converter NuGet package (or a meta-package depending on it) which provides the correct dependencies for all the HTML conversion features in the core library.

### Properties

- `AllowInsecureContent`: Allows loading insecure HTTP content in secure HTTPS pages. The default value is false
- `AutoSizeContentHeight`: Allows the PDF template content height to be auto resized based on the rendered HTML content height. The minimum and maximum heights are controlled by the MinContentHeight and MaxContentHeight properties. When this property is false, the content height is given by the Height property. When this property is true, if the Height property is set with a positive value and the FitHeight property is also true, the content may be scaled down to fit the specified height. The default value is true
- `BlockedHosts`: A list of hosts the converter is not allowed to access when loading the page and its resources. An entry also blocks the subdomains of the host. Hosts are compared by name, so blocking an IP address does not block the host names that resolve to it.
- `ConversionDelay`: An additional time in seconds to wait for asynchronous items to be completely loaded or for a web page redirect to finish before rendering the template HTML content. Default value is 0
- `CountEndPages`: Specifies whether the external PDF pages inserted after the HTML to PDF conversion result are included in the total page count used for page numbering. Is ignored if ShowOnlyInHtmlToPdfPages is explicitly set to false. The default value is true
- `CountStartPages`: Specifies whether the external PDF pages inserted before the HTML to PDF conversion result are included in the total page count used for page numbering. Is ignored if ShowOnlyInHtmlToPdfPages is explicitly set to false. The default value is true
- `CountTocPages`: In HTML to PDF conversion, determines whether the Table of Contents (TOC) pages inserted before the conversion result are included in the total page count used for page numbering. This setting has no effect for inline TOCs. It is ignored if ShowOnlyInHtmlToPdfPages is explicitly set to false. The default value is true
- `DestinationSize`: The template destination size in PDF page determined during rendering
- `DisableSiteIsolation`: Disables site isolation when loading content from different sources. The default value is false
- `DisableWebSecurity`: Disables web security features such as CORS and same-origin policy. The default value is false
- `EnableElevatedRendering`: Enables the HTML renderer to inherit the elevated privileges of the application using the library when the application is running elevated on Windows. The default value is true
- `EnableSoftwareGpuRendering`: When set to true, hardware GPU rendering is disabled and software rendering is used for both general page rendering and WebGL content. When set to false, hardware GPU rendering is allowed and is further controlled by GpuRenderingEnabled and GpuCompositingEnabled. The default value is false
- `FitHeight`: If true, when AutoSizeContentHeight is also true and Height was set with a positive value, the HTML content may be scaled down to fit the template height in PDF. The default value is true
- `GpuCompositingEnabled`: A flag indicating if hardware GPU compositing is enabled in the HTML to PDF converter. This property has effect only when EnableSoftwareGpuRendering is false. Hardware WebGL rendering may require both GpuRenderingEnabled and GpuCompositingEnabled to be enabled, depending on system and driver support. The default value is false
- `GpuRenderingEnabled`: A flag indicating if hardware GPU rendering is enabled in the HTML to PDF converter. This property has effect only when EnableSoftwareGpuRendering is false. The default value is false
- `Height`: Gets or sets the height of the PDF template. It is used together with AutoSizeContentHeight property to determine the destination height of the template in PDF page. When it is set with a positive value, it gives the template height if AutoSizeContentHeight is false or if AutoSizeContentHeight is true and FitHeight is also true. When it is set to a 0 or a negative value and the AutoSizeContentHeight is true, the PDF template height is auto determined based on the HTML content height. The default value is 0
- `HorizontalAlign`: Specifies the horizontal alignment of the template on the PDF page. Takes precedence over the X coordinate when set to a value other than None. This property is ignored for header and footer templates. The default value is None
- `Html`: Gets or sets the HTML content of the template. It can contain template variable like {page_number} or {total_pages}
- `HtmlBaseUrl`: Gets or sets the base URL used to resolve external resources in the HTML
- `HtmlLoaderFilePath`: Sets the full path of the HTML loader file used for HTML templates rendering
- `HtmlSourceUrl`: Gets or sets the URL from which the HTML content should be retrieved. The HTML content can contain template variable like {page_number} or {total_pages}. The Html property takes precedence if both are set
- `IgnoreCertificateErrors`: Ignores certificate validation errors such as expired, self-signed or invalid certificates. The default value is false
- `JavaScriptEnabled`: A flag indicating if JavaScript execution is enabled in HTML to PDF converter. The default is true.
- `LocalFilesEnabled`: This property controls if the converter can convert local file. The default value is true
- `LocalhostEnabled`: This property controls if the converter can convert URLs from localhost. The default value is true
- `Margins`: The margins to be applied to HTML template. The default margins are 0
- `MaxContentHeight`: Gets or sets the maximum height of the PDF template. This property is used when AutoSizeContentHeight is true to limit the height of the HTML content. The default value is 24000
- `MinContentHeight`: Gets or sets the minimum height of the PDF template. This property is used when AutoSizeContentHeight is true to limit the height of the HTML content. The defaul value is 0
- `OnPageRendering`: Optional callback invoked before the template is rendered on a page. Return true to allow rendering or false to skip rendering on the current page. This callback is evaluated after the ShowInFirstPage, ShowInOddPages and ShowInEvenPages conditions are applied. The callback receives the final placement of the template on the current page
- `Opacity`: The opacity as a value between 0 and 1 to be applied to entire template. For example, setting this property to 0.75 will lower the opacity to 75% . The default value is 1 and the rendered PDF content will have its original opacity. If you want to control only the opacity of the HTML template background, you can do this by setting the body element style to background-color: rgba(255, 255, 255, 0.75), which will set the opacity to 0.75
- `PageCounterAutoSize`: A flag indicating if the converter will automatically set the page counter width.
- `PageCounterFontScale`: A zoom factor applied to the font used to display the {page_number} and {total_pages} placeholders. It must be a value between 0.1 and 2.0. The default value is 1.0
- `PageCounterSize`: It represents the maximum number of digits for page numbering placeholders. This value is used if the PageCounterAutoSize property is false. The default value is 2
- `PageNumberOffset`: An optional offset (positive or negative) applied to the current page number when replacing the {page_number} placeholder in the template content. The default value is 0
- `PageNumbersBoldFont`: Gets or sets the default bold font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold font will be used
- `PageNumbersBoldItalicFont`: Gets or sets the default bold italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold italic font will be used
- `PageNumbersFont`: Gets or sets the default regular font used for page numbers. If not set, a default regular font will be used
- `PageNumbersItalicFont`: Gets or sets the default italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default italic font will be used
- `PrintZoom`: The zoom, in percent, at which the template HTML was drawn in the PDF. It is the zoom of the template, lowered when the browser window grew to the width of the content and by any scaling the browser applied. It is 0 before a conversion
- `RenderedSize`: The template rendered size in PDF page determined during rendering
- `RotationDegrees`: Rotation angle in degrees applied to the entire HTML template. Positive values rotate counter-clockwise. This property is ignored for header and footer templates. Default is 0
- `RotationPivot`: Pivot used when rotating the HTML template. This property is ignored for header and footer templates. Default is TopLeft
- `ShowInEvenPages`: Specifies whether the template should be displayed on even-numbered pages of the PDF document. In HTML to PDF conversion, if ShowOnlyInHtmlToPdfPages is true, the first page generated from HTML is considered odd, the second even, and so on. If ShowOnlyInHtmlToPdfPages is false, the same rule applies based on the final document. In PDF merging or editing, this refers to even-numbered pages in the final PDF. The default value is true
- `ShowInFirstPage`: Specifies whether the template should be displayed on the first page of the PDF document. In HTML to PDF conversion, the first page refers to the first page generated from HTML content if ShowOnlyInHtmlToPdfPages is true or to the first page of the final PDF document if ShowOnlyInHtmlToPdfPages is is false. In PDF merging or editing, it refers to the actual first page of the final PDF. The default value is true
- `ShowInOddPages`: Specifies whether the template should be displayed on odd-numbered pages of the PDF document. In HTML to PDF conversion, if ShowOnlyInHtmlToPdfPages is true, the first page generated from HTML is considered odd, the second even, the third odd, and so on. If ShowOnlyInHtmlToPdfPages is false, the same rule applies but based on the entire document. In PDF merging or editing, this refers to odd-numbered pages in the final PDF. The default value is true
- `ShowOnlyInHtmlToPdfPages`: In the context of HTML to PDF conversion, specifies whether the template should be displayed only on the PDF pages generated by the HTML to PDF converter, or on all PDF pages, including external PDF pages and Table of Contents (TOC) pages. The default value is true
- `SkipVariablesParsing`: A performance hint indicating that the template HTML does not contain any variable placeholders such as {page_number} or {total_pages}. When set to true and the template is provided via URL, the converter can skip downloading and inspecting the HTML content and render the HTML directly from the URL. This can improve performance by avoiding unnecessary network and parsing operations. The default value is false
- `TotalPagesOffset`: An optional offset (positive or negative) applied to the total number of pages when replacing the {total_pages} placeholder in the template content. The default value is 0
- `UncPathEnabled`: This property controls if the converter can convert files from local network. The default value is true
- `VerticalAlign`: Specifies the vertical alignment of the template on the PDF page. Takes precedence over the Y coordinate when set to a value other than None. This property is ignored for header and footer templates. The default value is None
- `Width`: Gets or sets the width of the PDF template. This property is ignored for header and footer templates because the width is determined from PDF page width
- `X`: Gets or sets the X position of the template in the PDF pages relative to the top left of the page. This property is ignored for header and footer templates
- `Y`: Gets or sets the Y position of the template in the PDF pages relative to the top left of the page. This property is ignored for header and footer templates
- `Zoom`: The zoom, in percent, at which the template HTML is laid out and drawn, from 10 to 200. For a stamp or a template added to a merged document, set it to the zoom of the document, available after a conversion in PrintZoom, so that a font size has the same size in the template and in the page. This property is ignored for header and footer templates, which use the zoom of the document. The default value is 100

## PdfHtmlTemplatePlacement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfHtmlTemplatePlacement.htm

Describes the final placement of a HTML template on a specific PDF page

### Properties

- `A`: Gets the affine transform coefficient a
- `B`: Gets the affine transform coefficient b
- `C`: Gets the affine transform coefficient c
- `D`: Gets the affine transform coefficient d
- `DocumentPageCount`: Gets the total number of pages currently available in the document
- `DocumentPageNumber`: Gets the one-based page number in the final PDF document on which the template is rendered. This value takes into account the pages counted by DocumentPagesBeforeTemplate
- `DocumentPagesBeforeTemplate`: Gets the number of pages in the final PDF document before the range of pages on which the template is rendered. This value can be affected by template settings, table of contents pages and PDF pages inserted before the HTML to PDF conversion result
- `E`: Gets the affine transform coefficient e
- `F`: Gets the affine transform coefficient f
- `IsEvenPage`: Gets a value indicating whether the current page number is even within the range of pages on which the template is rendered
- `IsFirstPage`: Gets a value indicating whether the current page is the first page in the range of pages on which the template is rendered
- `IsOddPage`: Gets a value indicating whether the current page number is odd within the range of pages on which the template is rendered
- `Opacity`: Gets the opacity applied to the template
- `PageHeight`: Gets the page height in points
- `PageWidth`: Gets the page width in points
- `RenderX`: Gets the final X position of the template relative to the top-left corner of the page
- `RenderY`: Gets the final Y position of the template relative to the top-left corner of the page
- `RotationDegrees`: Gets the rotation in degrees applied to the template
- `RotationPivot`: Gets the pivot used when rotating the template
- `TemplateHeight`: Gets the template height in points
- `TemplatePageCount`: Gets the total number of pages in the range of pages on which the template is rendered. The total number of pages currently available in the document is given by DocumentPageCount
- `TemplatePageIndex`: Gets the one-based page index within the range of pages on which the template is rendered. The actual page number in the final PDF document is given by DocumentPageNumber
- `TemplateWidth`: Gets the template width in points

## PdfTemplate

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTemplate.htm

Represents a reusable template object

### Properties

- `Height`: Gets the template height in points
- `HorizontalAlign`: Specifies the horizontal alignment of the template on the PDF page. Takes precedence over the X coordinate when set to a value other than None. The default value is None. This property cannot be changed after document rendering has started
- `OnPageRendering`: Optional callback invoked before the template is rendered on a page. Return true to allow rendering or false to skip rendering on the current page. This callback is evaluated after the ShowInFirstPage, ShowInOddPages and ShowInEvenPages conditions are applied. The callback receives the final placement of the template on the current page
- `Opacity`: The opacity as a value between 0 and 1 to be applied to entire template. For example, setting this property to 0.75 will lower the opacity to 75% . The default value is 1 and the rendered PDF content will have its original opacity
- `RotationDegrees`: Rotation angle in degrees applied to the entire template. Positive values rotate counter-clockwise. Default is 0 (no rotation). This property cannot be changed after document rendering has started
- `RotationPivot`: Pivot used when rotating the template. The default value is TopLeft. This property cannot be changed after document rendering has started
- `ShowInEvenPages`: Specifies whether the template should be displayed on even-numbered pages of the PDF document The default value is true
- `ShowInFirstPage`: Specifies whether the template should be displayed on the first page of the PDF document The default value is true
- `ShowInOddPages`: Specifies whether the template should be displayed on odd-numbered pages of the PDF document The default value is true
- `VerticalAlign`: Specifies the vertical alignment of the template on the PDF page. Takes precedence over the Y coordinate when set to a value other than None. The default value is None. This property cannot be changed after document rendering has started
- `Width`: Gets the template width in points
- `X`: Gets or sets the X position of the template in the PDF pages relative to the top left of the page. This property is ignored for header and footer templates. This property cannot be changed after document rendering has started
- `Y`: Gets or sets the Y position of the template in the PDF pages relative to the top left of the page. This property is ignored for header and footer templates. This property cannot be changed after document rendering has started

### Methods

- `AddArc`: Draws an elliptical arc into this template
- `AddCircle`: Draws a circle defined by center and radius into this template
- `AddEllipse`: Draws an ellipse inscribed in a bounding rectangle into this template
- `AddImage`: Adds an image element to the PDF template. Supports byte array or file path images, scaling and alignment. The image is rendered into the template coordinate space; the returned bounding box is relative to the template, not to any page where the template is later placed
- `AddLine`: Draws a straight line between two points into this template
- `AddPath`: Draws a generic path composed of move, line and curve operations into this template
- `AddPolygon`: Draws a closed polygon through a sequence of vertices into this template
- `AddPolyline`: Draws an open polyline through a sequence of points into this template
- `AddRectangle`: Draws a rectangle into this template
- `AddRoundedRectangle`: Draws a rectangle with rounded corners into this template
- `AddText`: Adds a text element to the PDF template. Supports placeholder variables such as {page_number} and {total_pages}, which are replaced with their actual values in the rendered PDF. When placeholder variables are used, the text element is rendered when the document is saved

## PdfTemplatePlacement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTemplatePlacement.htm

Describes the final placement of a template on a specific PDF page and maps coordinates from template space to page space

### Properties

- `A`: Gets the affine transform coefficient a
- `B`: Gets the affine transform coefficient b
- `C`: Gets the affine transform coefficient c
- `D`: Gets the affine transform coefficient d
- `DocumentPageCount`: Gets the total number of pages currently available in the document
- `E`: Gets the affine transform coefficient e
- `F`: Gets the affine transform coefficient f
- `IsEvenPage`: Gets a value indicating whether the current page number is even
- `IsFirstPage`: Gets a value indicating whether the current page is the first page
- `IsOddPage`: Gets a value indicating whether the current page number is odd
- `Opacity`: Gets the opacity applied to the template
- `PageHeight`: Gets the page height in points
- `PageNumber`: Gets the page number on which the template is rendered
- `PageWidth`: Gets the page width in points
- `RenderX`: Gets the final X position of the template relative to the top-left corner of the page
- `RenderY`: Gets the final Y position of the template relative to the top-left corner of the page
- `RotationDegrees`: Gets the rotation in degrees applied to the template
- `RotationPivot`: Gets the pivot used when rotating the template
- `TemplateHeight`: Gets the template height in points
- `TemplateWidth`: Gets the template width in points

### Methods

- `MapBoundingRectangle`: Maps a rectangle from template space to page space and returns the axis-aligned bounding rectangle
- `MapBoundingRectangle`: Maps an integer rectangle from template space to page space and returns the axis-aligned bounding rectangle
- `MapPoint`: Maps a point from template space to page space
- `MapPoints`: Maps multiple points from template space to page space
- `MapRectangle`: Maps a rectangle from template space to page space and returns the exact transformed corners

## PdfTemplateHorizontalAlign

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTemplateHorizontalAlign.htm

Specifies the horizontal alignment of a PDF template on a PDF page

### Fields and values

- `Center`: Centers the PDF template horizontally on the page
- `Left`: Aligns the left side of the PDF template with the left margin of the page
- `None`: No horizontal alignment is applied. X position will be used directly
- `Right`: Aligns the right side of the PDF template with the right margin of the page

## PdfTemplateVerticalAlign

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTemplateVerticalAlign.htm

Specifies the vertical alignment of a PDF template on a PDF page

### Fields and values

- `Bottom`: Aligns the bottom of the PDF template with the bottom of the page
- `Center`: Centers the PDF template vertically on the page
- `None`: No vertical alignment is applied. Y position will be used directly
- `Top`: Aligns the top of the PDF template with the top of the page
