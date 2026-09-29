---
name: evopdf-next-untrusted-html
description: "Harden EvoPdf Next when the HTML comes from users: BlockedHosts, LocalFilesEnabled, LocalhostEnabled, UncPathEnabled, MaxJsRedirects, NavigationTimeout, PageNumberLimit, JavaScriptEnabled, and why DisableWebSecurity, DisableSiteIsolation, IgnoreCertificateErrors, AllowInsecureContent and EnableElevatedRendering must stay off. Use for SSRF and resource-limit questions."
---

# EvoPdf Next: converting untrusted HTML safely

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## The threat model

The rendering engine is a real Chromium. Given a URL or an HTML string from a user, it will
follow links, resolve `file://` references, reach internal hosts and run JavaScript. The settings
below are properties of `HtmlToPdfConverter` and `HtmlToImageConverter`, and of `PdfHtmlTemplate` for
headers, footers and stamps. The five browser security settings of the second table also exist in
`GlobalSettings.PersistentRenderer`, for the shared engine process.

## Settings that reduce the attack surface

```csharp
var converter = new HtmlToPdfConverter();

// Refuse requests to hosts the document must never reach, each written as it appears in the URLs
converter.BlockedHosts.Add("169.254.169.254");
converter.BlockedHosts.Add("metadata.google.internal");

// Keep the renderer away from the file system and from the machine itself
converter.LocalFilesEnabled = false;
converter.LocalhostEnabled = false;
converter.UncPathEnabled = false;
converter.EnableElevatedRendering = false;

// Bound the work a single document can cause
converter.NavigationTimeout = 20;
converter.ConversionDelay = 0;
converter.MaxJsRedirects = 2;
converter.PdfDocumentOptions.PageNumberLimit = 200;

byte[] pdf = converter.ConvertHtml(untrustedHtml, null);
```

Passing `null` as the base URL keeps relative references from resolving against a host the
caller chose. An entry of `BlockedHosts` also blocks the subdomains of the host. Hosts are compared
by name, so blocking an IP address does not block the host names that resolve to it.

## Settings to keep off for untrusted content

These exist for controlled, first-party pages. Each one removes a browser protection or gives the
document access it does not need.

| Setting | Default | What it gives up |
| --- | --- | --- |
| `DisableWebSecurity` | false | same-origin policy and CORS |
| `DisableSiteIsolation` | false | process isolation between origins |
| `IgnoreCertificateErrors` | false | TLS validation |
| `AllowInsecureContent` | false | mixed content blocking |
| `EnableElevatedRendering` | true | on Windows, the elevated privileges of an application running elevated |
| `LocalFilesEnabled` | true | access to local files |
| `LocalhostEnabled` | true | access to the machine itself |
| `UncPathEnabled` | true | access to network shares |

## Process isolation

`GlobalSettings.HtmlRendererMode = HtmlRendererMode.ProcessPerConversion` gives each conversion
its own operating system process, so a crash or a hang caused by one document cannot affect
another. It costs the engine startup on every conversion. For untrusted input that trade is
usually worth making; see the rendering modes skill for the performance difference.

Under `PersistentProcess` each conversion still runs in its own browser and its own private
context with its own cookies, cache and storage, so conversions do not share state, only the
process.

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.
