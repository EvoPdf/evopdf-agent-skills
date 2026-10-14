---
name: evopdf-next-security-signatures
description: "Protect and sign PDF documents with EvoPdf Next: passwords, encryption and permissions; PAdES digital signatures at the B-B, B-T, B-LT and B-LTA levels with embedded revocation data, signed with a PFX file, a certificate from the Windows store, a smartcard or USB token, or a hardware security module or cloud key service; signing existing PDFs, long-term validation, signature fields for several signers, certification levels and signature appearance."
---

# EvoPdf Next: encryption, permissions and digital signatures

EVO HTML to PDF Converter allows you to set the permissions of the generated PDF document like printing and editing and to password protect the generated PDF document with separate user and owner passwords.

The digital signatures are PAdES signatures. The same `PdfDigitalSignature` object signs a PDF generated from HTML
(`HtmlToPdfConverter.DigitalSignature`), a PDF created from code (`PdfDocument.DigitalSignature`) and an existing PDF
(the `PdfEditor` constructor), with the same signature levels, signing keys and appearance options.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfSecurityOptions`**: This class encapsulates the options to control the PDF document security like permissions, encryption, password protection
- **`EncryptionAlgorithm`**: This enumeration contains the possible values of the encryption algorithm used to encrypt a PDF document
- **`EncryptionKeySize`**: This enumeration contains the possible values of the length of the encryption key used to encrypt a PDF document
- **`PdfDigitalSignature`**: This class encapsulates the options used to control the digital signature applied to a PDF document
- **`PdfDigitalSignatureAppearance`**: This class encapsulates the options used to control the appearance of the digital signature applied to a PDF document
- **`PdfDigitalSignatureProfile`**: The PAdES baseline profile applied when digitally signing a PDF document, as defined by ETSI EN 319 142-1. Higher profiles embed progressively more ma...
- **`PdfSignatureDigestAlgorithm`**: The digest algorithm of a digital signature
- **`IExternalDigitalSignature`**: Implement this interface to produce the signature value with a private key that never enters the application process: a hardware security module acces...
- **`PdfLongTermValidationOptions`**: The options used by AddLongTermValidation to add long-term validation data to the signatures of a PDF document. When TimestampServerUrl is set, a docu...
- **`PdfSignatureField`**: An empty signature field placed in a page of the document, to be signed later by a digital signature with the same field name. A form with a field for...

10 types, 65 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

When `EncryptionAlgorithm` and `KeySize` are not set, PDF 1.x documents are encrypted with RC4 and a 128 bit key and
PDF 2.0 documents (PDF/UA-2, PDF/A-4, WTPDF) with AES and a 256 bit key.

## Signature levels and signing keys

| `PdfDigitalSignature.Profile` | Result | Needs |
|---|---|---|
| `Auto` (default) | B-T when `TimestampServerUrl` is set, otherwise B-B | |
| `PadesBaselineB` | integrity and authenticity, signing time declared by the signer | |
| `PadesBaselineT` | adds a trusted timestamp | `TimestampServerUrl` |
| `PadesBaselineLT` | adds the certificate chain and its revocation information (OCSP or CRL), verifiable offline after the certificates expire | `TimestampServerUrl`, network access, a certificate whose issuer publishes revocation information |
| `PadesBaselineLTA` | adds a document timestamp over the validation data, for archiving | same as LT |

LT and LTA cannot be combined with document encryption. `DigestAlgorithm` selects SHA-256 (default), SHA-384 or SHA-512.
`TrustedCaCertificates` completes a chain that does not come with the signing certificate.

```csharp
// PFX file
var signature = new PdfDigitalSignature("signing.pfx", pfxPassword);
// X509Certificate2: Windows certificate store, smartcard, USB token (RSA or ECDSA); the key stays in its store
var signature2 = new PdfDigitalSignature(certificate);
// Hardware security module or cloud key service (Azure Key Vault, AWS KMS): implement IExternalDigitalSignature
var signature3 = new PdfDigitalSignature(new MyKeyVaultSignature(), new[] { signerCertificate.RawData });

signature.Profile = PdfDigitalSignatureProfile.PadesBaselineLT;
signature.TimestampServerUrl = timestampServerUrl;
signature.Reason = "Approved";

var converter = new HtmlToPdfConverter();
converter.DigitalSignature = signature;
byte[] pdf = converter.ConvertUrl(url);
```

`IExternalDigitalSignature` has two members: `GetSignatureAlgorithmName()` returns "RSA", "RSASSA-PSS", "DSA" or
"ECDSA", and `Sign(byte[] message)` returns the signature value of the DER encoded signed attributes, computed with the
configured digest. On Windows servers without a user profile (IIS application pools, Azure App Service), load a PFX
into an `X509Certificate2` with `X509KeyStorageFlags.MachineKeySet`; the PFX file constructor needs no such flag.

## Existing PDFs, several signers, long-term validation

```csharp
// Sign an existing PDF; signatures already in the document stay valid
using (var editor = new PdfEditor(pdfBytes, password, signature))
    signedPdf = editor.Save();

// Prepare a form with an empty field for each signer, then sign the fields one by one by name
using (var editor = new PdfEditor(pdfBytes))
{
    editor.AddSignatureField(new PdfSignatureField("Manager", pageNumber, bounds));   // bounds: PdfRectangleF
    editor.AddSignatureField(new PdfSignatureField("Client", 1, 320, 700, 200, 50));
    prepared = editor.Save();
}
signature.FieldName = "Manager";   // placed in the field of that name, on its page and in its rectangle

// Add validation data (timestamped signatures reach B-LT) and, with TimestampServerUrl, a document timestamp (B-LTA)
using (var editor = new PdfEditor(signedPdf))
{
    editor.AddLongTermValidation(new PdfLongTermValidationOptions { TimestampServerUrl = timestampServerUrl });
    archivedPdf = editor.Save();
}
```

A signed document accepts only new signatures and long-term validation data: adding content, pages, annotations,
fields or security options is refused because it would invalidate the signatures, so add the fields before the first
signature. A certification signature (`CertificationLevel` other than `NotCertified`) must be the first signature; a
document certified with `NoChanges` accepts no further signatures. Signing keeps the PDF/A and PDF/UA conformance of the
document. `PdfEditor.PdfSignatureInfo` reports the signatures already present. Adding long-term validation data needs
network access when the document is saved; encrypted documents do not accept it.

## Set Permissions and Password of the Generated PDF Document

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/set-pdf-permissions-and-password.htm

## Add a Digital Signature to Generated PDF Document

EVO HTML to PDF Converter allows you to add digital signatures to the generated PDF document. In order to add digital signatures you need a certificate with private and public keys. These certificates are usually stored in a .pfx or a .p12 file in PKCS#12 format and they can be password protected. A digital signature is represented by a `PdfDigitalSignature` object. An object of this type is exposed by the `HtmlToPdfConverter.DigitalSignature` property of the HTML to PDF Converter class.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-the-generated-pdf.htm

## Digitally Sign Existing PDF

The `PdfEditor` class adds a digital signature to an existing PDF document. The signature is given to the `PdfEditor.PdfEditor(Byte[], String, PdfDigitalSignature)` or `PdfEditor.PdfEditor(String, String, PdfDigitalSignature)` constructor as a `PdfDigitalSignature` object, the same object used by the HTML to PDF converter and the PDF creator, with the same signature levels, signing keys and appearance options described in the Digital Signatures topic. The signature is applied when the document is saved with `PdfEditor.Save()`. The signed document keeps the PDF/A and PDF/UA conformance of the source document.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-existing-pdf.htm

## Add Signature Fields from HTML Elements

A document that several people sign needs an empty signature field for each signer, placed where the layout shows the signature. EVO HTML to PDF Converter and the PDF editor prepare such a document from a HTML page: the page marks the place of each signature with an element, the converter returns the page and the rectangle where the element was rendered and `PdfEditor.AddSignatureField(PdfSignatureField)` adds an empty signature field there. Each signer then fills in the field prepared for them with a digital signature that has the same field name, as described in the Digitally Sign Existing PDF topic, and the signatures already in the document stay valid.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-signature-fields-from-html-elements.htm

- Set Permissions and Password of the Generated PDF Document: https://www.evopdf.com/help/evopdf-next-dotnet/html/set-pdf-permissions-and-password.htm
- Add a Digital Signature to Generated PDF Document: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-the-generated-pdf.htm
- Digitally Sign Existing PDF: https://www.evopdf.com/help/evopdf-next-dotnet/html/digitally-sign-existing-pdf.htm
- Add Signature Fields from HTML Elements: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-signature-fields-from-html-elements.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
