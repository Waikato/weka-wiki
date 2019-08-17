

# File
`weka/gui/scripting/Groovy.props`

# Description
This props file determines the look and feel of the minimalistic scripting IDE for [Groovy](https://groovy-lang.org/).

# Version
* > 3.5.8 or [snapshot](snapshots.md) later than 6/3/2009 (only developer version)

# Fields
* `FontName`
> Specifies the name of the font for displaying the code.
> default: 
monospaced
* `FontSize`
> The font size.
> default: 
12
* `ForegroundColor`
> The color of the font (if not comment or keyword). Can take R,G,B format.
> default: 
black
* `BackgroundColor`
> The background color. Can take R,G,B format.
> default: 
white
* `KeywordColor`
> The color for keywords (see list in field *Keywords*). Can take R,G,B format.
> default: 
blue
* `CommentColor`
> The color for comments (single-line and multi-line). Can take R,G,B format.
> default: 
gray
* `StringColor`
> The color for strings (enclosed in single or double quotes). Can take R,G,B format.
> default: 
red
* `Syntax`
> Whether syntax highlighting is turned on or not (true|false)
> default: 
true
* `Indentation`
> The number of spaces to use for indentation.
> default: 
2
* `Tabs`
> The number of spaces a tab represents.
> default: 
2
* `UseBlanks`
> Whether to use blanks instead of tabs (true|false).
> default: 
true
* `Delimiters`
> The characters that define word limits.
> default: 
;:{}()[]+-/%<=>!&|^~*
* `QuoteDelimiters`
> The characters that enclose a string.
> default: 
"'
* `QuoteEscape`
> The character to escape the `QuoteDelimiters` with.
> default: 
\ (backslash)
* `MultiLineComment`
> Whether to enable multi-line comments (true|false).
> default: 
true
* `MultiLineCommentStart`
> The character sequence that starts a multi-line comment.
> default: 
/*
* `MultiLineCommentEnd`
> The character sequence that ends a multi-line comment.
> default: 
*/
* `SingleLineCommentStart`
> The character sequence that starts a single-line comment.
> default: 
*
* `AddMatchingBlockEnd`
> Whether to automatically add matching block end character sequences while typing (true|false).
> default: 
true
* `BlockStart`
> The character sequence that starts a block.
> default: 
{
* `BlockEndd`
> The character sequence that ends a block.
> default: 
}
* `Keywords`
> Comma-separated list of keywords to highlight.

# Notes
* Recognized **color names**
	* black
	* blue
	* cyan
	* darkGray
	* gray
	* green
	* lightGray
	* magenta
	* orange
	* pink
	* red
	* white
	* yellow
* **R,G,B format**
> The RGB format is a comma-separated list three integer values (values ranging from 0-255) for RED, GREEN and BLUE.

# See also
* [Properties File](properties_file.md)
* [Using Weka from Groovy](using_weka_from_groovy.md)

# Links
* [Groovy homepage](https://groovy-lang.org/)
