# Merging and combining PDFs: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfMerge

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfMerge.htm

Merges PDF documents, or selected pages of them, blank pages and images into a single PDF document. The form fields, bookmarks, annotations, internal links and scripts of the merged documents are kept. The merged documents can be password protected

### Properties

- `Bookmarks`: The bookmarks added to the outline of the merged document, after the bookmarks kept from the merged documents. The page numbers refer to the pages of the merged document and the location in a page is given from the top left corner of the page
- `DigitalSignature`: The digital signature to apply to generated PDF document
- `Flatten`: A flag indicating if the merged PDF is flattened: the form fields become static page content and the form is removed from the document. The default value is false
- `KeepFieldsSeparate`: A flag indicating if the form fields with the same name coming from different documents are kept as separate fields. When true, the fields of a document whose names were already used by a previous document are renamed with a numeric suffix and keep their own values. When false, they become a single field with the value of the first document and the widgets in all the documents. The default value is true
- `KeepSourceBookmarks`: A flag indicating if the bookmarks of the merged documents are kept in the merged document, with their destinations moved to the pages of the merged document. The default value is true
- `OpenAction`: The action executed by the viewer when the merged document is opened: go to a page at a given location and zoom, go to a named destination, run JavaScript, a viewer action or open a web address. When null, the merged document has no open action. The default is null
- `PageCount`: The number of pages of the merged document
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfMergeInfo`: The total number of pages of the merged document and the number of pages of each added source
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer

### Methods

- `AddBlankPage`: Adds a blank page
- `AddFileAttachment`: Adds a file to the document's attachments. The attachment appears in the Attachments panel of PDF viewers but has no visible icon on any page. To also display a clickable icon on a specific page, use AddFileAttachmentAnnotation
- `AddFileAttachmentAnnotation`: Adds a file attachment displayed as a clickable icon on a specific page. Clicking the icon opens or extracts the attached file. The file is also listed in the Attachments panel when the AddToAttachmentsPanel property of the annotation is true
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The template has the given height. When AutoSizeContentHeight is true, the HTML content may be scaled down to fit the given height
- `AddImagePage`: Adds a page displaying an image from a file. The page has the size of the image when no page size is given, otherwise the image is fitted in the page and centered
- `AddImagePage`: Adds a page displaying an image from a memory buffer. The page has the size of the image when no page size is given, otherwise the image is fitted in the page and centered
- `AddLinkAnnotation`: Adds a clickable hyperlink annotation placed on a specific page. The link can open an external URL, go to another page of the document or to a location on another page. Use one of the static factory methods of PdfLinkAnnotation (FromUrl, ToPage, ToPageLocation) to build the link
- `AddPdf`: Adds all the pages of a PDF from a file
- `AddPdf`: Adds all the pages of a PDF from a memory buffer
- `AddPdfPages`: Adds a range of pages of a PDF from a file
- `AddPdfPages`: Adds a range of pages of a PDF from a memory buffer
- `AddPdfPages`: Adds the pages of a PDF from a file selected by page ranges, in the order of the ranges
- `AddPdfPages`: Adds the pages of a PDF from a memory buffer selected by page ranges, in the order of the ranges
- `AddTextAnnotation`: Adds a sticky-note text annotation displayed as a clickable icon on a specific page. Clicking the icon opens a popup with the note text. The annotation also appears in the viewer's Comments panel
- `Dispose`: Releases all resources used by this instance
- `Save`: Merges the PDFs and saves the result to a memory buffer
- `SaveAsync`: Asynchronously merges the PDFs and saves the result to a memory buffer
- `SaveToFile`: Merges the PDFs and saves the result to a file
- `SaveToFileAsync`: Asynchronously merges the PDFs and saves the result to a file

## PdfMergeInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfMergeInfo.htm

An object of this class is exposed by the PdfMerge class to provide details such as the total number of pages produced and the number of pages from each merged document

### Properties

- `PagesPerDocument`: The number of pages for each merged document, in the order they were added
- `TotalPagesProduced`: The total number of pages produced by the merge
