# Fonts: complete API surface

Generated from the XML documentation shipped in the EvoPdf Next NuGet packages.
Every public member of the types below is listed; the summaries are the shipped ones.

## PdfFontManager

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFontManager.htm

Provides methods for loading, registering and creating PDF fonts

### Methods

- `CreateBaseFont`: Creates a base font from a font file with optional Unicode range restrictions. When ranges are specified, only the glyphs in those ranges are included in the ToUnicode CMap, reducing file size for fonts with large character sets (e.g. CJK). The result is cached per unique combination of path, subset and unicodeRanges
- `CreateCjkBaseFont`: Creates a standard CJK base font built into Adobe Reader. These fonts do not require embedding - they are available in Adobe Reader when the CJK font pack is installed. Use for form fields that need to remain editable in Adobe Reader with CJK text. Supported names: STSong-Light, STHeiti-Regular (Simplified Chinese), MSung-Light, MHei-Medium (Traditional Chinese), HeiseiMin-W3, HeiseiKakuGo-W5 (Japanese), HYSMyeongJo-Medium, HYGoThic-Medium (Korean)
- `CreateFont`: Creates a font from a font file
- `CreateFont`: Creates a font from a font file using black as the font color
- `CreateFont`: Creates a font from a font file using the normal style
- `CreateFont`: Creates a font from a font file using the normal style and black as the font color
- `CreateFont`: Creates a font from an existing base font
- `CreateFont`: Creates a font from an existing base font using black as the font color
- `CreateFont`: Creates a font from an existing base font using the normal style
- `CreateFont`: Creates a font from an existing base font using the normal style and black as the font color
- `CreateRegisteredFont`: Creates a font from a registered font name or alias
- `CreateRegisteredFont`: Creates a font from a registered font name or alias using black as the font color
- `CreateRegisteredFont`: Creates a font from a registered font name or alias using the normal style
- `CreateRegisteredFont`: Creates a font from a registered font name or alias using the normal style and black as the font color
- `CreateStandardBaseFont`: Creates a standard built-in PDF base font
- `CreateStandardFont`: Creates a standard built-in PDF font
- `CreateStandardFont`: Creates a standard built-in PDF font using black as the font color
- `CreateStandardFont`: Creates a standard built-in PDF font using the normal style
- `CreateStandardFont`: Creates a standard built-in PDF font using the normal style and black as the font color
- `GetRegisteredBaseFont`: Loads a registered base font by name or alias
- `GetRegisteredFamilies`: Gets the names of all registered font families
- `GetRegisteredFonts`: Gets the names of all registered fonts
- `IsFontRegistered`: Determines whether a font name or alias is registered
- `RegisterFont`: Registers a font file
- `RegisterFont`: Registers a font file with an alias
- `RegisterFontDirectory`: Registers all supported font files from a directory
- `RegisterFonts`: Registers multiple font files with aliases
- `RegisterSystemFontDirectories`: Registers fonts from the default system font directories. Subsequent calls are ignored. Registration is performed only once per process lifetime

## PdfFont

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFont.htm

Represents a font used to render text in a PDF document

### Properties

- `BaseFont`: Gets the base font used to create this font
- `Color`: Gets the font color
- `Size`: Gets the font size in points
- `Style`: Gets the font style flags

### Methods

- `Create`: Creates a new font using the same base font with the specified size, style, and color
- `CreateWithColor`: Creates a new font using the same base font, size, and style with a different color
- `CreateWithSize`: Creates a new font using the same base font and color with a different size
- `CreateWithStyle`: Creates a new font using the same base font, size, and color with a different style

## PdfBaseFont

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfBaseFont.htm

Represents a base font that can be used to create styled fonts

### Properties

- `IsSubset`: Gets a value indicating whether the font is written to the PDF as a subset. This is configured at creation time via CreateBaseFont This setting is mainly relevant for embedded fonts loaded from font files. Standard built-in PDF fonts are not embedded

### Methods

- `CreateFont`: Creates a font using this base font
- `CreateFont`: Creates a font using this base font with black as the font color
- `CreateFont`: Creates a font using this base font with the normal style
- `CreateFont`: Creates a font using this base font with the normal style and black as the font color

## PdfStandardFont

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfStandardFont.htm

Represents standard built-in PDF fonts that are always available in PDF readers. These fonts do not require embedding and are useful for small file sizes

### Fields and values

- `Courier`: Standard Courier font
- `CourierBold`: Bold variant of Courier
- `CourierBoldOblique`: Bold and oblique variant of Courier
- `CourierOblique`: Oblique (italic) variant of Courier
- `Helvetica`: Standard Helvetica font
- `HelveticaBold`: Bold variant of Helvetica
- `HelveticaBoldOblique`: Bold and oblique variant of Helvetica
- `HelveticaOblique`: Oblique (italic) variant of Helvetica
- `Symbol`: Symbol font (includes Greek and special characters)
- `TimesBold`: Bold variant of Times Roman
- `TimesBoldItalic`: Bold and italic variant of Times Roman
- `TimesItalic`: Italic variant of Times Roman
- `TimesRoman`: Standard Times Roman font
- `ZapfDingbats`: ZapfDingbats font (includes symbols and icons)

## PdfFontStyle

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfFontStyle.htm

Represents font style flags used in text rendering

### Fields and values

- `Bold`: Bold text
- `Italic`: Italic text
- `Normal`: The default font style
- `Strikeout`: Text with a horizontal line through the middle
- `Underline`: Underlined text

## PdfUnicodeRanges

Reference: https://www.evopdf.com/help/evopdf-next-dotnet/html/T_EvoPdf_Next_PdfUnicodeRanges.htm

Provides predefined Unicode character ranges for use with CreateBaseFont. Each range is an array of two integers representing the inclusive start and end Unicode codepoints

### Properties

- `ChineseSimplified`: Convenience range list covering Simplified Chinese. Includes BasicLatin, CJK Symbols and Punctuation (U+3000-U+303F), and CjkUnifiedIdeographs
- `FullLatin`: Convenience range list covering the full Latin script. Includes BasicLatin, Latin1Supplement, and LatinExtendedA, sufficient for most European languages including Polish, Czech, Hungarian and Romanian with diacritics
- `WesternEuropean`: Convenience range list covering Western European languages. Includes BasicLatin and Latin1Supplement, sufficient for English, French, German, Spanish, Romanian and similar languages

### Fields and values

- `BasicLatin`: Basic Latin block (U+0020-U+007F). Covers English letters, digits and common punctuation
- `CjkUnifiedIdeographs`: CJK Unified Ideographs block (U+4E00-U+9FFF). Covers the most common Chinese, Japanese and Korean characters
- `Cyrillic`: Cyrillic block (U+0400-U+04FF). Covers languages that use the Cyrillic script such as Russian, Bulgarian and Serbian
- `HangulSyllables`: Hangul Syllables block (U+AC00-U+D7AF). Covers precomposed Korean syllable blocks
- `Hiragana`: Hiragana block (U+3040-U+309F). Covers the Japanese Hiragana syllabary
- `Katakana`: Katakana block (U+30A0-U+30FF). Covers the Japanese Katakana syllabary
- `Latin1Supplement`: Latin-1 Supplement block (U+0080-U+00FF). Covers Western European languages such as French, German, Spanish and Romanian
- `LatinExtendedA`: Latin Extended-A block (U+0100-U+017F). Covers Central and Eastern European languages such as Polish, Czech and Hungarian
