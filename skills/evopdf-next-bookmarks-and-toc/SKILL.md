---
name: evopdf-next-bookmarks-and-toc
description: "Generate a document outline and an automatic table of contents with EvoPdf Next: GenerateDocumentOutline from HTML headings or from CSS selectors per level (OutlineHeadingSelectors), GenerateTableOfContents with TableOfContentsOptions styling and HeadingSelectors, bookmarks added from code, read and edited in existing PDFs, the open action of the document, a table of contents in documents created from code, page counting and offsets, internal links."
---

# EvoPdf Next: bookmarks and table of contents

EVO HTML to PDF Converter can be configured to automatically create a hierarchy of bookmarks in PDF document based on H1 to H6 head tags found in HTML document by simply turning on the `PdfDocumentOptions.GenerateDocumentOutline` option. An object of `PdfDocumentOptions` type is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`TableOfContentsOptions`**: This class encapsulates the options to control the table of contents automatically generated in PDF from HTML heading tags (H1 to H6) or from any elem...
- **`PdfBookmarkCollection`**: An ordered collection of bookmarks: the top level bookmarks of a document outline or the children of a bookmark
- **`PdfBookmark`**: A bookmark of the document outline. A bookmark opens a location in a page of the document or a web address and can have child bookmarks
- **`PdfDocumentTableOfContents`**: The table of contents of a document created from code. The headings of the HTML content added to the document (the H1 to H6 tags and the elements mark...
- **`PdfFieldAction`**: An action performed by the PDF viewer when a form field event occurs, such as a click on a push button. The static methods of this class create the av...
- **`PdfGoToPageAction`**: An action that displays a page of the document
- **`PdfGoToNamedDestinationAction`**: An action that displays a named destination of the document
- **`PdfOpenUrlAction`**: An action that opens a URL in the web browser
- **`PdfNamedAction`**: A predefined action of the PDF viewer, such as going to the next page or opening the print dialog
- **`PdfNamedActionType`**: The predefined actions of a PDF viewer
- **`PdfJavaScriptAction`**: An action that runs JavaScript code in the PDF viewer

11 types, 70 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

| Task | API |
|---|---|
| Bookmarks from the H1 to H6 headings of the HTML | `PdfDocumentOptions.GenerateDocumentOutline = true` |
| Bookmarks from other elements, one CSS selector per level | `PdfDocumentOptions.OutlineHeadingSelectors = new[] { "h1.chapter", ".section-title" }` |
| Table of contents from the headings | `PdfDocumentOptions.GenerateTableOfContents = true`, styled with `TableOfContentsOptions` |
| Table of contents entries from other elements | `TableOfContentsOptions.HeadingSelectors`, separate from the bookmark selectors |
| Bookmarks from code, after the generated ones | `PdfDocumentOptions.Bookmarks.Add(title, pageNumber, location)`, `AddUrl(title, url)` |
| Open the document at a page, a zoom, a named destination, a script or a URL | `PdfDocumentOptions.OpenAction = new PdfGoToPageAction(2, PdfLinkPageLocation.FitWidth(0))` |
| The same in a document created from code | `PdfDocument.Bookmarks`, `PdfDocument.OpenAction`, `PdfDocument.TableOfContents` (`PdfDocumentTableOfContents`) |
| Read, change, reorder or remove the bookmarks of an existing PDF | `PdfEditor.Bookmarks` (`Find`, `Insert`, `Remove`, `Children`), `PdfEditor.OpenAction` |
| Bookmarks of merged documents | `PdfMerge.KeepSourceBookmarks`, `PdfMerge.Bookmarks` |

A bookmark (`PdfBookmark`) goes to a page at a `PdfLinkPageLocation` (`FitPage()`, `FitWidth(top)`, `FitHeight(left)`,
`AtCoordinates(left, top, zoom)`, from the top left corner of the page) or opens a URL; it can have `Children` and a
`Bold`, `Italic` or `Color` title. The page numbers of the converter bookmarks count the table of contents and the
PDF documents added before the HTML content.

## Auto Create Hierarchical Bookmarks

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/auto-create-hierarchical-bookmarks.htm

## Auto Create Table of Contents

EVO HTML to PDF Converter can be configured to automatically create a table of contents in a PDF document based on H1 to H6 heading tags found in HTML document by simply turning on the `PdfDocumentOptions.GenerateTableOfContents` option. An object of the `PdfDocumentOptions` type is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property. The code below can be used to enable the table of contents generation in the PDF document.

```csharp
// Create a HTML to PDF converter object with default settings
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

// Enable or disable the automatic creation of a table of contents in the PDF document based on H1 to H6 HTML tags
htmlToPdfConverter.PdfDocumentOptions.GenerateTableOfContents = true;
```

### Table Title

```csharp
// Set the table of contents title
htmlToPdfConverter.PdfDocumentOptions.TableOfContents.Title = "Table of Contents";
```

### Inline Table of Contents

```csharp
                  // Create the table of contents inline
htmlToPdfConverter.PdfDocumentOptions.TableOfContents.CreateInline = true;
```

All 10 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/auto-create-table-of-contents.htm

## Convert Internal Links from HTML to PDF

EVO HTML to PDF Converter automatically converts all the internal links from HTML to internal links in PDF.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-internal-links-from-html-to-pdf.htm

## Add Bookmarks and Open Action to Generated PDF

The bookmarks of the PDF document generated by the HTML to PDF converter can be created automatically from the headings of the HTML document, as described in the topic and they can be added from code to the `PdfDocumentOptions.Bookmarks` collection, after the bookmarks created from the headings. A bookmark goes to a page at a given location and zoom or opens a web address, it can have child bookmarks and a bold, italic or colored title.

### Add Bookmarks from Code

```csharp
PdfBookmarkCollection bookmarks = htmlToPdfConverter.PdfDocumentOptions.Bookmarks;

PdfBookmark contentBookmark = bookmarks.Add("HTML content", 1, PdfLinkPageLocation.FitPage());
contentBookmark.Open = true;
contentBookmark.Bold = true;
contentBookmark.Color = PdfColor.DarkBlue;
contentBookmark.Children.Add("Second page at 150%", 2, PdfLinkPageLocation.AtCoordinates(0, 0, 1.5f));

// A bookmark that opens a web address in the browser
bookmarks.AddUrl("Product web site", "https://www.evopdf.com");

// Display the bookmarks panel when the document is opened
htmlToPdfConverter.PdfViewerPreferences.PageMode = ViewerPageMode.UseOutlines;
```

### Set the Open Action

```csharp
// The viewer opens the document at the second page, fitted to the window width
htmlToPdfConverter.PdfDocumentOptions.OpenAction = new PdfGoToPageAction(2, PdfLinkPageLocation.FitWidth(0));
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-bookmarks-and-open-action.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
