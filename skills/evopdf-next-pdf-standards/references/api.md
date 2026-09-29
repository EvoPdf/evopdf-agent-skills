# PDF/UA and PDF/A compliance: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfStandard

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfStandard.htm

Specifies the PDF standards compliance level for the generated PDF document

### Fields and values

- `None`: Plain PDF output without an accessibility structure tree or archival metadata
- `PdfA2a`: PDF/A-2a (ISO 19005-2, conformance level A) accessible long-term archival. Requires a tagged PDF structure tree, Unicode character mappings and natural-language metadata
- `PdfA2b`: PDF/A-2b (ISO 19005-2, conformance level B) basic long-term archival. Does not require a tagged PDF structure tree. Choose PdfUa1PdfA2b when both archival and accessibility are required
- `PdfA3a`: PDF/A-3a (ISO 19005-3, conformance level A) accessible long-term archival with support for embedded files of arbitrary type
- `PdfA3b`: PDF/A-3b (ISO 19005-3, conformance level B) basic long-term archival with support for embedded files of arbitrary type. Commonly used for e-invoicing standards such as ZUGFeRD and Factur-X
- `PdfA3u`: PDF/A-3u (ISO 19005-3, conformance level U) long-term archival with guaranteed Unicode character mappings and support for embedded files of arbitrary type
- `PdfA4`: PDF/A-4 (ISO 19005-4) base conformance, based on PDF 2.0. Does not require a tagged structure tree. Use PdfUa2PdfA4 when both archival and accessibility are required
- `PdfA4f`: PDF/A-4f (ISO 19005-4, Files subset) long-term archival based on PDF 2.0 with explicit support for embedded files of arbitrary type. Modern equivalent of PDF/A-3
- `PdfUa1`: PDF/UA-1 (ISO 14289-1) Universal Accessibility standard. Recommended for documents that must be accessible to screen readers and other assistive technologies
- `PdfUa1PdfA2a`: PDF/UA-1 (ISO 14289-1) combined with PDF/A-2a (ISO 19005-2 level A). The highest combined PDF/UA-1 + PDF/A-2 compliance level
- `PdfUa1PdfA2b`: PDF/UA-1 (ISO 14289-1) combined with PDF/A-2b (ISO 19005-2 level B). Satisfies both universal-accessibility and basic long-term archival requirements
- `PdfUa1PdfA3a`: PDF/UA-1 (ISO 14289-1) combined with PDF/A-3a (ISO 19005-3 level A). Universal accessibility plus long-term archival plus support for embedded files of arbitrary type
- `PdfUa2`: PDF/UA-2 (ISO 14289-2) second-generation Universal Accessibility standard, based on PDF 2.0
- `PdfUa2PdfA2a`: PDF/UA-2 (ISO 14289-2) combined with PDF/A-2a (ISO 19005-2 level A)
- `PdfUa2PdfA2b`: PDF/UA-2 (ISO 14289-2) combined with PDF/A-2b (ISO 19005-2 level B). Second-generation accessibility combined with basic long-term archival
- `PdfUa2PdfA4`: PDF/UA-2 (ISO 14289-2) combined with PDF/A-4 (ISO 19005-4). The recommended combination for PDF 2.0: second-generation accessibility with base long-term archival conformance
- `PdfUa2PdfA4f`: PDF/UA-2 (ISO 14289-2) combined with PDF/A-4f (ISO 19005-4 Files). Recommended modern combination for accessibility, long-term archival, and support for embedded files of arbitrary type

## PdfAccessibilityOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfAccessibilityOptions.htm

Controls the accessibility settings of the generated PDF document. These settings take effect only when a PDF/UA or PDF/A standard requiring a tagged structure tree is selected using the PdfStandard property

### Properties

- `AddMissingImageAlternateText`: Gets or sets a value indicating whether images referenced in the PDF structure that do not have alternate text receive a default value. The default value is true
- `InsertMissingTableHeaders`: Gets or sets a value indicating whether an artificial header row is inserted at the top of tables that do not contain header cells in the generated PDF document. The default value is true

## PdfAccessibilityProperties

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfAccessibilityProperties.htm

Accessibility metadata controlling how an element is inserted into the structure tree of a tagged PDF

### Properties

- `ActualText`: Replacement text (PDF /ActualText) used by screen readers in place of the rendered content
- `AlternateText`: Alternate description (PDF /Alt) read by screen readers when the content cannot be reproduced. Required for Figure in PDF/UA
- `CustomRole`: Custom non-standard structure role registered in the document /RoleMap. Takes precedence over StructureType when set
- `CustomRoleMappedTo`: Standard structure type that CustomRole maps to in /RoleMap
- `Expansion`: Expansion text for an abbreviation or acronym (PDF /E)
- `Language`: Natural language tag (PDF /Lang) overriding the document language. Values follow BCP 47, such as "en-US"
- `StructureType`: PDF structure role for this element. Null means use the element-specific default
- `Title`: Short title describing the element (PDF /T)

## PdfStructureType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfStructureType.htm

Identifies the PDF logical structure role attached to a content element when the document is tagged

### Fields and values

- `Article`: Generic article such as a self-contained body of text
- `Artifact`: Decorative content excluded from the structure tree
- `BlockQuote`: Block-level quotation
- `Caption`: Caption accompanying a Figure, Formula or Table
- `Code`: Fragment of computer program text
- `Division`: Generic block-level grouping with no specific role
- `Figure`: Illustrative item such as a graphic or photograph. Default for PdfImageElement
- `Formula`: Mathematical formula
- `Heading1`: First-level heading
- `Heading2`: Second-level heading
- `Heading3`: Third-level heading
- `Heading4`: Fourth-level heading
- `Heading5`: Fifth-level heading
- `Heading6`: Sixth-level heading
- `NonStructural`: Non-structural grouping element whose descendants are treated by assistive technology as if attached to its parent
- `Note`: A footnote or endnote
- `Paragraph`: Paragraph. Default for PdfTextElement
- `Quote`: Inline portion attributed to someone other than the author
- `Reference`: Reference to other content, such as a footnote callout
- `Section`: Generic section grouping related elements
- `Span`: Generic inline portion of text
