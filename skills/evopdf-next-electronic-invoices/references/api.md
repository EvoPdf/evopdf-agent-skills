# Electronic invoices: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## ElectronicInvoice

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ElectronicInvoice.htm

A Factur-X / ZUGFeRD electronic invoice (or credit note, corrected invoice and the other invoice types) or an Order-X electronic order: the XML in the UN/CEFACT Cross Industry syntax that is embedded in a PDF/A-3 document together with the identification metadata the receiving software looks for. Create an instance with FromXml, FromXml or FromFile, then pass it to AddElectronicInvoice on the HTML to PDF document options, on the PDF document or on the PDF editor. The profile, the document number and the document type are read from the XML. For an invoice that is also an accessible PDF/UA-1 document, generate it with PdfUa1PdfA3a

### Properties

- `Description`: The description shown next to the file name in the Attachments panel of PDF viewers. The default names the standard and the type of the document, followed by its number, for example "Factur-X/ZUGFeRD credit note GS-2026-0007" or "Order-X order PO-2026-0101"
- `DocumentType`: The type of the document, read from its document type code: an invoice, a credit note, a corrected invoice, an order and the other types of ElectronicDocumentType
- `DocumentTypeCode`: The document type code of the document (UNTDID 1001), for example 380 for a commercial invoice, 381 for a credit note or 220 for an order, or an empty string when the XML does not contain one
- `FileName`: The name of the embedded XML file prescribed by the standard: factur-x.xml, xrechnung.xml for the XRECHNUNG profile, or order-x.xml for an Order-X document
- `InvoiceNumber`: The invoice number, or the order number for an Order-X document (the ID of the exchanged document), or an empty string when the XML does not contain one
- `IsOrder`: True for an Order-X order, order change or order response, false for a Factur-X / ZUGFeRD invoice
- `Profile`: The profile of the document, read from the guideline identifier in the XML
- `Relationship`: The relationship between the embedded XML and the visible document, written as AFRelationship. The default is Alternative: the XML and the page carry the same document. Source and Data can also be used. For the MINIMUM and BASIC WL profiles the XML does not carry the complete invoice and the relationship written is always Data, as the standard requires

### Methods

- `FromFile`: Creates an electronic invoice or order from an XML file
- `FromXml`: Creates an electronic invoice or order from the XML data
- `FromXml`: Creates an electronic invoice or order from the XML text. The XML is embedded encoded as UTF-8

## ElectronicInvoiceProfile

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ElectronicInvoiceProfile.htm

The profile of a Factur-X / ZUGFeRD electronic invoice or of an Order-X electronic order. The profile is read from the guideline identifier of the XML and defines how much of the document the XML carries

### Fields and values

- `Basic`: BASIC profile: invoice lines with the core elements, compliant with EN 16931
- `BasicWL`: BASIC WL profile: header data without invoice lines. The XML does not carry the complete invoice, so it is attached with the Data relationship
- `EN16931`: EN 16931 profile (named COMFORT in ZUGFeRD): the full European core invoice
- `Extended`: EXTENDED profile: EN 16931 with additional elements
- `Minimum`: MINIMUM profile: header data only (seller, buyer, totals). The XML does not carry the complete invoice, so it is attached with the Data relationship
- `OrderXBasic`: Order-X BASIC profile: the core elements of an order, embedded as order-x.xml
- `OrderXComfort`: Order-X COMFORT profile: the elements of an order most business processes need, embedded as order-x.xml
- `OrderXExtended`: Order-X EXTENDED profile: the complete order data model, embedded as order-x.xml
- `XRechnung`: XRECHNUNG profile: the invoice format of the German public sector, embedded as xrechnung.xml

## ElectronicDocumentType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_ElectronicDocumentType.htm

The type of an electronic document, read from the document type code (UNTDID 1001) of the Factur-X / ZUGFeRD invoice XML or of the Order-X order XML

### Fields and values

- `CorrectedInvoice`: Corrected invoice (code 384)
- `CreditNote`: Credit note (code 381)
- `Invoice`: Commercial invoice (code 380)
- `InvoiceInformation`: Invoice information for accounting purposes (code 751)
- `Order`: Order (code 220), an Order-X document
- `OrderChange`: Order change (code 230), an Order-X document
- `OrderResponse`: Order response (code 231), an Order-X document
- `Other`: Another document type code
- `PartialInvoice`: Partial invoice (code 326)
- `PrepaymentInvoice`: Prepayment invoice (code 386)
- `SelfBilledCreditNote`: Self-billed credit note (code 261)
- `SelfBilledInvoice`: Self-billed invoice (code 389)
