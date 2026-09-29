# Inspecting a PDF you received: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfPageInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPageInfo.htm

Represents page-specific information extracted from a loaded PDF document. Includes details like page size, rotation, crop box, media box, and other boundaries. All dimensions are expressed in points (1 point = 1/72 inch)

### Properties

- `ArtBox`: Gets the art box, which defines the meaningful content area.
- `BleedBox`: Gets the bleed box, indicating the region to which the contents should be clipped when printed.
- `CropBox`: Gets the crop box, which defines the visible area of the page.
- `MediaBox`: Gets the media box, which defines the physical dimensions of the page.
- `PageNumber`: Gets the page number (1-based).
- `PageSize`: Gets the page size
- `Rotation`: Gets the rotation angle of the page (0, 90, 180, 270).
- `TrimBox`: Gets the trim box, representing the intended dimensions of the finished page after trimming.

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
- `EncryptionAlgorithm`: The encryption algorithm used (e.g., RC4, AES)
- `IsEncrypted`: Indicates whether the PDF document is encrypted
- `IsOwnerPasswordUsed`: Indicates whether the document was opened using the owner password (full permissions)
- `KeySize`: The key size used for encrypting the document (e.g., 40-bit, 128-bit, 256-bit)

## PdfSignatureInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSignatureInfo.htm

Represents digital signature information extracted from a loaded PDF document. Reports the presence and metadata of signatures, but does NOT validate cryptographic integrity, certificate trust chains, or timestamp validity. Use a dedicated validator for cryptographic verification

### Properties

- `CertificationLevel`: The modification permissions allowed by the certification signature, or null if the document is not certified. IMPORTANT: editing a certified document with a level of NoChanges or FormFillingAndSigning may invalidate the certification signature
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
