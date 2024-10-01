## Double processing

Very complex documents can require multi-step processing. 
Templater still has few rough edges and this examples shows how to work around them. 

### Horizontal resize

While resize(n) will duplicate context vertically, sometimes in Excel it's often useful to replicate context horizontally (and dynamic resize is not really an appropriate solution).
In Templater this can be done with **:horizontal-resize** metadata which instructs low level API to instead of:

 * pushdown - does a push to the right
 * vertical resize - does a horizontal resize

Push to the right will work on the context detected by the tags, this can be overridden with **:whole-column** metadata
Horizontal resize will duplicate content to the right, as opposed to the bottom.

Often after horizontal resize document needs to be reanalyzed to help Templater copy with changes.

If 0 is used during horizontal resize and affected region include whole column, Templater will perform pull to the left by hiding the relevant columns.
Whole column can be defined in multiple ways:

 * by using named range which spans the whole column
 * via builtin **whole-column** metadata
 * when tags which are getting resized are located on both starting and ending row

### Formula rewriting

Templater will analyze and rewrite formula definitions. This allows for creating very generic templates which when expanded fit into place.
Templater doesn't evaluate formulas.

### Conversion of text to formula

Templater has an option to convert text input into formula (mostly due to Excel objecting to `{{tag}}`, `<<tag>>` and `[[tag]]` inside formulas).
This is done by placing `[[equals]]` text at the beginning of the cell.
After processing is done Templater converts all remaining cells which starts with `[[equals]]` into formulas.

In this example due to double processing, `[[equals]]` can't be used into the template since it will be rewritten before values are available.
Therefore another tag (`[[formula]]` is used, and at the end of first processing `[[formula]]` is converted into `[[equals]]` since:

 * Templater doesn't analyze values after replace (only after resize)
 * second processing will pick up inserted tag and will do formula replacement at the end

### Merge cell stretching with drawings

Merge cell require at least two cells to be defined. 
When drawing is placed inside merge cell, it will be stretched with the merge cell. If this is not expected behavior, Lock aspect ratio option can be checked.
Template contains tag `[[Groups.Definition]]` within shape, which is across two cells so that it's not duplicated on nested resize, but rather stretched.