# Rendering modes and the persistent engine: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## GlobalSettings

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_GlobalSettings.htm

Contains global settings that configure the behavior of the library within application

### Properties

- `HtmlRendererMode`: Selects how the HTML renderer process is managed for the HTML conversions of the application, including the conversions of the HTML templates used for headers and footers. The value can be changed at any time; each conversion uses the value read at its start. The default value is PersistentProcess. Use ProcessPerConversion when no process may stay resident between conversions or when each conversion must run in its own process.
- `MaxParallelConversions`: Gets or sets the maximum number of HTML pages loaded in the rendering engine at the same time. Every load counts, the HTML templates of headers and footers included. The loads above the limit wait for a free slot. A value of 0 allows an unlimited number of concurrent conversions. If not explicitly set by the application, the default is the number of logical processors of the system, which is the value that gives the highest throughput. Above it the loads contend for the same cores and the throughput falls. A value equal to the number of physical cores gives close to the same throughput with a lower latency per conversion. This property must be set before the first HTML conversion occurs. Changes made afterward will have no effect until the application is restarted.
- `PersistentRenderer`: The process-level settings of the persistent HTML renderer process used with PersistentProcess. A change takes effect at the next conversion, which replaces the running renderer process after it finishes its conversions in progress.
- `PersistentRendererStatus`: Gets the state of the persistent HTML renderer process: whether it runs, its process id, conversions sent and in progress, how many times it was replaced and why and the last start failure. For health checks and diagnostics.

### Methods

- `StartPersistentRenderer`: Starts the persistent HTML renderer process ahead of the first conversion, so that the first conversion does not pay the engine startup. Does nothing if the process is already running. Requires HtmlRendererMode to be PersistentProcess. Typically called at application startup.
- `StartPersistentRendererAsync`: Starts the persistent HTML renderer process ahead of the first conversion. See StartPersistentRenderer.
- `StopPersistentRenderer`: Stops the persistent HTML renderer process if it is running. The renderer finishes the conversions in progress before it stops. A subsequent conversion with PersistentProcess starts a new renderer process.
- `StopPersistentRendererAsync`: Stops the persistent HTML renderer process if it is running, without blocking the calling thread while the conversions in progress finish. See StopPersistentRenderer.

### Events

- `PersistentRendererRestarted`: Raised when the persistent HTML renderer process is replaced by a new one, with the reason (an unexpected exit, a settings change, a recycle rule). Handlers run on the thread pool.

## HtmlRendererMode

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_HtmlRendererMode.htm

How the HTML renderer process is managed for the HTML conversions of the application.

### Fields and values

- `PersistentProcess`: A single renderer process is started at the first conversion (or by StartPersistentRenderer) and reused by the following ones. Each conversion runs in its own browser and its own private context inside that process, so conversions share nothing but the process. The process stops when the application exits, when StopPersistentRenderer is called or after IdleTimeoutSeconds without conversions. This is the default mode.
- `ProcessPerConversion`: A renderer process is started for each conversion and exits when the conversion completes. Nothing stays resident between conversions and each conversion pays the process startup.

## PersistentRendererSettings

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PersistentRendererSettings.htm

Process-level settings of the persistent HTML renderer process used when HtmlRendererMode is PersistentProcess. They apply to all the HTML conversions of the application. A change takes effect at the next conversion: the running renderer process finishes its conversions in progress and is replaced by a new one started with the current settings. A setting enabled here applies to every conversion. A conversion whose converter, HTML template, header or footer enables a setting which is not enabled here fails with an exception.

### Properties

- `AllowInsecureContent`: Allows loading insecure HTTP content in secure HTTPS pages. The default value is false
- `DisableSiteIsolation`: Disables site isolation when loading content from different sources. The default value is false
- `DisableWebSecurity`: Disables web security features such as CORS and same-origin policy. The default value is false
- `EnableElevatedRendering`: Enables the HTML renderer to inherit the elevated privileges of the application using the library when the application is running elevated on Windows. The default value is true
- `EnableSoftwareGpuRendering`: When set to true, hardware GPU rendering is disabled and software rendering is used for both general page rendering and WebGL content. When set to false, hardware GPU rendering is allowed and is further controlled by GpuRenderingEnabled and GpuCompositingEnabled. The default value is false
- `GpuCompositingEnabled`: A flag indicating if hardware GPU compositing is enabled in the HTML renderer process. This property has effect only when EnableSoftwareGpuRendering is false. Hardware WebGL rendering may require both GpuRenderingEnabled and GpuCompositingEnabled to be enabled, depending on system and driver support. The default value is false
- `GpuRenderingEnabled`: A flag indicating if hardware GPU rendering is enabled in the HTML renderer process. This property has effect only when EnableSoftwareGpuRendering is false. The default value is false
- `IdleTimeoutSeconds`: Gets or sets the number of seconds after the last conversion after which an idle persistent renderer process is stopped. The next conversion starts a new process. 0 keeps the process running until the application exits or StopPersistentRenderer is called. The default value is 300 seconds.
- `IgnoreCertificateErrors`: Ignores certificate validation errors such as expired, self-signed or invalid certificates. The default value is false
- `MaxConversions`: Gets or sets the number of conversions after which the persistent renderer process is replaced by a new one. The running conversions finish in the old process. 0 means no limit. The default value is 0.
- `MaxLifetimeMinutes`: Gets or sets the number of minutes after which the persistent renderer process is replaced by a new one. The running conversions finish in the old process. 0 means no limit. The default value is 0.
- `RetryAfterUnexpectedExit`: Gets or sets a value indicating whether a conversion in progress when the persistent renderer process exits unexpectedly is retried once on a new process. When false, the conversion fails with an exception naming the exit code. The retry repeats the request; a page requested with HTTP POST fields is posted again. The default value is true.
- `StartTimeoutSeconds`: Gets or sets the number of seconds the library waits for the persistent renderer process to start before it gives up and fails the conversion. The first start on a machine reads the engine binaries from disk and can take much longer than the following ones; on a slow disk, such as the consumption plans of the cloud providers, it can exceed a minute. The default value is 90 seconds.

## PersistentRendererStatus

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PersistentRendererStatus.htm

The state of the persistent HTML renderer process, for health checks and diagnostics.

### Properties

- `ConversionsInFlight`: Conversions in progress in the running process.
- `ConversionsSent`: Conversions sent to the running process since it started.
- `IsRunning`: True while a persistent renderer process is running and accepting conversions.
- `LastError`: The message of the last failure to start a process, or null after a successful start.
- `LastRestartReason`: The reason of the last replacement or stop.
- `ProcessId`: The id of the running process, or 0.
- `Restarts`: How many times the process was replaced since the application started.
- `StartedAt`: When the running process was started (local time).

## PersistentRendererRestartReason

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PersistentRendererRestartReason.htm

The reason a persistent HTML renderer process was replaced or stopped.

### Fields and values

- `Idle`: The process was stopped after IdleTimeoutSeconds without conversions.
- `MaxConversions`: The process served MaxConversions conversions.
- `MaxLifetime`: The process reached MaxLifetimeMinutes.
- `None`: No restart happened yet.
- `ProcessExited`: The process exited unexpectedly; the conversions in flight were retried or failed.
- `SettingsChanged`: A setting of PersistentRenderer or MaxParallelConversions changed.

## PersistentRendererRestartedEventArgs

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PersistentRendererRestartedEventArgs.htm

Data of the PersistentRendererRestarted event.

### Properties

- `ProcessId`: The id of the new process.
- `Reason`: Why the previous process was replaced.
