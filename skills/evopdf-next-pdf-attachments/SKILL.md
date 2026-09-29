---
name: evopdf-next-pdf-attachments
description: "Attach files to a PDF with EvoPdf Next: document-level attachments, clickable attachment annotations, MIME types, attachment icons, and the PDF/A-3 and PDF/A-4f relationship values (Data, Source, Alternative, Supplement) required for hybrid documents such as electronic invoices."
---

# EvoPdf Next: file attachments and embedded files

EVO HTML to PDF Converter can embed arbitrary files directly into the generated PDF as document-level attachments. The attached files appear in the Attachments panel of the viewer and travel with the document. The receiver does not need access to the original source files.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfFileAttachment`**: Represents a file attached to a PDF document. The attachment is added to the document's Attachments panel in PDF viewers. To also display a clickable ...
- **`PdfFileAttachmentAnnotation`**: Represents a file attachment displayed as a clickable icon on a specific page of the PDF. Clicking the icon opens or extracts the attached file. The f...
- **`PdfAttachmentIcon`**: Icon displayed for an attachment annotation on a PDF page
- **`PdfAttachmentRelationship`**: Relationship between an attached file and the host PDF document, per PDF/A-3 and PDF/A-4 specifications

4 types, 34 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Add Attachments to Generated PDF

### Embed In-Memory Data with FromBytes

```csharp
byte[] xmlBytes = Encoding.UTF8.GetBytes(BuildSampleXml());
var xmlAttachment = PdfFileAttachment.FromBytes(xmlBytes, "data.xml");
xmlAttachment.MimeType = "application/xml";
xmlAttachment.Description = "Source XML data";
xmlAttachment.Relationship = PdfAttachmentRelationship.Source;

htmlToPdfConverter.PdfDocumentOptions.AddFileAttachment(xmlAttachment);
```

### Embed a File from Disk with FromFile

```csharp
string alphabetFilePath = Path.Combine(GetDemoTextsPath(), "Alphabet.txt");
var textAttachment = PdfFileAttachment.FromFile(alphabetFilePath);
textAttachment.MimeType = "text/plain";
textAttachment.Description = "Sample alphabet text";

htmlToPdfConverter.PdfDocumentOptions.AddFileAttachment(textAttachment);
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-attachments-to-generated-pdf.htm

## Add File Attachments to Existing PDF

The `PdfEditor` class lets you add new content to an existing PDF without rewriting the rest of the document. The editor is instantiated with the source PDF bytes or file path and an optional password. The standard claimed by the source document and its existing page geometry are inherited by the editor. Content is added at absolute page coordinates using the dedicated Add methods. Every Add method takes the target page number as the first argument. If the rendered content overflows the available space a new page is created automatically by the engine. The resulting modified PDF is produced with `PdfEditor.Save`.

### Document-Level Attachments

```csharp
byte[] xmlBytes = Encoding.UTF8.GetBytes(BuildSampleXml());
var xmlAttachment = PdfFileAttachment.FromBytes(xmlBytes, "data.xml");
xmlAttachment.MimeType = "application/xml";
xmlAttachment.Description = "Source XML data";
xmlAttachment.Relationship = PdfAttachmentRelationship.Source;
pdfEditor.AddFileAttachment(xmlAttachment);
```

### Page-Level Annotations

```csharp
byte[] csvBytes = Encoding.UTF8.GetBytes(BuildSampleCsv());
var ann = PdfFileAttachmentAnnotation.FromBytes(
    csvBytes, "inventory.csv", pageNumber: 1, x: 30, y: 200);
ann.MimeType = "text/csv";
ann.Description = "Inline CSV inventory data";
ann.TooltipText = "Click to open inventory.csv";
ann.Icon = PdfAttachmentIcon.Paperclip;
// AddToAttachmentsPanel stays at the default false. Adobe Reader picks
// up the annotation and lists the file in the panel anyway. Setting true
// would produce a duplicate entry.
pdfEditor.AddFileAttachmentAnnotation(ann);
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-file-attachments-to-existing-pdf.htm

## Create PDF Documents with File Attachments

A PDF created from scratch with `PdfDocument` can carry arbitrary embedded files alongside its rendered content. The library exposes two complementary mechanisms.

### Document-Level Attachments

```csharp
byte[] xmlBytes = Encoding.UTF8.GetBytes(BuildSampleXml());
var xmlAttachment = PdfFileAttachment.FromBytes(xmlBytes, "data.xml");
xmlAttachment.MimeType = "application/xml";
xmlAttachment.Description = "Source XML data";
xmlAttachment.Relationship = PdfAttachmentRelationship.Source;
pdfDocument.AddFileAttachment(xmlAttachment);
```

```csharp
string alphabetFilePath = Path.Combine(GetDemoTextsPath(), "Alphabet.txt");
var textAttachment = PdfFileAttachment.FromFile(alphabetFilePath);
textAttachment.MimeType = "text/plain";
textAttachment.Description = "Sample alphabet text";
pdfDocument.AddFileAttachment(textAttachment);
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-documents-with-file-attachments.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
