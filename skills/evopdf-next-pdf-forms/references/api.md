# PDF forms from HTML forms: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfFormOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormOptions.htm

Specifies options for converting HTML form controls into interactive PDF form fields. These options control field naming, button action generation, submit behavior, validation mapping and diagnostic logging for the HTML-to-PDF forms pipeline

### Properties

- `ApplyRequiredAsPdfRequiredFlag`: Gets or sets a value indicating whether HTML required fields are also marked as required in the generated PDF fields
- `EnableButtonActions`: Gets or sets a value indicating whether PDF actions are attached to generated PDF buttons
- `ExcludeDisabledFieldsFromSubmit`: Gets or sets a value indicating whether disabled HTML fields are excluded from submit actions
- `ExcludedFieldsSelector`: Gets or sets the CSS selector of the HTML form controls that are not converted to PDF form fields. The matching controls are rendered as they appear in the page. If null or empty, all the form controls are converted
- `FieldNamingMode`: Gets or sets how PDF field names are generated from HTML form controls
- `FormFieldBoldFont`: Gets or sets the default bold font used for interactive PDF form fields. If not set but FormFieldFont is set, that font will be used as a fallback. If FormFieldFont is not set either, a default bold font will be used. For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
- `FormFieldBoldItalicFont`: Gets or sets the default bold italic font used for interactive PDF form fields. If not set but FormFieldFont is set, that font will be used as a fallback. If FormFieldFont is not set either, a default bold italic font will be used. For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
- `FormFieldFont`: Gets or sets the default regular font used for interactive PDF form fields. If not set, Helvetica will be used. For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
- `FormFieldItalicFont`: Gets or sets the default italic font used for interactive PDF form fields. If not set but FormFieldFont is set, that font will be used as a fallback. If FormFieldFont is not set either, a default italic font will be used. For best compatibility with Adobe Reader interactive form fields, use open-source fonts such as Liberation Sans, Roboto or DejaVu. All modern browsers (Chrome, Edge) render PDF form fields correctly regardless of the font used
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

- `AcrobatJavaScriptHtmlSubmit`: Generates an Acrobat JavaScript submit action intended to behave more like an HTML form submission. This option is useful when browser-like GET or POST semantics are desired. It depends on support for Acrobat JavaScript in the target PDF viewer
- `NativePdfAction`: Generates a native PDF submit action. This is the most broadly compatible option at the PDF action level. The exact HTTP behavior still depends on the PDF viewer and is not guaranteed to match browser form submission semantics exactly

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

- `AcrobatJavaScriptBasic`: Generates basic Acrobat JavaScript validation for supported constraints. This mode can apply basic checks such as required fields, simple pattern checks and numeric min or max constraints. It depends on Acrobat JavaScript support in the viewer
- `None`: Does not generate any additional validation logic

## PdfForm

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfForm.htm

The interactive form of a loaded document. It reads the fields of the document, fills them with values, changes their appearance and flattens, removes or renames them. The changes are written when the document is saved

### Properties

- `FieldNames`: The names of the fields of the form, in the order they are stored in the document
- `Fields`: The fields of the form, in the order they are stored in the document
- `GenerateAppearances`: A flag indicating if the appearance of the fields is updated when their value changes, so the new value is displayed by any PDF viewer. The default value is true

### Methods

- `AddSubstitutionFont`: Adds a font used to display the characters missing from the font of a field, when a value is set in a script or language the field font does not cover
- `ContainsField`: Checks if the form has a field with the given name
- `ExportFdf`: Exports the current values of the fields as FDF data
- `FlattenAllFields`: Flattens all the fields when the document is saved: their current appearance becomes static page content and the form is removed from the document
- `FlattenField`: Flattens a field when the document is saved: its current appearance becomes static page content and the field is removed from the form
- `GetField`: Gets the description of a field
- `GetValue`: Gets the current value of a field: the text of a text field, the export value of the selected option, check box or radio button, or "Off" for a check box or radio button group without a selection
- `GetValues`: Gets the export values of all the selected options of a list box
- `ImportData`: Fills the fields with the values of FDF or XFDF data
- `ImportData`: Fills the fields with the values of a FDF or XFDF file
- `RemoveField`: Removes a field from the form, together with its appearance in the pages
- `RenameField`: Renames a field
- `SetBackgroundColor`: Sets the background color of a field
- `SetBorderColor`: Sets the border color of a field
- `SetChecked`: Selects or clears a check box
- `SetFont`: Sets the font, size and color of the text displayed in a field
- `SetFontSize`: Sets the size of the text displayed in a field, or 0 to size the text automatically to fit the field
- `SetOptions`: Replaces the options of a combo box or list box
- `SetReadOnly`: Makes a field read-only or editable
- `SetRequired`: Marks a field as required or optional when the form is submitted
- `SetTextColor`: Sets the color of the text displayed in a field
- `SetValue`: Sets the value of a field: the text of a text field, the export value of the option to select in a combo box or list box, the export value of the check box or radio button to select, or "Off" to clear a check box or radio button group
- `SetValues`: Selects several options of a list box that allows several selections
- `SetVisibility`: Sets the visibility of a field on screen and in print

## PdfFormField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormField.htm

The base class of the interactive form fields created from code: text fields, check boxes, radio button groups, combo boxes, list boxes and push buttons. The field name must be unique in the document. A dot in the name creates a hierarchy, for example "address.city"

### Properties

- `Actions`: The actions performed by the PDF viewer on the events of the field
- `Description`: The description of the field shown by PDF viewers as a tooltip and read by assistive technology. The field name is used when it is not set
- `Exportable`: A flag indicating if the value of the field is included when the form is submitted. The default value is true
- `MappingName`: The name used for the field when the form data is exported. The field name is used when it is not set
- `Name`: The name of the field, unique in the document
- `ReadOnly`: A flag indicating if the user cannot change the value of the field. The default value is false
- `Required`: A flag indicating if the field must have a value when the form is submitted. The default value is false

## PdfWidgetFormField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfWidgetFormField.htm

The base class of the form fields displayed in one rectangle of a page. The position is given in points from the top left corner of the content area of the page, inside the margins, in a document being created, and from the top left corner of the page in an edited document

### Properties

- `BackgroundColor`: The background color of the field, or null for a transparent background. The default value is null
- `BorderColor`: The color of the field border, or null for no border. The default value is null
- `BorderStyle`: The style of the field border. The default value is Solid
- `BorderWidth`: The width of the field border, in points. The default value is 1
- `Height`: The height of the field, in points
- `PageNumber`: The 1-based number of the page where the field is placed
- `Visibility`: The visibility of the field on screen and in print. The default value is Visible
- `Width`: The width of the field, in points
- `X`: The left position of the field, in points, from the left margin of a document being created or from the left edge of the page of an edited document
- `Y`: The top position of the field, in points, from the top margin of a document being created or from the top edge of the page of an edited document

## PdfTextField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfTextField.htm

A text field of an interactive form, where the user types a single line or several lines of text

### Properties

- `Alignment`: The horizontal alignment of the text in the field. The default value is Left
- `AutoFontSize`: A flag indicating if the font size is chosen automatically to fit the text in the field, ignoring the size of Font. The default value is false
- `Comb`: A flag indicating if the field is divided into equal cells, one for each character, which requires a MaxLength. The default value is false
- `DefaultText`: The text restored in the field when the form is reset. The Text is used when it is not set
- `DoNotScroll`: A flag indicating if the text cannot scroll beyond the field bounds, so the user cannot type more than the field can display. The default value is false
- `DoNotSpellCheck`: A flag indicating if the PDF viewer does not spell check the text. The default value is false
- `FileSelect`: A flag indicating if the text is the path of a file whose contents are submitted with the form. The default value is false
- `Font`: The font of the text. A standard Helvetica font of 12 points is used when it is not set
- `MaxLength`: The maximum number of characters the user can type, or 0 for no limit. The default value is 0
- `Multiline`: A flag indicating if the field accepts several lines of text. The default value is false
- `Password`: A flag indicating if the typed characters are displayed as asterisks. The default value is false
- `Text`: The text displayed in the field. The default value is an empty text

## PdfCheckBoxField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfCheckBoxField.htm

A check box of an interactive form

### Properties

- `Checked`: A flag indicating if the check box is selected. The default value is false
- `DefaultChecked`: The state restored when the form is reset. The Checked state is used when it is not set
- `ExportValue`: The value submitted when the check box is selected. The default value is "Yes"
- `Style`: The symbol displayed when the check box is selected. The default value is Check
- `SymbolColor`: The color of the symbol displayed when the check box is selected. The default value is black

## PdfCheckBoxStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfCheckBoxStyle.htm

The symbol displayed in a selected check box or radio button

### Fields and values

- `Check`: A check mark
- `Circle`: A filled circle
- `Cross`: A cross
- `Diamond`: A diamond
- `Square`: A filled square
- `Star`: A star

## PdfRadioGroupField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRadioGroupField.htm

A group of radio buttons of an interactive form, from which the user selects one button. The buttons are added with AddButton and can be placed on different pages

### Properties

- `AllowDeselect`: A flag indicating if the user can deselect the selected button by clicking it again, leaving the group without a selection. The default value is false
- `Buttons`: The radio buttons of the group
- `DefaultValue`: The export value of the button selected when the form is reset. The SelectedValue is used when it is not set
- `SelectedValue`: The export value of the selected button. No button is selected when it is not set
- `Style`: The symbol displayed in the selected button. The default value is Circle
- `SymbolColor`: The color of the symbol displayed in the selected button. The default value is black

### Methods

- `AddButton`: Adds a radio button to the group
- `AddButton`: Adds a radio button to the group in the given rectangle of a page

## PdfRadioButton

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfRadioButton.htm

A radio button of a radio button group, displayed in one rectangle of a page. The position is given in points from the top left corner of the content area of the page in a document being created, and from the top left corner of the page in an edited document

### Properties

- `BackgroundColor`: The background color of the button, or null for a transparent background. The default value is null
- `BorderColor`: The color of the button border, or null for no border. The default value is null
- `BorderStyle`: The style of the button border. The default value is Solid
- `BorderWidth`: The width of the button border, in points. The default value is 1
- `Description`: The description of the button shown by PDF viewers as a tooltip and read by assistive technology. The export value is used when it is not set
- `ExportValue`: The value of the group when this button is selected, unique in the group
- `Height`: The height of the button, in points
- `PageNumber`: The 1-based number of the page where the button is placed
- `Visibility`: The visibility of the button on screen and in print. The default value is Visible
- `Width`: The width of the button, in points
- `X`: The left position of the button, in points, from the left margin of a document being created or from the left edge of the page of an edited document
- `Y`: The top position of the button, in points, from the top margin of a document being created or from the top edge of the page of an edited document

## PdfChoiceFormField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfChoiceFormField.htm

The base class of the form fields where the user selects from a list of options: combo boxes and list boxes

### Properties

- `Alignment`: The horizontal alignment of the text in the field. The default value is Left
- `AutoFontSize`: A flag indicating if the font size is chosen automatically to fit the text in the field, ignoring the size of Font. The default value is false
- `CommitOnSelectionChange`: A flag indicating if a new selection is committed as soon as it is made, instead of when the field loses the focus. The default value is false
- `Font`: The font of the options text. A standard Helvetica font of 12 points is used when it is not set
- `Options`: The options the user can select from
- `Sorted`: A flag indicating if the PDF viewer sorts the options alphabetically. The default value is false

### Methods

- `AddOption`: Adds an option whose displayed text is also its export value
- `AddOption`: Adds an option with a displayed text and a different export value

## PdfComboBoxField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfComboBoxField.htm

A combo box of an interactive form, a drop-down list from which the user selects one option

### Properties

- `DefaultValue`: The export value of the option selected when the form is reset. The SelectedValue is used when it is not set
- `DoNotSpellCheck`: A flag indicating if the PDF viewer does not spell check the text typed in an editable combo box. The default value is false
- `Editable`: A flag indicating if the user can type a value that is not in the list of options. The default value is false
- `SelectedValue`: The export value of the selected option, or the text typed by the user in an editable combo box. No option is selected when it is not set

## PdfListBoxField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfListBoxField.htm

A list box of an interactive form, a scrollable list from which the user selects one or more options

### Properties

- `DefaultValues`: The export values of the options selected when the form is reset. The SelectedValues are used when it is not set
- `MultiSelect`: A flag indicating if the user can select several options. The default value is false
- `SelectedValues`: The export values of the selected options. Only the first value is used when MultiSelect is false
- `TopIndex`: The 0-based index of the first option visible in the list. The default value is 0

## PdfChoiceOption

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfChoiceOption.htm

An option of a combo box or list box

### Properties

- `ExportValue`: The value submitted when the option is selected
- `Text`: The text displayed in the field

## PdfPushButtonField

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPushButtonField.htm

A push button of an interactive form, which performs an action when it is clicked

### Properties

- `Action`: The action performed when the button is clicked, the same as Activate of Actions
- `AutoFontSize`: A flag indicating if the font size is chosen automatically to fit the caption in the button, ignoring the size of Font. The default value is false
- `Caption`: The text displayed on the button
- `Font`: The font of the caption. A standard Helvetica font of 12 points is used when it is not set
- `Icon`: The image displayed on the button as an icon, or null for no icon. The default value is null
- `Layout`: The arrangement of the caption and the icon. The default value is CaptionOnly, which changes to CaptionBelowIcon when an icon is set

### Methods

- `SetIcon`: Sets the image displayed on the button as an icon from a file
- `SetIcon`: Sets the image displayed on the button as an icon from a memory buffer

## PdfPushButtonLayout

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfPushButtonLayout.htm

The arrangement of the caption and the icon of a push button

### Fields and values

- `CaptionAboveIcon`: The caption is displayed above the icon
- `CaptionBelowIcon`: The caption is displayed below the icon
- `CaptionLeftOfIcon`: The caption is displayed to the left of the icon
- `CaptionOnly`: Only the caption is displayed
- `CaptionOverIcon`: The caption is displayed over the icon
- `CaptionRightOfIcon`: The caption is displayed to the right of the icon
- `IconOnly`: Only the icon is displayed

## PdfFormFieldType

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormFieldType.htm

The type of a form field

### Fields and values

- `CheckBox`: A check box
- `ComboBox`: A combo box
- `ListBox`: A list box
- `PushButton`: A push button
- `RadioGroup`: A group of radio buttons
- `Signature`: A signature field
- `Text`: A text field
- `Unknown`: A field of an unknown type

## PdfFormFieldBorderStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormFieldBorderStyle.htm

The border style of a form field

### Fields and values

- `Beveled`: A beveled border that makes the field appear raised
- `Dashed`: A dashed line
- `Inset`: An inset border that makes the field appear sunken
- `Solid`: A solid line
- `Underline`: A single line under the field

## PdfFormFieldVisibility

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormFieldVisibility.htm

The visibility of a form field on screen and in print

### Fields and values

- `Hidden`: The field is hidden on screen and not printed
- `HiddenPrinted`: The field is hidden on screen but printed
- `Visible`: The field is visible on screen and printed
- `VisibleNotPrinted`: The field is visible on screen but not printed

## PdfFormFieldInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormFieldInfo.htm

The description of a form field read from an existing document: its type, current value, options and placements in the pages

### Properties

- `Description`: The description of the field shown by PDF viewers as a tooltip, or null when it is not set
- `Editable`: A flag indicating if the user can type a value in a combo box that is not in its options
- `ExportValues`: The values a check box or radio button group can take when selected
- `Exportable`: A flag indicating if the value of the field is included when the form is submitted
- `MappingName`: The name used for the field when the form data is exported, or null when it is not set
- `MaxLength`: The maximum number of characters of a text field, or 0 for no limit
- `MultiSelect`: A flag indicating if a list box allows several selections or a combo box allows a typed value
- `Multiline`: A flag indicating if a text field accepts several lines of text
- `Name`: The fully qualified name of the field
- `Options`: The options of a combo box or list box
- `Password`: A flag indicating if a text field displays the typed characters as asterisks
- `ReadOnly`: A flag indicating if the user cannot change the value of the field
- `Required`: A flag indicating if the field must have a value when the form is submitted
- `Type`: The type of the field
- `Value`: The current value of the field: the text of a text field, the export value of the selected option, check box or radio button, or "Off" for a check box or radio button group without a selection. An empty text when the field has no value
- `Values`: The export values of all the selected options of a list box that allows several selections
- `Widgets`: The placements of the field in the pages of the document

## PdfFormFieldWidgetInfo

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormFieldWidgetInfo.htm

The placement of a form field in a page. A field with several placements, such as a radio button group, has one placement for each button

### Properties

- `Bounds`: The rectangle of the field in the page, in points from the top left corner of the page
- `ExportValue`: For a check box or a radio button, the value of the field when this placement is selected, otherwise null
- `PageNumber`: The 1-based number of the page where the field is placed

## PdfFormFieldActions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFormFieldActions.htm

The actions performed by the PDF viewer on the events of a form field. Each property holds the action performed on one event, or null when nothing is performed. The keystroke, format, validate and calculate events accept only JavaScript actions

### Properties

- `Activate`: The action performed when the field is activated, usually by a mouse click
- `Blur`: The action performed when the field loses the input focus
- `Calculate`: The JavaScript run to recalculate the value of the field when the value of another field changes
- `Focus`: The action performed when the field receives the input focus
- `Format`: The JavaScript run to format the value of the field before it is displayed
- `Keystroke`: The JavaScript run when the user types in a text field or changes the selection of a choice field
- `MouseDown`: The action performed when the mouse button is pressed inside the field
- `MouseEnter`: The action performed when the mouse cursor enters the field
- `MouseExit`: The action performed when the mouse cursor leaves the field
- `MouseUp`: The action performed when the mouse button is released inside the field
- `Validate`: The JavaScript run to validate the value of the field when it changes

## PdfFieldAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFieldAction.htm

An action performed by the PDF viewer when a form field event occurs, such as a click on a push button. The static methods of this class create the available actions. Several actions can be chained with Next, in which case they are performed in sequence

### Properties

- `Next`: The action performed after this one, or null when this is the last action of the sequence

### Methods

- `GoToNamedDestination`: Creates an action that displays a named destination of the document
- `GoToPage`: Creates an action that displays a page of the document
- `HideFields`: Creates an action that hides form fields
- `ImportData`: Creates an action that imports form field values from a FDF file
- `JavaScript`: Creates an action that runs JavaScript code in the PDF viewer
- `Named`: Creates a predefined viewer action, such as going to the next page or opening the print dialog
- `OpenUrl`: Creates an action that opens a URL in the web browser
- `ResetForm`: Creates an action that resets form fields to their default values. All the fields of the document are reset when no field names are given
- `ShowFields`: Creates an action that shows previously hidden form fields
- `SubmitForm`: Creates an action that submits the form field values to a URL. By default all the fields are submitted in HTML format with the POST method

## PdfSubmitFormAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfSubmitFormAction.htm

An action that submits the form field values to a URL

### Properties

- `Behavior`: How the submission is performed by the PDF viewer. The default value is NativePdfAction
- `CanonicalDateFormat`: A flag indicating if the dates are submitted in a canonical format. The default value is false
- `ExcludeFields`: A flag indicating if FieldNames lists the fields to exclude from the submission instead of the fields to submit. The default value is false
- `FieldNames`: The names of the fields to submit. All the fields are submitted when no names are set. When ExcludeFields is true, all the fields except the named ones are submitted
- `Format`: The format of the submitted data. The default value is Html
- `IncludeAnnotations`: A flag indicating if the annotations of the document are submitted together with the field values, for the Fdf and Xfdf formats. The default value is false
- `IncludeEmptyFields`: A flag indicating if the fields without a value are submitted too. The default value is true
- `Url`: The URL that receives the submitted values
- `UseHttpGet`: A flag indicating if the HTML submission uses the HTTP GET method instead of POST. It applies only to the Html format. The default value is false

## PdfResetFormAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfResetFormAction.htm

An action that resets form fields to their default values

### Properties

- `ExcludeFields`: A flag indicating if FieldNames lists the fields to keep unchanged instead of the fields to reset. The default value is false
- `FieldNames`: The names of the fields to reset. All the fields are reset when no names are set. When ExcludeFields is true, all the fields except the named ones are reset

## PdfHideFieldsAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfHideFieldsAction.htm

An action that hides or shows form fields

### Properties

- `FieldNames`: The names of the fields
- `Hide`: True when the fields are hidden, false when they are shown

## PdfImportDataAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfImportDataAction.htm

An action that imports form field values from a FDF file

### Properties

- `FilePath`: The path or URL of the FDF file

## PdfOpenUrlAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfOpenUrlAction.htm

An action that opens a URL in the web browser

### Properties

- `Url`: The URL to open

## PdfJavaScriptAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfJavaScriptAction.htm

An action that runs JavaScript code in the PDF viewer

### Properties

- `Code`: The JavaScript code

## PdfNamedAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfNamedAction.htm

A predefined action of the PDF viewer, such as going to the next page or opening the print dialog

### Properties

- `ActionType`: The predefined action

## PdfGoToPageAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfGoToPageAction.htm

An action that displays a page of the document

### Properties

- `Location`: The location and zoom displayed on the page
- `PageNumber`: The 1-based number of the page to display

## PdfGoToNamedDestinationAction

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfGoToNamedDestinationAction.htm

An action that displays a named destination of the document

### Properties

- `DestinationName`: The name of the destination
