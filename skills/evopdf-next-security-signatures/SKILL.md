---
name: evopdf-next-security-signatures
description: "Protect PDFs generated or edited with EvoPdf Next: user and owner passwords, encryption algorithm and key size, permissions (print, copy, edit, fill forms), digital signatures with PFX certificates and timestamp servers, document metadata (title, author, keywords, language). Use when a .NET PDF must be encrypted, permission-restricted, signed or tagged with metadata."
---

# EvoPdf Next — security, digital signatures, metadata

Available on every converter (`converter.PdfSecurityOptions`, `converter.DigitalSignature`, `converter.PdfDocumentInfo`) and on `PdfDocument` / `PdfEditor` for created or existing PDFs.

## Passwords and permissions
```csharp
var sec = converter.PdfSecurityOptions;
sec.UserPassword = "open-me";          // required to open
sec.OwnerPassword = "owner-secret";    // required to change permissions
sec.EncryptionAlgorithm = …;           // RC4 or AES; prefer AES
sec.KeySize = …;                       // e.g. 128 or 256 bits
sec.CanPrint = true;
sec.CanCopyContent = false;
sec.CanEditContent = false;
sec.CanEditAnnotations = false;
sec.CanFillFormFields = true;
sec.CanAssembleDocument = false;
sec.CanCopyAccessibilityContent = true; // keep screen readers working
```
Permissions are only enforced by viewers when an owner password is set. A user password alone encrypts the file; an owner password alone restricts without asking on open.

## Digital signature
```csharp
var sig = converter.DigitalSignature;
sig.PfxCertificateData = File.ReadAllBytes("company.pfx");
sig.PfxCertificatePassword = Environment.GetEnvironmentVariable("PFX_PASSWORD");
sig.Reason = "Approved"; sig.Location = "Bucharest"; sig.ContactInfo = "office@example.com";
sig.TimestampServerUrl = "http://timestamp.digicert.com";   // optional RFC 3161 TSA (+ Username/Password if required)
sig.AppearanceEnabled = true;                                 // visible signature; configure sig.Appearance
sig.FieldName = "Signature1";
```
To sign an **existing** PDF, open it with `new PdfEditor(pdfBytes, password, digitalSignature)` and save. Keep certificates and passwords out of source code.

## Metadata
```csharp
var info = converter.PdfDocumentInfo;
info.Title = "Invoice 2026-001"; info.AuthorName = "ACME"; info.Subject = "…"; info.Keywords = "invoice, 2026";
info.Language = "en-US";   // also used by PDF/UA readers
```
`Creator`, `Producer`, `CreatedDate`, `ModificationDate` are available too.

## Rules
- Encryption and PDF/A: PDF/A forbids encryption — do not combine `PdfSecurityOptions` passwords with `PdfStandard.PdfA*`.
- Signing after any other modification: the signature covers the final bytes; edit first, sign last.
- Read secrets (PFX password, owner password) from configuration or a key vault, never from literals.

Docs: https://www.evopdf.com/help/evopdf-next-dotnet/
