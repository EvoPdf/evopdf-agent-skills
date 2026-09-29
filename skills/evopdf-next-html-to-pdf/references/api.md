# HTML to PDF: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## HtmlToPdfConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_HtmlToPdfConverter.htm

This class is the main class of the HTML to PDF Converter which offers the necessary methods to create a PDF document from a web page at given URL or from a HTML string. The generated PDF document can be saved into a memory buffer or into a file. A new converter uses the layout set by FitBrowserWindowToPage: A4 pages in portrait orientation, with the HTML laid out as in a 1024 pixel browser window and scaled to the page width. The other layout methods change it. The HTML to PDF conversion features require installing the corresponding core runtime in addition to the core library. It is recommended to use at least the HTML to PDF Converter NuGet package (or a meta-package depending on it) which provides the correct dependencies for all the HTML conversion features in the core library.

### Properties

- `AllowInsecureContent`: Allows loading insecure HTTP content in secure HTTPS pages. The default value is false
- `AuthenticationOptions`: This property can be set with a username and password to authenticate with the web server before accessing the HTML page
- `AutoResizeHtmlViewerHeight`: This property controls if the HTML viewer will be automatically resized to the HTML content height. The auto resized height is limited by the MaxHtmlViewerHeight property. This is useful to render web pages which load only the content which is visible in browser viewport. By default this property is false
- `AutoResizeHtmlViewerWidth`: A flag indicating whether the browser window grows to the width of the HTML content when the content is wider than the window, so that the whole content fits the page instead of being scaled down or cut by the browser. The default value is true
- `BlockedHosts`: A list of hosts the converter is not allowed to access when loading the page and its resources. An entry also blocks the subdomains of the host. Hosts are compared by name, so blocking an IP address does not block the host names that resolve to it
- `ConversionDelay`: An additional time in seconds to wait for asynchronous items to be completely loaded or for a web page redirect to finish before starting the rendering in HTML to PDF converter. It is used when the conversion triggering mode is Auto. Default value is 0
- `ConversionInfo`: Gets detailed information about the HTML to PDF conversion process, including the number of pages inserted before and after the HTML content, the number of pages generated from the HTML, and the total page count in the final PDF document. This property is populated only after a conversion operation completes.
- `ConvertedElementsSelector`: Gets or sets the CSS selector used to identify the elements to include in the PDF. If this is not set or matches no elements, the entire HTML document will be converted
- `DigitalSignature`: The digital signature to apply to generated PDF document
- `DisableSiteIsolation`: Disables site isolation when loading content from different sources. The default value is false
- `DisableWebSecurity`: Disables web security features such as CORS and same-origin policy. The default value is false
- `EnableElevatedRendering`: Enables the HTML renderer to inherit the elevated privileges of the application using the library when the application is running elevated on Windows. The default value is true
- `EnableSoftwareGpuRendering`: When set to true, hardware GPU rendering is disabled and software rendering is used for both general page rendering and WebGL content. When set to false, hardware GPU rendering is allowed and is further controlled by GpuRenderingEnabled and GpuCompositingEnabled. The default value is false
- `EncodeHttpPostFields`: When true (default), the converter applies application/x-www-form-urlencoded encoding to the names and values in the HttpPostFields collection when building the POST request body: spaces are encoded as '+' and reserved characters are percent-encoded. Set to false only if the names and values are already form-urlencoded by the caller.
- `ExcludedElementsSelector`: Gets or sets the CSS selector used to identify elements that should be excluded from the PDF output. These elements will be hidden or removed based on configuration
- `GpuCompositingEnabled`: A flag indicating if hardware GPU compositing is enabled in the HTML to PDF converter. This property has effect only when EnableSoftwareGpuRendering is false. Hardware WebGL rendering may require both GpuRenderingEnabled and GpuCompositingEnabled to be enabled, depending on system and driver support. The default value is false
- `GpuRenderingEnabled`: A flag indicating if hardware GPU rendering is enabled in the HTML to PDF converter. This property has effect only when EnableSoftwareGpuRendering is false. The default value is false
- `HtmlElementsInfo`: Gets the collection of HTML element metadata collected during conversion. Will be null if no data collection was triggered by HtmlElementsInfoSelector
- `HtmlElementsInfoHiddenElementsSelector`: Gets or sets the CSS selector used to identify collected HTML elements that will be hidden in the generated PDF. If null or empty, no collected HTML elements are hidden. The default value is null
- `HtmlElementsInfoIncludeIFrames`: Gets or sets a value indicating whether HtmlElementsInfoSelector is also applied to iframes. The default value is true
- `HtmlElementsInfoSelector`: Gets or sets the CSS selector used to identify HTML elements for metadata collection in the HtmlElementsInfo object. If null or empty, element data will not be collected
- `HtmlLoaderFilePath`: Sets the full path of the HTML loader file
- `HtmlViewerHeight`: Gets or sets the initial HTML viewer height, in pixels. The default value is 2048 pixels. A page as tall as the content does not depend on this value.
- `HtmlViewerWidth`: Gets or sets the width of the browser window in pixels. The HTML is loaded and laid out in this window, then printed to the PDF page. When the width is not set, it is computed at conversion time from the page layout and the zoom. Setting a value ends the automatic layout of the layout methods: the value is used as it is, until a layout method is called again. The default value is 1024 How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `HtmlViewerZoom`: Gets or sets the zoom percentage, from 10 to 200, decimals allowed, like the zoom of a browser when it prints: a lower zoom fits more content on a page, at a smaller size. When the zoom is not set, it is computed at conversion time from the page layout and the browser window width. Setting a value ends the automatic layout of the layout methods: the value is used as it is, until a layout method is called again. The default value is 100 How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `HttpPostFields`: Returns the collection of HTTP POST fields to be used when accessing a web page in HTML to PDF converter. Each entry contains a parameter name and value. By default the converter form-urlencodes the names and values when building the POST body; see EncodeHttpPostFields if the values are already encoded. If there are elements in collection then the converter will make a POST request to the web page URL with the fields from this collection, otherwise it will make a GET request
- `HttpRequestCookies`: Gets a collection of custom HTTP cookies to be sent by the HTML to PDF converter to the web server when the web page to convert and the resources (image, css, etc) referenced by the web page are requested. A cookie is defined by a name and a value pair that can be added to the collection using the Add method of the HttpRequestCookies property. A valid cookie name and a RFC 6265-encoded value must be provided.
- `HttpRequestHeaders`: Gets a collection of custom HTTP headers to be sent by the HTML to PDF converter to the web server when the web page is requested from a URL. A custom HTTP header is defined by a name and a value pair that can be added to the collection using the Add method of the HttpRequestHeaders property. A valid HTTP header name and a printable ASCII value must be provided. The custom HTTP headers can be used to define cookies, authentication options, URL referrer or any other HTTP header to be sent to the web browser. The preferred method to send cookies is to use the HttpRequestCookies property.
- `IgnoreCertificateErrors`: Ignores certificate validation errors such as expired, self-signed or invalid certificates. The default value is false
- `JavaScriptEnabled`: A flag indicating if JavaScript execution is enabled in HTML to PDF converter. The default is true.
- `LayoutMethod`: Gets the layout method in use: the last layout method called, or Manual after HtmlViewerZoom or HtmlViewerWidth was set. A new converter uses FitBrowserWindowToPage The layouts are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `LazyImagesLoadMode`: The lazy image loading mode used by the HTML to PDF converter. This flag has effect only if LoadLazyImages is also true. The default mode is Browser
- `LoadLazyImages`: A flag indicating whether the lazy images are loaded during the HTML to PDF conversion. By default, this property is set to true. For optimal performance, you can set this property to false
- `LocalFilesEnabled`: This property controls if the converter can convert local file. The default value is true
- `LocalhostEnabled`: This property controls if the converter can convert URLs from localhost. The default value is true
- `MaxHtmlViewerHeight`: Gets or sets the maximum HTML viewer height in pixels. Value 0 means unlimited. The default value is 32000 pixels
- `MaxJsRedirects`: Gets or sets the maximum number of JavaScript-initiated redirects to follow in the main frame after the initial page load and before starting PDF rendering. Set this value to 0 to disable following JavaScript redirects. The default value is 10
- `MediaType`: Gets or sets the CSS media type used to render the HTML: "screen", the default, or "print". The print media type is also applied when RepeatTableHeaderFooter is true or when a header or footer hidden on some pages has ReserveSpaceAlways set to false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `NavigationTimeout`: The HTML to PDF converter navigation timeout in seconds. Default value is 120
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date.
- `PdfDocumentOptions`: Gets a reference to the object controlling the conversion process and the generated PDF document properties like PDF document margins, PDF page size and orientation, PDF document header and footer
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document.
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer.
- `PersistentHttpRequestHeaders`: This property can be set to `true` to instruct the HTML to PDF converter to send the custom headers defined by the `HttpRequestHeaders` property whenever an external resource (e.g., images, CSS, etc.) referenced by the web page is requested. The default value of this property is `false`
- `RemoveExcludedElements`: Gets or sets a value indicating whether elements matched by ExcludedElementsSelector should be completely removed from the layout rather than just hidden. The default value is true
- `RemoveUnselectedElements`: Gets or sets a value indicating whether elements that are not matched by ConvertedElementsSelector should be completely removed from the layout rather than just hidden. The default value is true
- `ScriptToExecuteAfterLoad`: A JavaScript code to execute immediately after the page was loaded. It can be used to automatically click buttons or for other operations before conversion. By default this property is null
- `TriggeringMode`: The conversion triggering mode used by the HTML to PDF converter. The default value is Auto.
- `UncPathEnabled`: This property controls if the converter can convert files from local network. The default value is true
- `UseDelayForViewerResize`: This property controls if the ConversionDelay is also applied during the initial loading when the HTML content size is determined. This propery has effect only if the AutoResizeHtmlViewerHeight flag is on. By default this property is false for faster conversion
- `WaitForAfterLoadScript`: A flag indicating if the HTML to PDF converter should wait for the script specified by ScriptToExecuteAfterLoad to finish execution before continuing conversion. By default, this flag is set to false and the HTML to PDF conversion continues immediately after the script is injected
- `WriteHtmlToFileBeforeConvert`: When true, the HTML string is first written to a temporary file before conversion. When false, the HTML string is passed directly to the converter without file system involvement. The default value is false

### Methods

- `ConvertHtml`: Converts a HTML string to PDF using a base URL to resolve external resources and returns the rendered PDF document into a memory buffer. A new instance of HtmlToPdfConverter must be created for each call to this method
- `ConvertHtmlAsync`: Asynchronously converts a HTML string to PDF using a base URL to resolve external resources and returns the rendered PDF document into a memory buffer. A new instance of HtmlToPdfConverter must be created for each call to this method
- `ConvertHtmlToFile`: Converts a HTML string to PDF using a base URL to resolve external resources and writes the rendered PDF document into a file. A new instance of HtmlToPdfConverter must be created for each call to this method
- `ConvertHtmlToFileAsync`: Asynchronously converts a HTML string to PDF using a base URL to resolve external resources and writes the rendered PDF document into a file. A new instance of HtmlToPdfConverter must be created for each call to this method
- `ConvertUrl`: Converts an URL to PDF and returns the rendered PDF document into a memory buffer. A new instance of HtmlToPdfConverter must be created for each call to this method
- `ConvertUrlAsync`: Asynchronously converts an URL to PDF and returns the rendered PDF document into a memory buffer. A new instance of HtmlToPdfConverter must be created for each call to this method
- `ConvertUrlToFile`: Converts an URL to PDF and writes the generated PDF document into a file. A new instance of HtmlToPdfConverter must be created for each call to this method
- `ConvertUrlToFileAsync`: Asynchronously converts an URL to PDF and writes the generated PDF document into a file. A new instance of HtmlToPdfConverter must be created for each call to this method
- `FitBrowserWindowToPage`: Renders the HTML in a browser window of the given width and scales the result to fit the page width, reducing it on a page narrower than the window and enlarging it on a wider one. The scale is computed when the PDF is generated, from the page size, the orientation and the margins. To render at the page width without scaling, use LayoutAtPageWidth. This is the layout of a new converter, with an A4 page in portrait orientation and a 1024 pixel browser window The window grows to the content when the content is wider, see AutoResizeHtmlViewerWidth. The page size, the orientation, the margins and the other options can be changed after the call: the zoom is computed from them when the PDF is generated. The single page of the parameter can also be asked for after the call, with AutoResizePdfPageHeight. Setting HtmlViewerZoom or HtmlViewerWidth after the call ends the automatic layout: the values set are used as they are, without adjusting them to the page, until a layout method is called again. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `LayoutAtPageWidth`: Renders the HTML at the width of the page, without scaling, with the screen media type unless another media type is given. For HTML templates designed for a paper size, like invoices and reports The window grows to the content when the content is wider, see AutoResizeHtmlViewerWidth. The page size, the orientation, the margins and the other options can be changed after the call: the browser window width is computed from them when the PDF is generated. The single page of the parameter can also be asked for after the call, with AutoResizePdfPageHeight. Setting HtmlViewerZoom or HtmlViewerWidth after the call ends the automatic layout: the values set are used as they are, without adjusting them to the page, until a layout method is called again. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PageWidthFromBrowserWindow`: The page width follows the browser window width and the HTML is rendered without scaling, one CSS pixel being 0.75 points, or drawn uniformly larger or smaller by the zoom, on a page as wide as the window at that zoom The browser window width and the zoom are set to fixed values. Margins set after the call are added to the page width, and the page size and the orientation give the page height. When the content is wider than the window, the window grows to it and the content is scaled down to the page, see AutoResizeHtmlViewerWidth. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PrintLikeChrome`: Renders the HTML the way the Chrome browser prints it to PDF: print media type, margins of 1 cm on each side and no background colors or images The margins, PrintBackgrounds and the other options can be changed after the call; for example, setting PrintBackgrounds to true keeps the background colors and images, like the Background graphics option of Chrome. The single page of the parameter can also be asked for after the call, with AutoResizePdfPageHeight. Setting HtmlViewerZoom or HtmlViewerWidth after the call ends the automatic layout: the values set are used as they are, without adjusting them to the page, until a layout method is called again. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `SinglePageOfWidth`: Renders the HTML on a single page of the given width, as tall as the content. For receipts and tickets: 80 mm paper is 227 points, 58 mm paper is 164 points The page has exactly the given width; margins set after the call narrow the content, as with LayoutAtPageWidth. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm

## PdfDocumentOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfDocumentOptions.htm

This class encapsulates the options to control the PDF document rendering process. The HtmlToPdfConverter class defines a reference to an object of this type

### Properties

- `AccessibilityOptions`: Gets the options that control accessibility of the generated PDF document These settings only take effect when a PDF/UA or PDF/A standard requiring a tagged structure tree is selected via PdfStandard
- `AutoResizePdfPageHeight`: When true, the PDF page is as tall as the HTML content, so the whole content is on one page; the page width comes from the page size or, when AutoResizePdfPageWidth is true, from the browser window. The browser window height is not used: the page is as tall as the content. The default value is false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `AutoResizePdfPageWidth`: When true, the PDF page width follows the browser window width, HtmlViewerWidth times 0.75 points, and the HTML is drawn at HtmlViewerZoom, 100 percent unless set. The window width and the zoom are then not computed by the layout methods: the values set are used, 1024 pixels and 100 percent by default. Setting the property back to false restores the layout of the last layout method called. When false, the page has the size given by PdfPageSize and the HTML is laid out for that page. PageWidthFromBrowserWindow sets this property to true; the other layout methods set it to false. To make the page follow the browser window, prefer calling PageWidthFromBrowserWindow, which also sets the window width and the zoom. The default value is false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `EnableHeaderFooter`: Indicates whether internal browser capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), url (document location), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied
- `GenerateDocumentOutline`: A flag indicating if the generated PDF document will have an outline with bookmarks created from HTML heading tags (H1 to H6) or from any element marked with the data-heading attribute. Any element can become a bookmark with data-heading set to a level from 1 to 6 (for example data-heading="2"), the bookmark title is taken from the data-heading-text attribute when present otherwise from the element text. A real heading can be excluded with data-heading="false". The custom data-heading attribute is honored only in custom mode when UseBrowserOutlineMode is false. The default is false
- `GeneratePdfFormFields`: Gets or sets a value indicating whether HTML form controls are converted into interactive PDF form fields
- `GenerateTableOfContents`: A flag indicating if a table of contents is automatically generated in the PDF document from HTML heading tags (H1 to H6) or from any element marked with the data-heading attribute. The table of contents appearance and behavior are controlled by the options exposed through the TableOfContents property. The default is false
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), url (document location), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire HTML document will be converted to PDF
- `PdfFormOptions`: Gets the options used to convert HTML form controls into interactive PDF form fields
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the HTML to PDF converter. The default orientation is Portrait How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PdfPageSize`: This property controls the page size of the PDF document generated by the HTML to PDF converter. When AutoResizePdfPageWidth is true (the default value is false), the converter automatically resizes the PDF page width to match the HtmlViewerWidth value at the default 96 DPI resolution. When AutoResizePdfPageHeight is true (the default value is false), the converter automatically resizes the PDF page height to match the HTML content height at the default 96 DPI resolution. The page has this exact size when both properties are false. The default size of the PDF document page is A4 How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PdfStandard`: Gets or sets the PDF standards compliance level for the generated document. The default is None which means plain PDF without tagging
- `PreferCssPageSize`: When true, a @page size rule in the HTML sets the PDF page size instead of PdfPageSize. The default value is false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PrintBackgrounds`: A flag indicating if background graphics are printted by HTML to PDF converter. The default is true
- `RepeatTableHeaderFooter`: Enables automatic repetition of HTML table header (thead) and footer (tfoot) sections on each page when converting the HTML to PDF. It is useful for long tables that span multiple pages, ensuring headers and footers remain visible throughout. Styling rules intended for print layout will be applied when rendering the repeated sections. The default value is false
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. By default the right margin is 0
- `TableOfContents`: The table of contents options controlling the title, style, page numbers, inline placement and creation mode. To enable the creation of the table of contents from HTML heading tags or data-heading elements, set the GenerateTableOfContents property to true.
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. By default the top margin is 0
- `UseBrowserOutlineMode`: A flag indicating if the document outline is created in browser mode. The custom data-heading, data-heading-text and data-heading="false" attributes are honored only in custom mode. In browser mode only the standard H1 to H6 heading tags are used. The default is false

### Methods

- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
- `AddFileAttachment`: Adds a file to the document's attachments. The attachment appears in the Attachments panel of PDF viewers
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddStartPdf`: Add a PDF from a file to the list of documents to be included before the converted HTML content in the final PDF
- `AddStartPdf`: Add a PDF from a memory buffer to the list of documents to be included before the converted HTML content in the final PDF

## PdfPadding

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPadding.htm

Represents padding for a PDF element.

### Properties

- `Bottom`: Gets or sets the bottom padding.
- `Left`: Gets or sets the left padding.
- `Right`: Gets or sets the right padding.
- `Top`: Gets or sets the top padding.

### Fields and values

- `Empty`: Represents an empty padding (all values set to 0).

## TriggeringMode

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_TriggeringMode.htm

This enumeration represents the possible modes to trigger the conversion of a HTML document

### Fields and values

- `Auto`: The conversion is starts automatically after the page was loaded in converter. Optionally a conversion delay can be specified. This is the default option
- `Manual`: The conversion will be triggered manually by a call from JavaScript to evoPdfConverter_startConversion() method available in a HTML document loaded in converter

## HtmlToPdfConversionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_HtmlToPdfConversionInfo.htm

Holds information about the result of an HTML to PDF conversion. This object is populated after the conversion completes and is exposed by the ConversionInfo property of the HTML to PDF converter. It contains details about the generated pages, inserted pages and the total output

### Properties

- `ExternalPagesInsertedAfter`: Gets the number of pages inserted after the HTML to PDF conversion result. These pages may come from external PDF sources.
- `ExternalPagesInsertedBefore`: Gets the number of pages inserted before the HTML to PDF conversion result from external PDF documents
- `PagesFromHtml`: Gets the number of pages generated by the HTML to PDF conversion process.
- `PrintZoom`: The zoom, in percent, at which the HTML was drawn in the PDF. It is the zoom of the layout, lowered when the browser window grew to the width of the content and by any scaling the browser applied. It is 0 before a conversion
- `TocPagesInsertedBefore`: Gets the number of pages inserted before the HTML to PDF conversion result by table of contents
- `TotalPages`: Gets the total number of pages in the final PDF document, including inserted pages and pages generated from HTML.
- `TotalPagesInsertedBefore`: Gets the number of pages inserted before the HTML to PDF conversion result from external PDF documents and from table of contents.

## PdfDocumentCreateSettings

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfDocumentCreateSettings.htm

Represents configuration settings for creating a PDF document

### Properties

- `Language`: Document-level natural language (BCP 47, such as "en-US"). Required by PDF/UA-1 and PDF/UA-2 unless every text element specifies its own language via Accessibility.Language
- `Margins`: Gets or sets the page margins in points. Defaults to 0 for all margins
- `PageOrientation`: Gets or sets the page orientation. Defaults to Portrait
- `PageSize`: Gets or sets the page size of the PDF. Defaults to A4
- `PdfStandard`: PDF standard targeted by the document. When set to a tagging-enabled standard (PDF/UA-1, PDF/UA-2, PDF/A-2a or any combined PDF/UA + PDF/A flavor), elements added via PdfDocument.AddText, AddImage and AddRectangle are automatically inserted into the structure tree. Default value is PdfStandard.None
