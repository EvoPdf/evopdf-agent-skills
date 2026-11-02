# Split PDF documents: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfSplit

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSplit.htm

Extracts pages from a PDF document into new PDF documents: a range of pages, the pages selected by page ranges, the document split in parts of a given number of pages, or the document without some of its pages. The extracted pages keep their form fields, annotations, bookmarks and scripts. The loaded document can be password protected

### Properties

- `PageCount`: The number of pages of the loaded document
- `PdfDocumentInfo`: Gets a reference to the object controlling the information of the extracted documents, like the document title, author, subject or creation date. The values of the loaded document are used by default
- `PdfSecurityOptions`: Gets a reference to the object controlling the security settings of the extracted documents, like user and owner password, restrict printing or editing
- `PdfViewerPreferences`: Gets a reference to the object controlling how the extracted documents are displayed by a PDF viewer. The values of the loaded document are used by default

### Methods

- `Dispose`: Releases all resources used by this instance
- `ExtractPage`: Extracts one page into a new PDF document
- `ExtractPages`: Extracts a range of pages into a new PDF document
- `ExtractPages`: Extracts the pages selected by page ranges into a new PDF document, in the order of the ranges
- `ExtractPagesToFile`: Extracts a range of pages into a new PDF file
- `ExtractPagesToFile`: Extracts the pages selected by page ranges into a new PDF file, in the order of the ranges
- `RemovePages`: Creates a new PDF document with all the pages except the ones selected by page ranges
- `SplitByPageCount`: Splits the document into consecutive parts with a given number of pages. The last part holds the remaining pages
- `SplitByPageCountToFiles`: Splits the document into consecutive parts with a given number of pages, saved to files. The last part holds the remaining pages
- `SplitByPageRanges`: Splits the document into parts, each part holding the pages selected by one page ranges text
