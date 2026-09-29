---
name: evopdf-next-fonts
description: "Work with fonts in EvoPdf Next PDF documents: the 14 standard PDF fonts, TrueType and OpenType font registration from files and directories, system font directories, CJK base fonts, font styles, and Unicode range subsetting."
---

# EvoPdf Next: fonts

Namespace `EvoPdf.Next`. Install one NuGet package for the target platform, for example
`EvoPdf.Next.Windows`, `EvoPdf.Next.Linux` or `EvoPdf.Next.MacOS`.

## Types covered here

- **`PdfFontManager`**: Provides methods for loading, registering and creating PDF fonts
- **`PdfFont`**: Represents a font used to render text in a PDF document
- **`PdfBaseFont`**: Represents a base font that can be used to create styled fonts
- **`PdfStandardFont`**: Represents standard built-in PDF fonts that are always available in PDF readers. These fonts do not require embedding and are useful for small file si...
- **`PdfFontStyle`**: Represents font style flags used in text rendering
- **`PdfUnicodeRanges`**: Provides predefined Unicode character ranges for use with CreateBaseFont. Each range is an array of two integers representing the inclusive start and ...

6 types, 71 public members. The complete member list with the shipped
summaries is in `references/api.md`; do not guess member names that are not there.

## Three kinds of font

**Standard fonts** are the 14 fonts every PDF reader has. They are not embedded, so they cost
nothing in file size.

```csharp
PdfFont font = PdfFontManager.CreateStandardFont(PdfStandardFont.Helvetica, 12f);
var title = new PdfTextElement("Report", font) { X = 20, Y = 20 };
```

**Registered fonts** are TrueType or OpenType files you supply. Register once, then create fonts
by family name.

```csharp
PdfFontManager.RegisterFont("fonts/Inter-Regular.ttf");
PdfFontManager.RegisterFontDirectory("fonts");
PdfFontManager.RegisterSystemFontDirectories();

PdfFont body = PdfFontManager.CreateRegisteredFont("Inter", 11f, PdfFontStyle.Normal);
```

`PdfFontManager.IsFontRegistered`, `GetRegisteredFamilies()` and `GetRegisteredFonts()` tell you
what is available at run time, which matters in containers where the host has no fonts installed.

**CJK base fonts** cover Chinese, Japanese and Korean.

```csharp
PdfBaseFont cjkBase = PdfFontManager.CreateCjkBaseFont("STSong-Light", "UniGB-UCS2-H");
PdfFont cjk = PdfFontManager.CreateFont(cjkBase, 12f);
```

## Subsetting

`PdfFontManager.CreateBaseFont(path, subset, unicodeRanges)` loads a font file. With `subset` set,
only the glyphs used in the document are written; editable form fields need the full font.
`PdfUnicodeRanges` holds predefined ranges for `unicodeRanges`, which limits the ToUnicode map of
fonts with large character sets such as CJK.

## Fonts in HTML

Fonts referenced by HTML come from the fonts installed on the machine or from `@font-face` rules in
the page, as in a browser; they do not come from `PdfFontManager`.

`PdfFontManager` applies to the Core PDF API: text you draw yourself with `PdfTextElement` into a
`PdfDocument`, a `PdfEditor` or a `PdfTemplate`.

## Rules that apply to every sample here

- Converter instances are single use. Create a new converter for every conversion; a second
  call on the same instance throws.
- `Licensing.LicenseKey` is a static field, assigned once per process before any conversion.
- Every conversion method has an asynchronous variant ending in `Async` that takes a `CancellationToken`.
