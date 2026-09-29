# HTML to image: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## HtmlToImageConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_HtmlToImageConverter.htm

This class offers the necessary methods to create a raster image from a web page at given URL or from a HTML string. The generated image can be saved into a memory buffer or into a file. The HTML to PDF conversion features require installing the corresponding core runtime in addition to the core library. It is recommended to use at least the HTML to PDF Converter NuGet package (or a meta-package depending on it) which provides the correct dependencies for all the HTML conversion features in the core library.

### Properties

- `AllowInsecureContent`: Allows loading insecure HTTP content in secure HTTPS pages. The default value is false
- `AuthenticationOptions`: This property can be set with a username and password to authenticate with the web server before accessing the HTML page
- `AutoResizeHtmlViewerHeight`: This property controls if the HTML viewer will be automatically resized to the HTML content height. The auto resized height is limited by the MaxHtmlViewerHeight property. This is useful to render web pages which load only the content which is visible in browser viewport. By default this property is false
- `BlockedHosts`: A list of hosts the converter is not allowed to access when loading the page and its resources. An entry also blocks the subdomains of the host. Hosts are compared by name, so blocking an IP address does not block the host names that resolve to it
- `CaptureEntirePage`: This property controls if the entire HTML page content will be captured in screenshot or only the area specified by the HtmlViewerHeight property. The captured content is limited by the MaxHtmlViewerHeight property. By default this property is true
- `CaptureEntirePageMode`: The content loading mode used by the HTML to Image converter. This flag has effect only if CaptureEntirePage is also true. The default mode is Browser
- `ConversionDelay`: An additional time in seconds to wait for asynchronous items to be completely loaded or for a web page redirect to finish before starting the rendering in HTML to Image converter. It is used when the conversion triggering mode is Auto. Default value is 0
- `ConvertedElementsSelector`: Gets or sets the CSS selector used to identify the elements to include in the image. If this is not set or matches no elements, the entire HTML document will be converted
- `DisableSiteIsolation`: Disables site isolation when loading content from different sources. The default value is false
- `DisableWebSecurity`: Disables web security features such as CORS and same-origin policy. The default value is false
- `EnableElevatedRendering`: Enables the HTML renderer to inherit the elevated privileges of the application using the library when the application is running elevated on Windows. The default value is true
- `EnableSoftwareGpuRendering`: When set to true, hardware GPU rendering is disabled and software rendering is used for both general page rendering and WebGL content. When set to false, hardware GPU rendering is allowed and is further controlled by GpuRenderingEnabled and GpuCompositingEnabled. The default value is false
- `EncodeHttpPostFields`: When true (default), the converter applies application/x-www-form-urlencoded encoding to the names and values in the HttpPostFields collection when building the POST request body: spaces are encoded as '+' and reserved characters are percent-encoded. Set to false only if the names and values are already form-urlencoded by the caller.
- `ExcludedElementsSelector`: Gets or sets the CSS selector used to identify elements that should be excluded from the image output. These elements will be hidden or removed based on configuration
- `GpuCompositingEnabled`: A flag indicating if hardware GPU compositing is enabled in the HTML to image converter. This property has effect only when EnableSoftwareGpuRendering is false. Hardware WebGL rendering may require both GpuRenderingEnabled and GpuCompositingEnabled to be enabled, depending on system and driver support. The default value is false
- `GpuRenderingEnabled`: A flag indicating if hardware GPU rendering is enabled in the HTML to image converter. This property has effect only when EnableSoftwareGpuRendering is false. The default value is false
- `HtmlLoaderFilePath`: Sets the full path of the HTML loader file
- `HtmlViewerHeight`: Gets or sets the intial HTML viewer height in pixels. The default value is 2048 pixels
- `HtmlViewerWidth`: Gets or sets the preferred HTML viewer width in pixels. The default value is 1024 pixels
- `HttpPostFields`: Returns the collection of HTTP POST fields to be used when accessing a web page in HTML to Image converter. Each entry contains a parameter name and value. By default the converter form-urlencodes the names and values when building the POST body; see EncodeHttpPostFields if the values are already encoded. If there are elements in collection then the converter will make a POST request to the web page URL with the fields from this collection, otherwise it will make a GET request
- `HttpRequestCookies`: Gets a collection of custom HTTP cookies to be sent by the HTML to Image converter to the web server when the web page to convert and the resources (image, css, etc) referenced by the web page are requested. A cookie is defined by a name and a value pair that can be added to the collection using the Add method of the HttpRequestCookies property. A valid cookie name and a RFC 6265-encoded value must be provided.
- `HttpRequestHeaders`: Gets a collection of custom HTTP headers to be sent by the HTML to Image converter to the web server when the web page is requested from a URL. A custom HTTP header is defined by a name and a value pair that can be added to the collection using the Add method of the HttpRequestHeaders property. A valid HTTP header name and a printable ASCII value must be provided. The custom HTTP headers can be used to define cookies, authentication options, URL referrer or any other HTTP header to be sent to the web browser. The preferred method to send cookies is to use the HttpRequestCookies property.
- `IgnoreCertificateErrors`: Ignores certificate validation errors such as expired, self-signed or invalid certificates. The default value is false
- `JavaScriptEnabled`: A flag indicating if JavaScript execution is enabled in HTML to Image converter The default is true.
- `LocalFilesEnabled`: This property controls if the converter can convert local file. The default value is true
- `LocalhostEnabled`: This property controls if the converter can convert URLs from localhost. The default value is true
- `MaxHtmlViewerHeight`: Gets or sets the maximum HTML viewer height in pixels. Value 0 means unlimited. The default value is 32000 pixels
- `MaxJsRedirects`: Gets or sets the maximum number of JavaScript-initiated redirects to follow in the main frame after the initial page load and before starting image rendering. Set this value to 0 to disable following JavaScript redirects. The default value is 10
- `MediaType`: Gets or sets the media type of the HTML document used by the HTML to Image converter. By default is not set to any value and the rendering will be for 'screen' CSS media
- `NavigationTimeout`: The HTML to Image converter navigation timeout in seconds. Default value is 120
- `PersistentHttpRequestHeaders`: This property can be set to `true` to instruct the HTML to PDF converter to send the custom headers defined by the `HttpRequestHeaders` property whenever an external resource (e.g., images, CSS, etc.) referenced by the web page is requested. The default value of this property is `false`
- `RemoveExcludedElements`: Gets or sets a value indicating whether elements matched by ExcludedElementsSelector should be completely removed from the layout rather than just hidden. The default value is true
- `RemoveUnselectedElements`: Gets or sets a value indicating whether elements that are not matched by ConvertedElementsSelector should be completely removed from the layout rather than just hidden. The default value is true
- `ScriptToExecuteAfterLoad`: A JavaScript code to execute immediately after the page was loaded. It can be used to automatically click buttons or for other operations before conversion. By default this property is null
- `TriggeringMode`: The conversion triggering mode used by the HTML to Image converter. The default value is Auto.
- `UncPathEnabled`: This property controls if the converter can convert files from local network. The default value is true
- `UseDelayForViewerResize`: This property controls if the ConversionDelay is also applied during the initial loading when the HTML content size is determined. This propery has effect only if the AutoResizeHtmlViewerHeight flag is on. By default this property is false for faster conversion
- `WaitForAfterLoadScript`: A flag indicating if the HTML to PDF converter should wait for the script specified by ScriptToExecuteAfterLoad to finish execution before continuing conversion. By default, this flag is set to false and the HTML to PDF conversion continues immediately after the script is injected
- `WriteHtmlToFileBeforeConvert`: When true, the HTML string is first written to a temporary file before conversion. When false, the HTML string is passed directly to the converter without file system involvement. The default value is false

### Methods

- `ConvertHtml`: Converts a HTML string to image using a base URL to resolve external resources and returns the rendered image into a memory buffer. A new instance of HtmlToImageConverter must be created for each call to this method
- `ConvertHtmlAsync`: Asynchronously converts a HTML string to image using a base URL to resolve external resources and returns the rendered image into a memory buffer. A new instance of HtmlToImageConverter must be created for each call to this method
- `ConvertHtmlToFile`: Converts a HTML string to image using a base URL to resolve external resources and writes the rendered image into a file. A new instance of HtmlToImageConverter must be created for each call to this method
- `ConvertHtmlToFileAsync`: Asynchronously converts a HTML string to image using a base URL to resolve external resources and writes the rendered image into a file. A new instance of HtmlToImageConverter must be created for each call to this method
- `ConvertUrl`: Converts an URL to image and returns the rendered image into a memory buffer. A new instance of HtmlToImageConverter must be created for each call to this method
- `ConvertUrlAsync`: Asynchronously converts an URL to image and returns the rendered image into a memory buffer. A new instance of HtmlToImageConverter must be created for each call to this method
- `ConvertUrlToFile`: Converts an URL to image and writes the generated image into a file. A new instance of HtmlToImageConverter must be created for each call to this method
- `ConvertUrlToFileAsync`: Asynchronously converts an URL to image and writes the generated image into a file. A new instance of HtmlToImageConverter must be created for each call to this method

## ImageType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ImageType.htm

The possible image formats

### Fields and values

- `Jpeg`: JPEG format
- `Png`: PNG format
- `Webp`: Webp format

## CaptureEntirePageMode

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_CaptureEntirePageMode.htm

This enumeration defines the possible options for capturing the image of the entire HTML page, not just the visible viewport

### Fields and values

- `Browser`: Browser mode
- `Custom`: Custom mode
