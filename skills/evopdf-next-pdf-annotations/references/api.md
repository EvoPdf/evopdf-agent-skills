# Link and text annotations: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfLinkAnnotation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLinkAnnotation.htm

A clickable hyperlink annotation placed on a PDF page. Use one of the static factory methods (FromUrl, ToPage, ToPageLocation to build the link, then pass it to AddLinkAnnotation on the document, editor, merger or HTML-to-PDF document options. For PDF/UA compliance, set Description to a meaningful alternative description

### Properties

- `BorderStyle`: Border style drawn around the clickable hotspot. Default: None (no visible border, typical for text hyperlinks where the link target is implied by the underlying text)
- `BorderWidth`: Border width in points when BorderStyle is not None. Default: 1.0
- `Description`: Alternative text description used by assistive technologies and written to the annotation's /Contents entry. Required by PDF/UA-1 and PDF/UA-2; recommended for all documents. When omitted, the URL (for URI links) or a generic phrase is used as a fallback so /Contents is never empty
- `Height`: Height of the clickable hotspot in points
- `HighlightMode`: Visual feedback shown when the user activates the link (mouse-down / tap). Default: Invert
- `PageNumber`: Page number on which the clickable hotspot is placed (1-based)
- `Width`: Width of the clickable hotspot in points
- `X`: X coordinate of the hotspot's top-left corner, in points (1/72 in), measured from the left edge of the page content area
- `Y`: Y coordinate of the hotspot's top-left corner, in points, measured from the top edge of the page content area

### Methods

- `FromUrl`: Creates a hyperlink that opens an external URL when activated. Produces a URI action (always allowed in PDF/A)
- `ToPage`: Creates a hyperlink that jumps to another page within the same document, fitting the whole page in the viewport. Produces a GoTo action with a /Fit destination (allowed in every PDF/A)
- `ToPageLocation`: Creates a hyperlink that jumps to a specific location on another page within the same document. Produces a GoTo action with an explicit destination (Xyz / FitH / FitV / FitR) controlled by the supplied PdfLinkPageLocation

## PdfLinkBorderStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLinkBorderStyle.htm

Border style drawn around a PdfLinkAnnotation hotspot. Default is None -- typical for text hyperlinks where the clickable area is implied by the underlying text content

### Fields and values

- `Dashed`: Dashed rectangular border. Width is controlled by BorderWidth
- `None`: No visible border. The annotation is still clickable, but the hotspot has no outline. Recommended for text hyperlinks
- `Solid`: Solid rectangular border. Width is controlled by BorderWidth

## PdfLinkHighlightMode

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLinkHighlightMode.htm

Visual feedback shown when the user activates a PdfLinkAnnotation hotspot

### Fields and values

- `Invert`: Invert the colors of the hotspot rectangle (the default and most universally supported feedback mode)
- `None`: No visual feedback when the link is activated
- `Outline`: Invert the annotation's border (only effective when the link has a visible PdfLinkBorderStyle)
- `Push`: Display the annotation as if it were being pushed below the surface of the page (button-like effect)

## PdfLinkPageFit

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLinkPageFit.htm

Page-fit type used by PdfLinkPageLocation to describe how the target page is displayed when the link is followed

### Fields and values

- `AtCoordinates`: Position the page so that (Left, Top) is at the top-left of the viewport, optionally applying an explicit Zoom factor (PDF /XYZ destination)
- `PageHeight`: Fit the page height in the viewport, scrolled to the specified Left position (PDF /FitV destination)
- `PageWidth`: Fit the page width in the viewport, scrolled to the specified Top position (PDF /FitH destination)
- `WholePage`: Display the entire page in the viewport (PDF /Fit destination)

## PdfLinkPageLocation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfLinkPageLocation.htm

Describes how the target page is positioned and zoomed when a PdfLinkAnnotation created with ToPageLocation is followed. Use one of the static factory methods (FitPage, FitWidth, FitHeight, AtCoordinates) to construct an instance

### Properties

- `Fit`: Fit type applied when the link is followed
- `Left`: Distance in points from the LEFT edge of the page to the destination point. Used by PageHeight and AtCoordinates
- `Top`: Distance in points from the TOP edge of the page to the destination point. Used by PageWidth and AtCoordinates
- `Zoom`: Zoom factor (1.0 = 100%) applied by AtCoordinates. When null, the viewer keeps the user's current zoom level

### Methods

- `AtCoordinates`: Positions the page so that the given (left, top) point is at the top-left of the viewport, optionally setting an explicit zoom factor
- `FitHeight`: Fits the page height in the viewport, with the left edge at the specified distance (in points) from the left edge of the page content area defined by PdfDocument.Margins when AddLinkAnnotation is called. The value is treated as page-relative by PdfEditor and PdfMerge
- `FitPage`: Fits the entire page in the viewport (PDF /Fit destination). Equivalent to using ToPage
- `FitWidth`: Fits the page width in the viewport, with the top edge at the specified distance (in points) from the top edge of the page content area defined by PdfDocument.Margins when AddLinkAnnotation is called. The value is treated as page-relative by PdfEditor and PdfMerge

## PdfTextAnnotation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextAnnotation.htm

Represents a sticky-note style annotation displayed as a clickable icon on a specific page of the PDF. Clicking the icon opens a popup with the note text. The annotation also appears in the viewer's Comments panel alongside any other comments. Use Create to create an instance, then pass it to AddTextAnnotation on the document, editor or merger

### Properties

- `Author`: Optional author name displayed in the popup title and the Comments panel (for example, "Reviewed by John"). When null, no author is recorded
- `Contents`: The note text shown in the popup and the Comments panel. Required for accessibility (PDF/UA) -- the value supplied at construction time is also used as the screen-reader text
- `Height`: Height of the icon, in points. Default is 24
- `Icon`: Icon used for the annotation marker. Default is Note
- `Open`: When true, the popup is displayed expanded as soon as the page is opened. Default is false
- `PageNumber`: 1-based page number on which the annotation icon is placed
- `Width`: Width of the icon, in points. Default is 24
- `X`: The X coordinate of the annotation icon's top-left corner, in points, measured from the left edge of the page content area
- `Y`: The Y coordinate of the annotation icon's top-left corner, in points, measured from the top of the page content area

### Methods

- `Create`: Creates a sticky-note text annotation. The note text is required and is used both as the popup content and as the accessibility description for screen readers

## PdfTextAnnotationIcon

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextAnnotationIcon.htm

Icon displayed on the page surface for a sticky-note text annotation

### Fields and values

- `Comment`: Speech bubble. Suggests a comment or discussion
- `Help`: Question mark in a circle. Suggests a query or clarification needed
- `Insert`: Caret pointing up. Suggests an editorial insertion at this position
- `Note`: Folded-corner note paper. Default icon used by Adobe Acrobat
