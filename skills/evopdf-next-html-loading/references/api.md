# Loading the HTML page: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## AuthenticationOptions

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_AuthenticationOptions.htm

Authentication options allow you to specify a username and password when accessing an HTML page

### Properties

- `Password`: The password used for authentication
- `Username`: The username used for authentication

## NameValuePair

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_NameValuePair.htm

Represents a name value pair

### Properties

- `Name`: Gets or sets the pair name
- `Value`: Gets or sets the pair value

## NameValuePairsCollection

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_NameValuePairsCollection.htm

Represents a collection of NameValuePair objects

### Properties

- `Count`: Gets the number of pairs in collection

### Methods

- `Add`: Adds a name value pair to collection
- `GetByIndex`: Gets the name value pair at the given index in collection
- `GetByName`: Gets the name value pair having a given name

## LazyImagesLoadMode

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_LazyImagesLoadMode.htm

This enumeration defines the possible options for the lazy image loading mode

### Fields and values

- `Browser`: Browser mode
- `Custom`: Custom mode
