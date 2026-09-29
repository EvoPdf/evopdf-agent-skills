# Installation and deployment: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## Installation

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_Installation.htm

Contains global installation instructions that configure the behavior of the library within the application

### Properties

- `ExecutePermissionGranted`: Indicates whether execute permission has been successfully granted.
- `GrantExecutePermissionErrOutput`: The standard error captured from the execute permission grant operation.
- `GrantExecutePermissionStdOutput`: The standard output captured from the execute permission grant operation.
- `InstallScriptErrOutput`: The standard error captured from the install script execution.
- `InstallScriptExecuted`: Indicates whether the install script has been successfully executed.
- `InstallScriptStdOutput`: The standard output captured from the install script execution.
- `RuntimeCopied`: Indicates whether the runtime has been successfully copied.

### Methods

- `ConfigureRuntime`: Enables copying the runtime to a different location and installing dependencies before running the converter. This function is intended for environments where installed dependencies are not persisted after a restart, It must be called before the first HTML conversion, which triggers the configuration setup. Changes made afterward will have no effect until the application is restarted
- `GetRuntimeConfig`: Retrieves the current runtime configuration settings
