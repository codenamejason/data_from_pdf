dataFromPdf
===========
About

A Pure-Python library built as a PDF toolkit. It is capable of:

    extracting document information (title, author, ...),
    splitting documents page by page,
    merging documents page by page,
    cropping pages,
    merging multiple pages into a single page,
    encrypting and decrypting PDF files.

By being Pure-Python, it should run on any Python platform without any dependencies on external libraries. It can also work entirely on StringIO objects rather than file streams, allowing for PDF manipulation in memory. It is therefore a useful tool for websites that manage or manipulate PDFs.
Download Latest

The latest release of pyPdf is version 1.13, released on December 4, 2010. All releases of pyPdf are distributed under the terms of a modified BSD license.

    pyPdf-1.13.tar.gz (src) http://pybrary.net/pyPdf/pyPdf-1.13.tar.gz
    pyPdf-1.13.zip (src) http://pybrary.net/pyPdf/pyPdf-1.13.zip
    pyPdf-1.13.win32.exe (Win32 installer) http://pybrary.net/pyPdf/pyPdf-1.13.win32.exe

Documentation
Documentation of the pyPdf module is available online. This documentation is produced by PythonDoc, and as a result can also be seen integrated with the source code.
Source Code Repository

pyPdf is distributed under the terms of a modified BSD license. The complete source code and history is available through a git repository for anyone who is interested, at http://github.com/mfenniak/pyPdf/tree/trunk. There is also a Python 3.0 compatible branch available at http://github.com/mfenniak/pyPdf/tree/py3.


Example:

    from pyPdf import PdfFileWriter, PdfFileReader

    output = PdfFileWriter()
    input1 = PdfFileReader(file("document1.pdf", "rb"))

    print the title of document1.pdf
    print "title = %s" % (input1.getDocumentInfo().title)

    add page 1 from input1 to output document, unchanged
    output.addPage(input1.getPage(0))

    add page 2 from input1, but rotated clockwise 90 degrees
    output.addPage(input1.getPage(1).rotateClockwise(90))

    add page 3 from input1, rotated the other way:
    output.addPage(input1.getPage(2).rotateCounterClockwise(90))
    alt: output.addPage(input1.getPage(2).rotateClockwise(270))

    add page 4 from input1, but first add a watermark from another pdf:
    page4 = input1.getPage(3)
    watermark = PdfFileReader(file("watermark.pdf", "rb"))
    page4.mergePage(watermark.getPage(0))

    add page 5 from input1, but crop it to half size:
    page5 = input1.getPage(4)
    page5.mediaBox.upperRight = (
    page5.mediaBox.getUpperRight_x() / 2,
    page5.mediaBox.getUpperRight_y() / 2
    )
    output.addPage(page5)

    print how many pages input1 has:
    print "document1.pdf has %s pages." % input1.getNumPages()

    finally, write "output" to document-output.pdf
    outputStream = file("document-output.pdf", "wb")
    output.write(outputStream)
    outputStream.close()

Changelog
Version 1.12, 2008-09-02

    Added support for XMP metadata.
    Fix reading files with xref streams with multiple /Index values.
    Fix extracting content streams that use graphics operators longer than 2 characters. Affects merging PDF files.

Version 1.11, 2008-05-09

    Patch from Hartmut Goebel to permit RectangleObjects to accept NumberObject or FloatObject values.
    PDF compatibility fixes.
    Fix to read object xref stream in correct order.
    Fix for comments inside content streams.

Version 1.10, 2007-10-04

    Text strings from PDF files are returned as Unicode string objects when pyPdf determines that they can be decoded (as UTF-16 strings, or as PDFDocEncoding strings). Unicode objects are also written out when necessary. This means that string objects in pyPdf can be either generic.ByteStringObject instances, or generic.TextStringObject instances.
    The extractText method now returns a unicode string object.
    All document information properties now return unicode string objects. In the event that a document provides docinfo properties that are not decoded by pyPdf, the raw byte strings can be accessed with an "_raw" property (ie. title_raw rather than title)
    generic.DictionaryObject instances have been enhanced to be easier to use. Values coming out of dictionary objects will automatically be de-referenced (.getObject will be called on them), unless accessed by the new "raw_get" method. DictionaryObjects can now only contain PdfObject instances (as keys and values), making it easier to debug where non-PdfObject values (which cannot be written out) are entering dictionaries.
    Support for reading named destinations and outlines in PDF files. Original patch by Ashish Kulkarni.
    Stream compatibility reading enhancements for malformed PDF files.
    Cross reference table reading enhancements for malformed PDF files.
    Encryption documentation.
    Replace some "assert" statements with error raising.
    Minor optimizations to FlateDecode algorithm increase speed when using PNG predictors.

Version 1.9, 2006-12-15

    Fix several serious bugs introduced in version 1.8, caused by a failure to run through our PDF test suite before releasing that version.
    Fix bug in NullObject reading and writing.

Version 1.8, 2006-12-14

    Add support for decryption with the standard PDF security handler. This allows for decrypting PDF files given the proper user or owner password.
    Add support for encryption with the standard PDF security handler.
    Add new pythondoc documentation.
    Fix bug in ASCII85 decode that occurs when whitespace exists inside the two terminating characters of the stream.

Version 1.7, 2006-12-10

    Fix a bug when using a single page object in two PdfFileWriter objects.
    Adjust PyPDF to be tolerant of whitespace characters that don't belong during a stream object.
    Add documentInfo property to PdfFileReader.
    Add numPages property to PdfFileReader.
    Add pages property to PdfFileReader.
    Add extractText function to PdfFileReader.

Version 1.6, 2006-06-06

    Add basic support for comments in PDF files. This allows us to read some ReportLab PDFs that could not be read before.
    Add "auto-repair" for finding xref table at slightly bad locations.
    New StreamObject backend, cleaner and more powerful. Allows the use of stream filters more easily, including compressed streams.
    Add a graphics state push/pop around page merges. Improves quality of page merges when one page's content stream leaves the graphics in an abnormal state.
    Add PageObject.compressContentStreams function, which filters all content streams and compresses them. This will reduce the size of PDF pages, especially after they could have been decompressed in a mergePage operation.
    Support inline images in PDF content streams.
    Add support for using .NET framework compression when zlib is not available. This does not make pyPdf compatible with IronPython, but it is a first step.
    Add support for reading the document information dictionary, and extracting title, author, subject, producer and creator tags.
    Add patch to support NullObject and multiple xref streams, from Bradley Lawrence.

Version 1.5, 2006-01-28

    Fix a bug where merging pages did not work in "no-rename" cases when the second page has an array of content streams.
    Remove some debugging output that should not have been present.

Version 1.4, 2006-01-27

    Add capability to merge pages from multiple PDF files into a single page using the PageObject.mergePage function. See example code (README or web site) for more information.
    Add ability to modify a page's MediaBox, CropBox, BleedBox, TrimBox, and ArtBox properties through PageObject. See example code (README or web site) for more information.
    Refactor pdf.py into multiple files: generic.py (contains objects like NameObject, DictionaryObject), filters.py (contains filter code), utils.py (various). This does not affect importing PdfFileReader or PdfFileWriter.
    Add new decoding functions for standard PDF filters ASCIIHexDecode and ASCII85Decode.
    Change url and download_url to refer to new pybrary.net web site. 

Version 1.3, 2006-01-23

    Fix new bug introduced in 1.2 where PDF files with \r line endings did not work properly anymore. A new test suite developed with various PDF files should prevent regression bugs from now on.
    Fix a bug where inheriting attributes from page nodes did not work.

Version 1.2, 2006-01-23

    Improved support for files with CRLF-based line endings, fixing a common reported problem stating "assertion error: assert line == "%%EOF"".
    Software author/maintainer is now officially a proud married person, which is sure to result in better software... somehow.

Version 1.1, 2006-01-18

    Add capability to rotate pages.
    Improved PDF reading support to properly manage inherited attributes from /Type=/Pages nodes. This means that page groups that are rotated or have different media boxes or whatever will now work properly.
    Added PDF 1.5 support. Namely cross-reference streams and object streams. This release can mangle Adobe's PDFReference16.pdf successfully.

Version 1.0, 2006-01-17

    First distutils-capable true public release. Supports a wide variety of PDF files that I found sitting around on my system.
    Does not support some PDF 1.5 features, such as object streams, cross-reference streams.

[XHTML 1.0 Compliant!] [Valid CSS!]
Jason Romero
The Romero Agency
