# Word, Excel, RTF and Markdown to PDF: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## WordToPdfConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_WordToPdfConverter.htm

Provides functionality for converting Word (.docx) documents to PDF format with support for customization options such as metadata, security and viewer preferences

### Properties

- `ConversionInfo`: Information about the last conversion: the pages produced and the pages inserted before and after them
- `DigitalSignature`: The digital signature to apply to generated PDF document
- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfDocumentOptions`: Gets a reference to the object controlling the conversion process and the generated PDF document properties like PDF document margins, PDF page size and orientation, PDF document header and footer
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer
- `ProcessPageBreakMarks`: Gets or sets a value indicating whether page break marks from Word documents should be processed and rendered in the output PDF. The default value is true.
- `UseEmbeddedFonts`: True to render the content with the fonts stored in the Word document by its author, when it has any. The default is true
- `WordLoaderFilePath`: Sets the full path of Word loader file

### Methods

- `ConvertToPdf`: Converts a Word document to a PDF byte array. The input must be a valid DOCX file provided as a byte array. The resulting PDF content is returned as a byte array, which can be saved, streamed or transmitted as needed.
- `ConvertToPdf`: Converts a Word file from the specified file path to a PDF byte array. The input file must be a valid DOCX format document. The resulting PDF content is returned as a byte array, which can be saved to disk, sent over a network, or written directly to a response stream.
- `ConvertToPdfAsync`: Asynchronously converts a Word document to a PDF byte array. The input must be a valid DOCX file provided as a byte array. The resulting PDF content is returned as a byte array, which can be saved, streamed or transmitted as needed.
- `ConvertToPdfAsync`: Asynchronously converts a Word file from the specified file path to a PDF byte array. The input file must be a valid DOCX format document. The resulting PDF content is returned as a byte array, which can be saved to disk, sent over a network, or written directly to a response stream.
- `ConvertToPdfFile`: Converts a Word document from a byte array to PDF and saves the result to a specified file path
- `ConvertToPdfFile`: Converts a Word document from a file path to PDF and saves the result to a specified file path
- `ConvertToPdfFileAsync`: Asynchronously Converts a Word document from a byte array to PDF and saves the result to a specified file path
- `ConvertToPdfFileAsync`: Asynchronously converts a Word document from a file path to PDF and saves the result to a specified file path

## WordToPdfDocumentOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_WordToPdfDocumentOptions.htm

This class encapsulates the options to control the PDF document rendering process. The WordToPdfConverter class defines a reference to an object of this type

### Properties

- `AutoResizePdfPageHeight`: Automatically resize PDF page height to match content height. The default value is false
- `Bookmarks`: The bookmarks added to the outline of the generated document, after the bookmarks created from the headings of the Word document. The page numbers refer to the pages of the generated document, including the pages of the table of contents and of the PDF documents added before the converted content, and the location in a page is given from the top left corner of the page
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the page, such as a table whose columns hold codes or addresses that cannot be broken between letters, which also makes the rest of the content smaller. Images are reduced to the width of the page and long words are wrapped on their own, so the option is normally not needed; it matters only for documents with content wider than the page. The default value is false
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `GenerateDocumentOutline`: True to create an outline for the generated PDF document with a bookmark for each heading of the Word document. The default is false
- `GenerateTableOfContents`: A flag indicating if a table of contents is automatically generated in the PDF document from headings. The default is false
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `OpenAction`: The action executed by the viewer when the generated document is opened: go to a page at a given location and zoom, go to a named destination, run JavaScript, a viewer action or open a web address. When null, the document has no open action. The default is null
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire Word document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the Word to PDF converter. This property is used only when UsePageSettingsFromWord is set to false. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the Word to PDF converter. This property is used only when UsePageSettingsFromWord is set to false. The default page size is A4
- `PdfStandard`: The PDF standard the generated document conforms to. When a PDF/UA or a PDF/A standard that requires tagging is selected, the structure of the generated document follows the headings, paragraphs, tables, images and links of the converted document. The default is None
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `TableOfContents`: The table of contents options. To enable the creation of table of contents from heading tags set the GenerateTableOfContents property on true
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `UseHeaderFooterFromWord`: Whether the running header and footer of the Word document are rendered on the PDF pages, with the page numbers they contain and without them on a title page. They are not used when a header or footer is set in the converter options. The default value is true
- `UsePageSettingsFromWord`: Whether to use page settings from the Word document. The default value is true
- `Zoom`: Gets or sets the Word viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddElectronicInvoice`: Embeds a Factur-X / ZUGFeRD electronic invoice in the generated PDF document: the invoice XML is attached under the file name the standard prescribes, with the relationship of its profile, and the Factur-X identification metadata is written in the XMP metadata. The document must be generated as PDF/A-3 (PdfA3b, PdfA3u, PdfA3a or PdfUa1PdfA3a for an accessible invoice). A document carries one electronic invoice
- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
- `AddFileAttachment`: Adds a file to the attachments of the generated document. The attachment appears in the Attachments panel of PDF viewers
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height.
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddStartPdf`: Add a PDF from a file to the list of documents to be included before the converted HTML content in the final PDF
- `AddStartPdf`: Add a PDF from a memory buffer to the list of documents to be included before the converted Word content in the final PDF

## ExcelToPdfConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ExcelToPdfConverter.htm

Provides functionality for converting Excel (.xlsx) documents to PDF format with support for customization options such as metadata, security and viewer preferences

### Properties

- `ConversionInfo`: Information about the last conversion: the pages produced and the pages inserted before and after them
- `DigitalSignature`: The digital signature to apply to the generated the PDF document
- `ExcelLoaderFilePath`: Sets the full path of the Excel loader file
- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `KeepColumnWidths`: When set to true, keeps the original Excel column widths percentage in the generated PDF document. The cell content can be clipped if the column is too narrow. When set to false, the column widths are adjusted to fit the PDF page width and the Excel drawings like images might not align with the cells. The default value is true
- `KeepRowHeights`: When set to true, keeps the original Excel row heights in the generated PDF document. When set to false, the row heights are adjusted to fit the cell content height and the Excel drawings like images might not align with the cells. The default value is true
- `PdfDocumentInfo`: Gets a reference to the object controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfDocumentOptions`: Gets a reference to the object controlling the conversion process and the generated PDF document properties like PDF document margins, PDF page size and orientation, PDF document header and footer
- `PdfSecurityOptions`: Gets a reference to the object controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer

### Methods

- `ConvertToPdf`: Converts an Excel document to a PDF byte array. The input must be a valid .xlsx file provided as a byte array. The resulting PDF content is returned as a byte array, which can be saved, streamed or transmitted as needed
- `ConvertToPdf`: Converts an Excel file from the specified file path to a PDF byte array. The input file must be a valid .xlsx format document. The resulting PDF content is returned as a byte array, which can be saved to disk, sent over a network, or written directly to a response stream
- `ConvertToPdfAsync`: Asynchronously converts an Excel document to a PDF byte array. The input must be a valid .xlsx file provided as a byte array. The resulting PDF content is returned as a byte array, which can be saved, streamed or transmitted as needed
- `ConvertToPdfAsync`: Asynchronously converts an Excel file from the specified file path to a PDF byte array. The input file must be a valid .xlsx format document. The resulting PDF content is returned as a byte array, which can be saved to disk, sent over a network, or written directly to a response stream
- `ConvertToPdfFile`: Converts an Excel document from a byte array to PDF and saves the result to a specified file path
- `ConvertToPdfFile`: Converts an Excel document from a file path to PDF and saves the result to a specified file path
- `ConvertToPdfFileAsync`: Asynchronously converts an Excel document from a byte array to PDF and saves the result to a specified file path
- `ConvertToPdfFileAsync`: Asynchronously converts an Excel document from a file path to PDF and saves the result to a specified file path

## ExcelToPdfDocumentOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ExcelToPdfDocumentOptions.htm

This class encapsulates the options to control the PDF document rendering process. The ExcelToPdfConverter class defines a reference to an object of this type

### Properties

- `AutoResizePdfPageHeight`: Automatically resize PDF page height to match content height. The default value is false
- `Bookmarks`: The bookmarks added to the outline of the generated document, after the bookmarks created from the headings of the Excel workbook. The page numbers refer to the pages of the generated document, including the pages of the table of contents and of the PDF documents added before the converted content, and the location in a page is given from the top left corner of the page
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `ConvertOnlyFirstWorksheet`: Gets or sets a value indicating whether only the first worksheet of the workbook should be converted. When false, all worksheets in the workbook are converted. The default value is false
- `DisplayWorksheetTitle`: Gets or sets a value indicating whether the worksheet title is displayed at the top of each worksheet in the PDF document. The default value is true
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the page, such as a picture, which also makes the rest of the content smaller; false to keep the content at its size, with the part beyond the right margin cut off. The worksheets take the width of the page, so the option matters only for such elements. The default value is true
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `OpenAction`: The action executed by the viewer when the generated document is opened: go to a page at a given location and zoom, go to a named destination, run JavaScript, a viewer action or open a web address. When null, the document has no open action. The default is null
- `PageBreakBetweenWorksheets`: Gets or sets a value indicating whether each worksheet starts on a new PDF page. The default value is true
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire Excel document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the Excel to PDF converter. This property is used only when UsePageSettingsFromExcel is set to false. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the Excel to PDF converter. This property is used only when UsePageSettingsFromExcel is set to false. The default page size is A4
- `PdfStandard`: The PDF standard the generated document conforms to. When a PDF/UA or a PDF/A standard that requires tagging is selected, the structure of the generated document follows the headings, paragraphs, tables, images and links of the converted document. The default is None
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `UseFirstWorksheetPageSettings`: Applies only when UsePageSettingsFromExcel is true. If true, page settings from the first worksheet are used for all worksheets. If false, page settings from the workbook's active worksheet are used for all worksheets (if an active worksheet is set). Has no effect when UsePageSettingsFromExcel is false. The default value is true
- `UsePageSettingsFromExcel`: Gets or sets a value indicating whether to import page setup settings (paper size, margins, orientation) from the workbook and apply them to PdfDocumentOptions before rendering. The default value is true
- `Zoom`: Gets or sets the Excel viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddElectronicInvoice`: Embeds a Factur-X / ZUGFeRD electronic invoice in the generated PDF document: the invoice XML is attached under the file name the standard prescribes, with the relationship of its profile, and the Factur-X identification metadata is written in the XMP metadata. The document must be generated as PDF/A-3 (PdfA3b, PdfA3u, PdfA3a or PdfUa1PdfA3a for an accessible invoice). A document carries one electronic invoice
- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
- `AddFileAttachment`: Adds a file to the attachments of the generated document. The attachment appears in the Attachments panel of PDF viewers
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height.
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddStartPdf`: Add a PDF from a file to the list of documents to be included before the converted HTML content in the final PDF
- `AddStartPdf`: Add a PDF from a memory buffer to the list of documents to be included before the converted Excel content in the final PDF

## RtfToPdfConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_RtfToPdfConverter.htm

Provides functionality for converting RTF documents to PDF format with support for customization options such as metadata, security and viewer preferences

### Properties

- `ConversionInfo`: Information about the last conversion: the pages produced and the pages inserted before and after them
- `DigitalSignature`: The digital signature to apply to generated PDF document
- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfDocumentOptions`: Gets a reference to the object controlling the conversion process and the generated PDF document properties like PDF document margins, PDF page size and orientation, PDF document header and footer
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer
- `RtfLoaderFilePath`: Sets the full path of RTF loader file

### Methods

- `ConvertFileToPdf`: Converts an RTF file to a PDF byte array
- `ConvertFileToPdfAsync`: Asynchronously converts an RTF file to a PDF byte array
- `ConvertFileToPdfFile`: Converts an RTF file to PDF and writes the result to the specified output path
- `ConvertFileToPdfFileAsync`: Asynchronously converts an RTF file to PDF and writes the result to the specified output path
- `ConvertStreamToPdf`: Converts an RTF stream to a PDF byte array
- `ConvertStreamToPdfAsync`: Asynchronously converts an RTF stream to a PDF byte array
- `ConvertStreamToPdfFile`: Converts an RTF stream to PDF and writes the result to the specified output path
- `ConvertStreamToPdfFileAsync`: Asynchronously converts an RTF stream to PDF and writes the result to the specified output path
- `ConvertStringToPdf`: Converts an RTF string (raw RTF markup like "{\rtf1...}") to a PDF byte array
- `ConvertStringToPdfAsync`: Asynchronously converts an RTF string (raw RTF markup like "{\rtf1...}") to a PDF byte array
- `ConvertStringToPdfFile`: Converts an RTF string (raw RTF markup) to PDF and writes the result to the specified output path
- `ConvertStringToPdfFileAsync`: Asynchronously converts an RTF string (raw RTF markup) to PDF and writes the result to the specified output path
- `ConvertToPdf`: Converts an RTF byte array to a PDF byte array
- `ConvertToPdfAsync`: Asynchronously converts an RTF byte array to a PDF byte array
- `ConvertToPdfFile`: Converts an RTF byte array to PDF and writes the result to the specified output path
- `ConvertToPdfFileAsync`: Asynchronously converts an RTF byte array to PDF and writes the result to the specified output path

## RtfToPdfDocumentOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_RtfToPdfDocumentOptions.htm

This class encapsulates the options to control the PDF document rendering process. The RtfToPdfConverter class defines a reference to an object of this type

### Properties

- `AutoResizePdfPageHeight`: Automatically resize PDF page height to match content height. The default value is false
- `Bookmarks`: The bookmarks added to the outline of the generated document, after the bookmarks created from the headings of the RTF document. The page numbers refer to the pages of the generated document, including the pages of the table of contents and of the PDF documents added before the converted content, and the location in a page is given from the top left corner of the page
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromRtf is set to false. By default the left margin is 0
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the page, such as a table whose columns hold codes or addresses that cannot be broken between letters, which also makes the rest of the content smaller. Images are reduced to the width of the page and long words are wrapped on their own, so the option is normally not needed; it matters only for documents with content wider than the page. The default value is false
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `GenerateDocumentOutline`: True to create an outline for the generated PDF document with a bookmark for each heading of the RTF document. The default is false
- `GenerateTableOfContents`: A flag indicating if a table of contents is automatically generated in the PDF document from headings. The default is false
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromRtf is set to false. By default the left margin is 0
- `OpenAction`: The action executed by the viewer when the generated document is opened: go to a page at a given location and zoom, go to a named destination, run JavaScript, a viewer action or open a web address. When null, the document has no open action. The default is null
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire RTF document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the RTF to PDF converter. This property is used only when UsePageSettingsFromRtf is set to false. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the RTF to PDF converter. This property is used only when UsePageSettingsFromRtf is set to false. The default page size is A4
- `PdfStandard`: The PDF standard the generated document conforms to. When a PDF/UA or a PDF/A standard that requires tagging is selected, the structure of the generated document follows the headings, paragraphs, tables, images and links of the converted document. The default is None
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromRtf is set to false. By default the left margin is 0
- `TableOfContents`: The table of contents options. To enable the creation of table of contents from heading tags set the GenerateTableOfContents property on true
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromRtf is set to false. By default the left margin is 0
- `UseHeaderFooterFromRtf`: Whether the running header and footer of the RTF document are rendered on the PDF pages, with the page numbers they contain and without them on a title page. They are not used when a header or footer is set in the converter options. The default value is true
- `UsePageSettingsFromRtf`: Whether the page size, orientation and margins of the RTF document are used for the PDF pages. The default value is true
- `Zoom`: Gets or sets the RTF viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddElectronicInvoice`: Embeds a Factur-X / ZUGFeRD electronic invoice in the generated PDF document: the invoice XML is attached under the file name the standard prescribes, with the relationship of its profile, and the Factur-X identification metadata is written in the XMP metadata. The document must be generated as PDF/A-3 (PdfA3b, PdfA3u, PdfA3a or PdfUa1PdfA3a for an accessible invoice). A document carries one electronic invoice
- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
- `AddFileAttachment`: Adds a file to the attachments of the generated document. The attachment appears in the Attachments panel of PDF viewers
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height.
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddStartPdf`: Add a PDF from a file to the list of documents to be included before the converted HTML content in the final PDF
- `AddStartPdf`: Add a PDF from a memory buffer to the list of documents to be included before the converted RTF content in the final PDF

## MarkdownToPdfConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_MarkdownToPdfConverter.htm

Provides functionality for converting Markdown documents to PDF format with support for customization options such as metadata, security and viewer preferences

### Properties

- `ConversionInfo`: Information about the last conversion: the pages produced and the pages inserted before and after them
- `DigitalSignature`: The digital signature to apply to generated PDF document
- `DisableRawHtml`: Controls whether raw HTML embedded in Markdown is ignored. The default value is false
- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `MarkdownLoaderFilePath`: Sets the full path of Markdown loader file
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfDocumentOptions`: Gets a reference to the object controlling the conversion process and the generated PDF document properties like PDF document margins, PDF page size and orientation, PDF document header and footer
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer

### Methods

- `ConvertFileToPdf`: Converts a Markdown file to a PDF byte array
- `ConvertFileToPdfAsync`: Asynchronously converts a Markdown file to a PDF byte array
- `ConvertFileToPdfFile`: Converts a Markdown file to PDF and writes the result to the specified output path
- `ConvertFileToPdfFileAsync`: Asynchronously converts a Markdown file to PDF and writes the result to the specified output path
- `ConvertStringToPdf`: Converts a Markdown string to a PDF byte array
- `ConvertStringToPdfAsync`: Asynchronously converts a Markdown string to a PDF byte array
- `ConvertStringToPdfFile`: Converts a Markdown string to PDF and writes the result to the specified output path
- `ConvertStringToPdfFileAsync`: Asynchronously converts a Markdown string to PDF and writes the result to the specified output path

## MarkdownToPdfDocumentOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_MarkdownToPdfDocumentOptions.htm

This class encapsulates the options to control the PDF document rendering process. The MarkdownToPdfConverter class defines a reference to an object of this type

### Properties

- `AutoResizePdfPageHeight`: Automatically resize PDF page height to match content height. The default value is false
- `Bookmarks`: The bookmarks added to the outline of the generated document, after the bookmarks created from the headings of the Markdown document. The page numbers refer to the pages of the generated document, including the pages of the table of contents and of the PDF documents added before the converted content, and the location in a page is given from the top left corner of the page
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the page, such as an HTML table or block with a fixed width, which also makes the rest of the content smaller. Images are reduced to the width of the page and long words are wrapped on their own, so the option is normally not needed; it matters only for documents with content wider than the page. The default value is false
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `GenerateDocumentOutline`: True to create an outline for the generated PDF document with a bookmark for each heading of the Markdown document. The default is false
- `GenerateTableOfContents`: A flag indicating if a table of contents is automatically generated in the PDF document from headings. The default is false
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `OpenAction`: The action executed by the viewer when the generated document is opened: go to a page at a given location and zoom, go to a named destination, run JavaScript, a viewer action or open a web address. When null, the document has no open action. The default is null
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire Markdown document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the Markdown to PDF converter. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the Markdown to PDF converter. The default page size is A4
- `PdfStandard`: The PDF standard the generated document conforms to. When a PDF/UA or a PDF/A standard that requires tagging is selected, the structure of the generated document follows the headings, paragraphs, tables, images and links of the converted document. The default is None
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `StyleSheet`: Custom styling rules, using CSS syntax, applied to the Markdown content before the PDF document is generated
- `TableOfContents`: The table of contents options. To enable the creation of table of contents from heading tags set the GenerateTableOfContents property on true
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `Zoom`: Gets or sets the Markdown viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddElectronicInvoice`: Embeds a Factur-X / ZUGFeRD electronic invoice in the generated PDF document: the invoice XML is attached under the file name the standard prescribes, with the relationship of its profile, and the Factur-X identification metadata is written in the XMP metadata. The document must be generated as PDF/A-3 (PdfA3b, PdfA3u, PdfA3a or PdfUa1PdfA3a for an accessible invoice). A document carries one electronic invoice
- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
- `AddFileAttachment`: Adds a file to the attachments of the generated document. The attachment appears in the Attachments panel of PDF viewers
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on URL from which the HTML content should be retrieved. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. Specifies the horizontal and the vertical alignment of the template on the PDF page. The alignment takes precedence over the X and Y coordinates when set to a value other than None. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height.
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is auto-resized. To create a template with fixed height use the method with height parameter
- `AddHtmlTemplate`: Creates a HTML Template object to be rendered in PDF pages based on the HTML string and an optional base URL. The height of template is fixed. If AutoResizeHeight is true, the HTML content may be scaled down to fit the specified height
- `AddStartPdf`: Add a PDF from a file to the list of documents to be included before the converted HTML content in the final PDF
- `AddStartPdf`: Add a PDF from a memory buffer to the list of documents to be included before the converted Markdown content in the final PDF

## WordToPdfConversionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_WordToPdfConversionInfo.htm

Information about the result of a Word to PDF conversion: the pages produced and the pages inserted before and after them

### Properties

- `ExternalPagesInsertedAfter`: The number of pages of the external documents inserted after the converted content
- `ExternalPagesInsertedBefore`: The number of pages of the external documents inserted before the converted content
- `PagesFromDocument`: The number of pages produced from the Word document
- `TocPagesInsertedBefore`: The number of pages of the table of contents inserted before the converted content
- `TotalPages`: The total number of pages of the generated PDF document
- `TotalPagesInsertedBefore`: The number of pages inserted before the converted content: the external documents and the table of contents

## PdfWordElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfWordElement.htm

A Word document rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the PDF document, and keeps its structure, headings, links and images

### Properties

- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the content area, such as a table whose columns hold codes or addresses that cannot be broken between letters, which also makes the rest of the content smaller. Images are reduced to the width of the content area and long words are wrapped on their own, so the option is normally not needed; it matters only for documents with content wider than the content area. The default is false
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `GenerateDocumentOutline`: True to add a bookmark for each heading of the Word document to the outline of the PDF document. The default is false
- `ProcessPageBreakMarks`: True to keep the page breaks of the Word document. The default is true
- `UseEmbeddedFonts`: True to render the content with the fonts stored in the Word document by its author, when it has any. The default is true
- `Width`: The width of the content in points, or 0 to extend it to the right margin of the page
- `X`: The X position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. The content keeps this left offset on the next pages
- `Y`: The Y position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. On the next pages the content starts at the top margin
- `Zoom`: The zoom of the rendered content in percents, from 10 to 200. The default is 100

## WordPdfDocumentExtensions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_WordPdfDocumentExtensions.htm

Renders Word documents in the pages of a PdfDocument

### Methods

- `AddWord`: Renders a Word document in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps its structure, headings, links and images, and its headings can join the outline of the PDF document. The elements added after this call go to the last page of the content, below LastPageRectangle
- `AddWordAsync`: Asynchronously renders a Word document in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps its structure, headings, links and images, and its headings can join the outline of the PDF document. The elements added after this call go to the last page of the content, below LastPageRectangle

## ExcelToPdfConversionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ExcelToPdfConversionInfo.htm

Information about the result of a Excel to PDF conversion: the pages produced and the pages inserted before and after them

### Properties

- `ExternalPagesInsertedAfter`: The number of pages of the external documents inserted after the converted content
- `ExternalPagesInsertedBefore`: The number of pages of the external documents inserted before the converted content
- `PagesFromDocument`: The number of pages produced from the Excel workbook
- `TocPagesInsertedBefore`: The number of pages of the table of contents inserted before the converted content
- `TotalPages`: The total number of pages of the generated PDF document
- `TotalPagesInsertedBefore`: The number of pages inserted before the converted content: the external documents and the table of contents

## PdfExcelElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfExcelElement.htm

An Excel workbook rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the PDF document, and keeps the formatting, the images and the shapes of the worksheets

### Properties

- `ConvertOnlyFirstWorksheet`: True to render only the first worksheet of the workbook. The default is false
- `DisplayWorksheetTitle`: True to render the title of each worksheet above its content. The default is true
- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the content area, such as a picture, which also makes the rest of the content smaller; false to keep the content at its size, with the part beyond the right edge of the content area cut off. The worksheets take the width of the content area, so the option matters only for such elements. The default is true
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `KeepColumnWidths`: True to keep the column widths of the worksheets, when the content of a cell can be clipped by a narrow column; false to adjust the columns to the width of the content, when the drawings of the worksheet may not align with the cells. The default is true
- `KeepRowHeights`: True to keep the row heights of the worksheets; false to adjust the rows to the height of the content, when the drawings of the worksheet may not align with the cells. The default is true
- `PageBreakBetweenWorksheets`: True to start each worksheet on a new page. The default is true
- `Width`: The width of the content in points, or 0 to extend it to the right margin of the page
- `X`: The X position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. The content keeps this left offset on the next pages
- `Y`: The Y position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. On the next pages the content starts at the top margin
- `Zoom`: The zoom of the rendered content in percents, from 10 to 200. The default is 100

## ExcelPdfDocumentExtensions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ExcelPdfDocumentExtensions.htm

Renders Excel workbooks in the pages of a PdfDocument

### Methods

- `AddExcel`: Renders an Excel workbook in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps the formatting, the images and the shapes of the worksheets, and each worksheet can start on a new page. The elements added after this call go to the last page of the content, below LastPageRectangle
- `AddExcelAsync`: Asynchronously renders an Excel workbook in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps the formatting, the images and the shapes of the worksheets, and each worksheet can start on a new page. The elements added after this call go to the last page of the content, below LastPageRectangle

## RtfToPdfConversionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_RtfToPdfConversionInfo.htm

Information about the result of a RTF to PDF conversion: the pages produced and the pages inserted before and after them

### Properties

- `ExternalPagesInsertedAfter`: The number of pages of the external documents inserted after the converted content
- `ExternalPagesInsertedBefore`: The number of pages of the external documents inserted before the converted content
- `PagesFromDocument`: The number of pages produced from the RTF document
- `TocPagesInsertedBefore`: The number of pages of the table of contents inserted before the converted content
- `TotalPages`: The total number of pages of the generated PDF document
- `TotalPagesInsertedBefore`: The number of pages inserted before the converted content: the external documents and the table of contents

## PdfRtfElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRtfElement.htm

An RTF document rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the PDF document, and keeps its formatting, headings, links and images

### Properties

- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the content area, such as a table whose columns hold codes or addresses that cannot be broken between letters, which also makes the rest of the content smaller. Images are reduced to the width of the content area and long words are wrapped on their own, so the option is normally not needed; it matters only for documents with content wider than the content area. The default is false
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `GenerateDocumentOutline`: True to add a bookmark for each heading of the RTF document to the outline of the PDF document. The default is false
- `Width`: The width of the content in points, or 0 to extend it to the right margin of the page
- `X`: The X position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. The content keeps this left offset on the next pages
- `Y`: The Y position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. On the next pages the content starts at the top margin
- `Zoom`: The zoom of the rendered content in percents, from 10 to 200. The default is 100

## RtfPdfDocumentExtensions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_RtfPdfDocumentExtensions.htm

Renders RTF documents in the pages of a PdfDocument

### Methods

- `AddRtf`: Renders an RTF document in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps its formatting, headings, links and images, and its headings can join the outline of the PDF document. The elements added after this call go to the last page of the content, below LastPageRectangle
- `AddRtfAsync`: Asynchronously renders an RTF document in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps its formatting, headings, links and images, and its headings can join the outline of the PDF document. The elements added after this call go to the last page of the content, below LastPageRectangle

## MarkdownToPdfConversionInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_MarkdownToPdfConversionInfo.htm

Information about the result of a Markdown to PDF conversion: the pages produced and the pages inserted before and after them

### Properties

- `ExternalPagesInsertedAfter`: The number of pages of the external documents inserted after the converted content
- `ExternalPagesInsertedBefore`: The number of pages of the external documents inserted before the converted content
- `PagesFromDocument`: The number of pages produced from the Markdown document
- `TocPagesInsertedBefore`: The number of pages of the table of contents inserted before the converted content
- `TotalPages`: The total number of pages of the generated PDF document
- `TotalPagesInsertedBefore`: The number of pages inserted before the converted content: the external documents and the table of contents

## PdfMarkdownElement

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfMarkdownElement.htm

A Markdown document rendered in the pages of a PDF document being created. The content starts at a position in the current page and flows to as many new pages as it needs, laid out inside the margins of the PDF document, and keeps its structure, headings, links, code blocks and images

### Properties

- `DisableRawHtml`: True to ignore the raw markup embedded in the Markdown document. The default is false
- `FallbackFontFiles`: The TrueType or OpenType font files used for the characters that the fonts of the content do not provide, such as the fonts of other scripts
- `FitWideContentToPage`: True to scale down the content as much as needed to show entirely an element wider than the content area, such as an HTML table or block with a fixed width, which also makes the rest of the content smaller. Images are reduced to the width of the content area and long words are wrapped on their own, so the option is normally not needed; it matters only for documents with content wider than the content area. The default is false
- `FontFiles`: The TrueType or OpenType font files made available to the content, in addition to the fonts installed on the machine; a font file provides the family it is named after, so it also takes the place of a font of that name that is not installed
- `GenerateDocumentOutline`: True to add a bookmark for each heading of the Markdown document to the outline of the PDF document. The default is false
- `StyleSheet`: The CSS style sheet applied to the rendered content, or empty for the default styles
- `Width`: The width of the content in points, or 0 to extend it to the right margin of the page
- `X`: The X position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. The content keeps this left offset on the next pages
- `Y`: The Y position where the content starts, in points and relative to the top left corner of the content area of the current page, inside the margins. On the next pages the content starts at the top margin
- `Zoom`: The zoom of the rendered content in percents, from 10 to 200. The default is 100

## MarkdownPdfDocumentExtensions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_MarkdownPdfDocumentExtensions.htm

Renders Markdown documents in the pages of a PdfDocument

### Methods

- `AddMarkdown`: Renders a Markdown document in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps its structure, headings, links, code blocks and images, and its headings can join the outline of the PDF document. The elements added after this call go to the last page of the content, below LastPageRectangle
- `AddMarkdownAsync`: Asynchronously renders a Markdown document in the PDF document: the content starts at the position given by the element in the current page and continues on as many new pages as it needs, inside the margins of the PDF document. The content keeps its structure, headings, links, code blocks and images, and its headings can join the outline of the PDF document. The elements added after this call go to the last page of the content, below LastPageRectangle

## PdfContentRenderInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfContentRenderInfo.htm

Describes where content that flows over one or more pages was rendered in the document: the pages it occupies and the area it covers on each of them

### Properties

- `FirstPageNumber`: The 1-based number of the document page where the content starts
- `LastPageNumber`: The 1-based number of the document page where the content ends
- `LastPageRectangle`: The area covered by the content on the last page it occupies, or null when nothing was rendered. The elements added after the content can be placed below this rectangle
- `PagesCount`: The number of document pages the content occupies
- `RenderedRectangles`: The area covered by the content on each page it occupies, in order, in points and relative to the top left corner of the content area of the page in a document, of the page in an edited document or of the template
