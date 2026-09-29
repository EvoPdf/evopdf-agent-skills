---
name: evopdf-next-pdf-forms
description: "Turn HTML form controls into interactive PDF form fields with EvoPdf Next PdfFormOptions: field naming, fonts for field text, required and read-only mapping, submit and reset button actions, submit format and scope, validation behaviour, iframe fields."
---

# EvoPdf Next: PDF forms from HTML forms

EVO HTML to PDF Converter can be configured to automatically convert HTML form controls to interactive PDF form fields by setting the `PdfDocumentOptions.GeneratePdfFormFields` property to true. An object of type `PdfDocumentOptions` is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfFormOptions`**: Specifies options for converting HTML form controls into interactive PDF form fields These options control field naming, button action generation, sub...
- **`PdfFieldNamingMode`**: Specifies how PDF field names are generated from HTML form controls
- **`PdfSubmitBehavior`**: Specifies how submit actions are generated for PDF buttons
- **`PdfSubmitFormat`**: Specifies the format used when a PDF form is submitted
- **`PdfValidationBehavior`**: Specifies how HTML validation metadata is mapped to PDF form behavior

5 types, 28 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Create PDF Forms from HTML Forms

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-forms-from-html-forms.htm

- Create PDF Forms from HTML Forms: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-forms-from-html-forms.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
