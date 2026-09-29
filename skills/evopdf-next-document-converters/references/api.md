# Word, Excel, RTF and Markdown to PDF: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## WordToPdfConverter

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_WordToPdfConverter.htm

Provides functionality for converting Word (.docx) documents to PDF format with support for customization options such as metadata, security and viewer preferences

### Properties

- `DigitalSignature`: The digital signature to apply to generated PDF document
- `PdfDocumentInfo`: Gets a reference to the object to controlling the generated PDF document information like the document title, author, subject or creation date
- `PdfDocumentOptions`: Gets a reference to the object controlling the conversion process and the generated PDF document properties like PDF document margins, PDF page size and orientation, PDF document header and footer
- `PdfSecurityOptions`: Gets a reference to the object to controlling the generated PDF document security settings like user and owner password, restrict printing or editing of the generated PDF document
- `PdfViewerPreferences`: Gets a reference to the object controlling how the generated PDF is displayed by a PDF viewer
- `ProcessPageBreakMarks`: Gets or sets a value indicating whether page break marks from Word documents should be processed and rendered in the output PDF. The default value is true.
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
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `GenerateTableOfContents`: A flag indicating if a table of contents is automatically generated in the PDF document from headings. The default is false
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire Word document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the Word to PDF converter. This property is used only when UsePageSettingsFromWord is set to false. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the Word to PDF converter. This property is used only when UsePageSettingsFromWord is set to false. The default page size is A4
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `TableOfContents`: The table of contents options. To enable the creation of table of contents from heading tags set the GenerateTableOfContents property on true
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromWord is set to false. By default the left margin is 0
- `UsePageSettingsFromWord`: Whether to use page settings from the Word document. The default value is true
- `Zoom`: Gets or sets the Word viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
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

- `DigitalSignature`: The digital signature to apply to the generated the PDF document
- `ExcelLoaderFilePath`: Sets the full path of the Excel loader file
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
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `ConvertOnlyFirstWorksheet`: Gets or sets a value indicating whether only the first worksheet of the workbook should be converted. When false, all worksheets in the workbook are converted. The default value is false
- `DisplayWorksheetTitle`: Gets or sets a value indicating whether the worksheet title is displayed at the top of each worksheet in the PDF document. The default value is true
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `PageBreakBetweenWorksheets`: Gets or sets a value indicating whether each worksheet starts on a new PDF page. The default value is true
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire Excel document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the Excel to PDF converter. This property is used only when UsePageSettingsFromExcel is set to false. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the Excel to PDF converter. This property is used only when UsePageSettingsFromExcel is set to false. The default page size is A4
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. This property is used only when UsePageSettingsFromExcel is set to false. By default the left margin is 0
- `UseFirstWorksheetPageSettings`: Applies only when UsePageSettingsFromExcel is true. If true, page settings from the first worksheet are used for all worksheets. If false, page settings from the workbook's active worksheet are used for all worksheets (if an active worksheet is set). Has no effect when UsePageSettingsFromExcel is false. The default value is true
- `UsePageSettingsFromExcel`: Gets or sets a value indicating whether to import page setup settings (paper size, margins, orientation) from the workbook and apply them to PdfDocumentOptions before rendering. The default value is true
- `Zoom`: Gets or sets the Excel viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
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

- `DigitalSignature`: The digital signature to apply to generated PDF document
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
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `GenerateTableOfContents`: A flag indicating if a table of contents is automatically generated in the PDF document from headings. The default is false
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire RTF document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the RTF to PDF converter. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the RTF to PDF converter. The default page size is A4
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `TableOfContents`: The table of contents options. To enable the creation of table of contents from heading tags set the GenerateTableOfContents property on true
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `Zoom`: Gets or sets the RTF viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
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

- `DigitalSignature`: The digital signature to apply to generated PDF document
- `DisableRawHtml`: Controls whether raw HTML embedded in Markdown is ignored. The default value is false
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
- `BottomMargin`: The rendered PDF document bottom margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `EnableHeaderFooter`: Indicates whether internal capabilities will be used to generate the header and footer based on the HeaderTemplate and FooterTemplate properties. This provides basic support for adding HTML with page numbering in the header and footer. For more advanced options, use the PdfHtmlHeader and PdfHtmlFooter properties instead. The default value is false.
- `FooterTemplate`: The HTML template used for the PDF document footer. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlFooter property. If null or empty, a default template will be applied.
- `GenerateTableOfContents`: A flag indicating if a table of contents is automatically generated in the PDF document from headings. The default is false
- `HeaderTemplate`: The HTML template used for the PDF document header. It should be valid HTML markup and may include the following CSS classes to inject dynamic values: date (formatted print date), title (document title), pageNumber (current page number) and totalPages (total number of pages). For example, <span class="title"></span> will render a span containing the document title. This template is used only if the EnableHeaderFooter property is true. For advanced scenarios, use the PdfHtmlHeader property. If null or empty, a default template will be applied.
- `LeftMargin`: The rendered PDF document left margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `PageNumberLimit`: The maximum number of PDF pages to generate from conversion. The default value is 0 and the entire Markdown document will be converted to PDF
- `PdfHtmlFooter`: The generated PDF document footer based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfHtmlHeader`: The generated PDF document header based on a HTML template given by Html and HtmlBaseUrl properties or by the HtmlSourceUrl property
- `PdfPageOrientation`: This property controls the orientation of the pages of the PDF document generated by the Markdown to PDF converter. The default orientation is Portrait
- `PdfPageSize`: This property controls the page size of the PDF document generated by the Markdown to PDF converter. The default page size is A4
- `RightMargin`: The rendered PDF document right margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `StyleSheet`: Custom styling rules, using CSS syntax, applied to the Markdown content before the PDF document is generated
- `TableOfContents`: The table of contents options. To enable the creation of table of contents from heading tags set the GenerateTableOfContents property on true
- `TopMargin`: The rendered PDF document top margin in points. 1 point is 1/72 inch. By default the left margin is 0
- `Zoom`: Gets or sets the Markdown viewer zoom percentage, from 10 to 200, decimals allowed. The default value of this property is 100

### Methods

- `AddEndPdf`: Add a PDF from a file to the list of documents to be included after the converted HTML content in the final PDF
- `AddEndPdf`: Add a PDF to the list of documents to be included after the converted HTML content in the final PDF
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
