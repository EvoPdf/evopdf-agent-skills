# PDF forms from HTML forms: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Create PDF Forms from HTML Forms

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-forms-from-html-forms.htm

### Set the Form Options

```csharp
PdfFormOptions formOptions = htmlToPdfConverter.PdfDocumentOptions.PdfFormOptions;
formOptions.FieldNamingMode = PdfFieldNamingMode.PreserveHtmlNamesWhenSafe;
formOptions.ApplyRequiredAsPdfRequiredFlag = true;
formOptions.SubmitBehavior = PdfSubmitBehavior.NativePdfAction;
formOptions.SubmitFormat = PdfSubmitFormat.Html;
formOptions.ValidationBehavior = PdfValidationBehavior.AcrobatJavaScriptBasic;
formOptions.FormFieldFont = PdfFontManager.CreateBaseFont(fontFilePath);
```

### Code Sample - Auto Create PDF Forms from HTML Forms

```csharp
using System;
using System.IO;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.HTML_to_PDF;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.HTML_to_PDF
{
    public class Create_PDF_Forms_from_HTML_FormsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;
        public Create_PDF_Forms_from_HTML_FormsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();

            return View(model);
        }

        [HttpPost]
        public ActionResult ConvertHtmlToPdf(Create_PDF_Forms_from_HTML_Forms_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            // Create a HTML to PDF converter object with default settings
            HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

            // Specifies whether HTML form controls are converted to interactive PDF form fields
            htmlToPdfConverter.PdfDocumentOptions.GeneratePdfFormFields = model.GeneratePdfFormFields;

            PdfFormOptions formOptions = htmlToPdfConverter.PdfDocumentOptions.PdfFormOptions;

            // The names of the PDF fields: the HTML names when they are unique, the HTML names as they are, or
            // names qualified with the form and the position of the control
            formOptions.FieldNamingMode = (PdfFieldNamingMode)Enum.Parse(typeof(PdfFieldNamingMode), model.FieldNamingMode);

            // The actions of the generated buttons: the HTML submit and reset buttons submit and reset the PDF form
            formOptions.EnableButtonActions = model.EnableButtonActions;
            formOptions.MapHtmlSubmitButtonsToPdfActions = model.MapHtmlSubmitButtonsToPdfActions;
            formOptions.MapHtmlResetButtonsToPdfActions = model.MapHtmlResetButtonsToPdfActions;

            // The fields submitted or reset by a button: only the fields of its HTML form, or all the fields
            formOptions.SubmitOnlyFieldsFromButtonForm = model.SubmitOnlyFieldsFromButtonForm;
            formOptions.ResetOnlyFieldsFromButtonForm = model.ResetOnlyFieldsFromButtonForm;
            formOptions.ExcludeDisabledFieldsFromSubmit = model.ExcludeDisabledFieldsFromSubmit;
            formOptions.IncludeReadOnlyFieldsInSubmit = model.IncludeReadOnlyFieldsInSubmit;

            // The HTML required attribute marks the PDF field as required
            formOptions.ApplyRequiredAsPdfRequiredFlag = model.ApplyRequiredAsPdfRequiredFlag;

            // The submit action is a native PDF action or a script that submits the form like a browser does;
            // the format is the format of the submitted data
            formOptions.SubmitBehavior = (PdfSubmitBehavior)Enum.Parse(typeof(PdfSubmitBehavior), model.SubmitBehavior);
            formOptions.SubmitFormat = (PdfSubmitFormat)Enum.Parse(typeof(PdfSubmitFormat), model.SubmitFormat);

            // A basic validation script can be generated from the HTML validation attributes
            formOptions.ValidationBehavior = (PdfValidationBehavior)Enum.Parse(typeof(PdfValidationBehavior), model.ValidationBehavior);

            if (model.UseFormFieldFont)
            {
                // The font of the text displayed in the fields; a standard font is used otherwise
                string fontFilePath = Path.Combine(m_hostingEnvironment.ContentRootPath, "wwwroot", "DemoAppFiles", "Input", "Font_Files", "DejaVuSerif.ttf");
                formOptions.FormFieldFont = PdfFontManager.CreateBaseFont(fontFilePath);
            }

            // The buffer to receive the generated PDF document
            byte[] outPdfBuffer = null;

            if (model.HtmlPageSource == "Html")
            {
                string htmlString = model.HtmlString;
                string baseUrl = model.BaseUrl;

                // Convert a HTML string with a base URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertHtml(htmlString, baseUrl);
            }
            else
            {
                string url = model.Url;

                // Convert the HTML page given by an URL to a PDF document in a memory buffer
                outPdfBuffer = htmlToPdfConverter.ConvertUrl(url);
            }

            // Send the PDF file to browser
            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Create_PDF_Forms.pdf";

            return fileResult;
        }

        private Create_PDF_Forms_from_HTML_Forms_ViewModel SetViewModel()
        {
            var model = new Create_PDF_Forms_from_HTML_Forms_ViewModel();

            var contentRootPath = m_hostingEnvironment.ContentRootPath + "/wwwroot";

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder();
            uriBuilder.Scheme = request.Scheme;
            uriBuilder.Host = request.Host.Host;
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;
            uriBuilder.Path = request.PathBase.ToString() + request.Path.ToString();
            uriBuilder.Query = request.QueryString.ToString();

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Create_PDF_Forms_from_HTML_Forms".Length);

            model.HtmlString = System.IO.File.ReadAllText(System.IO.Path.Combine(contentRootPath, "DemoAppFiles/Input/HTML_Files/HTML_Form.html"));
            model.BaseUrl = rootUrl + "DemoAppFiles/Input/HTML_Files/";
            model.Url = rootUrl + "DemoAppFiles/Input/HTML_Files/HTML_Form.html";

            return model;
        }
    }
}
```

## Create PDF Forms from Code

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/create-pdf-forms-from-code.htm

### Create Fields of Several Types

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

### Add Scripts and Actions

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

### Code Sample - Create PDF Forms from Code

```csharp
using System.IO;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Creator;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Creator
{
    public class Create_PDF_Forms_from_CodeController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Create_PDF_Forms_from_CodeController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = new Create_PDF_Forms_from_Code_ViewModel();
            return View(model);
        }

        [HttpPost]
        public ActionResult CreatePdf(Create_PDF_Forms_from_Code_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            PdfDocumentCreateSettings pdfCreateSettings = new PdfDocumentCreateSettings()
            {
                PageSize = PdfPageSize.A4,
                PageOrientation = PdfPageOrientation.Portrait,
                Margins = new PdfMargins(40, 40, 40, 40),
                PdfStandard = model.PdfStandard,
                Language = "en-US"
            };

            using PdfDocument pdfDocument = new PdfDocument(pdfCreateSettings);
            pdfDocument.PdfDocumentInfo.Title = "Conference Registration";

            // The fields are drawn as static content instead of interactive fields
            pdfDocument.Flatten = model.FlattenFields;

            // PDF/A documents have no actions and no scripts
            bool pdfA = PdfStandardIsPdfA(model.PdfStandard);
            bool addActions = model.AddFieldActions && !pdfA;

            string fontsPath = GetDemoFontsPath();
            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(Path.Combine(fontsPath, "DejaVuSerif.ttf"));
            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 18f, PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont labelFont = PdfFontManager.CreateFont(baseFont, 11f, PdfFontStyle.Normal, PdfColor.Black);
            PdfFont noteFont = PdfFontManager.CreateFont(baseFont, 9f, PdfFontStyle.Normal, PdfColor.DarkGray);

            // The text of the fields is written with a standard font in a document without a standard and with
            // the embedded font in a PDF/A or PDF/UA document
            PdfFont fieldFont = model.PdfStandard == PdfStandard.None
                ? PdfFontManager.CreateStandardFont(PdfStandardFont.Helvetica, 10f, PdfColor.Black)
                : PdfFontManager.CreateFont(PdfFontManager.CreateBaseFont(Path.Combine(fontsPath, "DejaVuSerif.ttf"), false), 10f, PdfColor.Black);

            PdfColor borderColor = PdfColor.Gray;
            const float fieldLeft = 130;
            const float fieldWidth = 260;
            float crtYPos = 0;

            pdfDocument.AddText(new PdfTextElement("Conference Registration", titleFont) { X = 0, Y = crtYPos });
            crtYPos += 40;

            // The field positions are relative to the content area of the page, like the positions of the text elements
            AddLabel(pdfDocument, "Full name", labelFont, crtYPos);
            PdfTextField fullNameField = new PdfTextField("fullName", 1, fieldLeft, crtYPos, fieldWidth, 22)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                BackgroundColor = PdfColor.FromHex("#F4F6F8"),
                Description = "Full name",
                Required = model.RequiredFields,
                Text = "Jane Doe"
            };
            if (addActions)
            {
                // The name is shown in capitals while it is typed and when the field loses the focus
                fullNameField.Actions.Keystroke = new PdfJavaScriptAction("event.change = event.change.toUpperCase();");
                fullNameField.Actions.Format = new PdfJavaScriptAction("event.value = event.value.toUpperCase();");
            }
            pdfDocument.AddFormField(fullNameField);
            crtYPos += 34;

            AddLabel(pdfDocument, "Email", labelFont, crtYPos);
            PdfTextField emailField = new PdfTextField("email", 1, fieldLeft, crtYPos, fieldWidth, 22)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Description = "Email address",
                Required = model.RequiredFields
            };
            if (addActions)
            {
                // The address is validated when the field is committed. The validation uses the function
                // defined by the document level script when the script is added to the document
                string validateScript = model.AddDocumentScript
                    ? "if (!isValidEmail(event.value)) { app.alert('The email address is not valid'); event.rc = false; }"
                    : "if (event.value.length > 0 && event.value.indexOf('@') < 0) { app.alert('The email address is not valid'); event.rc = false; }";
                emailField.Actions.Validate = new PdfJavaScriptAction(validateScript);
            }
            pdfDocument.AddFormField(emailField);
            crtYPos += 34;

            AddLabel(pdfDocument, "Access code", labelFont, crtYPos);
            pdfDocument.AddFormField(new PdfTextField("accessCode", 1, fieldLeft, crtYPos, 120, 22)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                MaxLength = 6,
                Comb = true,
                Description = "Six character access code"
            });
            crtYPos += 34;

            AddLabel(pdfDocument, "Country", labelFont, crtYPos);
            PdfComboBoxField countryField = new PdfComboBoxField("country", 1, fieldLeft, crtYPos, 180, 22)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Description = "Country"
            };
            countryField.AddOption("Austria", "AT");
            countryField.AddOption("France", "FR");
            countryField.AddOption("Germany", "DE");
            countryField.AddOption("Italy", "IT");
            countryField.AddOption("Spain", "ES");
            countryField.SelectedValue = "DE";
            pdfDocument.AddFormField(countryField);
            crtYPos += 34;

            AddLabel(pdfDocument, "Ticket type", labelFont, crtYPos);
            PdfRadioGroupField ticketField = new PdfRadioGroupField("ticketType")
            {
                SelectedValue = "standard",
                SymbolColor = PdfColor.DarkBlue,
                Description = "Ticket type"
            };
            string[] ticketValues = { "standard", "premium", "student" };
            string[] ticketCaptions = { "Standard", "Premium", "Student" };
            for (int i = 0; i < ticketValues.Length; i++)
            {
                PdfRadioButton button = ticketField.AddButton(ticketValues[i], 1, fieldLeft + i * 100, crtYPos + 3, 14, 14);
                button.BorderColor = borderColor;
                button.BorderStyle = model.BorderStyle;
                pdfDocument.AddText(new PdfTextElement(ticketCaptions[i], labelFont) { X = fieldLeft + 20 + i * 100, Y = crtYPos + 3 });
            }
            pdfDocument.AddFormField(ticketField);
            crtYPos += 34;

            AddLabel(pdfDocument, "Sessions", labelFont, crtYPos);
            PdfListBoxField sessionsField = new PdfListBoxField("sessions", 1, fieldLeft, crtYPos, fieldWidth, 70)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                MultiSelect = true,
                Description = "Sessions to attend"
            };
            sessionsField.AddOption("Keynote");
            sessionsField.AddOption("Workshop: PDF generation");
            sessionsField.AddOption("Workshop: Accessibility");
            sessionsField.AddOption("Panel discussion");
            sessionsField.AddOption("Networking dinner");
            sessionsField.SelectedValues.Add("Keynote");
            pdfDocument.AddFormField(sessionsField);
            crtYPos += 82;

            AddLabel(pdfDocument, "Options", labelFont, crtYPos);
            pdfDocument.AddFormField(new PdfCheckBoxField("newsletter", 1, fieldLeft, crtYPos + 3, 14, 14)
            {
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Style = model.CheckBoxStyle,
                Checked = true,
                Description = "Receive the newsletter"
            });
            pdfDocument.AddText(new PdfTextElement("Receive the newsletter", labelFont) { X = fieldLeft + 20, Y = crtYPos + 3 });
            pdfDocument.AddFormField(new PdfCheckBoxField("terms", 1, fieldLeft, crtYPos + 25, 14, 14)
            {
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Style = model.CheckBoxStyle,
                Required = model.RequiredFields,
                Description = "Accept the terms"
            });
            pdfDocument.AddText(new PdfTextElement("I accept the terms and conditions", labelFont) { X = fieldLeft + 20, Y = crtYPos + 25 });
            crtYPos += 56;

            AddLabel(pdfDocument, "Comments", labelFont, crtYPos);
            pdfDocument.AddFormField(new PdfTextField("comments", 1, fieldLeft, crtYPos, fieldWidth, 70)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Multiline = true,
                DoNotSpellCheck = true,
                Description = "Comments"
            });
            crtYPos += 82;

            AddLabel(pdfDocument, "Total (EUR)", labelFont, crtYPos);
            PdfTextField totalField = new PdfTextField("total", 1, fieldLeft, crtYPos, 100, 22)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                ReadOnly = true,
                Alignment = PdfTextAlignment.Right,
                Text = "350",
                Description = "Total amount"
            };
            if (addActions)
            {
                // The total follows the ticket type and the number of sessions
                totalField.Actions.Calculate = new PdfJavaScriptAction(
                    "var prices = { standard: 350, premium: 500, student: 120 };" +
                    "var ticket = this.getField('ticketType').value;" +
                    "var sessions = this.getField('sessions').value;" +
                    "var count = sessions == null ? 0 : (typeof sessions == 'string' ? 1 : sessions.length);" +
                    "event.value = (prices[ticket] || 0) + count * 25;");
            }
            pdfDocument.AddFormField(totalField);
            crtYPos += 40;

            // The buttons: submit, reset, print and the navigation to the second page
            PdfPushButtonField submitButton = new PdfPushButtonField("submit", 1, fieldLeft, crtYPos, 100, 26, "Submit")
            {
                Font = fieldFont,
                BackgroundColor = PdfColor.LightGray,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Description = "Submit the form"
            };
            PdfPushButtonField resetButton = new PdfPushButtonField("reset", 1, fieldLeft + 110, crtYPos, 100, 26, "Reset")
            {
                Font = fieldFont,
                BackgroundColor = PdfColor.LightGray,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Description = "Reset the form"
            };
            PdfPushButtonField printButton = new PdfPushButtonField("print", 1, fieldLeft + 220, crtYPos, 100, 26, "Print")
            {
                Font = fieldFont,
                BackgroundColor = PdfColor.LightGray,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Description = "Print the form"
            };
            if (addActions)
            {
                // The form data is sent as FDF to the given address; the reset action clears all the fields
                // and the named action opens the print dialog of the viewer
                submitButton.Action = new PdfSubmitFormAction("https://www.example.com/register") { Format = PdfSubmitFormat.Fdf };
                resetButton.Action = new PdfResetFormAction(null);
                printButton.Action = new PdfNamedAction(PdfNamedActionType.Print);
            }
            pdfDocument.AddFormField(submitButton);
            pdfDocument.AddFormField(resetButton);
            pdfDocument.AddFormField(printButton);
            crtYPos += 40;

            PdfPushButtonField hideButton = new PdfPushButtonField("hideComments", 1, fieldLeft, crtYPos, 100, 26, "Hide comments")
            {
                Font = fieldFont,
                BackgroundColor = PdfColor.LightGray,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Exportable = false
            };
            PdfPushButtonField nextPageButton = new PdfPushButtonField("nextPage", 1, fieldLeft + 110, crtYPos, 100, 26, "Next page")
            {
                Font = fieldFont,
                BackgroundColor = PdfColor.LightGray,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Exportable = false
            };
            PdfPushButtonField siteButton = new PdfPushButtonField("website", 1, fieldLeft + 220, crtYPos, 100, 26, "Web site")
            {
                Font = fieldFont,
                BackgroundColor = PdfColor.LightGray,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Exportable = false
            };
            if (addActions)
            {
                hideButton.Action = new PdfHideFieldsAction(new[] { "comments" }, true);
                nextPageButton.Action = new PdfGoToPageAction(2, PdfLinkPageLocation.FitPage());
                siteButton.Action = new PdfOpenUrlAction("https://www.example.com");
            }
            pdfDocument.AddFormField(hideButton);
            pdfDocument.AddFormField(nextPageButton);
            pdfDocument.AddFormField(siteButton);
            crtYPos += 44;

            pdfDocument.AddText(new PdfTextElement("The fields marked as required must be filled before the form is submitted", noteFont) { X = 0, Y = crtYPos });

            // The second page holds a field of the same radio group and a hidden field
            pdfDocument.AddPage();
            pdfDocument.AddText(new PdfTextElement("Accommodation", titleFont) { X = 0, Y = 0 });
            AddLabel(pdfDocument, "Hotel room", labelFont, 40);
            PdfRadioGroupField roomField = new PdfRadioGroupField("hotelRoom")
            {
                SymbolColor = PdfColor.DarkBlue,
                AllowDeselect = true,
                Description = "Hotel room"
            };
            roomField.AddButton("single", 2, fieldLeft, 43, 14, 14).BorderColor = borderColor;
            pdfDocument.AddText(new PdfTextElement("Single", labelFont) { X = fieldLeft + 20, Y = 43 });
            roomField.AddButton("double", 2, fieldLeft + 100, 43, 14, 14).BorderColor = borderColor;
            pdfDocument.AddText(new PdfTextElement("Double", labelFont) { X = fieldLeft + 120, Y = 43 });
            pdfDocument.AddFormField(roomField);

            AddLabel(pdfDocument, "Arrival date", labelFont, 74);
            pdfDocument.AddFormField(new PdfTextField("arrivalDate", 2, fieldLeft, 74, 120, 22)
            {
                Font = fieldFont,
                BorderColor = borderColor,
                BorderStyle = model.BorderStyle,
                Description = "Arrival date"
            });

            if (model.AddDocumentScript && !pdfA)
            {
                // The document level script defines the functions used by the scripts of the fields.
                // The script does not change the document when it is opened
                pdfDocument.AddJavaScript("function isValidEmail(value) { return value.length == 0 || value.indexOf('@') > 0; }");
            }

            byte[] outPdfBuffer = pdfDocument.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Create_PDF_Forms_from_Code.pdf";
            return fileResult;
        }

        private static void AddLabel(PdfDocument pdfDocument, string text, PdfFont font, float yPos)
        {
            pdfDocument.AddText(new PdfTextElement(text, font) { X = 0, Y = yPos + 4 });
        }

        private static bool PdfStandardIsPdfA(PdfStandard pdfStandard)
        {
            return pdfStandard.ToString().Contains("PdfA");
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```

## Fill, Flatten and Edit PDF Forms

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/fill-flatten-and-edit-pdf-forms.htm

### List the Fields of the Form

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

### Flatten the Filled Fields

```csharp
// Only the fields with a value become static content; the other fields stay editable
foreach (string fieldName in form.FieldNames)
{
    string value = form.GetValue(fieldName);
    if (!string.IsNullOrEmpty(value) && value != "Off")
        form.FlattenField(fieldName);
}
```

### Code Sample - Fill, Flatten and Edit PDF Forms

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using EvoPdf_Next_AspNetDemo.Models;
using EvoPdf_Next_AspNetDemo.Models.PDF_Editor;

// Use EVO PDF Namespace
using EvoPdf.Next;

namespace EvoPdf_Next_AspNetDemo.Controllers.PDF_Editor
{
    public class Fill_PDF_FormsController : Controller
    {
        private readonly IWebHostEnvironment m_hostingEnvironment;

        public Fill_PDF_FormsController(IWebHostEnvironment hostingEnvironment)
        {
            m_hostingEnvironment = hostingEnvironment;
        }

        public IActionResult Index()
        {
            var model = SetViewModel();
            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> FillForm(Fill_PDF_Forms_ViewModel model)
        {
            if (!ModelState.IsValid)
            {
                var errorMessage = ModelStateHelper.GetModelErrors(ModelState);
                throw new ValidationException(errorMessage);
            }

            // Set the license key received after purchase to use the library in licensed mode; leave it commented for demo mode
            // Licensing.LicenseKey = "your-license-key";

            byte[] inputPdfBytes = await ReadInputPdf(model);
            string password = !string.IsNullOrEmpty(model.OwnerPassword) ? model.OwnerPassword :
                !string.IsNullOrEmpty(model.UserPassword) ? model.UserPassword : null;

            using PdfEditor pdfEditor = new PdfEditor(inputPdfBytes, password);

            if (!pdfEditor.ContainsForm)
                throw new Exception("The loaded document does not contain a form");

            PdfForm form = pdfEditor.Form;

            // The appearance of a field is regenerated when its value changes, so the new value is displayed
            // by any viewer; a viewer regenerates the appearances itself when the flag is false
            form.GenerateAppearances = model.GenerateAppearances;

            if (model.UseSubstitutionFont)
            {
                // The characters missing from the font of a field are displayed with this font
                form.AddSubstitutionFont(PdfFontManager.CreateBaseFont(Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf")));
            }

            if (model.ReplaceCountryOptions && form.ContainsField("country"))
            {
                // The options of a combo box or list box are replaced; the value selected below must be an export value of the new options
                form.SetOptions("country", new List<PdfChoiceOption>
                {
                    new PdfChoiceOption("Canada", "CA"),
                    new PdfChoiceOption("Germany", "DE"),
                    new PdfChoiceOption("Japan", "JP"),
                    new PdfChoiceOption("United States", "US")
                });
            }

            if (model.ImportSampleData || (model.DataFile != null && model.DataFile.Length > 0))
            {
                // The values of a FDF or XFDF file fill the fields with the same names
                byte[] formData;
                if (model.DataFile != null && model.DataFile.Length > 0)
                {
                    using var ms = new MemoryStream();
                    await model.DataFile.CopyToAsync(ms);
                    formData = ms.ToArray();
                }
                else
                {
                    formData = await System.IO.File.ReadAllBytesAsync(Path.Combine(GetDemoFilesPath(), "PDF_Files", "PDF_Form_Data.fdf"));
                }
                form.ImportData(formData);
            }

            if (model.FillFields)
            {
                // The values typed in the demo page are set field by field; a field missing from the loaded document is skipped
                SetValue(form, "fullName", model.FullName);
                SetValue(form, "email", model.Email);
                SetValue(form, "accessCode", "A1B2C3");
                SetValue(form, "country", model.Country);
                SetValue(form, "ticketType", model.TicketType);
                SetValue(form, "comments", model.Comments);

                if (form.ContainsField("newsletter"))
                    form.SetChecked("newsletter", model.Newsletter);
                if (form.ContainsField("terms"))
                    form.SetChecked("terms", model.Terms);

                if (form.ContainsField("sessions"))
                {
                    // Several options of a multiple selection list box are selected at once
                    string[] sessions = (model.Sessions ?? string.Empty).Split(',', StringSplitOptions.RemoveEmptyEntries).Select(s => s.Trim()).ToArray();
                    form.SetValues("sessions", sessions);
                }

                if (form.ContainsField("total"))
                {
                    // The total is computed by the demo from the ticket type and the number of sessions
                    int ticketPrice = model.TicketType == "premium" ? 500 : model.TicketType == "student" ? 120 : 350;
                    int sessionsCount = form.GetValues("sessions")?.Length ?? 0;
                    form.SetValue("total", (ticketPrice + sessionsCount * 25).ToString());
                }
            }

            if (model.SetFieldColors)
            {
                foreach (string fieldName in form.FieldNames)
                {
                    PdfFormFieldInfo field = form.GetField(fieldName);
                    if (field.Type == PdfFormFieldType.Text || field.Type == PdfFormFieldType.ComboBox || field.Type == PdfFormFieldType.ListBox)
                    {
                        form.SetBackgroundColor(fieldName, PdfColor.FromHex("#FFF8E1"));
                        form.SetBorderColor(fieldName, PdfColor.DarkOrange);
                        form.SetTextColor(fieldName, PdfColor.DarkBlue);
                    }
                }
            }

            if (model.SetFieldFont)
            {
                // The font is created with a positive size; the size of the field text is set separately below
                PdfFont fieldFont = PdfFontManager.CreateFont(Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf"),
                    model.FieldFontSize > 0 ? model.FieldFontSize : 10, PdfFontStyle.Normal, PdfColor.Black);
                foreach (string fieldName in form.FieldNames)
                {
                    PdfFormFieldInfo field = form.GetField(fieldName);
                    if (field.Type == PdfFormFieldType.Text || field.Type == PdfFormFieldType.ComboBox || field.Type == PdfFormFieldType.ListBox)
                    {
                        // The font replaces the font of the field; a size of 0 sizes the text to fit the field
                        form.SetFont(fieldName, fieldFont);
                        form.SetFontSize(fieldName, model.FieldFontSize);
                    }
                }
            }

            if (model.ReadOnlyFields)
            {
                foreach (string fieldName in form.FieldNames)
                {
                    if (form.GetField(fieldName).Type != PdfFormFieldType.PushButton)
                    {
                        form.SetReadOnly(fieldName, true);
                        form.SetRequired(fieldName, false);
                    }
                }
            }

            if (form.ContainsField("comments"))
            {
                PdfFormFieldVisibility visibility = (PdfFormFieldVisibility)Enum.Parse(typeof(PdfFormFieldVisibility), model.CommentsVisibility);
                if (visibility != PdfFormFieldVisibility.Visible)
                    form.SetVisibility("comments", visibility);

                if (model.RenameCommentsField)
                    form.RenameField("comments", "remarks");
            }

            if (model.RemoveAccessCodeField && form.ContainsField("accessCode"))
            {
                // The field and its appearance in the page are removed
                form.RemoveField("accessCode");
            }

            if (model.OutputFormat == "Fdf")
            {
                // The current values of the fields, with the names of the fields and the path of the document
                byte[] fdfData = form.ExportFdf();
                FileResult fdfResult = new FileContentResult(fdfData, "application/vnd.fdf");
                fdfResult.FileDownloadName = "Fill_PDF_Forms.fdf";
                return fdfResult;
            }

            if (model.AddFieldsListPage)
                AddFieldsListPage(pdfEditor);

            if (model.FlattenMode == "FilledFields")
            {
                // Only the fields with a value become static content; the other fields stay editable
                foreach (string fieldName in form.FieldNames)
                {
                    PdfFormFieldInfo field = form.GetField(fieldName);
                    string value = form.GetValue(fieldName);
                    if (field.Type != PdfFormFieldType.PushButton && !string.IsNullOrEmpty(value) && value != "Off")
                        form.FlattenField(fieldName);
                }
            }
            else if (model.FlattenMode == "AllFields")
            {
                form.FlattenAllFields();
            }

            byte[] outPdfBuffer = pdfEditor.Save();

            FileResult fileResult = new FileContentResult(outPdfBuffer, "application/pdf");
            fileResult.FileDownloadName = "Fill_PDF_Forms.pdf";
            return fileResult;
        }

        private static void SetValue(PdfForm form, string fieldName, string value)
        {
            if (form.ContainsField(fieldName))
                form.SetValue(fieldName, value ?? string.Empty);
        }

        // Lists the fields of the form with their type and value on a page added at the end of the document
        private void AddFieldsListPage(PdfEditor pdfEditor)
        {
            int pageNumber = pdfEditor.AddPage(PdfPageSize.A4, PdfPageOrientation.Portrait);

            PdfBaseFont baseFont = PdfFontManager.CreateBaseFont(Path.Combine(GetDemoFontsPath(), "DejaVuSerif.ttf"));
            PdfFont titleFont = PdfFontManager.CreateFont(baseFont, 14f, PdfFontStyle.Bold, PdfColor.DarkBlue);
            PdfFont textFont = PdfFontManager.CreateFont(baseFont, 9f, PdfFontStyle.Normal, PdfColor.Black);

            StringBuilder text = new StringBuilder();
            foreach (PdfFormFieldInfo field in pdfEditor.Form.Fields)
            {
                string value = field.Type == PdfFormFieldType.ListBox && field.Values != null && field.Values.Count > 0
                    ? string.Join(", ", field.Values) : field.Value;
                text.Append(field.Name).Append(" (").Append(field.Type).Append(")");
                if (field.ReadOnly)
                    text.Append(" read-only");
                if (field.Required)
                    text.Append(" required");
                text.Append(": ").Append(value ?? string.Empty).Append('\n');
            }

            // The positions are relative to the page in an edited document
            pdfEditor.AddText(pageNumber, new PdfTextElement("Form Fields", titleFont) { X = 40, Y = 40 });
            pdfEditor.AddText(pageNumber, new PdfTextElement(text.ToString(), textFont) { X = 40, Y = 70, Width = 515 });
        }

        private async Task<byte[]> ReadInputPdf(Fill_PDF_Forms_ViewModel model)
        {
            // If an uploaded file exists, use it with priority
            if (model.PdfFile != null && model.PdfFile.Length > 0)
            {
                try
                {
                    using var ms = new MemoryStream();
                    await model.PdfFile.CopyToAsync(ms);
                    return ms.ToArray();
                }
                catch (Exception ex)
                {
                    throw new Exception("Failed to read the uploaded PDF file", ex);
                }
            }

            // Otherwise, fall back to the URL
            string pdfUrl = model.PdfFileUrl?.Trim();
            if (string.IsNullOrWhiteSpace(pdfUrl))
                throw new Exception("No PDF file provided: upload a file or specify a URL");

            try
            {
                if (pdfUrl.StartsWith("file://", StringComparison.OrdinalIgnoreCase))
                {
                    string localPath = new Uri(pdfUrl).LocalPath;
                    return await System.IO.File.ReadAllBytesAsync(localPath);
                }

                using var httpClient = new System.Net.Http.HttpClient();
                return await httpClient.GetByteArrayAsync(pdfUrl);
            }
            catch (Exception ex)
            {
                throw new Exception("Could not download the PDF file from URL", ex);
            }
        }

        private Fill_PDF_Forms_ViewModel SetViewModel()
        {
            var model = new Fill_PDF_Forms_ViewModel();

            HttpRequest request = ControllerContext.HttpContext.Request;
            UriBuilder uriBuilder = new UriBuilder
            {
                Scheme = request.Scheme,
                Host = request.Host.Host,
                Path = request.PathBase.ToString() + request.Path.ToString(),
                Query = request.QueryString.ToString()
            };
            if (request.Host.Port != null)
                uriBuilder.Port = (int)request.Host.Port;

            string currentPageUrl = uriBuilder.Uri.AbsoluteUri;
            string rootUrl = currentPageUrl.Substring(0, currentPageUrl.Length - "Fill_PDF_Forms".Length);

            model.PdfFileUrl = rootUrl + "/DemoAppFiles/Input/PDF_Files/PDF_Form.pdf";

            return model;
        }

        private string GetDemoFilesPath() => m_hostingEnvironment.ContentRootPath + "/wwwroot" + "/DemoAppFiles/Input/";
        private string GetDemoFontsPath() => Path.Combine(GetDemoFilesPath(), "Font_Files");
    }
}
```
