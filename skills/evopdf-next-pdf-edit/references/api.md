# Editing an existing PDF: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfEditor

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfEditor.htm

Allows editing of existing PDF documents by adding HTML content, setting metadata, applying security options, and digital signatures. Use Save or SaveToFile to generate the final PDF and release resources

### Properties

- `ContainsEmbeddedFiles`: Indicates whether the loaded PDF contains embedded files
- `ContainsForm`: Indicates whether the loaded PDF contains an interactive form
- `ContainsJavaScript`: Indicates whether the loaded PDF contains JavaScript code
- `DigitalSignature`: Gets the digital signature applied to the PDF document when it is saved, as configured at construction time. The returned object can still be configured after construction (for example by setting the timestamp server URL, reason, or appearance options) but the reference itself is fixed. To enable digital signing, pass a configured PdfDigitalSignature instance to the editor constructor. The constructors without a signature parameter save the document without a digital signature.
- `FlattenPdf`: A flag indicating if the output PDF is flattened. The default value is false
- `IsLinearized`: Indicates whether the loaded PDF is linearized (Fast Web View). Linearized PDFs are optimized for rendering the first page before the entire file is downloaded
- `IsSigned`: Indicates whether the loaded PDF document contains at least one digital signature. This property checks for the presence of signature dictionaries. It does NOT validate the signatures cryptographically
- `IsTaggedPdf`: Indicates whether the loaded PDF is a tagged PDF
- `PdfDocumentInfo`: Gets or sets the metadata (title, author, etc.) to apply to the output PDF
- `PdfDocumentReadInfo`: Gets the document metadata as read from the original input PDF
- `PdfSecurityInfo`: Gets the security information extracted from the loaded PDF (permissions, encryption, etc.)
- `PdfSecurityOptions`: Gets or sets the security options (passwords, permissions) to apply to the output PDF
- `PdfSignatureInfo`: Gets digital signature information extracted from the loaded PDF document (signature count, certification status, certification level, etc.)
- `PdfVersion`: Gets the PDF specification version of the loaded document (for example "1.4", "1.7", "2.0")
- `PdfViewerPreferences`: Gets or sets the viewer preferences to apply to the generated PDF
- `PdfViewerPreferencesInfo`: Gets the viewer preferences extracted from the loaded PDF

### Methods

- `AddArc`: Adds an elliptical arc to a specified page in the existing PDF document
- `AddCircle`: Adds a circle to a specified page in the existing PDF document
- `AddEllipse`: Adds an ellipse to a specified page in the existing PDF document
- `AddFileAttachment`: Adds a file to the document's attachments. The attachment appears in the Attachments panel of PDF viewers but has no visible icon on any page. To also display a clickable icon on a specific page, use AddFileAttachmentAnnotation
- `AddFileAttachmentAnnotation`: Adds a file attachment displayed as a clickable icon on a specific page. Clicking the icon opens or extracts the attached file. The file is also added to the Attachments panel alongside any other attachments
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height.
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddImage`: Adds an image element to the current PDF page. Supports byte array or file path images, scaling and alignment
- `AddLine`: Adds a line to a specified page in the existing PDF document
- `AddLinkAnnotation`: Adds a clickable hyperlink annotation placed on a specific page. The link can target an external URL, another page in the same document, a specific location on another page, or a named destination defined in the document catalog. Use one of the static factory methods on PdfLinkAnnotation (FromUrl, ToPage, ToPageLocation, ToNamedDestination) to build the link
- `AddPage`: Adds a new blank page at the end of the document. The new page inherits the size of the last existing page
- `AddPath`: Adds a generic path composed of move, line and curve operations to a specified page in the existing PDF document
- `AddPdfTemplate`: Creates a blank PDF Template object with the specified position and size in the PDF page. Specifies the horizontal and vertical alignment of the template on the page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The template can later be filled with custom content such as text, images or graphics
- `AddPolygon`: Adds a closed polygon through a sequence of vertices to a specified page in the existing PDF document
- `AddPolyline`: Adds an open polyline through a sequence of points to a specified page in the existing PDF document
- `AddRectangle`: Adds a rectangle to a specified page in the existing PDF document
- `AddRoundedRectangle`: Adds a rectangle with rounded corners to a specified page in the existing PDF document
- `AddText`: Adds a text element to the current PDF page. If the text exceeds the current page, it will continue on subsequent pages based on element settings. Supports placeholder variables such as {page_number} and {total_pages}, which are replaced with their actual values in the rendered PDF.
- `AddTextAnnotation`: Adds a sticky-note text annotation displayed as a clickable icon on a specific page. Clicking the icon opens a popup with the note text. The annotation also appears in the viewer's Comments panel
- `Dispose`: Performs the actual resource cleanup
- `Dispose`: Releases all resources used by this instance
- `GetPageCount`: Gets the number of pages in the loaded PDF document
- `GetPdfPageInfo`: Gets detailed information about a specific page in the loaded PDF document.
- `Save`: Saves the edited PDF to a memory buffer and releases all resources
- `SaveAsync`: Asynchronously saves the edited PDF to a memory buffer and releases all resources
- `SaveToFile`: Saves the edited PDF to a file and releases all resources
- `SaveToFileAsync`: Asynchronously saves the edited PDF to a file and releases all resources
