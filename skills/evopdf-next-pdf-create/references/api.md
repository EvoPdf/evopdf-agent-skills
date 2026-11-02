# Creating PDF documents from code: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfDocument

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfDocument.htm

Represents a new PDF document being constructed in memory

### Properties

- `Bookmarks`: The outline of the document, displayed as bookmarks in the navigation panel of PDF viewers. The bookmarks open pages of the document or web addresses and can be nested. The location in a page is relative to the content area inside the margins, like the other positions in the document
- `ContentHeight`: Gets the available content height of the current PDF page, after applying the top and bottom margins
- `ContentWidth`: Gets the available content width of the current PDF page, after applying the left and right margins
- `DigitalSignature`: The digital signature to apply to generated PDF document
- `Flatten`: A flag indicating if the resulted PDF is flattened: the form fields are drawn as static page content instead of interactive fields. The default value is false
- `Margins`: Gets a copy of the current PDF document margins or sets the new margins used when adding new PDF elements to document
- `OpenAction`: The action executed by the viewer when the document is opened: go to a page at a given location and zoom, go to a named destination, run JavaScript, a viewer action or open a web address. Set it to null to open the document on the first page. The default is null
- `PageNumber`: Gets the current page number in the PDF document (1-based index)
- `PageSize`: Gets the page size of the PDF document used when creating new PDF pages
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfStandard`: PDF standard targeted by this document. Set through PdfDocumentCreateSettings.PdfStandard at construction time
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer
- `TableOfContents`: The table of contents of the document, generated when the document is saved from the headings of the HTML content added to it. The table is disabled by default

### Methods

- `AddArc`: Adds an elliptical arc to the current PDF page
- `AddCircle`: Adds a circle defined by center and radius to the current PDF page
- `AddElectronicInvoice`: Embeds a Factur-X / ZUGFeRD electronic invoice in the PDF document: the invoice XML is attached under the file name the standard prescribes, with the relationship of its profile, and the Factur-X identification metadata is written in the XMP metadata. The document must be created as PDF/A-3 (PdfA3b, PdfA3u, PdfA3a or PdfUa1PdfA3a for an accessible invoice). A document carries one electronic invoice
- `AddEllipse`: Adds an ellipse inscribed in a bounding rectangle to the current PDF page
- `AddFileAttachment`: Adds a file to the document's attachments. The attachment appears in the Attachments panel of PDF viewers but has no visible icon on any page. To also display a clickable icon on a specific page, use AddFileAttachmentAnnotation
- `AddFileAttachmentAnnotation`: Adds a file attachment displayed as a clickable icon on a specific page. Clicking the icon opens or extracts the attached file. The file is also listed in the Attachments panel when the AddToAttachmentsPanel property of the annotation is true
- `AddFormField`: Adds an interactive form field to a page of the document: a text field, check box, radio button group, combo box, list box or push button. The field name must be unique in the document. When Flatten is true the field is drawn as static content instead
- `AddHtml`: Renders HTML content in the document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the document. The links, the form fields and, when the document is tagged, the structure of the rendered content are kept, and the outline generated from the HTML headings, when the converter is set to generate one, is added to the bookmarks of the document. The elements added after this call go to the last page of the content, below LastPageRectangle
- `AddHtmlAsync`: Asynchronously renders HTML content in the document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the document. The links, the form fields and, when the document is tagged, the structure of the rendered content are kept, and the outline generated from the HTML headings, when the converter is set to generate one, is added to the bookmarks of the document. The elements added after this call go to the last page of the content, below LastPageRectangle
- `AddHtmlImage`: Renders HTML content as an image in the document: the HTML is converted to an image with the converter of the element and the image is drawn from the position given by the element in the current page. When the ContinueOnNextPage property of the element is true, the default, an image taller than the space left on the page continues on new pages, inside the margins of the document, and the current page becomes the last page of the image. The elements added after this call go to that page, below LastPageRectangle
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddImage`: Adds an image element to the current PDF page. Supports byte array or file path images, scaling and alignment. When the ContinueOnNextPage property of the element is true, an image taller than the space left on the page continues on new pages, inside the margins of the document, and the current page becomes the last page of the image
- `AddJavaScript`: Adds JavaScript code that runs when the document is opened in a PDF viewer. The functions defined by the code can be called from the actions of the form fields
- `AddLine`: Adds a straight line between two points to the current PDF page
- `AddLinkAnnotation`: Adds a clickable hyperlink annotation placed on a specific page. The link can open an external URL, go to another page of the document or to a location on another page. Use one of the static factory methods of PdfLinkAnnotation (FromUrl, ToPage, ToPageLocation) to build the link
- `AddPage`: Adds a new page to the document. The page has the given size and orientation, which become the current ones for the pages added next, or the current page size when no size is given
- `AddPath`: Adds a generic path composed of move, line and curve operations to the current PDF page
- `AddPdfPage`: Draws a page of an existing PDF document in the current page, at the position and with the size, rotation and opacity given by the element. The links and the form fields of the drawn page are kept, and its structure is kept when the document is tagged and the drawn page comes from a document tagged for the same standard. The links between the pages drawn from the same document data or file lead to the drawn pages
- `AddPdfTemplate`: Creates a PDF template object with the specified position and size in the PDF page. Static content added to this template is rendered on the current page immediately. Dynamic text containing {page_number} or {total_pages} is rendered when the document is saved
- `AddPolygon`: Adds a closed polygon through a sequence of vertices to the current PDF page
- `AddPolyline`: Adds an open polyline through a sequence of points to the current PDF page
- `AddRectangle`: Adds a rectangle shape to the current page
- `AddRoundedRectangle`: Adds a rectangle with rounded corners to the current PDF page
- `AddSignatureField`: Adds an empty signature field to a page of the document. The field is signed later by a digital signature with the same field name, which is placed on the field's page and in its rectangle. A form with a field for each signer is prepared this way. The field name must be unique in the document
- `AddText`: Adds a text element to the current PDF page. When the ContinueOnNextPage property of the element is true, the text that does not fit on the current page continues on the next pages of the document. The text can contain the {page_number} and {total_pages} placeholders, replaced with the page number and the page count of the document. A text with placeholders is rendered on the current page only, even when it continues on the next pages. A text with {total_pages} is rendered when the document is saved: the returned object has no rendered rectangles and the page callbacks of the element are called at save time, when the document cannot be changed anymore
- `AddTextAnnotation`: Adds a sticky-note text annotation displayed as a clickable icon on a specific page. Clicking the icon opens a popup with the note text. The annotation also appears in the viewer's Comments panel
- `Dispose`: Releases all resources used by this instance
- `Save`: Saves the PDF document to a byte array and releases all resources; the document cannot be used afterwards
- `SaveAsync`: Asynchronously saves the PDF document to a byte array and releases all resources; the document cannot be used afterwards
- `SaveToFile`: Saves the PDF document to the specified file
- `SaveToFileAsync`: Asynchronously saves the PDF document to the specified file
- `SetPageSize`: Sets the page size and the orientation of the pages added next

## PdfTextElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextElement.htm

Represents a block of text to be rendered in a PDF document. Supports positioning, styling and multi-page continuation

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for a text block is Paragraph; set StructureType to Heading1..Heading6 for headings, or to Artifact for decorative fragments
- `Alignment`: The horizontal alignment of the text
- `BackgroundColor`: Optional background color drawn behind the text on each page where the text is rendered. The background covers the area available to the text on the page and follows the rotation of the element. When null, the default, no background is drawn
- `BackgroundOpacity`: The opacity of the background color in the range 0..1. The default value is 1 which produces a fully opaque background
- `ContinuationBottomMargin`: In a PdfEditor, the distance in points from the bottom of the pages to the lowest point the text can reach on each page, starting with the page where it starts. Has no effect in a PdfDocument, where the bottom margin of the document applies. The default value is 0
- `ContinuationTopMargin`: In a PdfEditor, the distance in points from the top of the pages where the text continues to the top of the continued text. Has no effect in a PdfDocument, where the top margin of the document applies. The default value is 0
- `ContinueOnNextPage`: When true, text that does not fit on the page continues on the next pages. In a PdfDocument the next pages are new pages and the margins of the document apply. In a PdfEditor the next pages are the following pages of the document, or pages inserted after the current one when InsertContinuationPages is true, and the ContinuationTopMargin and ContinuationBottomMargin properties give the area of the pages the text can use. The default value is false
- `Direction`: The direction in which the text is rendered (LTR or RTL) Default is LeftToRight.
- `Font`: The font used to render the text content created by PdfFontManager
- `Height`: The height of the text block. If zero or negative, the available page height is used
- `InsertContinuationPages`: In a PdfEditor, when true, the pages where the text continues are inserted after the page where it starts, so the following pages of the document keep their content. When false, the default, the text continues on the following pages of the document and new pages are added only at its end. Has no effect in a PdfDocument
- `Leading`: The line spacing multiplier used when rendering text. This value is multiplied by the font size to determine the space between lines. If zero, a default value of 1.2 is used
- `OnAfterPageRender`: Optional callback called after the text was rendered on each page, with the PdfTextPageRenderInfo of that page: the rectangle of the rendered text, its four corners when the text is rotated, its visible part, the size of the page and the part of the text drawn on the page
- `OnBeforePageRender`: Optional callback called before the text is drawn on each page, with the PdfTextPageRenderInfo of that page: the rectangle the text is about to use and the part of the text drawn on the page. The content drawn on the page from the callback, like a background or a decoration, appears under the text
- `OnPageRendered`: Optional callback called after the text was rendered on each page, with the page number and the rectangle of the rendered text. The rectangle is relative to the content area of the page in a document, to the top left corner of the page in an edited document and to the template when the text was added to a template
- `Opacity`: Opacity of the rendered text (0..1). Default is 1 (fully opaque)
- `PageNumberOffset`: An optional offset (positive or negative) applied to the current page number when replacing the {page_number} placeholder. The default value is 0
- `RotationDegrees`: Rotation angle in degrees applied to the entire text block. Positive values rotate counter-clockwise. Default is 0 (no rotation)
- `RotationPivot`: Gets or sets the pivot point used when rotating this element. The pivot determines which point of the element's bounding box remains fixed during rotation. The default value is TopLeft.
- `Text`: The text content to be rendered. Supports placeholder variables such as {page_number} and {total_pages} which are replaced with their actual values in the rendered PDF. Placeholders also work when the text is added inside a PdfTemplate that is repeated across PDF pages.
- `TotalPagesOffset`: An optional offset (positive or negative) applied to the total number of pages when replacing the {total_pages} placeholder. The default value is 0
- `Width`: The width of the text block. If zero or negative, the available page width is used
- `X`: The X position of the text block, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template
- `Y`: The Y position of the text block, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

## PdfImageElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfImageElement.htm

Represents an image element that can be rendered into a PDF document. Supports positioning, scaling and alignment

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for an image is Figure; PDF/UA-1 requires a non-empty AlternateText or ActualText. Set StructureType to Artifact to mark a purely decorative image
- `ContinuationBottomMargin`: In a PdfEditor, the distance in points from the bottom of the pages to the lowest point the image can reach on each page, starting with the page where it starts. Has no effect in a PdfDocument, where the bottom margin of the document applies. The default value is 0
- `ContinuationTopMargin`: In a PdfEditor, the distance in points from the top of the pages where the image continues to the top of the continued image. Has no effect in a PdfDocument, where the top margin of the document applies. The default value is 0
- `ContinueOnNextPage`: When true, an image taller than the space left on the page continues on the next pages: the part that does not fit above the bottom margin is drawn on the following page from the top margin, with the same horizontal position and width, and so on until the whole image is drawn. In a PdfDocument the next pages are new pages and the margins of the document apply. In a PdfEditor the next pages are the following pages of the document, or pages inserted after the current one when InsertContinuationPages is true, and the ContinuationTopMargin and ContinuationBottomMargin properties give the area of the pages the image can use. A rotated image is always drawn on one page. The default value is false: the image is drawn on one page and the part outside the page is cut
- `EnlargeToFit`: If true, the image will be enlarged to fit the specified dimensions if needed
- `Height`: The target height for the image. If zero, original height is used unless scaling rules apply
- `HorizontalAlign`: The horizontal alignment of the image inside the content area of the page, or inside the page in an edited document. When not None, it takes precedence over X
- `ImageBytes`: The image content as a byte array. Optional if ImagePath is used
- `ImagePath`: The file path to the image. Optional if ImageBytes is used
- `InsertContinuationPages`: In a PdfEditor, when true, the pages where the image continues are inserted after the page where it starts, so the following pages of the document keep their content. When false, the default, the image continues on the following pages of the document and new pages are added only at its end. Has no effect in a PdfDocument
- `KeepAspectRatio`: If true, the default, the image keeps its aspect ratio when it is scaled to the specified dimensions. If false and both Width and Height are set, the image is stretched to exactly those dimensions
- `Opacity`: Opacity of the rendered image (0..1). Default is 1 (fully opaque)
- `RotationDegrees`: Rotation angle in degrees applied to the image. Positive values rotate counter-clockwise. Default is 0 (no rotation)
- `RotationPivot`: Gets or sets the pivot point used when rotating this image. The pivot determines which point of the image bounding box remains fixed during rotation. The default value is TopLeft
- `ScaleDownToFit`: If true, the image will be scaled down to fit the specified dimensions if needed
- `VerticalAlign`: The vertical alignment of the image inside the content area of the page, or inside the page in an edited document. When not None, it takes precedence over Y
- `Width`: The target width for the image. If zero, original width is used unless scaling rules apply
- `X`: The X position of the image, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template. Ignored when HorizontalAlign is set
- `Y`: The Y position of the image, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template. Ignored when VerticalAlign is set

## PdfRectangleElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRectangleElement.htm

Defines a rectangle to be drawn in the PDF document. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for a rectangle is Artifact, marking it as decorative graphic excluded from the structure tree. To make the rectangle semantic, set Accessibility.StructureType to a real role such as Figure and provide AlternateText
- `Border`: The stroke style for the rectangle border, including width, dash pattern, line cap and corner join style
- `BorderColor`: The border color of the rectangle. When null the rectangle is only filled
- `BorderOpacity`: The opacity of the rectangle border in the range 0..1. The default value is 1 which produces a fully opaque border
- `FillColor`: The fill color of the rectangle. When null the rectangle is only stroked
- `FillOpacity`: The opacity of the rectangle fill in the range 0..1. The default value is 1 which produces a fully opaque fill
- `GradientFill`: A gradient painted inside the rectangle instead of FillColor. When null the fill color is used
- `Height`: The height of the rectangle in points
- `RotationDegrees`: The rotation angle in degrees applied to the rectangle. Positive values rotate counter-clockwise. The default value is 0 which produces no rotation
- `RotationPivot`: The pivot point used when rotating this rectangle. The pivot determines which point of the rectangle bounding box remains fixed during rotation. The default value is TopLeft
- `Width`: The width of the rectangle in points
- `X`: The X position from the top-left corner of the page in points
- `Y`: The Y position from the top-left corner of the page in points

## PdfRoundedRectangleElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRoundedRectangleElement.htm

Defines a rectangle with rounded corners drawn in a PDF document. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type is Artifact, marking the element as decorative graphic excluded from the structure tree
- `Border`: The stroke style for the border, including width, dash pattern and corner join style
- `BorderColor`: The border color of the rectangle. When null the rectangle is only filled
- `BorderOpacity`: The opacity of the rectangle border in the range 0..1. The default value is 1 which produces a fully opaque border
- `CornerRadius`: The radius applied to each of the four corners in points. Values are automatically clamped to half of the shorter side at render time to keep the geometry consistent
- `FillColor`: The fill color of the rectangle. When null the rectangle is only stroked
- `FillOpacity`: The opacity of the rectangle fill in the range 0..1. The default value is 1 which produces a fully opaque fill
- `GradientFill`: A gradient painted inside the rectangle instead of FillColor. When null the fill color is used
- `Height`: The height of the rectangle in points
- `RotationDegrees`: The rotation angle in degrees applied to the rectangle. Positive values rotate counter-clockwise. The default value is 0 which produces no rotation
- `RotationPivot`: The pivot point used when rotating this rectangle. The default value is TopLeft
- `Width`: The width of the rectangle in points
- `X`: The X position from the top-left corner of the page in points
- `Y`: The Y position from the top-left corner of the page in points

## PdfCircleElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfCircleElement.htm

Defines a circle drawn in a PDF document, specified by its center point and radius. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for a circle is Artifact, marking it as decorative graphic excluded from the structure tree
- `Border`: The stroke style for the circle outline, including width and dash pattern
- `BorderColor`: The border color of the circle. When null the circle is only filled
- `BorderOpacity`: The opacity of the circle border in the range 0..1. The default value is 1 which produces a fully opaque border
- `CenterX`: The X coordinate of the center in points, measured from the top-left corner of the page
- `CenterY`: The Y coordinate of the center in points, measured from the top-left corner of the page
- `FillColor`: The fill color of the circle. When null the circle is only stroked
- `FillOpacity`: The opacity of the circle fill in the range 0..1. The default value is 1 which produces a fully opaque fill
- `GradientFill`: A gradient painted inside the circle instead of FillColor. When null the fill color is used
- `Radius`: The radius of the circle in points

## PdfEllipseElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfEllipseElement.htm

Defines an ellipse drawn in a PDF document, inscribed within an axis-aligned bounding rectangle before any rotation is applied. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for an ellipse is Artifact, marking it as decorative graphic excluded from the structure tree
- `Border`: The stroke style for the ellipse outline, including width and dash pattern
- `BorderColor`: The border color of the ellipse. When null the ellipse is only filled
- `BorderOpacity`: The opacity of the ellipse border in the range 0..1. The default value is 1 which produces a fully opaque border
- `FillColor`: The fill color of the ellipse. When null the ellipse is only stroked
- `FillOpacity`: The opacity of the ellipse fill in the range 0..1. The default value is 1 which produces a fully opaque fill
- `GradientFill`: A gradient painted inside the ellipse instead of FillColor. When null the fill color is used
- `Height`: The height of the bounding rectangle in points. The vertical axis of the ellipse equals this value
- `RotationDegrees`: The rotation angle in degrees applied to the ellipse. Positive values rotate counter-clockwise. The default value is 0 which produces no rotation
- `RotationPivot`: The pivot point used when rotating this ellipse. The pivot is interpreted on the bounding rectangle of the ellipse. The default value is TopLeft
- `Width`: The width of the bounding rectangle in points. The horizontal axis of the ellipse equals this value
- `X`: The X position of the bounding rectangle from the top-left corner of the page in points
- `Y`: The Y position of the bounding rectangle from the top-left corner of the page in points

## PdfArcElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfArcElement.htm

Defines an elliptical arc drawn in a PDF document. The arc is a portion of the ellipse inscribed in the axis-aligned rectangle defined by X, Y, Width and Height. The arc starts at StartAngleDegrees and spans SweepAngleDegrees degrees, measured counter-clockwise from the positive X axis of that bounding rectangle. When Width equals Height the underlying ellipse is a circle and the arc is a circular arc. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for an arc is Artifact, marking it as decorative graphic excluded from the structure tree
- `Border`: The style of the arc border: width, dash pattern and endpoint cap style
- `BorderColor`: The border color of the arc. When null the arc is only filled
- `BorderOpacity`: The opacity of the arc border in the range 0..1. The default value is 1 which produces a fully opaque border
- `Closure`: How the arc is closed and whether it can be filled. The default value is Open which produces a stroked curve without fill
- `FillColor`: The fill color used when Closure is Chord or Pie. When null, or when the closure is Open, the arc is only stroked
- `FillOpacity`: The opacity of the arc fill in the range 0..1. The default value is 1 which produces a fully opaque fill
- `GradientFill`: A gradient painted inside the closed arc instead of FillColor. When null the fill color is used
- `Height`: The height, in points, of the bounding rectangle of the ellipse the arc is part of. Equals the length of the vertical axis of that ellipse
- `RotationDegrees`: The rotation angle in degrees applied to the arc, in addition to the angular range defined by StartAngleDegrees and SweepAngleDegrees. Positive values rotate counter-clockwise. The default value is 0 which produces no extra rotation
- `RotationPivot`: The pivot point used when rotating this arc. The pivot is interpreted on the bounding rectangle of the arc. The default value is TopLeft
- `StartAngleDegrees`: The starting angle of the arc in degrees, measured counter-clockwise from the positive X axis of the bounding rectangle of the ellipse the arc is part of
- `SweepAngleDegrees`: The angular extent of the arc in degrees. Positive values sweep counter-clockwise, negative values sweep clockwise. A full ellipse corresponds to a sweep of 360 or -360 degrees
- `Width`: The width, in points, of the bounding rectangle of the ellipse the arc is part of. Equals the length of the horizontal axis of that ellipse
- `X`: The X position, in points, of the top-left corner of the bounding rectangle of the ellipse the arc is part of. Measured from the top-left corner of the page
- `Y`: The Y position, in points, of the top-left corner of the bounding rectangle of the ellipse the arc is part of. Measured from the top-left corner of the page

## PdfArcClosureType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfArcClosureType.htm

Specifies how an arc element is closed and whether it can be filled

### Fields and values

- `Chord`: The start and end points of the arc are connected by a straight line. The resulting closed region can be filled
- `Open`: The arc remains an open curve between its start and end points. Only the stroke is drawn; fill color is ignored
- `Pie`: The start and end points of the arc are connected to the center of the bounding rectangle, producing a pie slice that can be filled

## PdfLineElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLineElement.htm

Defines a straight line segment drawn between two points in a PDF document. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for a line is Artifact, marking it as decorative graphic excluded from the structure tree
- `LineColor`: The stroke color of the line. The default value is black
- `LineOpacity`: The opacity of the line in the range 0..1. The default value is 1 which produces a fully opaque line
- `LineStyle`: The stroke style for the line, including width, dash pattern and endpoint cap style
- `X1`: The X coordinate of the start point in points, measured from the top-left corner of the page
- `X2`: The X coordinate of the end point in points, measured from the top-left corner of the page
- `Y1`: The Y coordinate of the start point in points, measured from the top-left corner of the page
- `Y2`: The Y coordinate of the end point in points, measured from the top-left corner of the page

## PdfPathElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPathElement.htm

Defines a generic path composed of move, line, cubic Bezier and close operations. Use this element when none of the dedicated shape elements is expressive enough to describe the desired geometry. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type is Artifact, marking the path as decorative graphic excluded from the structure tree
- `Border`: The line style for the path, including width, dash pattern, endpoint cap style and corner join style
- `BorderColor`: The border color of the path. When null the path is only filled (provided FillColor is non-null)
- `BorderOpacity`: The opacity of the path border in the range 0..1. The default value is 1 which produces a fully opaque border
- `FillColor`: The fill color of the path. When null the path is not filled. When non-null the path is filled using the non-zero winding rule before any line stroke is applied
- `FillOpacity`: The opacity of the path fill in the range 0..1. The default value is 1 which produces a fully opaque fill
- `GradientFill`: A gradient painted inside the path instead of FillColor. When null the fill color is used
- `Operations`: The ordered list of operations that build up the path. Operations are evaluated in order, with the current point tracked across them. The first operation must be a MoveTo; rendering throws an InvalidOperationException when the first operation is anything else
- `RotationDegrees`: The rotation angle in degrees applied to the path. Positive values rotate counter-clockwise. The pivot is interpreted on the axis-aligned bounding box of the path
- `RotationPivot`: The pivot point used when rotating this path. The pivot is interpreted on the axis-aligned bounding box of the path. The default value is TopLeft

### Methods

- `Close`: Appends a close operation that closes the current subpath with a straight line segment back to its starting point. Returns this instance so calls can be chained
- `CurveTo`: Appends a curve-to operation that draws a cubic Bezier curve from the current point to the specified end point, using two control points. Returns this instance so calls can be chained
- `LineTo`: Appends a line-to operation that draws a straight line from the current point to the specified end point. Returns this instance so calls can be chained
- `MoveTo`: Appends a move-to operation that begins a new subpath at the specified point. Returns this instance so calls can be chained

## PdfPathOperation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPathOperation.htm

Represents a single drawing operation that contributes to the geometry of a PdfPathElement. Use the factory methods to construct operations rather than directly populating fields, since each operation type uses a different subset of coordinates

### Properties

- `Control1`: First control point of a cubic Bezier curve. Only meaningful for CurveTo
- `Control2`: Second control point of a cubic Bezier curve. Only meaningful for CurveTo
- `OperationType`: The kind of operation this instance represents
- `Point`: Destination point of the operation. For MoveTo this is the new current point; for LineTo and CurveTo this is the end point of the segment. Null for Close

### Methods

- `Close`: Creates a close operation that closes the current subpath with a straight line segment back to its starting point
- `CurveTo`: Creates a curve-to operation that appends a cubic Bezier curve segment from the current point to the specified end point, using two control points
- `LineTo`: Creates a line-to operation that appends a straight line segment from the current point to the specified end point
- `MoveTo`: Creates a move-to operation that begins a new subpath at the specified point without producing a line segment

## PdfPathOperationType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPathOperationType.htm

Identifies the type of operation stored in a PdfPathOperation

### Fields and values

- `Close`: Closes the current subpath by appending a straight line segment back to the starting point of the subpath
- `CurveTo`: Appends a cubic Bezier curve segment from the current point using two control points and an end point
- `LineTo`: Appends a straight line segment from the current point to the operation point
- `MoveTo`: Begins a new subpath by moving the current point to the operation point, without producing any line segment

## PdfPolygonElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPolygonElement.htm

Defines a closed polygon drawn through a sequence of points in a PDF document. The polygon can be both stroked and filled. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for a polygon is Artifact, marking it as decorative graphic excluded from the structure tree
- `Border`: The stroke style for the polygon outline, including width, dash pattern and corner join style
- `BorderColor`: The border color of the polygon. When null the polygon is only filled
- `BorderOpacity`: The opacity of the polygon border in the range 0..1. The default value is 1 which produces a fully opaque border
- `FillColor`: The fill color of the polygon. When null the polygon is only stroked
- `FillOpacity`: The opacity of the polygon fill in the range 0..1. The default value is 1 which produces a fully opaque fill
- `GradientFill`: A gradient painted inside the polygon instead of FillColor. When null the fill color is used
- `Points`: The ordered list of polygon vertices, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template. The polygon is automatically closed by connecting the last vertex back to the first
- `RotationDegrees`: The rotation angle in degrees applied to the polygon. Positive values rotate counter-clockwise
- `RotationPivot`: The pivot point used when rotating this polygon. The pivot is interpreted on the axis-aligned bounding box of the vertices. The default value is TopLeft

## PdfPolylineElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPolylineElement.htm

Defines an open polyline drawn through a sequence of points in a PDF document. The polyline is stroked but not filled. The coordinates are in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

### Properties

- `Accessibility`: Accessibility metadata used when the host PdfDocument is tagged. The default structure type for a polyline is Artifact, marking it as decorative graphic excluded from the structure tree
- `LineColor`: The stroke color of the polyline. The default value is black
- `LineOpacity`: The opacity of the line in the range 0..1. The default value is 1 which produces a fully opaque line
- `LineStyle`: The stroke style for the polyline, including width, dash pattern, endpoint cap style and corner join style
- `Points`: The ordered list of points the polyline passes through, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template. The polyline is drawn as a sequence of straight line segments connecting consecutive points
- `RotationDegrees`: The rotation angle in degrees applied to the polyline. Positive values rotate counter-clockwise. The pivot is interpreted on the axis-aligned bounding box of the points
- `RotationPivot`: The pivot point used when rotating this polyline. The pivot is interpreted on the axis-aligned bounding box of the points. The default value is TopLeft

## PdfLineStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLineStyle.htm

Defines the appearance of a stroked line, including width, dash pattern, endpoint shape and corner join style

### Properties

- `CustomDashPattern`: A custom dash pattern expressed as alternating on/off segment lengths in points. When non-null and non-empty it takes precedence over DashStyle. A null or empty value selects the dash pattern from DashStyle
- `DashPhase`: The phase offset, in points, applied to the dash pattern. The default value is 0
- `DashStyle`: The predefined dash pattern of the line. Ignored when CustomDashPattern is set to a non-null, non-empty array
- `LineCap`: The shape used at the endpoints of stroked open paths such as lines and arcs. The default value is Butt
- `LineJoin`: The shape used at corners where two stroked segments meet, such as the corners of a rectangle or the vertices of a polygon. The default value is Miter
- `LineWidth`: The width of the line in points. The default value is 1
- `MiterLimit`: The maximum ratio between the miter length and the line width before a miter join is automatically converted to a bevel join. The default value is 10

## PdfLineCapStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLineCapStyle.htm

Specifies the shape used at the endpoints of a stroked open path

### Fields and values

- `Butt`: The stroke is squared off exactly at the endpoint, with no projection beyond it
- `ProjectingSquare`: The stroke continues beyond the endpoint by a distance equal to half the line width. The endpoint is squared off
- `Round`: A semicircular arc with diameter equal to the line width is drawn around the endpoint. The stroke is rounded off at the endpoint

## PdfLineDashStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLineDashStyle.htm

Specifies the dash style for line rendering

### Fields and values

- `Dashed`: Dashed line style
- `Dotted`: Dotted line style
- `Solid`: Solid continuous line

## PdfLineJoinStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLineJoinStyle.htm

Specifies the shape used at corners where two stroked path segments meet

### Fields and values

- `Bevel`: The corner is filled with a flat triangle, producing a chamfered effect
- `Miter`: The outer edges of the strokes extend until they meet at a sharp corner. When the corner is too acute, the join falls back to Bevel based on the miter limit
- `Round`: An arc with diameter equal to the line width is drawn around the corner, producing a smooth rounded join

## PdfColor

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfColor.htm

Represents a color used in PDF content

### Properties

- `AliceBlue`: Gets the AliceBlue color
- `AntiqueWhite`: Gets the AntiqueWhite color
- `Aqua`: Gets the Aqua color
- `Aquamarine`: Gets the Aquamarine color
- `Azure`: Gets the Azure color
- `B`: Gets the blue component value for RGB colors
- `Beige`: Gets the Beige color
- `Bisque`: Gets the Bisque color
- `Black`: Gets the black color
- `BlackComponent`: Gets the black component value for CMYK colors
- `BlanchedAlmond`: Gets the BlanchedAlmond color
- `Blue`: Gets the blue color
- `BlueViolet`: Gets the BlueViolet color
- `Brown`: Gets the Brown color
- `BurlyWood`: Gets the BurlyWood color
- `CadetBlue`: Gets the CadetBlue color
- `Chartreuse`: Gets the Chartreuse color
- `Chocolate`: Gets the Chocolate color
- `ColorModel`: Gets the color model
- `Coral`: Gets the Coral color
- `CornflowerBlue`: Gets the CornflowerBlue color
- `Cornsilk`: Gets the Cornsilk color
- `Crimson`: Gets the Crimson color
- `Cyan`: Gets the cyan color
- `CyanComponent`: Gets the cyan component value for CMYK colors
- `DarkBlue`: Gets the DarkBlue color
- `DarkCyan`: Gets the DarkCyan color
- `DarkGoldenRod`: Gets the DarkGoldenRod color
- `DarkGray`: Gets the dark gray color
- `DarkGreen`: Gets the DarkGreen color
- `DarkKhaki`: Gets the DarkKhaki color
- `DarkMagenta`: Gets the DarkMagenta color
- `DarkOliveGreen`: Gets the DarkOliveGreen color
- `DarkOrange`: Gets the DarkOrange color
- `DarkOrchid`: Gets the DarkOrchid color
- `DarkRed`: Gets the DarkRed color
- `DarkSalmon`: Gets the DarkSalmon color
- `DarkSeaGreen`: Gets the DarkSeaGreen color
- `DarkSlateBlue`: Gets the DarkSlateBlue color
- `DarkSlateGray`: Gets the DarkSlateGray color
- `DarkSlateGrey`: Gets the DarkSlateGrey color
- `DarkTurquoise`: Gets the DarkTurquoise color
- `DarkViolet`: Gets the DarkViolet color
- `DeepPink`: Gets the DeepPink color
- `DeepSkyBlue`: Gets the DeepSkyBlue color
- `DimGray`: Gets the DimGray color
- `DimGrey`: Gets the DimGrey color
- `DodgerBlue`: Gets the DodgerBlue color
- `FireBrick`: Gets the FireBrick color
- `FloralWhite`: Gets the FloralWhite color
- `ForestGreen`: Gets the ForestGreen color
- `Fuchsia`: Gets the Fuchsia color
- `G`: Gets the green component value for RGB colors
- `Gainsboro`: Gets the Gainsboro color
- `GhostWhite`: Gets the GhostWhite color
- `Gold`: Gets the Gold color
- `GoldenRod`: Gets the GoldenRod color
- `Gray`: Gets the gray color
- `Green`: Gets the green color
- `GreenYellow`: Gets the GreenYellow color
- `Grey`: Gets the Grey color
- `HoneyDew`: Gets the HoneyDew color
- `HotPink`: Gets the HotPink color
- `IndianRed`: Gets the IndianRed color
- `Indigo`: Gets the Indigo color
- `IsCmyk`: Gets a value indicating whether the color uses the CMYK model
- `IsRgb`: Gets a value indicating whether the color uses the RGB model
- `Ivory`: Gets the Ivory color
- `Khaki`: Gets the Khaki color
- `Lavender`: Gets the Lavender color
- `LavenderBlush`: Gets the LavenderBlush color
- `LawnGreen`: Gets the LawnGreen color
- `LemonChiffon`: Gets the LemonChiffon color
- `LightBlue`: Gets the LightBlue color
- `LightCoral`: Gets the LightCoral color
- `LightCyan`: Gets the LightCyan color
- `LightGoldenRodYellow`: Gets the LightGoldenRodYellow color
- `LightGray`: Gets the light gray color
- `LightGreen`: Gets the LightGreen color
- `LightGrey`: Gets the LightGrey color
- `LightPink`: Gets the LightPink color
- `LightSalmon`: Gets the LightSalmon color
- `LightSeaGreen`: Gets the LightSeaGreen color
- `LightSkyBlue`: Gets the LightSkyBlue color
- `LightSlateGray`: Gets the LightSlateGray color
- `LightSlateGrey`: Gets the LightSlateGrey color
- `LightSteelBlue`: Gets the LightSteelBlue color
- `LightYellow`: Gets the LightYellow color
- `Lime`: Gets the Lime color
- `LimeGreen`: Gets the LimeGreen color
- `Linen`: Gets the Linen color
- `Magenta`: Gets the magenta color
- `MagentaComponent`: Gets the magenta component value for CMYK colors
- `Maroon`: Gets the Maroon color
- `MediumAquaMarine`: Gets the MediumAquaMarine color
- `MediumBlue`: Gets the MediumBlue color
- `MediumOrchid`: Gets the MediumOrchid color
- `MediumPurple`: Gets the MediumPurple color
- `MediumSeaGreen`: Gets the MediumSeaGreen color
- `MediumSlateBlue`: Gets the MediumSlateBlue color
- `MediumSpringGreen`: Gets the MediumSpringGreen color
- `MediumTurquoise`: Gets the MediumTurquoise color
- `MediumVioletRed`: Gets the MediumVioletRed color
- `MidnightBlue`: Gets the MidnightBlue color
- `MintCream`: Gets the MintCream color
- `MistyRose`: Gets the MistyRose color
- `Moccasin`: Gets the Moccasin color
- `NavajoWhite`: Gets the NavajoWhite color
- `Navy`: Gets the Navy color
- `OldLace`: Gets the OldLace color
- `Olive`: Gets the Olive color
- `OliveDrab`: Gets the OliveDrab color
- `Orange`: Gets the orange color
- `OrangeRed`: Gets the OrangeRed color
- `Orchid`: Gets the Orchid color
- `PaleGoldenRod`: Gets the PaleGoldenRod color
- `PaleGreen`: Gets the PaleGreen color
- `PaleTurquoise`: Gets the PaleTurquoise color
- `PaleVioletRed`: Gets the PaleVioletRed color
- `PapayaWhip`: Gets the PapayaWhip color
- `PeachPuff`: Gets the PeachPuff color
- `Peru`: Gets the Peru color
- `Pink`: Gets the pink color
- `Plum`: Gets the Plum color
- `PowderBlue`: Gets the PowderBlue color
- `Purple`: Gets the Purple color
- `R`: Gets the red component value for RGB colors
- `RebeccaPurple`: Gets the RebeccaPurple color
- `Red`: Gets the red color
- `RosyBrown`: Gets the RosyBrown color
- `RoyalBlue`: Gets the RoyalBlue color
- `SaddleBrown`: Gets the SaddleBrown color
- `Salmon`: Gets the Salmon color
- `SandyBrown`: Gets the SandyBrown color
- `SeaGreen`: Gets the SeaGreen color
- `SeaShell`: Gets the SeaShell color
- `Sienna`: Gets the Sienna color
- `Silver`: Gets the Silver color
- `SkyBlue`: Gets the SkyBlue color
- `SlateBlue`: Gets the SlateBlue color
- `SlateGray`: Gets the SlateGray color
- `SlateGrey`: Gets the SlateGrey color
- `Snow`: Gets the Snow color
- `SpringGreen`: Gets the SpringGreen color
- `SteelBlue`: Gets the SteelBlue color
- `Tan`: Gets the Tan color
- `Teal`: Gets the Teal color
- `Thistle`: Gets the Thistle color
- `Tomato`: Gets the Tomato color
- `Turquoise`: Gets the Turquoise color
- `Violet`: Gets the Violet color
- `Wheat`: Gets the Wheat color
- `White`: Gets the white color
- `WhiteSmoke`: Gets the WhiteSmoke color
- `Yellow`: Gets the yellow color
- `YellowComponent`: Gets the yellow component value for CMYK colors
- `YellowGreen`: Gets the YellowGreen color

### Methods

- `CreateBrighter`: Creates a brighter color from the current color
- `CreateDarker`: Creates a darker color from the current color
- `Equals`: Determines whether the specified color is equal to the current color
- `Equals`: Determines whether the specified object is equal to the current color
- `FromCmyk`: Creates a color from cyan, magenta, yellow, and black components
- `FromHex`: Creates a color from a hexadecimal color string
- `FromName`: Creates a color from a named web color
- `FromRgb`: Creates a color from red, green, and blue components
- `GetHashCode`: Serves as the default hash function
- `ToHex`: Returns a hexadecimal representation of the color
- `ToString`: Returns the string representation of the current color
- `TryFromName`: Attempts to create a color from a named web color
- `op_Equality`: Determines whether two specified colors are equal
- `op_Inequality`: Determines whether two specified colors are different

## PdfColorModel

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfColorModel.htm

Represents the color model used by a color instance

### Fields and values

- `Cmyk`: CMYK color model
- `Rgb`: RGB color model

## PdfElementHorizontalAlign

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfElementHorizontalAlign.htm

Specifies the horizontal alignment of a PDF element on a PDF page

### Fields and values

- `Center`: Centers the PDF element horizontally on the page
- `Left`: Aligns the left side of the PDF element with the left margin of the page
- `None`: No horizontal alignment is applied. X position will be used directly
- `Right`: Aligns the right side of the PDF element with the right margin of the page

## PdfElementVerticalAlign

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfElementVerticalAlign.htm

Specifies the vertical alignment of a PDF element on a PDF page

### Fields and values

- `Bottom`: Aligns the bottom of the PDF element with the bottom of the page
- `Center`: Centers the PDF element vertically on the page
- `None`: No vertical alignment is applied. Y position will be used directly
- `Top`: Aligns the top of the PDF element with the top of the page

## PdfTextAlignment

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextAlignment.htm

Specifies the horizontal alignment of text when rendering in a PDF document

### Fields and values

- `Center`: Centers the text horizontally within the bounding area
- `Justified`: Justifies the text so that it spans the entire width of the bounding area
- `Left`: Aligns the text to the left edge of the bounding area
- `Right`: Aligns the text to the right edge of the bounding area

## PdfTextDirection

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextDirection.htm

Specifies the direction in which the text flows in the PDF

### Fields and values

- `LeftToRight`: Text flows from left to right
- `RightToLeft`: Text flows from right to left

## PdfRotationPivot

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRotationPivot.htm

Defines the pivot point used when rotating a PDF element. The pivot represents the fixed point around which the element is rotated

### Fields and values

- `BottomCenter`: Rotates the element around the midpoint of its bottom edge
- `BottomLeft`: Rotates the element around its bottom-left corner
- `BottomRight`: Rotates the element around its bottom-right corner
- `Center`: Rotates the element around the center of its bounding box
- `LeftCenter`: Rotates the element around the midpoint of its left edge
- `RightCenter`: Rotates the element around the midpoint of its right edge
- `TopCenter`: Rotates the element around the midpoint of its top edge
- `TopLeft`: Rotates the element around its top-left corner
- `TopRight`: Rotates the element around its top-right corner

## PdfPointF

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPointF.htm

Represents a point in the public PDF coordinate space used by the library

### Properties

- `X`: Gets the X coordinate
- `Y`: Gets the Y coordinate

## PdfRectangle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRectangle.htm

Encapsulates a rectangle coordinates

### Properties

- `Bottom`: The Y coordinate of the bottom edge of the rectangle (Y + Height)
- `Height`: The rectangle height in points
- `Left`: The X coordinate of the left edge of the rectangle (same as X)
- `Right`: The X coordinate of the right edge of the rectangle (X + Width)
- `Top`: The Y coordinate of the top edge of the rectangle (same as Y)
- `Width`: The rectangle width in points
- `X`: The top left corner X coordinate in points
- `Y`: The top left corner Y coordinate in points

## PdfRectangleF

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRectangleF.htm

Represents a rectangle in the PDF coordinate space used by the library

### Properties

- `Bottom`: Gets the bottom edge
- `Height`: Gets the height
- `Left`: Gets the left edge
- `Right`: Gets the right edge
- `Top`: Gets the top edge
- `Width`: Gets the width
- `X`: Gets the X coordinate of the top-left corner
- `Y`: Gets the Y coordinate of the top-left corner

### Methods

- `FromRectangle`: Creates a floating-point rectangle from an integer rectangle
- `ToRectangle`: Converts the floating-point rectangle to an integer rectangle by rounding outward

## PdfQuadF

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfQuadF.htm

Represents the four transformed corners of a rectangle in page space

### Properties

- `BottomLeft`: Gets the transformed bottom-left corner
- `BottomRight`: Gets the transformed bottom-right corner
- `TopLeft`: Gets the transformed top-left corner
- `TopRight`: Gets the transformed top-right corner

## PdfElementRenderInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfElementRenderInfo.htm

Contains metadata about where an element was rendered in the document. Each entry in RenderedRectangles describes one occurrence on a specific page or template

### Properties

- `LastPageRectangle`: The last entry from RenderedRectangles, or null when the element was not rendered
- `RenderedRectangles`: The list of areas where the element was rendered, in order. Static graphic elements such as lines, circles, ellipses, rectangles and paths produce a single entry; text elements that span multiple pages produce one entry per page

## PdfTextRenderInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextRenderInfo.htm

Contains metadata about where and how text was rendered across pages. For each page on which a portion of the text was drawn, the corresponding entry in Pages exposes both the geometric rectangle and the text actually rendered on that page

### Properties

- `LastPage`: The last entry from Pages, or null when the text was not rendered
- `LastPageRectangle`: The last entry from RenderedRectangles, or null when the text was not rendered
- `Pages`: The per-page render information, one entry per page on which a portion of the text was drawn. Each entry combines the page rectangle (bounds, rotated quad, page dimensions) with the substring of text rendered on that page
- `RenderedRectangles`: The geometric per-page entries, kept for backward compatibility. For new code prefer Pages, which also exposes the rendered text for each page. The list is parallel with Pages and contains the same rectangle instances

## PdfTextPageRenderInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextPageRenderInfo.htm

Carries the per-page render result for a text element: the geometric rectangle on a specific page together with the portion of text drawn on that page. One instance is produced for each page on which a portion of the text was rendered

### Properties

- `RenderedRectangle`: The geometric information about where the text was rendered on this page, including the axis-aligned bounds, the rotated four-corner outline, the visible portion of the bounds and the page dimensions
- `RenderedText`: The portion of the original text that was actually drawn on this page. For single-page text this equals the full input text; for multi-page text each page receives the substring of the input text that was laid out on it, sliced at word boundaries

## PdfImageRenderInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfImageRenderInfo.htm

Contains metadata about how and where an image was rendered on a PDF page. Exposes both the axis-aligned bounding box and the rotated four-corner outline, together with the page dimensions and the portion of the bounding box that remains visible inside the page

### Properties

- `BoundingBox`: The axis-aligned bounding rectangle of the rendered image, or of its first part when the image continues on the next pages, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template. For rotated images this is the smallest axis-aligned rectangle that fully contains the rendered shape
- `LastPage`: The 1-based number of the last page where a part of the image was drawn. Equals Page when the image was drawn on one page
- `LastPageRectangle`: The last entry from RenderedRectangles: the part of the image drawn on the last page. The elements that follow the image go below the bottom of this rectangle, on its page
- `Page`: The 1-based page number where the image was rendered: the page of its first part when the image continues on the next pages, whose parts are listed by RenderedRectangles
- `PageHeight`: The height of the page on which the image was rendered, in points
- `PageWidth`: The width of the page on which the image was rendered, in points
- `QuadPoints`: The four corners of the rendered image as they appear after any rotation has been applied, in points and with the same origin as BoundingBox. The corners follow the original orientation of the image: TopLeft is the corner that was top-left before rotation, and so on. Use this property to draw connecting lines or annotations that follow the rotation of the image instead of an axis-aligned bounding box
- `RenderedRectangles`: The rectangles where the image was drawn, one entry per page. There is one entry, with the values of BoundingBox and QuadPoints, when the image was drawn on one page. When the image continues on the next pages (see ContinueOnNextPage), each entry gives the part of the image drawn on that page, in points and with the same origin as BoundingBox
- `VisibleBounds`: The intersection of BoundingBox with the page rectangle, in points and with the same origin as BoundingBox. Equals BoundingBox when the image fits entirely inside the page and is null when the image is fully outside the page area

## PdfRenderedRectangle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRenderedRectangle.htm

Represents a single occurrence of an element on a PDF page. Carries the axis-aligned bounding rectangle, the rotated four-corner outline and the page dimensions

### Properties

- `Bounds`: The axis-aligned bounding rectangle of the rendered area, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template. For rotated elements this is the smallest axis-aligned rectangle that fully contains the rendered shape
- `PageHeight`: The height of the page or template on which the element was rendered, in points
- `PageNumber`: The number of the PDF page on which the element was rendered (1-based index). For elements rendered inside a template this value is 0
- `PageWidth`: The width of the page or template on which the element was rendered, in points
- `QuadPoints`: The four corners of the rendered area as they appear after any rotation has been applied, in points and with the same origin as Bounds. The corners follow the original orientation of the element: TopLeft is the corner that was top-left before rotation, and so on. Use this property to draw connecting lines or annotations that follow the rotation of the element instead of an axis-aligned bounding box
- `VisibleBounds`: The intersection of Bounds with the page rectangle, in points and with the same origin as Bounds. Equals Bounds when the element fits entirely inside the page and is null when the element is fully outside the page area

## PdfHtmlElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfHtmlElement.htm

HTML content rendered in the pages of a document. In a document being created the content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the document. In an edited document the content starts at a position in a page and continues on the following pages or on pages inserted after it, between the continuation margins of the element. The conversion is controlled by the converter exposed by Converter, whose page size and margins are taken from the document

### Properties

- `ContinuationBottomMargin`: In a PdfEditor, the distance in points from the bottom of the pages to the lowest point the content can reach on each page, starting with the page where it starts. Has no effect in a PdfDocument, where the bottom margin of the document applies. The default value is 0
- `ContinuationTopMargin`: In a PdfEditor, the distance in points from the top of the pages where the content continues to the top of the continued content. Has no effect in a PdfDocument, where the top margin of the document applies. The default value is 0
- `Converter`: The converter that renders the HTML content: its browser, JavaScript, navigation, authentication and layout settings apply to the rendering. The page size, the margins and the PDF standard of its document options are set from the document when the element is added
- `HtmlElementsInfoSelector`: A CSS selector of the HTML elements whose positions in the document are reported in the render information of the element, or null
- `InsertContinuationPages`: In a PdfEditor, when true, the pages where the content continues are inserted after the page where it starts, so the following pages of the document keep their content. When false, the default, the content continues on the following pages of the document and new pages are added only at its end. Has no effect in a PdfDocument
- `Width`: The width of the HTML content in points, or 0 to extend it to the right margin of the page, or to the right edge of the page in a PdfEditor
- `X`: The X position where the HTML content starts, in points and relative to the top left corner of the content area of the current page, inside the margins, or to the top left corner of the page in a PdfEditor. The content keeps this left offset on the next pages
- `Y`: The Y position where the HTML content starts, in points and relative to the top left corner of the content area of the current page, inside the margins, or to the top left corner of the page in a PdfEditor. On the next pages the content starts at the top margin of the document, or at ContinuationTopMargin in a PdfEditor

## PdfHtmlImageElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfHtmlImageElement.htm

Represents HTML content rendered as an image in a PDF document. The HTML is converted to an image by the HTML to Image converter of the element, at the resolution given by the Resolution property, and the image is drawn at the size the HTML content has on screen, one CSS pixel being 0.75 points, scaled down when it is wider than the available width. The image keeps that size at any resolution, so a higher resolution gives a sharper image in the PDF document. An image taller than the space left on the page continues on the next pages by default. The element is rendered with AddHtmlImage in a document being created or with AddHtmlImage in an edited document

### Properties

- `Accessibility`: Accessibility metadata used when the host document is tagged. The default structure type of the image is Figure and PDF/UA requires a non-empty AlternateText or ActualText. Set StructureType to Artifact to mark a purely decorative image
- `BaseUrl`: The base URL of the HTML code, or null
- `ContinuationBottomMargin`: In a PdfEditor, the distance in points from the bottom of the pages to the lowest point the image can reach on each page, starting with the page where it starts. Has no effect in a PdfDocument, where the bottom margin of the document applies. The default value is 0
- `ContinuationTopMargin`: In a PdfEditor, the distance in points from the top of the pages where the image continues to the top of the continued image. Has no effect in a PdfDocument, where the top margin of the document applies. The default value is 0
- `ContinueOnNextPage`: When true, the default, an image taller than the space left on the page continues on the next pages: the part that does not fit above the bottom margin is drawn on the following page from the top margin, with the same horizontal position and width, and so on until the whole image is drawn. When false the image is drawn on one page and the part outside the page is cut
- `Converter`: The converter that renders the HTML content as an image: its viewer width, JavaScript, navigation, authentication and resolution settings apply to the rendering. The converter is set to capture the entire page and the image is cut at the height of the HTML content
- `Height`: The maximum height of the image in points, or 0 for no limit. An image taller than this value is scaled down to this height, keeping its aspect ratio
- `HorizontalAlign`: The horizontal alignment of the image in the content area of the page. When it is not None it takes precedence over X
- `Html`: The HTML code to render, or null when the element renders the HTML document from a URL
- `InsertContinuationPages`: In a PdfEditor, when true, the pages where the image continues are inserted after the page where it starts, so the following pages of the document keep their content. When false, the default, the image continues on the following pages of the document and new pages are added only at its end. Has no effect in a PdfDocument, where the image continues on new pages
- `Opacity`: The opacity of the image, from 0 to 1. The default value is 1, fully opaque
- `Resolution`: The resolution of the image in DPI, from 1 to 1200. The default value is 96, one image pixel for each CSS pixel. The image has the same size in the PDF document at any resolution, so a higher resolution gives a sharper image, with more pixels for the same content, at the cost of a larger PDF file. The property is the Resolution property of the Converter
- `Url`: The URL of the HTML document, or null when the element renders an HTML string
- `Width`: The width of the image in points. When it is 0, the default, the image has the width of the HTML content on screen, one CSS pixel being 0.75 points, and it is scaled down to the width available from X to the right margin when it is wider than that
- `X`: The X position of the image, in points and relative to the top left corner of the content area of the page, inside the margins of a document being created, or to the top left corner of the page of an edited document. The image keeps this left offset on the next pages
- `Y`: The Y position of the image, in points and relative to the top left corner of the content area of the page, inside the margins of a document being created, or to the top left corner of the page of an edited document. On the next pages the image continues from the top margin of the document, or from ContinuationTopMargin in an edited document

## PdfPageElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPageElement.htm

A page of an existing PDF document drawn in a page of the document being created or edited, at a given position and size, optionally rotated and with a given opacity. The page keeps its content, its resources, its links and form fields, and its structure when both documents are tagged

### Properties

- `Height`: The height of the drawn page in points, or 0 to use its own height. When both the width and the height are set, the page is scaled to fit inside them and keeps its proportions
- `IncludeAnnotations`: True to add the annotations of the page, such as its links and form fields, to the target page, placed with the page content. The default is true
- `Opacity`: The opacity of the drawn page, from 0 (transparent) to 1 (opaque). The default is 1
- `PageNumber`: The 1-based number of the drawn page in its document
- `Password`: The password that opens the document when it is protected, or null
- `RenderUnderContent`: True to draw the page under the existing content of the target page, false to draw it over the content. The default is false
- `RotationDegrees`: The rotation of the drawn page in degrees, counter-clockwise for a positive value, around RotationPivot. The default is 0
- `RotationPivot`: The point of the drawn page the rotation turns around. The default is the top left corner
- `Width`: The width of the drawn page in points, or 0 to use its own width. When both the width and the height are set, the page is scaled to fit inside them and keeps its proportions
- `X`: The X position of the top left corner of the drawn page, in points. In a document being created the position is relative to the content area of the current page, inside the margins; in an edited document it is relative to the top left corner of the page
- `Y`: The Y position of the top left corner of the drawn page, in points. In a document being created the position is relative to the content area of the current page, inside the margins; in an edited document it is relative to the top left corner of the page

## PdfHtmlRenderInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfHtmlRenderInfo.htm

Describes where the content of a PdfHtmlElement was rendered in the document: the pages it occupies, the area it covers on each of them and the positions of the HTML elements selected for inspection

### Properties

- `HtmlElementsInfo`: The HTML elements matched by the selector of the element, with their positions in the document pages, in points and relative to the top left corner of the content area of the page in a document or of the page in an edited document. Empty when no selector was set

## PdfContentRenderInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfContentRenderInfo.htm

Describes where content that flows over one or more pages was rendered in the document: the pages it occupies and the area it covers on each of them

### Properties

- `FirstPageNumber`: The 1-based number of the document page where the content starts
- `LastPageNumber`: The 1-based number of the document page where the content ends
- `LastPageRectangle`: The area covered by the content on the last page it occupies, or null when nothing was rendered. The elements added after the content can be placed below this rectangle
- `PagesCount`: The number of document pages the content occupies
- `RenderedRectangles`: The area covered by the content on each page it occupies, in order, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template

## PdfGradientFill

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfGradientFill.htm

A fill that changes smoothly from one color to another across the bounds of a shape. When set on a shape it replaces its fill color

### Properties

- `Direction`: The direction of the gradient across the shape. The default value is Vertical
- `EndColor`: The color at the end of the gradient
- `StartColor`: The color at the start of the gradient

## PdfGradientDirection

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfGradientDirection.htm

The direction in which the colors of a gradient change inside a shape

### Fields and values

- `BackwardDiagonal`: From the bottom left corner to the top right corner of the shape
- `ForwardDiagonal`: From the top left corner to the bottom right corner of the shape
- `Horizontal`: From the left edge to the right edge of the shape
- `Vertical`: From the top edge to the bottom edge of the shape

## PdfSizeF

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSizeF.htm

A size in points: a width and a height

### Properties

- `Height`: The height in points
- `Width`: The width in points
