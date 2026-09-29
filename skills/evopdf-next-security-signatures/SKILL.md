---
name: evopdf-next-security-signatures
description: "Protect and sign PDF documents with EvoPdf Next: user and owner passwords, encryption algorithm and key size, permission flags, digital signatures from a PFX certificate, timestamp servers, signature appearance and certification levels."
---

# EvoPdf Next: encryption, permissions and digital signatures

EVO HTML to PDF Converter allows you set the permissions of generated PDF document like printing and editing and to password protect the generated PDF document with separate user and owner passwords.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfSecurityOptions`**: This class encapsulates the options to control the PDF document security like permissions, encryption, password protection
- **`EncryptionAlgorithm`**: This enumeration contains the possible values of the encryption algorithm used to encrypt a PDF document
- **`EncryptionKeySize`**: This enumeration contains the possible values of the length of the encryption key used to encrypt a PDF document
- **`PdfDigitalSignature`**: This class encapsulates the options used to control the digital signature applied to a PDF document
- **`PdfDigitalSignatureAppearance`**: This class encapsulates the options used to control the appearance of the digital signature applied to a PDF document

5 types, 35 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Set Permissions and Password of the Generated PDF Document

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/set-pdf-permissions-and-password.htm

## Add a Digital Signature to Generated PDF Document

EVO HTML to PDF Converter allows you to add digital signatures to the generated PDF document. In order to add digital signatures you need a certificate with private and public keys. These certificates are usually stored in a .pfx or a .p12 file in PKCS#12 format and they can be password protected. A digital signature is represented by a `PdfDigitalSignature` object. An object of this type is exposed by the `HtmlToPdfConverter.DigitalSignature` property of the HTML to PDF Converter class.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-the-generated-pdf.htm

- Set Permissions and Password of the Generated PDF Document: https://www.evopdf.com/help/evopdf-next-dotnet/html/set-pdf-permissions-and-password.htm
- Add a Digital Signature to Generated PDF Document: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-the-generated-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
