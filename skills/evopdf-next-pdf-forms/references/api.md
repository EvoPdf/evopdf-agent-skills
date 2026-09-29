# PDF forms from HTML forms: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfFormOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormOptions.htm

Specifies options for converting HTML form controls into interactive PDF form fields These options control field naming, button action generation, submit behavior, validation mapping and diagnostic logging for the HTML-to-PDF forms pipeline

### Properties

- `ApplyRequiredAsPdfRequiredFlag`: Gets or sets a value indicating whether HTML required fields are also marked as required in the generated PDF fields
- `EnableButtonActions`: Gets or sets a value indicating whether PDF actions are attached to generated PDF buttons
- `ExcludeDisabledFieldsFromSubmit`: Gets or sets a value indicating whether disabled HTML fields are excluded from submit actions
- `FieldNamingMode`: Gets or sets how PDF field names are generated from HTML form controls
- `FormFieldBoldFont`: Gets or sets the default bold font used for interactive PDF form fields. If not set but FormFieldFont is set, that font will be used as a fallback. If FormFieldFont is not set either, a default bold font will be used For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
- `FormFieldBoldItalicFont`: Gets or sets the default bold italic font used for interactive PDF form fields. If not set but FormFieldFont is set, that font will be used as a fallback. If FormFieldFont is not set either, a default bold italic font will be used For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
- `FormFieldFont`: Gets or sets the default regular font used for interactive PDF form fields. If not set, Helvetica will be used For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
- `FormFieldItalicFont`: Gets or sets the default italic font used for interactive PDF form fields. If not set but FormFieldFont is set, that font will be used as a fallback. If FormFieldFont is not set either, a default italic font will be used For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
- `IncludeIFrameFields`: Gets or sets a value indicating whether form fields from iframes are converted to interactive PDF form fields The default value is true
- `IncludeReadOnlyFieldsInSubmit`: Gets or sets a value indicating whether read-only fields are included in submit actions
- `MapHtmlResetButtonsToPdfActions`: Gets or sets a value indicating whether HTML reset buttons are mapped to PDF reset actions
- `MapHtmlSubmitButtonsToPdfActions`: Gets or sets a value indicating whether HTML submit buttons are mapped to PDF submit actions
- `ResetOnlyFieldsFromButtonForm`: Gets or sets a value indicating whether reset buttons affect only fields that belong to the same HTML form
- `SubmitBehavior`: Gets or sets how submit actions are generated for HTML submit buttons
- `SubmitFormat`: Gets or sets the format used when generated submit buttons submit form data
- `SubmitOnlyFieldsFromButtonForm`: Gets or sets a value indicating whether submit buttons include only fields that belong to the same HTML form
- `ValidationBehavior`: Gets or sets how HTML validation metadata is mapped to PDF behavior

## PdfFieldNamingMode

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFieldNamingMode.htm

Specifies how PDF field names are generated from HTML form controls

### Fields and values

- `FullyQualified`: Generates fully qualified field names that are stable and unique across multiple forms
- `PreserveHtmlNames`: Preserves original HTML field names whenever possible, even if this increases the risk of collisions
- `PreserveHtmlNamesWhenSafe`: Preserves original HTML field names when they are safe to use, otherwise falls back to generated names

## PdfSubmitBehavior

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSubmitBehavior.htm

Specifies how submit actions are generated for PDF buttons

### Fields and values

- `AcrobatJavaScriptHtmlSubmit`: Generates an Acrobat JavaScript submit action intended to behave more like an HTML form submission This option is useful when browser-like GET or POST semantics are desired. It depends on support for Acrobat JavaScript in the target PDF viewer
- `NativePdfAction`: Generates a native PDF submit action This is the most broadly compatible option at the PDF action level. The exact HTTP behavior still depends on the PDF viewer and is not guaranteed to match browser form submission semantics exactly

## PdfSubmitFormat

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSubmitFormat.htm

Specifies the format used when a PDF form is submitted

### Fields and values

- `Fdf`: Submits the form data as FDF
- `Html`: Submits the form data in an HTML-compatible format
- `Pdf`: Submits the entire PDF document or a PDF-based payload
- `Xfdf`: Submits the form data as XFDF

## PdfValidationBehavior

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfValidationBehavior.htm

Specifies how HTML validation metadata is mapped to PDF form behavior

### Fields and values

- `AcrobatJavaScriptBasic`: Generates basic Acrobat JavaScript validation for supported constraints This mode can apply basic checks such as required fields, simple pattern checks and numeric min or max constraints. It depends on Acrobat JavaScript support in the viewer
- `None`: Does not generate any additional validation logic
