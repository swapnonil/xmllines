# Specification
## Definition
An **XML Line Record** is defined as a XML String beginning with an XML element and ending with the same XML Element written on a single line with a single line feed character appended at the end of line to distinguish one XML Line Record from the other.

An **XML Lines File** may contain `1 to n` XML Line Records and will carry a file extension of `.xlr` or any other mutually agreed file extension between the producer and the consumer. However the file extension `.xml` must not be used to denote XML Lines files.
## Format
1. Each XML Line Record must be a well formed XML where the definition of well formed XML is as stated by [W3 here under the specifications of Extensible Markup Language (XML) 1.0 (Fifth Edition)](https://www.w3.org/TR/xml/#sec-well-formed).
2. Each XML Line Record is subject to [stricter restrictions compared to standard XML](https://www.w3.org/TR/xml/#charsets) regarding the characters acceptable in a record. There must be no occurrence of any character that results in a single XML Line Record overflowing into multiple lines. Specifically XML Lines prohibits
 1. Any occurrence of line feed and carriage return character in between elements. E.g. the line feed character between author and date `<author>Name</author>\n<date>21/09/1976</date>` makes this XML Line Record invalid.
 2. Any occurrence of any line feed character inside text content of elements. E.g the line feed characters inside the text contained within the description element `<description>This is a \n multiline text consisting of many \n other lines</description>` makes this XML Line Record invalid.
 3. Any occurrence of any line feed character inside text denoting attribute values. E.g the line feed characters inside the value of the attribute named text for the description element `<description text="This is a \n multiline text consisting of many \n other lines">in-valid</description>` makes this XML Line Record invalid.
 4. Any occurrence of any space in between elements. E.g. the space between author and date `<author>Name</author> <date>23/04/1980</date>` makes this XML Line Record invalid.
 5. Any occurrence of any tab character in between elements. E.g. the tab character between author and date `<author>Name</author>\t<date>23/04/1980</date>` makes this XML Line Record invalid.
3. Each XML Line Record must be separated by a single line feed character.

## Encoding
XML Lines are to be encoded with UTF-8 only and all of the 1,112,064 valid code points in the Unicode Table are allowed, subject to the restrictions placed under the [Format Section](#) of this document. However XML Lines encoded with any other encoding scheme other than UTF-8 can be considered valid XML Lines only when both the sender and receiver agree to use an alternative mutually agreeable encoding scheme.

## Line Feed Character
XML Line Records are to be terminated with a single `\n` character. However `\r\n` are also allowed to denote line feed. This implies that XML Line Records created with Text Editors on Unix, Mac OSX or Windows are all valid. Modern languages such as Java and C# are able to cater for such differences out of the box. However older languages such as C and C++ that use the line feed character of the underlying OS may find portability issues with XML Line Records created in other environments. Tools such as dos2unix are recommended to overcome such differences.

XML Lines Files will  permit the use of a line feed at the end of the last line of the file.

## Character Substitution
Where the text content of any element or any attribute value contains one or more line feed character, those must be substituted with a single space or any other UNICODE character mutually agreed between the producer and consumer.
