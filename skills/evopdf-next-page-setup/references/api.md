# Page setup and scaling: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## HtmlToPdfConverter members used here

`HtmlToPdfConverter` is documented in full in the skill that owns it; these are the members this skill is about.

- `FitBrowserWindowToPage`: Renders the HTML in a browser window of the given width and scales the result to fit the page width, reducing it on a page narrower than the window and enlarging it on a wider one. The scale is computed when the PDF is generated, from the page size, the orientation and the margins. To render at the page width without scaling, use LayoutAtPageWidth. This is the layout of a new converter, with an A4 page in portrait orientation and a 1024 pixel browser window The window grows to the content when the content is wider, see AutoResizeHtmlViewerWidth. The page size, the orientation, the margins and the other options can be changed after the call: the zoom is computed from them when the PDF is generated. The single page of the parameter can also be asked for after the call, with AutoResizePdfPageHeight. Setting HtmlViewerZoom or HtmlViewerWidth after the call ends the automatic layout: the values set are used as they are, without adjusting them to the page, until a layout method is called again. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `LayoutAtPageWidth`: Renders the HTML at the width of the page, without scaling, with the screen media type unless another media type is given. For HTML templates designed for a paper size, like invoices and reports The window keeps the width of the paper: AutoResizeHtmlViewerWidth is set to true. The page size, the orientation, the margins and the other options can be changed after the call: the browser window width is computed from them when the PDF is generated. The single page of the parameter can also be asked for after the call, with AutoResizePdfPageHeight. Setting HtmlViewerZoom or HtmlViewerWidth after the call ends the automatic layout: the values set are used as they are, without adjusting them to the page, until a layout method is called again. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PrintLikeChrome`: Renders the HTML the way the Chrome browser prints it to PDF: print media type, margins of 1 cm on each side and no background colors or images The margins, PrintBackgrounds and the other options can be changed after the call; for example, setting PrintBackgrounds to true keeps the background colors and images, like the Background graphics option of Chrome. The single page of the parameter can also be asked for after the call, with AutoResizePdfPageHeight. Setting HtmlViewerZoom or HtmlViewerWidth after the call ends the automatic layout: the values set are used as they are, without adjusting them to the page, until a layout method is called again. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `SinglePageOfWidth`: Renders the HTML on a single page of the given width, as tall as the content. For receipts and tickets: 80 mm paper is 227 points, 58 mm paper is 164 points The page has exactly the given width; margins set after the call narrow the content, as with LayoutAtPageWidth. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PageWidthFromBrowserWindow`: The page width follows the browser window width and the HTML is rendered without scaling, one CSS pixel being 0.75 points, or drawn uniformly larger or smaller by the zoom, on a page as wide as the window at that zoom The browser window width and the zoom are set to fixed values. Margins set after the call are added to the page width, and the page size and the orientation give the page height. When the content is wider than the window, the window grows to it and the content is scaled down to the page, see AutoResizeHtmlViewerWidth. The layouts for the usual cases are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `LayoutMethod`: Gets the layout method in use: the last layout method called, or Manual after HtmlViewerZoom or HtmlViewerWidth was set. A new converter uses FitBrowserWindowToPage The layouts are described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `HtmlViewerWidth`: Gets or sets the width of the browser window in pixels. The HTML is loaded and laid out in this window, then printed to the PDF page. When the width is not set, it is computed at conversion time from the page layout and the zoom. Setting a value ends the automatic layout of the layout methods: the value is used as it is, until a layout method is called again. The default value is 1024 How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `HtmlViewerHeight`: Gets or sets the initial HTML viewer height, in pixels. The default value is 2048 pixels. A page as tall as the content does not depend on this value.
- `HtmlViewerZoom`: Gets or sets the zoom percentage, from 10 to 200, decimals allowed, like the zoom of a browser when it prints: a lower zoom fits more content on a page, at a smaller size. When the zoom is not set, it is computed at conversion time from the page layout and the browser window width. Setting a value ends the automatic layout of the layout methods: the value is used as it is, until a layout method is called again. The default value is 100 How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `MediaType`: Gets or sets the CSS media type used to render the HTML: "screen", the default, or "print". The print media type is also applied when RepeatTableHeaderFooter is true or when a header or footer hidden on some pages has ReserveSpaceAlways set to false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `AutoResizeHtmlViewerWidth`: A flag indicating whether the browser window grows to the width of the HTML content when the content is wider than the window, so that the whole content fits the page instead of being scaled down or cut by the browser. The default value is true

## PdfDocumentOptions members used here

`PdfDocumentOptions` is documented in full in the skill that owns it; these are the members this skill is about.

- `PdfPageSize`: This property controls the page size of the PDF document generated by the HTML to PDF converter. When AutoResizePdfPageWidth is true (the default value is false), the converter automatically resizes the PDF page width to match the HtmlViewerWidth value at the default 96 DPI resolution. When AutoResizePdfPageHeight is true (the default value is false), the converter automatically resizes the PDF page height to match the HTML content height at the default 96 DPI resolution. The page has this exact size when both properties are false. The default size of the PDF document page is A4 How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the HTML to PDF converter. The default orientation is Portrait How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. By default the right margin is 0
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. By default the top margin is 0
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `AutoResizePdfPageWidth`: When true, the PDF page width follows the browser window width, HtmlViewerWidth times 0.75 points, and the HTML is drawn at HtmlViewerZoom, 100 percent unless set. The window width and the zoom are then not computed by the layout methods: the values set are used, 1024 pixels and 100 percent by default. Setting the property back to false restores the layout of the last layout method called. When false, the page has the size given by PdfPageSize and the HTML is laid out for that page. PageWidthFromBrowserWindow sets this property to true; the other layout methods set it to false. To make the page follow the browser window, prefer calling PageWidthFromBrowserWindow, which also sets the window width and the zoom. The default value is false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `AutoResizePdfPageHeight`: When true, the PDF page is as tall as the HTML content, so the whole content is on one page; the page width comes from the page size or, when AutoResizePdfPageWidth is true, from the browser window. The browser window height is not used: the page is as tall as the content. The default value is false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm
- `PreferCssPageSize`: When true, a @page size rule in the HTML sets the PDF page size instead of PdfPageSize. The default value is false How the page width, the layout width and the zoom work together, with the settings for the usual cases, is described in the "HTML to PDF Page Setup and Scaling" topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-page-setup-and-scaling.htm

## HtmlToPdfConversionInfo members used here

`HtmlToPdfConversionInfo` is documented in full in the skill that owns it; these are the members this skill is about.

- `PrintZoom`: The zoom, in percent, at which the HTML was drawn in the PDF. It is the zoom of the layout, lowered when the browser window grew to the width of the content and by any scaling the browser applied. It is 0 before a conversion

## PageLayoutMethod

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PageLayoutMethod.htm

The layout method that sets how the HTML is laid out on the PDF page

### Fields and values

- `FitBrowserWindowToPage`: A fixed page with the browser window scaled to the page width. The layout of a new converter
- `LayoutAtPageWidth`: A fixed page with the HTML laid out at the page width, without scaling
- `Manual`: The browser window width and the zoom have values set directly; they are used as they are
- `PageWidthFromBrowserWindow`: A page as wide as the browser window, with the HTML drawn without scaling
- `PrintLikeChrome`: The output of the Save as PDF command of the Chrome browser
- `SinglePageOfWidth`: One page of a fixed width, as tall as the content

## PdfPageSize

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPageSize.htm

This class represents a PDF page size

### Properties

- `Height`: Gets or sets the page height in points. 1 point is 1/72 inch
- `Width`: Gets or sets the page width in points. 1 point is 1/72 inch

### Fields and values

- `A0`: Represents the A0 size of a PDF page
- `A1`: Represents the A1 size of a PDF page
- `A10`: Represents the A10 size of a PDF page
- `A2`: Represents the A2 size of a PDF page
- `A3`: Represents the A3 size of a PDF page
- `A4`: Represents the A4 size of a PDF page
- `A5`: Represents the A5 size of a PDF page
- `A6`: Represents the A6 size of a PDF page
- `A7`: Represents the A7 size of a PDF page
- `A8`: Represents the A8 size of a PDF page
- `A9`: Represents the A9 size of a PDF page
- `ArchA`: Represents the ArchA size of a PDF page
- `ArchB`: Represents the ArchB size of a PDF page
- `ArchC`: Represents the ArchC size of a PDF page
- `ArchD`: Represents the ArchD size of a PDF page
- `ArchE`: Represents the ArchE size of a PDF page
- `B0`: Represents the B0 size of a PDF page
- `B1`: Represents the B1 size of a PDF page
- `B2`: Represents the B2 size of a PDF page
- `B3`: Represents the B3 size of a PDF page
- `B4`: Represents the B4 size of a PDF page
- `B5`: Represents the B5 size of a PDF page
- `Executive`: Represents the Executive size of a PDF page
- `Flsa`: Represents the Flsa size of a PDF page
- `HalfLetter`: Represents the HalfLetter size of a PDF page
- `Ledger`: Represents the Ledger size of a PDF page
- `Legal`: Represents the Legal size of a PDF page
- `Letter`: Represents the Letter size of a PDF page
- `Letter11x17`: Represents the 11x17 size of a PDF page
- `Note`: Represents the Note size of a PDF page

## PdfPageOrientation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPageOrientation.htm

This enumeration represents the possible orientations of the PDF pages of a PDF document

### Fields and values

- `Landscape`: Landscape
- `Portrait`: Portrait

## PdfMargins

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfMargins.htm

The PDF document margins class used by PDF pages

### Properties

- `Bottom`: The bottom margin in points
- `Left`: The left margin in points
- `Right`: The right margin in points
- `Top`: The top margin in points of the PDF page

### Fields and values

- `Empty`: Represents an empty margin (all values set to 0)
