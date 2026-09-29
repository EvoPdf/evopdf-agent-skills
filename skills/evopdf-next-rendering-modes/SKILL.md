---
name: evopdf-next-rendering-modes
description: "Control the Chromium rendering process of EvoPdf Next: PersistentProcess (the default) versus ProcessPerConversion, GlobalSettings.PersistentRenderer settings, warm start, idle and lifetime recycling, MaxParallelConversions, restart telemetry and health checks. Use for throughput, memory and long-running service questions."
---

# EvoPdf Next: rendering modes and the persistent engine

The HTML to PDF and HTML to Image converters render the HTML pages in a separate process, the HTML rendering engine, which is a headless Chromium browser shipped with the library. The library manages this process for the application and offers two ways of doing it, called rendering modes. The mode is a global setting of the library and applies to every HTML conversion of the application, including the conversions of the HTML templates used for headers and footers.

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`GlobalSettings`**: Contains global settings that configure the behavior of the library within application
- **`HtmlRendererMode`**: How the HTML renderer process is managed for the HTML conversions of the application.
- **`PersistentRendererSettings`**: Process-level settings of the persistent HTML renderer process used when HtmlRendererMode is PersistentProcess. They apply to all the HTML conversions...
- **`PersistentRendererStatus`**: The state of the persistent HTML renderer process, for health checks and diagnostics.
- **`PersistentRendererRestartReason`**: The reason a persistent HTML renderer process was replaced or stopped.
- **`PersistentRendererRestartedEventArgs`**: Data of the PersistentRendererRestarted event.

6 types, 40 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## HTML to PDF Rendering Modes and the Persistent Rendering Engine

### The Two Rendering Modes

```csharp
// The default: one rendering engine process for the application
GlobalSettings.HtmlRendererMode = HtmlRendererMode.PersistentProcess;

// A rendering engine process for each conversion
GlobalSettings.HtmlRendererMode = HtmlRendererMode.ProcessPerConversion;
```

### Monitoring the Persistent Process

```csharp
GlobalSettings.PersistentRendererRestarted += (sender, e) =>
    logger.LogWarning("HTML rendering engine replaced: {Reason}, new process {ProcessId}", e.Reason, e.ProcessId);

var status = GlobalSettings.PersistentRendererStatus;
if (status.IsRunning)
    logger.LogInformation("Engine process {ProcessId}: {Sent} conversions sent, {InFlight} in progress",
        status.ProcessId, status.ConversionsSent, status.ConversionsInFlight);
```

### Using the Persistent Process in ASP.NET Core

```csharp
var app = builder.Build();

app.Lifetime.ApplicationStarted.Register(() => GlobalSettings.StartPersistentRenderer());
app.Lifetime.ApplicationStopping.Register(() => GlobalSettings.StopPersistentRenderer());
```

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/html-to-pdf-rendering-modes.htm

## Convert Multiple HTML Pages to PDF in Parallel

EvoPdf Next can convert multiple HTML documents to PDF in parallel tasks using the asynchronous HTML to PDF methods such as `HtmlToPdfConverter.ConvertUrlAsync` and merge the resulted PDFs using the `PdfMerge.AddPdf` method of `PdfMerge` class. The PdfMerge class allows you to set security options, configure PDF viewer preferences and apply a digital signature to the resulting PDF.

All 1 samples of this topic, complete: `references/examples.md`.

Full topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/convert-multiple-html-to-pdf-in-parallel.htm

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.

## Runnable code

Compilable versions of the samples above: https://github.com/EvoPdf/evopdf-next-samples/tree/main/docs-samples
