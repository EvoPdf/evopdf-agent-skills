# Selecting HTML elements and reading their PDF position: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## HtmlElementInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_HtmlElementInfo.htm

Represents detailed style and content information about a single HTML element

### Properties

- `CssClass`: Gets or sets the CSS class attribute of the HTML element
- `Id`: Gets or sets the ID of the HTML element
- `RenderedRectangles`: Gets or sets the list of rendered rectangles showing where the element appears in the PDF
- `TagName`: Gets or sets the HTML tag name of the element (e.g., "div", "span").
- `Text`: Gets or sets the plain text content of the element

## HtmlElementInfoCollection

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_HtmlElementInfoCollection.htm

Represents a collection of HTML elements and provides lookup functionality

### Properties

- `Elements`: Gets all collected HTML elements

### Methods

- `Add`: Adds an HTML element to the collection
- `FindByClass`: Finds all elements that contain the specified CSS class
- `FindById`: Finds all elements with the specified ID
- `FindByTagName`: Finds all elements with the specified HTML tag name
