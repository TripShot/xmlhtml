xmlhtml - XML and HTML 5 parsing and rendering
----------------------------------------------

[![Haskell-CI](https://github.com/snapframework/xmlhtml/actions/workflows/haskell-ci.yml/badge.svg)](https://github.com/snapframework/xmlhtml/actions/workflows/haskell-ci.yml)

This library implements both parsers and renderers for XML and HTML 5 document
fragments.  The two share data structures to represent the document tree, so
that you can write code to easily work with either XML or HTML 5.  Convenience
functions are also available to work with the internal data structure in
several natural ways.

Caveats:

- Both parsers are written to parse document fragments, not complete
  documents.  This means that they do not enforce rules about overall
  document structure.  There does not need to be only a single root node,
  and the HTML 5 implementation never inserts any missing start tags.

- The XML parser is incapable of handling processing instructions, or defined
  entities.  If will silently drop processing instructions, and will fail if
  encounters an entity reference for anything by the predefined entities
  (apos, quot, amp, lt, and gt).

- The HTML parser is really an XML parser with HTML 5 quirks mode.  It should
  be just fine for parsing documents that conform to the HTML 5 specification.
  However, it is *not* a compliant HTML 5 parser, as compliant parsers are
  required to be compatible with non-compliant documents in many ways that we
  aren't interested in.  So this is a great basis for a template system, for
  example, but a very poor basis for a web browser or web spider.

To get started, just use the parseHTML or parseXML functions from Text.XmlHtml
to parse a ByteString into a document tree.  On the other side, use render to
write the document tree back to a ByteString.

Working with document trees is easily done in two ways.

1. Text.XmlHtml exports the document tree types (notably, Document and Node)
   and functions like getAttribute, setAttribute, tagName, childNodes, etc. for
   working with them.

2. Text.XmlHtml.Cursor exports a zipper for node forests, which you can use to
   navigate and modify the document tree positionally.

That's it, basically.  This is hopefully a pretty simple package to use.

TO DO Items:

1. Do something better with character encodings.  For now, they are basically
   ignored, and we just use the byte order mark to distinguish between the
   three required encodings.  We should implement the encoding sniffing rules
   for both XML (the <?xml ... ?> declaration) and HTML 5.

2. Benchmark and improve performance of the parsers and renderers.

3. Ensure that rendering always gives an error rather than writing an invalid
   document. (Is this a good idea?  It does limit rendering speed.)
