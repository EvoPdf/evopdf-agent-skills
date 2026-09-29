# Merging and combining PDFs: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfMerge

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfMerge.htm

The PdfMerge class provides functionality to merge multiple PDF files or streams into a single PDF document. It supports adding password protected PDFs, preserving bookmarks, annotations and internal links from the source files. Tagging and AcroForms are not preserved during the merge process

### Properties

- `DigitalSignature`: The digital signature to apply to generated PDF document
- `FlattenMergedPdf`: A flag indicating if the merged PDF is flattened. The default value is false
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfMergeInfo`: This property provides details about the total number of pages produced and the number of pages from each merged document
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer

### Methods

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
- `AddLinkAnnotation`: Adds a clickable hyperlink annotation placed on a specific page. The link can target an external URL, another page in the same document, a specific location on another page, or a named destination defined in the document catalog. Use one of the static factory methods on PdfLinkAnnotation (FromUrl, ToPage, ToPageLocation, ToNamedDestination) to build the link
- `AddPdf`: Adds a PDF from a file
- `AddPdf`: Adds a PDF from a memory buffer
- `AddTextAnnotation`: Adds a sticky-note text annotation displayed as a clickable icon on a specific page. Clicking the icon opens a popup with the note text. The annotation also appears in the viewer's Comments panel
- `Dispose`: Performs the actual resource cleanup
- `Dispose`: Releases all resources used by this instance
- `Save`: Merges the PDFs and saves the result to a memory buffer
- `SaveAsync`: Asynchronously merges the PDFs and saves the result to a memory buffer
- `SaveToFile`: Merges the PDFs and saves the result to a file
- `SaveToFileAsync`: Asynchronously merges the PDFs and saves the result to a file

## PdfMergeInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfMergeInfo.htm

An object of this class is exposed by the PdfMerge class to provide details such as the total number of pages produced and the number of pages from each merged document

### Fields and values

- `PagesPerDocument`: The number of pages for each merged document, in the order they were added
- `TotalPagesProduced`: The total number of pages produced by the merge
