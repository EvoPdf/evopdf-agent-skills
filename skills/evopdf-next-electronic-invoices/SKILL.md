---
name: evopdf-next-electronic-invoices
description: "Create Factur-X, ZUGFeRD and XRechnung electronic invoices and Order-X electronic orders with EvoPdf Next: a PDF/A-3 document that shows the invoice from HTML and embeds the invoice XML, in all the profiles and document types (credit notes, corrected and self-billed invoices), accessible with PDF/UA-1, from HTML, from code or added to an existing PDF/A-3."
---

# EvoPdf Next: electronic invoices

EVO HTML to PDF Converter creates electronic invoices in the Factur-X and ZUGFeRD formats (including the XRechnung profile) and electronic orders in the Order-X format. An electronic invoice is a PDF/A-3 document that shows the invoice as a page a person reads and carries the same invoice as an XML file that accounting software reads automatically.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`ElectronicInvoice`**: A Factur-X / ZUGFeRD electronic invoice (or credit note, corrected invoice and the other invoice types) or an Order-X electronic order: the XML in the...
- **`ElectronicInvoiceProfile`**: The profile of a Factur-X / ZUGFeRD electronic invoice or of an Order-X electronic order. The profile is read from the guideline identifier of the XML...
- **`ElectronicDocumentType`**: The type of an electronic document, read from the document type code (UNTDID 1001) of the Factur-X / ZUGFeRD invoice XML or of the Order-X order XML

3 types, 32 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Invoice from HTML

```csharp
ElectronicInvoice invoice = ElectronicInvoice.FromFile("factur-x.xml");   // or FromXml(byte[]) / FromXml(string)

var converter = new HtmlToPdfConverter();
converter.PdfDocumentOptions.PdfStandard = PdfStandard.PdfA3b;           // PdfA3b, PdfA3u, PdfA3a or PdfUa1PdfA3a
converter.PdfDocumentOptions.AddElectronicInvoice(invoice);
byte[] pdf = converter.ConvertHtml(invoiceHtml, baseUrl);
```

- The profile, the document type and the number are read from the XML: `invoice.Profile`, `invoice.DocumentType`,
  `invoice.InvoiceNumber`, `invoice.IsOrder`. Nothing else has to be set; the XML is embedded as given.
- Profiles: `Minimum`, `BasicWL`, `Basic`, `EN16931` (ZUGFeRD COMFORT), `Extended`, `XRechnung`, and for orders
  `OrderXBasic`, `OrderXComfort`, `OrderXExtended`. UBL invoices and ZUGFeRD 1.0 are not supported.
- The document must be PDF/A-3. `PdfStandard.PdfUa1PdfA3a` makes the invoice page accessible as well, for invoices
  delivered to consumers; the HTML should then use headings, table header cells and alternate text for images.
- `Relationship` defaults to `PdfAttachmentRelationship.Alternative`; MINIMUM and BASIC WL are always attached as `Data`.
- One electronic invoice per document. Other attachments can be added with `PdfDocumentOptions.AddFileAttachment`.
- `PdfViewerPreferences.PageMode = ViewerPageMode.UseAttachments` opens the Attachments panel, so the reader sees the XML.
- The same `AddElectronicInvoice` method exists on `PdfDocument` (documents created from code) and on `PdfEditor`
  (an existing document that is already PDF/A-3).
- A digital signature can be added to the same converter; the document keeps its PDF/A-3 conformance.

## Create Factur-X, ZUGFeRD and XRechnung Electronic Invoices

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-electronic-invoice-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
