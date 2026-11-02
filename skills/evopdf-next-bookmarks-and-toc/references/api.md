# Bookmarks and table of contents: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## TableOfContentsOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_TableOfContentsOptions.htm

This class encapsulates the options to control the table of contents automatically generated in PDF from HTML heading tags (H1 to H6) or from any element marked with the data-heading attribute. Any element can become a table of contents entry with data-heading set to a level from 1 to 6 (for example data-heading="2"), the entry text is taken from the data-heading-text attribute when present otherwise from the element text, and a real heading can be excluded with data-heading="false". The custom data-heading attribute is honored only in custom mode

### Properties

- `CountStartPages`: Controls if the external PDF pages inserted before it are counted in page numbers displayed in table of contents. The property does not have any effect for inline table of contents. The default value is true
- `CountTocAndStartPages`: A flag indicating if the table of contents pages and the PDF pages inserted before the HTML content are counted when calculating the page numbers displayed in table of contents. The property does not apply for inline table of contents. By default this property is true
- `CountTocPages`: Controls if table of contents pages are counted in page numbers. The property does not have any effect for inline table of contents. The default value is true
- `CreateInline`: A flag indicating whether the table of contents is integrated into the document being converted. In the context of the HTML to PDF Converter, its position is determined by a DIV element with the ID 'html-to-pdf-toc'. If such an element is not predefined in the HTML, it will be created by the converter. When creating a table of contents for other types of documents, the inline table of contents is created at the beginning of the document.
- `HeadingSelectors`: The CSS selectors of the elements that become entries of the table of contents, one selector for each level: the first selector gives the level 1 entries, the second one the level 2 entries and so on, up to level 6. When this property is set the H1 to H6 headings and the data-heading attribute no longer select the entries, an element still being excluded with data-heading="false" and its title still being taken from data-heading-text when present. The selectors are honored in custom mode. The default is null, the entries being created from the headings of the document
- `PageNumbersBoldFont`: Gets or sets the default bold font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold font will be used
- `PageNumbersBoldItalicFont`: Gets or sets the default bold italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold italic font will be used
- `PageNumbersFont`: Gets or sets the default regular font used for page numbers. If not set, a default regular font will be used
- `PageNumbersItalicFont`: Gets or sets the default italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default italic font will be used
- `PageNumbersOffset`: An offset to be applied to all page numbers in the table of contents. This property can be useful when merging a PDF with a table of contents with other PDF documents. By default this property is 0
- `ShowPageNumbers`: A flag indicating if the table of contents includes page numbers for entries. By default this property is true
- `Style`: The table of contents CSS style
- `Title`: The table of contents title
- `UseBrowserMode`: This property can be used to select between using the browser or custom mode to create the table of contents. This property has effect only when CreateInline is false. By default this property is false. The custom data-heading, data-heading-text and data-heading="false" attributes and the HeadingSelectors are honored only in custom mode. In browser mode only the standard H1 to H6 heading tags are used

## PdfBookmarkCollection

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfBookmarkCollection.htm

An ordered collection of bookmarks: the top level bookmarks of a document outline or the children of a bookmark

### Properties

- `Count`: The number of bookmarks in the collection
- `Item`

### Methods

- `Add`: Adds a bookmark at the end of the collection
- `Add`: Adds a bookmark that opens a page of the document
- `AddUrl`: Adds a bookmark that opens a web address
- `Clear`: Removes all the bookmarks from the collection
- `Find`: Finds the first bookmark with the given title in the collection and, when searching recursively, below it
- `GetAllBookmarks`: Enumerates the bookmarks of the collection and all the bookmarks below them, in outline order
- `GetEnumerator`: Enumerates the bookmarks of the collection
- `IndexOf`: Gets the position of a bookmark in the collection, or -1 when the bookmark is not in the collection
- `Insert`: Inserts a bookmark at the given position
- `Remove`: Removes a bookmark and its children from the collection
- `RemoveAt`: Removes the bookmark at the given position and its children

## PdfBookmark

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfBookmark.htm

A bookmark of the document outline. A bookmark opens a location in a page of the document or a web address and can have child bookmarks

### Properties

- `Bold`: True when the bookmark title is displayed in bold. The default is false
- `Children`: The child bookmarks
- `Color`: The color of the bookmark title, or null for the default color of the viewer
- `Italic`: True when the bookmark title is displayed in italic. The default is false
- `Location`: The location and zoom shown in the page. When null, the viewer shows the top of the page with the current zoom
- `Open`: True when the child bookmarks are displayed expanded when the document is opened. The default is true
- `PageNumber`: The 1-based number of the page the bookmark opens, or 0 when the bookmark opens a web address or has no destination
- `Parent`: The bookmark this bookmark is a child of, or null for a top level bookmark
- `Title`: The text displayed in the outline
- `Url`: The web address opened by the bookmark, or null when the bookmark opens a page of the document

## PdfDocumentTableOfContents

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfDocumentTableOfContents.htm

The table of contents of a document created from code. The headings of the HTML content added to the document (the H1 to H6 tags and the elements marked with the data-heading attribute) are listed with their page numbers on pages inserted at the beginning of the document when the document is saved, each entry linking to its heading

### Properties

- `CountTocPages`: Controls if the table of contents pages are counted in the page numbers displayed in the table of contents. The default value is true
- `Enabled`: A flag indicating whether the table of contents is generated when the document is saved. By default this property is false
- `HeadingSelectors`: The CSS selectors of the elements of the HTML content that become entries of the table of contents, one selector for each level: the first selector gives the level 1 entries, the second one the level 2 entries and so on, up to level 6. When this property is set the H1 to H6 headings and the data-heading attribute no longer select the entries, an element still being excluded with data-heading="false" and its title still being taken from data-heading-text when present. The default is null, the entries being created from the headings of the HTML content
- `PageNumbersBoldFont`: Gets or sets the default bold font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold font will be used
- `PageNumbersBoldItalicFont`: Gets or sets the default bold italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold italic font will be used
- `PageNumbersFont`: Gets or sets the default regular font used for page numbers. If not set, a default regular font will be used
- `PageNumbersItalicFont`: Gets or sets the default italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default italic font will be used
- `PageNumbersOffset`: An offset to be applied to all the page numbers displayed in the table of contents. By default this property is 0
- `ShowPageNumbers`: A flag indicating if the table of contents includes page numbers for entries. By default this property is true
- `Style`: The table of contents CSS style. The default style can be replaced with a style for the html-to-pdf-toc element, its toc-title, level-N, toc-link, toc-link-level-N and page-link classes
- `Title`: The table of contents title

## PdfFieldAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFieldAction.htm

An action performed by the PDF viewer when a form field event occurs, such as a click on a push button. The static methods of this class create the available actions. Several actions can be chained with Next, in which case they are performed in sequence

### Properties

- `Next`: The action performed after this one, or null when this is the last action of the sequence

### Methods

- `GoToNamedDestination`: Creates an action that displays a named destination of the document
- `GoToPage`: Creates an action that displays a page of the document
- `HideFields`: Creates an action that hides form fields
- `ImportData`: Creates an action that imports form field values from a FDF file
- `JavaScript`: Creates an action that runs JavaScript code in the PDF viewer
- `Named`: Creates a predefined viewer action, such as going to the next page or opening the print dialog
- `OpenUrl`: Creates an action that opens a URL in the web browser
- `ResetForm`: Creates an action that resets form fields to their default values. All the fields of the document are reset when no field names are given
- `ShowFields`: Creates an action that shows previously hidden form fields
- `SubmitForm`: Creates an action that submits the form field values to a URL. By default all the fields are submitted in HTML format with the POST method

## PdfGoToPageAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfGoToPageAction.htm

An action that displays a page of the document

### Properties

- `Location`: The location and zoom displayed on the page
- `PageNumber`: The 1-based number of the page to display

## PdfGoToNamedDestinationAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfGoToNamedDestinationAction.htm

An action that displays a named destination of the document

### Properties

- `DestinationName`: The name of the destination

## PdfOpenUrlAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfOpenUrlAction.htm

An action that opens a URL in the web browser

### Properties

- `Url`: The URL to open

## PdfNamedAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfNamedAction.htm

A predefined action of the PDF viewer, such as going to the next page or opening the print dialog

### Properties

- `ActionType`: The predefined action

## PdfNamedActionType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfNamedActionType.htm

The predefined actions of a PDF viewer

### Fields and values

- `FirstPage`: Displays the first page of the document
- `LastPage`: Displays the last page of the document
- `NextPage`: Displays the next page of the document
- `PreviousPage`: Displays the previous page of the document
- `Print`: Opens the print dialog

## PdfJavaScriptAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfJavaScriptAction.htm

An action that runs JavaScript code in the PDF viewer

### Properties

- `Code`: The JavaScript code
