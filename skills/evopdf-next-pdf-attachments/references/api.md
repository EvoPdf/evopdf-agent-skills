# File attachments and embedded files: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfFileAttachment

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFileAttachment.htm

Represents a file attached to a PDF document. The attachment is added to the document's Attachments panel in PDF viewers. To also display a clickable icon on a specific page, use PdfFileAttachmentAnnotation instead. Use one of the static factory methods (FromBytes, FromFile, FromExternalPath, FromUrl) to create an instance, then pass it to AddFileAttachment on the document, editor, merger or HTML to PDF document options

### Properties

- `Description`: Optional description displayed next to the file name in the Attachments panel
- `FileName`: File name shown in the Attachments panel, including extension (for example, "invoice.xml"). Ignored for URL attachments
- `MimeType`: Optional MIME type of the file (for example, "application/xml", "text/csv"). Helps PDF viewers select the correct application when opening the attachment
- `Relationship`: Relationship between the attached file and the PDF document. Required for PDF/A-3 and PDF/A-4f compliance, ignored for other standards. Default is Unspecified

### Methods

- `FromBytes`: Creates an attachment from a byte array. The file content is embedded in the PDF
- `FromExternalPath`: Creates an attachment that references an external file by path. The file is not embedded and the reference depends on the PDF reader being able to access the same path. Use FromFile for portable PDFs
- `FromFile`: Creates an attachment from a file on disk. The file is read and its content is embedded in the PDF. The file name shown in the Attachments panel is taken from the path
- `FromUrl`: Creates an attachment that references a URL. Opens in the default browser when activated

## PdfFileAttachmentAnnotation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFileAttachmentAnnotation.htm

Represents a file attachment displayed as a clickable icon on a specific page of the PDF. Clicking the icon opens or extracts the attached file. The file is also added to the Attachments panel alongside any other attachments. Use one of the static factory methods (FromBytes, FromFile, FromExternalPath, FromUrl) to create an instance, then pass it to AddFileAttachmentAnnotation on the document, editor, merger, or HTML-to-PDF options

### Properties

- `AddToAttachmentsPanel`: When true, also adds the file to the document-level /Names /EmbeddedFiles tree, producing a separate entry in the viewer's Attachments panel alongside the page annotation. Default is false
- `Description`: Optional description displayed in the Attachments panel
- `FileName`: File name shown in the Attachments panel, including extension (for example, "invoice.xml"). Ignored for URL attachments
- `Height`: Height of the icon, in points. Default is 24
- `Icon`: Icon used for the attachment annotation. Default is Paperclip
- `MimeType`: Optional MIME type of the file (for example, "application/xml")
- `PageNumber`: 1-based page number on which the attachment icon is placed
- `Relationship`: Relationship between the attached file and the PDF document. Required for PDF/A-3 and PDF/A-4f compliance, ignored for other standards. Default is Unspecified
- `TooltipText`: Tooltip shown when the mouse hovers over the icon
- `Width`: Width of the icon, in points. Default is 24
- `X`: The X coordinate of the annotation icon's top-left corner, in points, measured from the left edge of the page content area
- `Y`: The Y coordinate of the annotation icon's top-left corner, in points, measured from the top of the page content area

### Methods

- `FromBytes`: Creates an attachment annotation from a byte array. The file content is embedded in the PDF and an icon is placed on the specified page
- `FromExternalPath`: Creates an attachment annotation that references an external file by path. The file is NOT embedded; the reference depends on the PDF reader being able to access the same path
- `FromFile`: Creates an attachment annotation from a file on disk. The file is read, embedded in the PDF, and an icon is placed on the specified page
- `FromUrl`: Creates an attachment annotation that references a URL. Clicking the icon opens the URL in the default browser

## PdfAttachmentIcon

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfAttachmentIcon.htm

Icon displayed for an attachment annotation on a PDF page

### Fields and values

- `Graph`: Graph icon
- `Paperclip`: Paperclip icon. Default for new attachments
- `PushPin`: Push pin icon
- `Tag`: Tag icon

## PdfAttachmentRelationship

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfAttachmentRelationship.htm

Relationship between an attached file and the host PDF document, per PDF/A-3 and PDF/A-4 specifications

### Fields and values

- `Alternative`: The attached file is an alternative representation of the PDF (for example, an audio version)
- `Data`: The attached file is the primary content; the PDF is a visual presentation of it
- `EncryptedPayload`: The attached file is an externally encrypted payload, and the PDF acts as a wrapper or cover sheet. Valid only for PDF/A-4f. The library does not encrypt the attachment -- the file must already be encrypted by an external mechanism (PKCS#7, PGP, etc.) before being added
- `Source`: The attached file is the source data the PDF was derived from (for example, an XML invoice for an invoice PDF). Common for e-invoicing workflows such as ZUGFeRD and Factur-X
- `Supplement`: The attached file is supplementary material (for example, additional documentation referenced from the PDF)
- `Unspecified`: Relationship is not specified. Default value, used for documents that do not target PDF/A-3 or PDF/A-4f
