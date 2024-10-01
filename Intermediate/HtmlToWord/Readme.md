## HTML Plugin

Inserting HTML into docx.

Using external library for converting HTML into DOCX format.

Using *XElement* to paste raw XML into Templater.

Alternative method of using special Word feature of document embedding.

### Special data type - XElement

Templater supports XElement and collection of XElements as raw input. This means it will be injected into Word as-is instead of first translated into string value (or some special value such as Image).
This allows customization such as dynamically specifying color of text element, inserting list or various other ones supported as simple XML insert.

By default Templater will inject XElement after the specified tag into a new paragraph (or other appropriate level).
This will leave old paragraph empty, which might lead to whitespace bloat. To deal with those scenario, Templater has few builtin metadata keywords:

 * remove-old-xml - which will remove previous paragraph after XML is inserted
 * replace-xml - which will insert XML at the place of the paragraph (instead of after it)
 * merge-xml - which will merge provided XML into the found structure

To ease the usage, instead of specifying this through tag metadata, they can be sent in XML via templater-xml attribute, e.g. templater-xml="merge-xml"

Its common to adjust the XML before inserting, especially when dealing with HTML, which can be used to:

  * rewrite hyperlinks into alternative form
  * keep style consistent with the document

### Document merging
With version 6.1 Templater also supports document embedding in Word. This can be used for document merging, HTML/RTF import and similar purposes. To import embedded document, special type must be used:

  * System.IO.FileInfo in C#
  * java.io.File in Java

This works for non-trivial cases, as images, tables and other objects can be imported this way. When replacing such tag, Templater will check if previous tag location would leave paragraph empty. In that case it will remove old paragraph to not leave whitespace around.

Templater will recognize tags inside embedded documents, so this will work seamlessly in various use cases.

#### Special support for HTML extensions

By default embedding is added as a new paragraph after the tag location. In cases when HTML is embedded, Templater will try to add it at the tag location,
which enables its uses as tag inside a table and regular resize behavior.

### Plugin example

String is usually pasted as-is into the document (except in case when it contains newlines, in which case it must be slightly modified during insert). This example shows how metadata can be used to specify string -> XElement conversion. For this external HTML -> DOCX library is used.

Templater doesn't have built-in HTML -> DOCX conversion, but this example shows off how this can be done with external library and custom plugins.

As of v2.5 instead of custom metadata (simple-html/custom-html) custom types can be used with the appropriate low level replace plugin.