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
- `PageNumbersBoldFont`: Gets or sets the default bold font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold font will be used
- `PageNumbersBoldItalicFont`: Gets or sets the default bold italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default bold italic font will be used
- `PageNumbersFont`: Gets or sets the default regular font used for page numbers. If not set, a default regular font will be used
- `PageNumbersItalicFont`: Gets or sets the default italic font used for page numbers. If not set but PageNumbersFont is set, that font will be used as a fallback. If PageNumbersFont is not set either, a default italic font will be used
- `PageNumbersOffset`: An offset to be applied to all page numbers in the table of contents. This property can be useful when merging a PDF with a table of contents with other PDF documents. By default this property is 0
- `ShowPageNumbers`: A flag indicating if the table of contents includes page numbers for entries. By default this property is true
- `Style`: The table of contents CSS style
- `Title`: The table of contents title
- `UseBrowserMode`: This property can be used to select between using the browser or custom mode to create the table of contents. This property has effect only when CreateInline is false. By default this property is false. The custom data-heading, data-heading-text and data-heading="false" attributes are honored only in custom mode. In browser mode only the standard H1 to H6 heading tags are used
