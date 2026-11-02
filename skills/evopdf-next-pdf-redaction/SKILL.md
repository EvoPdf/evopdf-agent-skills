---
name: evopdf-next-pdf-redaction
description: "Permanently redact existing PDF documents in .NET with EvoPdf Next PdfRedactor: remove text found by search or regular expression, personal data such as email addresses, phone numbers, IBANs and payment card numbers, areas and whole pages, with fill color, overlay text or image, hidden content, metadata, scripts and attachments removal, redaction annotations for review and the redaction result."
---

# EvoPdf Next: PDF redaction

The `PdfRedactor` class permanently removes the text, images and graphics of some areas of an existing PDF document. The texts to remove can be searched in all the pages, the personal data like email addresses, phone numbers, bank accounts and payment card numbers is found by its structure and the redacted areas are covered with a fill color and an overlay text. The removed content cannot be recovered from the saved document, unlike the content hidden under a shape drawn over the page.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfRedactor`**: Removes text, images and graphics from areas of the pages of a PDF document. The content of an area is removed from the document, not only covered: te...
- **`PdfRedactionArea`**: An area of a page whose content is removed by PdfRedactor. The area is measured in points from the top left corner of the page as it is displayed
- **`PdfRedactionResult`**: What the redaction removed from the document, available from Result after the document is saved
- **`PdfPersonalDataTypes`**: The kinds of personal data found by FindPersonalData. The values can be combined
- **`PdfTextMatch`**: A text found in a page by FindText or FindPersonalData, with the areas it covers, one for each line, measured in points from the top left corner of th...

5 types, 70 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## How a redaction works

1. Load the document: `new PdfRedactor(pdfBytes)` or a file path, with the password of a protected document.
2. Mark what to remove: `RedactText` (a text or a regular expression), `RedactPersonalData`,
   `AddRedaction` (an area of a page, or the matches of `FindText`), `RedactPage` and `RedactPages`,
   `AddRedactionAnnotations` (the redaction annotations of a reviewed document).
3. Optionally choose the appearance: `FillColor` (black by default, null leaves the area empty),
   `OverlayText`, `OverlayTextColor`, `OverlayFontSize`, `RepeatOverlayText`, `SetOverlayImage`; a
   `PdfRedactionArea` can override them for one area.
4. Optionally clean the rest of the document: `RemoveHiddenContent`, `RemoveMetadata`, `RemoveJavaScript`,
   `RemoveEmbeddedFiles`; `RemoveIntersectingAnnotations` is on by default.
5. `Save` or `SaveToFile` applies the redactions; `Result` then reports what was removed.

`FindText` and `FindPersonalData` return `PdfTextMatch` objects without marking anything, so the matches can
be reviewed first. `SaveRedactionMarks` saves the marks as redaction annotations without removing content, for
a review in a PDF viewer; the reviewed document is redacted later with `AddRedactionAnnotations`. A redactor
saves once: create a new one for every document. A signed document loses the signatures that no longer match
the redacted content (`PdfRedactionResult.RemovedSignatures`).

## Redact Text and Personal Data in PDF

### Redact Texts

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

// Every occurrence of a name, as a whole word
pdfRedactor.RedactText("Jane Smith", false, true);

// The texts that match a regular expression, for example the customer numbers
pdfRedactor.RedactText(new Regex(@"C-\d{4}-\d{5}"));

byte[] redactedPdf = pdfRedactor.Save();
```

### Redact Personal Data

```csharp
// All the email addresses, phone numbers and bank accounts
pdfRedactor.RedactPersonalData(PdfPersonalDataTypes.EmailAddress | PdfPersonalDataTypes.PhoneNumber |
    PdfPersonalDataTypes.Iban);

// The payment card numbers found, reviewed one by one
foreach (PdfTextMatch match in pdfRedactor.FindPersonalData(PdfPersonalDataTypes.PaymentCardNumber))
{
    if (match.PageNumber == 1)
        pdfRedactor.AddRedaction(match);
}
```

### Redaction Marks for a Review

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);
pdfRedactor.RedactText("Jane Smith", false, true);
pdfRedactor.RedactPersonalData(PdfPersonalDataTypes.All);
// The document with the redaction marks, to review before the content is removed
byte[] markedPdf = pdfRedactor.SaveRedactionMarks();
```

All 4 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/redact-text-and-personal-data-in-pdf.htm

## Redact Areas and Pages of PDF

The `PdfRedactor` class permanently removes the content of some areas and pages of an existing PDF document. The text, images and graphics inside an area are removed, the content partially inside an area is cut at the area boundary, so the text characters and the image pixels outside the area are kept. The area is then covered with a fill color, an image and an overlay text.

### Redact Areas

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

// The payment details table of the first page
pdfRedactor.AddRedaction(1, new PdfRectangleF(36, 288, 525, 62));

// The signature, covered with an image that keeps its aspect ratio
PdfRedactionArea signatureArea = pdfRedactor.AddRedaction(1, new PdfRectangleF(50, 505, 200, 60));
signatureArea.SetOverlayImage(System.IO.File.ReadAllBytes("Redacted_Stamp.png"), true);

byte[] redactedPdf = pdfRedactor.Save();
```

### Redact Pages

```csharp
// The second page loses all its content and receives its own overlay text
PdfRedactionArea pageArea = pdfRedactor.RedactPage(2);
pageArea.OverlayText = "PAGE REMOVED";
pageArea.OverlayFontSize = 24;
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/redact-areas-and-pages-in-pdf.htm

## Apply Redaction Annotations in PDF

Redaction annotations mark the areas of a PDF document that must be redacted, during a review in a PDF viewer or with `PdfRedactor.SaveRedactionMarks`, without removing the content yet. The `PdfRedactor` class applies these annotations: the content of each marked area is permanently removed, the area is covered with the fill color and the overlay text of the annotation and the annotations are removed from the saved document.

### Apply Redaction Annotations

```csharp
using PdfRedactor pdfRedactor = new PdfRedactor(inputPdfBytes, password);

// The areas marked in a PDF viewer
int markedCount = pdfRedactor.AddRedactionAnnotations();

// Other redactions can be added to the same operation
pdfRedactor.RedactPersonalData(PdfPersonalDataTypes.All);

byte[] redactedPdf = pdfRedactor.Save();
```

### Create Redaction Marks

```csharp
// First step: the marks, saved for a review
using (PdfRedactor marker = new PdfRedactor(inputPdfBytes, password))
{
    marker.OverlayText = "REDACTED";
    marker.RedactPersonalData(PdfPersonalDataTypes.All);
    marker.SaveRedactionMarksToFile("Document_with_Redaction_Marks.pdf");
}

// Second step: the reviewed marks, applied
using (PdfRedactor pdfRedactor = new PdfRedactor("Document_with_Redaction_Marks.pdf"))
{
    pdfRedactor.AddRedactionAnnotations();
    pdfRedactor.SaveToFile("Redacted_Document.pdf");
}
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/apply-redaction-annotations-in-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
