---
name: evopdf-next-html-loading
description: "Control how EvoPdf Next fetches the HTML: authentication, cookies, custom HTTP headers, GET and POST, the current ASP.NET session, local files, UNC paths, localhost, lazy images, web fonts, redirects and navigation timeout."
---

# EvoPdf Next: loading the HTML page

The web page you want to convert might be protected by different types of authentication. The most common authentication methods are Integrated Windows Authentication, Forms Authentication and custom Login pages. EVO HTML to PDF Converter offers support for resolving all these types of authentication.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`AuthenticationOptions`**: Authentication options allow you to specify a username and password when accessing an HTML page
- **`NameValuePair`**: Represents a name value pair
- **`NameValuePairsCollection`**: Represents a collection of NameValuePair objects
- **`LazyImagesLoadMode`**: This enumeration defines the possible options for the lazy image loading mode

4 types, 10 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Convert HTML Pages with Authentication

### Code Sample - Explicitly Set the Authentication Options

```csharp
// Create the HTML to PDF converter
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();
// Set authentication options
htmlToPdfConverter.AuthenticationOptions.Username = username;
htmlToPdfConverter.AuthenticationOptions.Password = password;            

// Create the HTML to Image converter
HtmlToImageConverter htmlToImageConverter = new HtmlToImageConverter();
// Set authentication options
htmlToImageConverter.AuthenticationOptions.Username = username;
htmlToImageConverter.AuthenticationOptions.Password = password;
```

### Code Sample - Explicitly Set the Forms Authentication Cookie

```csharp
HtmlToPdfConverter htmlToPdfConverter = new HtmlToPdfConverter();

// Add the authentication cookie to request
htmlToPdfConverter.HttpRequestCookies.Add(AuthCookieName, AuthCookieValue);

htmlToPdfConverter.ConvertUrl(urlToConvert);
```

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-pages-with-authentication.htm

## Add Cookies to HTML Page Request

EVO HTML to PDF Converter allows you to add HTTP cookies when you request the HTML page. The HTTP cookies to be used when the HTML page to convert is requested can be added to `HtmlToPdfConverter.HttpRequestCookies` collection.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-cookies-to-html-page-request.htm

## Add HTTP Headers to HTML Page Request

EVO HTML to PDF Converter allows you to add HTTP headers when you request the HTML page. There are various standard HTTP headers offering important information to web server about the capabilities of the browser like the accepted content type, accepted encoding, accepted language, connection mode, user agent.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/add-http-headers-to-html-page-request.htm

## Access a HTML Page Using GET and POST HTTP Methods

EVO HTML to PDF Converter allows you to access a HTML page using both GET and POST HTTP Methods. By default the GET method is used by converter to access the HTML page. When you access the page using GET method you can transmit the parameters in query string. When you access the page using the POST method you transmit the parameters in HTTP request form.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/access-html-pages-with-get-and-post.htm

## Convert a HTML Page to PDF in Same Session

EVO HTML to PDF Converter allows you to convert another page of the same application. The values you filled in the HTML form will be transferred and used in the other HTML page converted to PDF.

### Code Sample - Display Session Variables in Converted HTML Page

```html
@{
    Layout = null;
}

<!DOCTYPE html>

<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <base href="@Url.Content("~/")" />
    <link href="img/evo.ico" rel="shortcut icon" />
    <link href="styles/styles.css" type="text/css" rel="stylesheet">
    <title>Display Session Variables</title>
</head>
<body>
    <div>
        <b style="font-size: 16px">The session variables</b><br />
        <br />
        <b>First Name:</b>&nbsp;<span id="firstNameLabel">@(ViewData["firstName"] != null ? ViewData["firstName"].ToString() : String.Empty)</span><br />
        <b>Last Name:</b>&nbsp;<span id="lastNameLabel">@(ViewData["lastName"] != null ? ViewData["lastName"].ToString() : String.Empty)</span><br />
        <b>Gender:</b>&nbsp;<span id="genderLabel">@(ViewData["gender"] != null && ViewData["gender"].ToString() == "maleRadioButton" ? "Male" : "Female")</span><br />
        <b>I have a car:</b>&nbsp;<span id="haveCarLabel">@(ViewData["haveCar"] != null && ViewData["haveCar"].ToString() != "false" ? "Yes" : "No")</span><br />
        <div id="carTypePanel" style="display : @(ViewData["haveCar"] != null && ViewData["haveCar"].ToString() == "false" ? "none" : "inline")">
            <b>Car Type:</b>&nbsp;<span id="carTypeLabel">@(ViewData["carType"] != null ? ViewData["carType"].ToString() : String.Empty)</span><br />
        </div>
        <b>Comments:</b>&nbsp;<span id="commentsLabel">@(ViewData["comments"] != null ? ViewData["comments"].ToString() : String.Empty)</span><br />
    </div>
</body>
</html>
```

All 2 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-page-to-pdf-in-same-session.htm

## Convert HTML with Web Fonts to PDF

EVO HTML to PDF Converter offers full support for web fonts. The web fonts are referenced in a HTML page using @font-face rules and they don't have to be installed on the computer were the converter runs. The converter will automatically download the fonts and use them in the generated PDF document.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-html-with-web-fonts-to-pdf.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
