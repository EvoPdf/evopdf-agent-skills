---
name: evopdf-next-pdf-forms
description: "Work with interactive PDF forms in EvoPdf Next: convert HTML form controls to PDF fields with PdfFormOptions (field naming, fonts, required and read-only mapping, submit and reset actions, excluded controls), create text, check box, radio button, combo box, list box and push button fields from code, and read, fill, import FDF or XFDF, export, flatten, rename and remove the fields of existing PDFs with PdfEditor.Form."
---

# EvoPdf Next: PDF forms from HTML forms

EVO HTML to PDF Converter can be configured to automatically convert HTML form controls to interactive PDF form fields by setting the `PdfDocumentOptions.GeneratePdfFormFields` property to true. An object of type `PdfDocumentOptions` is exposed by the `HtmlToPdfConverter.PdfDocumentOptions` property

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfFormOptions`**: Specifies options for converting HTML form controls into interactive PDF form fields. These options control field naming, button action generation, su...
- **`PdfFieldNamingMode`**: Specifies how PDF field names are generated from HTML form controls
- **`PdfSubmitBehavior`**: Specifies how submit actions are generated for PDF buttons
- **`PdfSubmitFormat`**: Specifies the format used when a PDF form is submitted
- **`PdfValidationBehavior`**: Specifies how HTML validation metadata is mapped to PDF form behavior
- **`PdfForm`**: The interactive form of a loaded document. It reads the fields of the document, fills them with values, changes their appearance and flattens, removes...
- **`PdfFormField`**: The base class of the interactive form fields created from code: text fields, check boxes, radio button groups, combo boxes, list boxes and push butto...
- **`PdfWidgetFormField`**: The base class of the form fields displayed in one rectangle of a page. The position is given in points from the top left corner of the content area o...
- **`PdfTextField`**: A text field of an interactive form, where the user types a single line or several lines of text
- **`PdfCheckBoxField`**: A check box of an interactive form
- **`PdfCheckBoxStyle`**: The symbol displayed in a selected check box or radio button
- **`PdfRadioGroupField`**: A group of radio buttons of an interactive form, from which the user selects one button. The buttons are added with AddButton and can be placed on dif...
- **`PdfRadioButton`**: A radio button of a radio button group, displayed in one rectangle of a page. The position is given in points from the top left corner of the content ...
- **`PdfChoiceFormField`**: The base class of the form fields where the user selects from a list of options: combo boxes and list boxes
- **`PdfComboBoxField`**: A combo box of an interactive form, a drop-down list from which the user selects one option
- **`PdfListBoxField`**: A list box of an interactive form, a scrollable list from which the user selects one or more options
- **`PdfChoiceOption`**: An option of a combo box or list box
- **`PdfPushButtonField`**: A push button of an interactive form, which performs an action when it is clicked
- **`PdfPushButtonLayout`**: The arrangement of the caption and the icon of a push button
- **`PdfFormFieldType`**: The type of a form field
- **`PdfFormFieldBorderStyle`**: The border style of a form field
- **`PdfFormFieldVisibility`**: The visibility of a form field on screen and in print
- **`PdfFormFieldInfo`**: The description of a form field read from an existing document: its type, current value, options and placements in the pages
- **`PdfFormFieldWidgetInfo`**: The placement of a form field in a page. A field with several placements, such as a radio button group, has one placement for each button
- **`PdfFormFieldActions`**: The actions performed by the PDF viewer on the events of a form field. Each property holds the action performed on one event, or null when nothing is ...
- **`PdfFieldAction`**: An action performed by the PDF viewer when a form field event occurs, such as a click on a push button. The static methods of this class create the av...
- **`PdfSubmitFormAction`**: An action that submits the form field values to a URL
- **`PdfResetFormAction`**: An action that resets form fields to their default values
- **`PdfHideFieldsAction`**: An action that hides or shows form fields
- **`PdfImportDataAction`**: An action that imports form field values from a FDF file
- **`PdfOpenUrlAction`**: An action that opens a URL in the web browser
- **`PdfJavaScriptAction`**: An action that runs JavaScript code in the PDF viewer
- **`PdfNamedAction`**: A predefined action of the PDF viewer, such as going to the next page or opening the print dialog
- **`PdfGoToPageAction`**: An action that displays a page of the document
- **`PdfGoToNamedDestinationAction`**: An action that displays a named destination of the document

35 types, 228 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

Three ways to work with PDF forms:

| Task | API |
|---|---|
| Convert the controls of an HTML form to PDF fields | `HtmlToPdfConverter.PdfDocumentOptions.PdfFormOptions` (`ExcludedFieldsSelector` keeps the matching controls static) |
| Create fields from code in a new or an existing document | `PdfDocument.AddFormField`, `PdfEditor.AddFormField` with `PdfTextField`, `PdfCheckBoxField`, `PdfRadioGroupField`, `PdfComboBoxField`, `PdfListBoxField`, `PdfPushButtonField` |
| Read, fill, import, export, flatten, rename or remove the fields of an existing document | `PdfEditor.Form` (`PdfForm`) |

Field names are unique in a document. The actions of push buttons and the field events (`PdfFormFieldActions`) use the
`PdfFieldAction` types: submit, reset, hide fields, import data, open a URL, go to a page or a named destination,
JavaScript and named viewer actions; `PdfDocument.AddJavaScript` and `PdfEditor.AddJavaScript` add document scripts
whose functions the actions can call. `PdfEditor.Flatten = true` turns the fields into static page content.

## Create PDF Forms from HTML Forms

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-forms-from-html-forms.htm

- Create PDF Forms from HTML Forms: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-forms-from-html-forms.htm

## Create PDF Forms from Code

The fields of an interactive PDF form can be created from code in a document being created with `PdfDocument` or added to an existing document loaded in `PdfEditor`, with the `PdfDocument.AddFormField(PdfFormField)` and `PdfEditor.AddFormField(PdfFormField)` methods. The fields have a name, a position, a size and an appearance, they can be required or read-only and they can carry scripts and actions executed by the PDF viewer. The fields of a HTML form are converted automatically by the HTML to PDF converter, as described in the topic; this topic describes the fields created from code.

### Field Types

```csharp
// A text field with a maximum length displayed in separate cells
PdfTextField accessCodeField = new PdfTextField("accessCode", 1, 130, 110, 120, 22)
{
    Font = fieldFont,
    BorderColor = PdfColor.Gray,
    MaxLength = 6,
    Comb = true,
    Description = "Six character access code"
};
pdfDocument.AddFormField(accessCodeField);

// A combo box with a selected option
PdfComboBoxField countryField = new PdfComboBoxField("country", 1, 130, 144, 180, 22) { Font = fieldFont };
countryField.AddOption("Austria", "AT");
countryField.AddOption("Romania", "RO");
countryField.SelectedValue = "RO";
pdfDocument.AddFormField(countryField);

// A group of radio buttons; each button is placed in a page
PdfRadioGroupField ticketField = new PdfRadioGroupField("ticketType") { SelectedValue = "standard" };
ticketField.AddButton("standard", 1, 130, 181, 14, 14);
ticketField.AddButton("premium", 1, 230, 181, 14, 14);
pdfDocument.AddFormField(ticketField);
```

### Scripts and Actions

```csharp
// The total is computed by the viewer from the other fields
totalField.Actions.Calculate = new PdfJavaScriptAction(
    "var prices = { standard: 350, premium: 500, student: 120 };" +
    "event.value = prices[this.getField('ticketType').value] || 0;");

// The buttons submit the form as FDF, reset it and print the document
submitButton.Action = new PdfSubmitFormAction("https://www.example.com/register") { Format = PdfSubmitFormat.Fdf };
resetButton.Action = new PdfResetFormAction(null);
printButton.Action = new PdfNamedAction(PdfNamedActionType.Print);

// The document level script defines a function used by the validate script of the email field
pdfDocument.AddJavaScript("function isValidEmail(value) { return value.length == 0 || value.indexOf('@') > 0; }");
emailField.Actions.Validate = new PdfJavaScriptAction(
    "if (!isValidEmail(event.value)) { app.alert('The email address is not valid'); event.rc = false; }");
```

All 3 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-forms-from-code.htm

## Fill, Flatten and Edit PDF Forms

The `PdfEditor.Form` property of `PdfEditor` gives the interactive form of a loaded document as a `PdfForm` object. The form reads the fields of the document, fills them with values, imports the values of a FDF or XFDF file, changes the appearance of the fields, makes them read-only or hidden, renames and removes them and flattens them to static content. The current values can be exported as FDF. The changes are written when the document is saved with `PdfEditor.Save`. `PdfEditor.ContainsForm` tells if the loaded document has a form.

### Read the Fields

```csharp
foreach (PdfFormFieldInfo field in pdfEditor.Form.Fields)
{
    string value = field.Type == PdfFormFieldType.ListBox && field.Values.Count > 0
        ? string.Join(", ", field.Values) : field.Value;
    Console.WriteLine("{0} ({1}): {2}", field.Name, field.Type, value);
}
```

### Fill the Fields

```csharp
PdfForm form = pdfEditor.Form;

form.SetValue("fullName", "John Smith");
form.SetValue("country", "DE");
form.SetValue("ticketType", "premium");
form.SetChecked("newsletter", true);
form.SetValues("sessions", new[] { "Keynote", "Panel discussion" });

// The values of a FDF file fill the fields with the same names
form.ImportData(fdfData);
```

### Flatten the Fields

```csharp
// Only the fields with a value become static content; the other fields stay editable
foreach (string fieldName in form.FieldNames)
{
    string value = form.GetValue(fieldName);
    if (!string.IsNullOrEmpty(value) && value != "Off")
        form.FlattenField(fieldName);
}
```

All 4 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/fill-flatten-and-edit-pdf-forms.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
