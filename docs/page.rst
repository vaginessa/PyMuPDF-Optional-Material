.. _Page:

================
Page
================

Class representing a document page. A page object is created by :meth:`Document.loadPage` or, equivalently, via indexing the document like ``doc[n]``) - it has no independent constructor.

There is a parent-child relationship between a document and its pages. If the document is closed or deleted, all page objects (and their respective children, too) in existence will become unusable. If a page property or method is being used, an exception is raised saying that the page object is "orphaned".

Several page methods have a :ref:`Document` counterpart for convenience. At the end of this chapter you will find a synopsis.

Methods ``insertText()``, ``insertTextbox()`` and ``draw*()`` are for PDF pages only. They provide "stand-alone" shortcut versions for the same-named methods of the :ref:`Shape` class. For detailed descriptions have a look in that chapter.

In contrast to :ref:`Shape`, the results of page methods are not interconnected: they do not share properties like colors, line width / dashing, morphing, etc.

Each page ``draw*()`` method invokes a :meth:`Shape.finish` and then a :meth:`Shape.commit` and consequently accepts the combined arguments of both these methods.

Text insertion methods (``insertText()`` and ``insertTextbox()``) do not need :meth:`Shape.finish` and therefore only invoke :meth:`Shape.commit`.

========================== =========================================
**Method / Attribute**     **Short Description**
========================== =========================================
:meth:`Page.bound`         rectangle (mediabox) of the page
:meth:`Page.deleteAnnot`   PDF only: delete an annotation
:meth:`Page.deleteLink`    PDF only: delete a link
:meth:`Page.drawBezier`    PDF only: draw a cubic Bézier curve
:meth:`Page.drawCircle`    PDF only: draw a circle
:meth:`Page.drawCurve`     PDF only: draw a special Bézier curve
:meth:`Page.drawLine`      PDF only: draw a line
:meth:`Page.drawOval`      PDF only: draw an oval / ellipse
:meth:`Page.drawPolyline`  PDF only: connect a point sequence
:meth:`Page.drawRect`      PDF only: draw a rectangle
:meth:`Page.drawSector`    PDF only: draw a circular sector
:meth:`Page.drawSquiggle`  PDF only: draw a squiggly line
:meth:`Page.drawZigzag`    PDF only: draw a zig-zagged line
:meth:`Page.getFontList`   PDF only: get list of used fonts
:meth:`Page.getImageList`  PDF only: get list of used images
:meth:`Page.getLinks`      get all links
:meth:`Page.getPixmap`     create a :ref:`Pixmap`
:meth:`Page.getText`       extract the page's text
:meth:`Page.insertImage`   PDF only: insert an image
:meth:`Page.insertLink`    PDF only: insert a new link
:meth:`Page.insertText`    PDF only: insert text
:meth:`Page.insertTextbox` PDF only: insert a text box
:meth:`Page.loadLinks`     return the first link on a page
:meth:`Page.newShape`      PDF only: start a new :ref:`Shape`
:meth:`Page.searchFor`     search for a string
:meth:`Page.setRotation`   PDF only: set page rotation
:meth:`Page.updateLink`    PDF only: modify a link
:attr:`Page.firstAnnot`    first :ref:`Annot` on the page
:attr:`Page.firstLink`     first :ref:`Link` on the page
:attr:`Page.number`        page number
:attr:`Page.parent`        owning document object
:attr:`Page.rect`          rectangle (mediabox) of the page
:attr:`Page.rotation`      PDF only: page rotation
========================== =========================================

**Class API**

.. class:: Page

   .. method:: bound()

      Determine the rectangle ("mediabox", before transformation) of the page.

      :rtype: :ref:`Rect`

   .. method:: deleteAnnot(annot)

      PDF only: Delete the specified annotation from the page and (for all document types) return the next one.

      :arg annot: the annotation to be deleted.
      :type annot: :ref:`Annot`

      :rtype: :ref:`Annot`
      :returns: the next annotation of the deleted one.

   .. method:: deleteLink(linkdict)

      PDF only: Delete the specified link from the page. The parameter must be a dictionary of format as provided by the ``getLinks()`` method (see below).

      :arg dict linkdict: the link to be deleted.

   .. method:: insertLink(linkdict)

      PDF only: Insert a new link on this page. The parameter must be a dictionary of format as provided by the ``getLinks()`` method (see below).

      :arg dict linkdict: the link to be inserted.

   .. method:: updateLink(linkdict)

      PDF only: Modify the specified link. The parameter must be a dictionary of format as provided by the ``getLinks()`` method (see below).

      :arg dict linkdict: the link to be modified.

   .. method:: getLinks()

      Retrieves **all** links of a page.

      :rtype: list
      :returns: A list of dictionaries. The entries are in the order as specified during PDF generation. For a description of the dictionary entries see below. Always use this method if you intend to make changes to the links of a page.

   .. method:: insertText(point, text = text, fontsize = 11, fontname = "Helvetica", fontfile = None, color = (0, 0, 0), rotate = 0, morph = None, overlay = True)

      PDF only: Insert text.


   .. method:: insertTextbox(rect, buffer, fontsize = 11, fontname = "Helvetica", fontfile = None, color = (0, 0, 0), expandtabs = 8, align = TEXT_ALIGN_LEFT, charwidths = None, rotate = 0, morph = None, overlay = True)

      PDF only: Insert text into the specified rectangle.

   .. method:: drawLine(p1, p2, color = (0, 0, 0), width = 1, dashes = None, roundCap = True, overlay = True, morph = None)

      PDF only: Draw a line from :ref:`Point` objects ``p1`` to ``p2``.

   .. method:: drawZigzag(p1, p2, breadth = 2, color = (0, 0, 0), width = 1, dashes = None, roundCap = True, overlay = True, morph = None)

      PDF only: Draw a zigzag line from :ref:`Point` objects ``p1`` to ``p2``.

   .. method:: drawSquiggle(p1, p2, breadth = 2, color = (0, 0, 0), width = 1, dashes = None, roundCap = True, overlay = True, morph = None)

      PDF only: Draw a squiggly (wavy, undulated) line from :ref:`Point` objects ``p1`` to ``p2``.

   .. method:: drawCircle(center, radius, color = (0, 0, 0), fill = None, width = 1, dashes = None, roundCap = True, overlay = True, morph = None)

      PDF only: Draw a circle around ``center`` with a radius of ``radius``.

   .. method:: drawOval(rect, color = (0, 0, 0), fill = None, width = 1, dashes = None, roundCap = True, overlay = True, morph = None)

      PDF only: Draw an oval (ellipse) within the given rectangle.

   .. method:: drawSector(center, point, angle, color = (0, 0, 0), fill = None, width = 1, dashes = None, roundCap = True, fullSector = True, overlay = True, closePath = False, morph = None)

      PDF only: Draw a circular sector, optionally connecting the arc to the circle's center (like a piece of pie).

   .. method:: drawPolyline(points, color = (0, 0, 0), fill = None, width = 1, dashes = None, roundCap = True, overlay = True, closePath = False, morph = None)

      PDF only: Draw several connected lines defined by a sequence of points.


   .. method:: drawBezier(p1, p2, p3, p4, color = (0, 0, 0), fill = None, width = 1, dashes = None, roundCap = True, overlay = True, closePath = False, morph = None)

      PDF only: Draw a cubic Bézier curve from ``p1`` to ``p4`` with the control points ``p2`` and ``p3``.

   .. method:: drawCurve(p1, p2, p3, color = (0, 0, 0), fill = None, width = 1, dashes = None, roundCap = True, overlay = True, closePath = False, morph = None)

      PDF only: This is a special case of ``drawBezier()``.


   .. method:: drawRect(rect, color = (0, 0, 0), fill = None, width = 1, dashes = None, roundCap = True, overlay = True, morph = None)

      PDF only: Draw a rectangle.

      .. note:: An efficient way to background-color a PDF page with the old Python paper color is ``page.drawRect(page.rect, color = py_color, fill = py_color, overlay = False)``, where ``py_color = getColor("py_color")``.

   .. method:: insertImage(rect, filename = None, pixmap = None, overlay = True)

      PDF only: Fill the given rectangle with an image. Width and height need not have the same proportions as the image: it will be adjusted to fit. The image is either taken from a pixmap or from a file - **exactly one** of these parameters **must** be specified.

      :arg rect: where to put the image on the page. ``rect`` must be finite, not empty and be completely contained in the page's rectangle.
      :type rect: :ref:`Rect`

      :arg str filename: name of an image file (all MuPDF supported formats - see :ref:`Pixmap` chapter).

      :arg pixmap: pixmap containing the image. When inserting the same image multiple times, this should be the preferred option, because the overhead of opening the image and decompressing its content will occur every time with the filename option.
      :type pixmap: :ref:`Pixmap`

      For a description of the other parameters see :ref:`CommonParms`.

      :returns: zero

      This example puts the same image on every page of a document:

      >>> doc = fitz.open(...)
      >>> rect = fitz.Rect(0, 0, 50, 50)   # put thumbnail in upper left corner
      >>> pix = fitz.Pixmap("some.jpg")    # an image file
      >>> for page in doc:
              page.insertImage(rect, pixmap = pix)
      >>> doc.save(...)

      Notes:
      
      1. If that same image had already been present in the PDF, then only a reference will be inserted. This of course considerably saves disk space and processing time. But to detect this fact, existing PDF images need to be compared with the new one. This is achieved by storing an MD5 code for each image in a table and only compare the new image's code against its entries. Generating this MD5 table, however, is done only when triggered by the first image insertion - which therefore may have an extended response time.

      2. You can use this method to provide a background image for the page, like a copyright, a watermark or a background color. Or you can combine it with ``searchFor()`` to achieve a textmarker effect.

      3. The image may be inserted uncompressed, e.g. if a ``Pixmap`` is used or if the image has an alpha channel. Therefore, consider using ``deflate = True`` when saving the file.

   .. method:: getText(output = 'text')

      Retrieves the text of a page. Depending on the output parameter, the results of the :ref:`TextPage` extract methods are returned.

      If ``'text'`` is specified, plain text is returned in the order as specified during PDF creation (which is not necessarily the normal reading order). As this may not always look as expected, consider using the example program ``PDF2TextJS.py``. It is based on ``output = 'json'`` (``= TextPage.extractJSON()``) and re-arranges text according to the Western reading layout convention "from top-left to bottom-right".

      :arg str output: A string indicating the requested text format, one of ``text`` (default), ``html``, ``json``, or ``xml``.

      :rtype: string
      :returns: The page's text as one string.

   .. method:: getFontList()

      PDF only: Return a list of fonts referenced by the page. Same as :meth:`Document.getPageFontList`.

   .. method:: getImageList()

      PDF only: Return a list of images referenced by the page. Same as :meth:`Document.getPageImageList`.

   .. method:: getPixmap(matrix = fitz.Identity, colorspace = "RGB", clip = None, alpha = True)

     Creates a Pixmap from the page. This is probably the most often used method to create pixmaps.

     :arg matrix: A :ref:`Matrix` object. Default is the :ref:`Identity` matrix.
     :type matrix: :ref:`Matrix`

     :arg colorspace: Defines the required colorspace, one of ``GRAY``, ``RGB`` (default) or ``CMYK`` (case insensitive). Alternatively specify a :ref:`Colorspace`, conveniently one of the predefined ones (:data:`csGRAY`, :data:`csRGB` or :data:`csCMYK`).
     :type colorspace: string, :ref:`Colorspace`

     :arg clip: An ``Irect`` to restrict rendering of the page to the rectangle's area. If not specified, the complete page will be rendered.
     :type clip: :ref:`IRect`

     :arg bool alpha: A bool indicating whether an alpha channel should be included in the pixmap. Choose ``False`` if you do not really need transparency. This will save a lot of memory (25% in case of RGB ... and pixmaps are typically **large**!), and also processing time in most cases. Also note an important difference in how the pixmap will be allocated:

        * ``True``: the pixmap will be cleared with ``0x00``, including the alpha byte. This will result in **transparent** areas where the page is empty (i.e. no text, no image).

        .. image:: alpha-1.png

        * ``False``: the pixmap will be cleared with ``0xff``. This will result in **white** where the page has nothing to show.

        .. image:: alpha-0.png

     :rtype: :ref:`Pixmap`
     :returns: Pixmap of the page.

   .. method:: loadLinks()

      Return the first link on a page. Synonym of property ``firstLink``.

      :rtype: :ref:`Link`
      :returns: first link on the page (or ``None``).

   .. method:: setRotation(rot)

      PDF only: Sets the rotation of the page.

      :arg int rot: An integer specifying the required rotation in degrees. Should be a (positive or negative) multiple of 90.

      :returns: zero if successfull, ``-1`` if not a PDF.

   .. method:: newShape()

      PDF only: Create a new :ref:`Shape` object for the page.

      :rtype: :ref:`Shape`
      :returns: a new :ref:`Shape` to use for compound drawings. See description there.

   .. method:: setRotation(rot)

      PDF only: Sets the rotation of the page.

      :arg int rot: An integer specifying the required rotation in degrees. Should be a (positive or negative) multiple of 90.

      :returns: zero if successfull, ``-1`` if not a PDF.

   .. method:: searchFor(text, hit_max = 16)

      Searches for ``text`` on a page. Identical to :meth:`TextPage.search`.

      :arg str text: Text to searched for. Upper / lower case is ignored.

      :arg int hit_max: Maximum number of occurrences accepted.

      :rtype: list

      :returns: A list of :ref:`Rect` rectangles each of which surrounds one occurrence of ``text``.

   .. attribute:: rotation

      PDF only: contains the rotation of the page in degrees and ``-1`` for other document types.

      :type: int

   .. attribute:: firstLink

      Contains the first :ref:`Link` of a page (or ``None``).

      :type: :ref:`Link`

   .. attribute:: firstAnnot

      Contains the first :ref:`Annot` of a page (or ``None``).

      :type: :ref:`Annot`

   .. attribute:: number

      The page number.

      :type: int

   .. attribute:: parent

      The owning document object.

      :type: :ref:`Document`


   .. attribute:: rect

      Contains the rectangle ("mediabox", before transformation) of the page. Same as result of method ``bound()``.

      :type: :ref:`Rect`

-----

.. _CommonParms:

Common Parameters
-------------------


**fontname** (*str*)

  In general, there are three options:

  1. Use one of the standard :ref:`Base-14-Fonts`. In this case, ``fontfile`` must not be specified and ``"Helvetica"`` is used if this parameter is omitted, too.
  2. Choose a font already in use by the page. Then specify its name **reference** prefixed with a slash ``/``, see below.
  3. Specify a ``fontfile`` present on your system. In this case choose an arbitrary unique new name for this parameter (without prefix).

  If inserted text should re-use one of the page's fonts, use its reference name in ``getFontList()`` like so:
  
  Suppose the font list has the entry ``[1024, 0, 'Type1', 'CJXQIC+NimbusMonL-Bold', 'R366']``, then specify ``fontname = "/R366", fontfile = None`` to use font ``CJXQIC+NimbusMonL-Bold``.

  .. note:: We currently only support single byte characters and horizontal, left-to-right text orientation with our text methods (the ``rotate`` parameter is not influenced by this). Keep this in mind if you re-use an existing font, or use an external fontfile (next parameter).

----

**fontfile** (*str*)

  File path of a font existing on your computer. If you specify ``fontfile``, make sure you use a ``fontname`` **not occurring** in the above list. This new font will be embedded in the PDF upon ``doc.save()``. Similar to new images, a font will be embedded only once. A table of ``md5`` codes for the binary font contents is maintained and checked against by MuPDF.

----

**fontsize** (*float*)

  Font size of text. This also determines the line height as ``fontsize * 1.2``.

----

**dashes** (*str*)

  Causes lines to be dashed. A continuous line with no dashes is drawn with ``"[]0"`` or ``None``. For (the rather complex) details on how to achieve dashing effects, see :ref:`AdobeManual`, page 217. Simple versions look like ``"[3 4]"``, which means dashes of 3 and gaps of 4 pixels length follow each other. ``"[3 3]"`` and ``"[3]"`` do the same thing.

----

**color / fill** (*list, tuple*)

  Line and fill colors are always specified as RGB triples of floats from 0 to 1. To simplify color specification, method ``getColor()`` in ``fitz.utils`` may be used. It accepts a string as the name of the color and returns the corresponding triple. The method knows over 540 color names - see section :ref:`ColorDatabase`.

----

**overlay** (*bool*)

  Causes the item to appear in foreground (default) or background.

----

**morph** (*sequence*)

  Causes "morphing" of either a shape, created by the ``draw*()`` methods, or the text inserted by page methods ``insertTextbox()`` / ``insertText()``. If not ``None``, it must be a pair ``(pivot, matrix)``, where ``pivot`` is a :ref:`Point` and ``matrix`` is a :ref:`Matrix`. The matrix can be anything except translations, i.e. ``matrix.e == matrix.f == 0`` must be true. The point is used as a pivotal point for the matrix operation. For example, if ``matrix`` is a rotation or scaling operation, then ``pivot`` is its center. Similarly, if ``matrix`` is a left-right or up-down flip, then the mirroring axis will be the vertical, respectively horizontal line going through ``pivot``, etc.

  .. note:: Several methods contain checks whether the to be inserted items will actually fit into the page (like :meth:`Shape.insertText`, or :meth:`Shape.drawRect`). For the result of a morphing operation there is however no such guaranty: this is entirely the rpogrammer's responsibility.

----

**roundCap** (*bool*)

  Cause lines, dashes and edges to be rounded (default). If false, sharp edges and square line and dashes ends will be generated. Rounded lines / dashes will end in a semi-circle with a diameter equal to line width and make longer by the radius of this semi-circle.

----

**closePath** (*bool*)

  Causes the end point of a drawing to be automatically connected with the starting point (by a straight line).

----

Description of ``getLinks()`` Entries
----------------------------------------
Each entry of the ``getLinks()`` list is a dictionay with the following keys:

* ``kind``:  (required) an integer indicating the kind of link. This is one of ``LINK_NONE``, ``LINK_GOTO``, ``LINK_GOTOR``, ``LINK_LAUNCH``, or ``LINK_URI``. For values and meaning of these names refer to :ref:`linkDest Kinds`.

* ``from``:  (required) a :ref:`Rect` describing the "hot spot" location on the page's visible representation (where the cursor changes to a hand image, usually).

* ``page``:  a 0-based integer indicating the destination page. Required for ``LINK_GOTO`` and ``LINK_GOTOR``, else ignored.

* ``to``:   either a ``fitz.Point``, specifying the destination location on the provided page, default is ``fitz.Point(0, 0)``, or a symbolic (indirect) name. If an indirect name is specified, ``page = -1`` is required and the name must be defined in the PDF in order for this to work. Required for ``LINK_GOTO`` and ``LINK_GOTOR``, else ignored.

* ``file``: a string specifying the destination file. Required for ``LINK_GOTOR`` and ``LINK_LAUNCH``, else ignored.

* ``uri``:  a string specifying the destination internet resource. Required for ``LINK_URI``, else ignored.

* ``xref``: an integer specifying the PDF cross reference entry of the link object. Do not change this entry in any way. Required for link deletion and update, otherwise ignored. For non-PDF documents, this entry contains ``-1``. It is also ``-1`` for **all** entries in the ``getLinks()`` list, if **any** of the links is not supported by MuPDF - see the note below.

Notes on Supporting Links
---------------------------
MuPDF's support for links has changed in **v1.10a**. These changes affect link types :data:`LINK_GOTO` and :data:`LINK_GOTOR`.

Reading (pertains to method ``getLinks()`` and the ``firstLink`` property chain)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If MuPDF detects a link to another file, it will supply either a ``LINK_GOTOR`` or a ``LINK_LAUNCH`` link kind. In case of ``LINK_GOTOR`` destination details may either be given as page number (eventually including position information), or as an indirect destination.

If an indirect destination is given, then this is indicated by ``page = -1``, and ``link.dest.dest`` will contain this name. The dictionaries in the ``getLinks()`` list will contain this information as the ``to`` value.

**Internal links are always** of kind ``LINK_GOTO``. If an internal link specifies an indirect destination, it **will always be resolved** and the resulting direct destination will be returned. Names are **never returned for internal links**, and undefined destinations will cause the link to be ignored.

Writing
~~~~~~~~~

PyMuPDF writes (updates, inserts) links by constructing and writing the appropriate PDF object **source**. This makes it possible to specify indirect destinations for ``LINK_GOTOR`` **and** ``LINK_GOTO`` link kinds (pre ``PDF 1.2`` file formats are **not supported**).

.. caution:: If a ``LINK_GOTO`` indirect destination specifies an undefined name, this link can later on not be found / read again with MuPDF / PyMuPDF. Other readers however **will** detect it, but flag it as erroneous.

Indirect ``LINK_GOTOR`` destinations can in general of course not be checked for validity and are therefore **always accepted**.

Homologous Methods of :ref:`Document` and :ref:`Page`
--------------------------------------------------------
This is an overview of homologous methods on the :ref:`Document` and on the :ref:`Page` level.

============================= =====================================
**Document Level**            **Page Level**
============================= =====================================
doc.getPageFontlist(pno)      doc[pno].getFontlist()
doc.getPageImageList(pno)     doc[pno].getImageList()
doc.getPagePixmap(pno, ...)   doc[pno].getPixmap(...)
doc.getPageText(pno, ...)     doc[pno].getText(...)
doc.searchPageFor(pno, ...)   doc[pno].searchFor(...)
doc._getPageXref(pno)         doc[pno]._getXref()
============================= =====================================

The list assumes a document object ``doc``. The page number ``pno`` is 0-based and can be any positive or negative number ``< len(doc)``.