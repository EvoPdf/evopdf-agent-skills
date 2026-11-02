# Inspecting a PDF you received: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfPageInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPageInfo.htm

The information of a page of a loaded document: its size, its rotation and its boxes. The dimensions are in points and the boxes are relative to the top left corner of the media box of the page

### Properties

- `ArtBox`: The art box, the area of the meaningful content, or null when the page does not define it
- `BleedBox`: The bleed box, the area the content is clipped to when the page is printed, or null when the page does not define it
- `CropBox`: The crop box, the displayed area of the page, or null when the page does not define it
- `MediaBox`: The media box, the whole area of the page
- `PageNumber`: The 1-based number of the page
- `PageSize`: The size of the page, before its rotation
- `Rotation`: The rotation of the page: 0, 90, 180 or 270 degrees
- `TrimBox`: The trim box, the size of the finished page after trimming, or null when the page does not define it

## PdfSecurityInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSecurityInfo.htm

Represents security-related information extracted from a loaded PDF document. Includes encryption status, permission flags and encryption algorithm details

### Properties

- `CanAssembleDocument`: Indicates whether assembling the document (e.g., page manipulation) is allowed.
- `CanCopyAccessibilityContent`: Indicates whether screen readers can access the content
- `CanCopyContent`: Indicates whether copying content is allowed
- `CanEditAnnotations`: Indicates whether editing annotations is allowed
- `CanEditContent`: Indicates whether editing existing content is allowed
- `CanFillFormFields`: Indicates whether filling in form fields is allowed
- `CanPrint`: Indicates whether printing is allowed
- `CanPrintHighResolution`: Indicates whether printing at high resolution is allowed
- `EncryptionAlgorithm`: The encryption algorithm used (e.g., RC4, AES)
- `IsEncrypted`: Indicates whether the PDF document is encrypted
- `IsOwnerPasswordUsed`: Indicates whether the document was opened using the owner password (full permissions)
- `KeySize`: The key size used for encrypting the document (e.g., 40-bit, 128-bit, 256-bit)

## PdfSignatureInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSignatureInfo.htm

Represents digital signature information extracted from a loaded PDF document. Reports the presence and metadata of signatures, but does NOT validate cryptographic integrity, certificate trust chains, or timestamp validity. Use a dedicated validator for cryptographic verification

### Properties

- `CertificationLevel`: The modification permissions allowed by the certification signature, or null if the document is not certified. A document certified with NoChanges does not accept a new signature; with the other levels the new signature is saved as an incremental update that keeps the certification valid
- `ContainsTimestamp`: Indicates whether at least one signature in the document is a document timestamp signature
- `IsCertified`: Indicates whether the loaded PDF is certified. Certified documents may forbid further modifications; see CertificationLevel
- `IsSigned`: Indicates whether the loaded PDF contains at least one digital signature
- `SignatureCount`: The number of digital signatures present in the loaded PDF

## PdfCertificationLevel

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfCertificationLevel.htm

PDF certification levels

### Fields and values

- `FormFillingAndSigning`: Form filling and signing are permitted. Other modifications invalidate the signature
- `FormFillingSigningAndAnnotations`: Form filling, signing and annotations are permitted. Modifications outside these categories invalidate the signature
- `NoChanges`: No changes to the document are permitted. Any modification invalidates the signature
- `NotCertified`: An approval signature: the document is not certified and accepts further signatures and changes, each signature covering the revision it signed
