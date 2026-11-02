# PDF redaction: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfRedactor

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRedactor.htm

Removes text, images and graphics from areas of the pages of a PDF document. The content of an area is removed from the document, not only covered: text found in an area can no longer be selected, copied or extracted and the pixels of images inside the area are replaced. Areas are given as rectangles or whole pages, found by searching the text or the personal data of the document, or read from the redaction annotations of the document. Each area can be covered with a color, an image and an overlay text. The metadata and the hidden content of the document can be removed too

### Properties

- `FillColor`: The color that covers the redacted areas. The default is black; null leaves the areas empty
- `OverlayFontSize`: The font size of the overlay text in points. The default 0 fits the text in each area
- `OverlayText`: The text written over each redacted area, for example "REDACTED". The default is no text
- `OverlayTextColor`: The color of the overlay text. The default is white
- `OverlayTextLanguage`: The language of the overlay text, as a language tag like en-US, written in accessible documents. When not set, the language of the document is used
- `PageCount`: The number of pages of the loaded document
- `PdfDocumentInfo`: Gets a reference to the object controlling the information of the redacted document, like the document title, author, subject or creation date. The values of the loaded document are used by default
- `PdfSecurityOptions`: Gets a reference to the object controlling the security settings of the redacted document, like user and owner password, restrict printing or editing
- `PdfViewerPreferences`: Gets a reference to the object controlling how the redacted document is displayed by a PDF viewer. The values of the loaded document are used by default
- `Redactions`: The areas marked for redaction
- `RemoveEmbeddedFiles`: Removes the files attached to the document and the file attachment annotations. The default is false
- `RemoveHiddenContent`: Removes from all the pages the content a viewer does not show: invisible text such as the searchable text of scanned pages, text outside the visible area, layers turned off and hidden annotations and form fields. The default is false
- `RemoveIntersectingAnnotations`: Removes the annotations, links and form fields that intersect a redacted area. The default is true
- `RemoveJavaScript`: Removes the document level scripts and the scripts run when the document is opened. The default is false
- `RemoveMetadata`: Removes the metadata of the document, pages, images and fonts, the page thumbnails and the document information. The title, author, subject, keywords and creator set after loading the document are still written, as well as the title of PDF/UA documents and the identification of the PDF/A and PDF/UA standards. The default is false
- `RepeatOverlayText`: Repeats the overlay text to fill each area. The default is false
- `Result`: What the redaction removed, available after the document is saved, null before

### Methods

- `AddRedaction`: Marks an area for redaction
- `AddRedaction`: Marks an area of a page for redaction
- `AddRedaction`: Marks the areas of a text found in the document for redaction
- `AddRedactionAnnotations`: Marks for redaction the areas of the redaction annotations of the document, with the fill color and the overlay text set in each annotation. The annotations are removed from the saved document
- `Dispose`: Releases all resources used by this instance
- `FindPersonalData`: Finds personal data recognized by its format in the text of the document, like email addresses, phone numbers, bank account and payment card numbers. Review the results before marking them for redaction
- `FindText`: Finds a text in all the pages of the document. The white space of the searched text matches any white space, including a line break
- `FindText`: Finds the texts that match a regular expression in all the pages of the document. The text of a page is read in reading order, with one space between words and one line break between lines
- `GetPdfPageInfo`: Gets the size, rotation and boxes of a page of the loaded document
- `RedactPage`: Marks a whole page for redaction. All the content of the page is removed, including the content outside the visible area of the page
- `RedactPages`: Marks a range of pages for redaction, as RedactPage does for each page
- `RedactPersonalData`: Marks for redaction the personal data found by FindPersonalData. The same texts are replaced with OverlayText, or with the word Redacted when it is not set, in the document information, metadata, bookmarks, annotation and form field values and alternate descriptions
- `RedactText`: Marks for redaction every occurrence of a text in the document. The same texts are replaced with OverlayText, or with the word Redacted when it is not set, in the document information, metadata, bookmarks, annotation and form field values and alternate descriptions
- `RedactText`: Marks for redaction every text of the document that matches a regular expression. The same texts are replaced with OverlayText, or with the word Redacted when it is not set, in the document information, metadata, bookmarks, annotation and form field values and alternate descriptions
- `Save`: Applies the redactions and returns the redacted PDF document. The redactor cannot be used after the document is saved
- `SaveAsync`: Asynchronously applies the redactions and returns the redacted PDF document
- `SaveRedactionMarks`: Saves the document with the marked areas as redaction annotations, without removing any content. The marks can be reviewed and changed in a PDF viewer, then applied with AddRedactionAnnotations. The redactor cannot be used after the document is saved
- `SaveRedactionMarksToFile`: Saves the document with the marked areas as redaction annotations to a file, without removing any content, as SaveRedactionMarks does
- `SaveToFile`: Applies the redactions and saves the redacted PDF document to a file
- `SaveToFileAsync`: Asynchronously applies the redactions and saves the redacted PDF document to a file
- `SetOverlayImage`: Sets an image drawn over each redacted area from an image file. The supported formats are Jpeg, Png and Webp
- `SetOverlayImage`: Sets an image drawn over each redacted area, after the fill color and under the overlay text. The image format is determined from the data. The supported formats are Jpeg, Png and Webp

## PdfRedactionArea

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRedactionArea.htm

An area of a page whose content is removed by PdfRedactor. The area is measured in points from the top left corner of the page as it is displayed

### Properties

- `Bounds`: The bounds of the area, in points from the top left corner of the displayed page
- `FillColor`: The color that covers the area after its content is removed. When not set, the FillColor of the redactor is used; null leaves the area empty
- `OverlayFontSize`: The font size of the overlay text in points. Zero uses the OverlayFontSize of the redactor
- `OverlayText`: The text written over the area. When not set, the OverlayText of the redactor is used; an empty text writes nothing
- `OverlayTextColor`: The color of the overlay text. When null, the OverlayTextColor of the redactor is used
- `PageNumber`: The 1-based number of the page
- `RepeatOverlayText`: Repeats the overlay text to fill the area. When not set, the RepeatOverlayText of the redactor is used

### Methods

- `SetOverlayImage`: Sets an image drawn over the area from an image file. The supported formats are Jpeg, Png and Webp
- `SetOverlayImage`: Sets an image drawn over the area, after the fill color and under the overlay text. The image format is determined from the data. The supported formats are Jpeg, Png and Webp. When not set, the image set with SetOverlayImage is used; null draws no image

## PdfRedactionResult

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRedactionResult.htm

What the redaction removed from the document, available from Result after the document is saved

### Properties

- `RedactedImages`: The number of images whose pixels inside the redacted areas were replaced
- `RedactedShapes`: The number of shapes, lines and clipping paths cut around the redacted areas
- `RemovedAnnotations`: The number of annotations removed from the redacted areas
- `RemovedCharacters`: The number of characters removed from the text of the pages
- `RemovedFormFields`: The number of form fields removed from the redacted areas
- `RemovedHiddenItems`: The number of hidden items removed when RemoveHiddenContent is set: invisible characters, hidden shapes and images and hidden annotations
- `RemovedImages`: The number of images removed entirely, instead of having their pixels replaced
- `RemovedSignatures`: The number of digital signatures removed, since they no longer match the redacted content

## PdfPersonalDataTypes

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPersonalDataTypes.htm

The kinds of personal data found by FindPersonalData. The values can be combined

### Fields and values

- `All`: All the kinds of personal data
- `EmailAddress`: Email addresses
- `Iban`: International bank account numbers with valid check digits
- `IpAddress`: IPv4 and IPv6 addresses
- `None`: No personal data
- `PaymentCardNumber`: Payment card numbers with a valid check digit
- `PhoneNumber`: Phone numbers in national or international format
- `RomanianPersonalNumericCode`: Romanian personal numeric codes (CNP) with a valid check digit
- `UsSocialSecurityNumber`: US social security numbers written as 3, 2 and 4 digits separated by hyphens

## PdfTextMatch

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextMatch.htm

A text found in a page by FindText or FindPersonalData, with the areas it covers, one for each line, measured in points from the top left corner of the displayed page

### Properties

- `Bounds`: The bounds of the text on each line it occupies
- `PageNumber`: The 1-based number of the page
- `PersonalDataType`: The kind of personal data found by FindPersonalData, or None for a text found by the other searches
- `Quads`: The corners of the text on each line it occupies, following the direction of the text, which gives the exact area of rotated text
- `Text`: The text found
