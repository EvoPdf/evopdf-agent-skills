---
name: evopdf-next-pdf-inspect
description: "Read the properties of an existing PDF with EvoPdf Next before processing it: page count and page boxes, PDF version, tagged, signed, linearized, contains form, JavaScript or embedded files, encryption and permission flags, signature metadata, document information and viewer preferences read from the file, bookmarks, open action and form fields."
---

# EvoPdf Next: inspecting a PDF you received

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfPageInfo`**: The information of a page of a loaded document: its size, its rotation and its boxes. The dimensions are in points and the boxes are relative to the t...
- **`PdfSecurityInfo`**: Represents security-related information extracted from a loaded PDF document. Includes encryption status, permission flags and encryption algorithm de...
- **`PdfSignatureInfo`**: Represents digital signature information extracted from a loaded PDF document. Reports the presence and metadata of signatures, but does NOT validate ...
- **`PdfCertificationLevel`**: PDF certification levels

4 types, 29 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Reading a PDF before you process it

`PdfEditor` opens an existing document and answers what it is before anything is changed.
Nothing here modifies the file; do not call `Save` unless you meant to.

```csharp
using var editor = new PdfEditor(pdfBytes, null);   // the owner password of a protected PDF, or null

int pages = editor.PageCount;
string version = editor.PdfVersion;

bool tagged = editor.IsTaggedPdf;
bool signed = editor.IsSigned;
bool linearized = editor.IsLinearized;
bool hasForm = editor.ContainsForm;
bool hasScript = editor.ContainsJavaScript;
bool hasAttachments = editor.ContainsEmbeddedFiles;
```

## Page geometry

```csharp
for (int i = 1; i <= editor.PageCount; i++)
{
    PdfPageInfo page = editor.GetPdfPageInfo(i);
    // page.PageSize, page.Rotation, page.MediaBox, page.CropBox,
    // page.BleedBox, page.TrimBox, page.ArtBox
}
```

All boxes are in points. `Rotation` is 0, 90, 180 or 270 and applies on top of the box, so a
page can be A4 portrait in `MediaBox` and land as landscape in a viewer.

## Encryption and permissions

```csharp
PdfSecurityInfo security = editor.SecurityInfo;
if (security.IsEncrypted)
{
    // security.IsOwnerPasswordUsed tells you whether you opened it with full permissions
    // security.EncryptionAlgorithm, security.KeySize
    // security.CanPrint, security.CanEditContent, security.CanCopyContent,
    // security.CanEditAnnotations, security.CanFillFormFields,
    // security.CanAssembleDocument, security.CanCopyAccessibilityContent
}
```

## Signatures

```csharp
PdfSignatureInfo signature = editor.SignatureInfo;
// signature.IsSigned, signature.SignatureCount, signature.IsCertified,
// signature.CertificationLevel, signature.ContainsTimestamp
```

`PdfSignatureInfo` reports the presence and the metadata of the signatures. It does not verify
cryptographic integrity, certificate trust chains or revocation. Do not present its result to a
user as "the signature is valid".

## Metadata and viewer preferences of a loaded document

The properties read from the loaded document and the ones it is saved with are separate:

| Read from the loaded document | Written when the document is saved |
|---|---|
| `editor.DocumentInfo` (`Title`, `AuthorName`, `Subject`, `Keywords`, `CreatedDate`) | `editor.PdfDocumentInfo`; the values read are kept when they are not set |
| `editor.ViewerPreferencesInfo` | `editor.PdfViewerPreferences` |
| `editor.SecurityInfo` | `editor.PdfSecurityOptions` |
| `editor.SignatureInfo` | `editor.DigitalSignature` |

The bookmarks (`editor.Bookmarks`), the open action (`editor.OpenAction`) and the form fields (`editor.Form`) of
the loaded document can be read and changed: see `evopdf-next-bookmarks-and-toc` and `evopdf-next-pdf-forms`.

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.
