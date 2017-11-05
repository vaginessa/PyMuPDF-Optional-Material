.. _Shape:

Shape
================

This class allows creating interconnected graphical elements on a PDF page. Its methods have the same meaning and name as the corresponding :ref:`Page` methods. Their :ref:`CommonParms` are however exported to a separate method, ``finish()``. In addition, all draw methods return a :ref:`Point` object to support connected drawing paths. This point always equals the **"current point"**, that PDF maintains during path construction.

The class now also supports the text insertion methods ``insertText()`` and ``insertTextbox()``. They need a slightly different handling compared to the draw methods (see below for details):

1. They do not use :attr:`Shape.contents`. Instead they directly modify :attr:`Shape.totalcont`.
2. They do not use nor need :meth:`Shape.finish`.
3. They provide their own ``color`` and ``morph`` arguments.
4. They do not use nor change :attr:`Shape.lastPoint`.

As with the draw methods, text insertion requires using :meth:`Shape.commit` to update the page.

================================ =================================================
**Method / Attribute**             **Description**
================================ =================================================
:meth:`Shape.commit`             update the page's ``/Contents`` object
:meth:`Shape.drawBezier`         draw a cubic Bézier curve
:meth:`Shape.drawCircle`         draw a circle around a point
:meth:`Shape.drawCurve`          draw a cubic Bézier using one helper point
:meth:`Shape.drawLine`           draw a line
:meth:`Shape.drawOval`           draw an ellipse
:meth:`Shape.drawPolyline`       connect a sequence of points
:meth:`Shape.drawRect`           draw a rectangle
:meth:`Shape.drawSector`         draw a circular sector or piece of pie
:meth:`Shape.drawSquiggle`       draw a squiggly line
:meth:`Shape.drawZigzag`         draw a zigzag line
:meth:`Shape.finish`             finish a set of draws
:meth:`Shape.insertText`         insert text lines
:meth:`Shape.insertTextbox`      insert text into a rectangle
:attr:`Shape.contents`           draw commands since last ``finish()``
:attr:`Shape.doc`                stores the page's document
:attr:`Shape.height`             stores the page's height
:attr:`Shape.lastPoint`          stores the current point
:attr:`Shape.page`               stores the owning page
:attr:`Shape.width`              stores the page's width
:attr:`Shape.totalcont`          accumulated string to be stored in ``/Contents``
================================ =================================================

**Class API**

.. class:: Shape

   .. method:: __init__(self, page)

      Create a new drawing. During importing PyMuPDF, the ``fitz.Page`` object is being given the convenience method ``newShape()`` to construct a ``Shape`` object. During instantiation, a check will be made whether we do have a PDF page. An exception is otherwise raised.

      :arg page: an existing page of a PDF document.
      :type page: :ref:`Page`

   .. method:: drawLine(p1, p2)

      Draw a line from :ref:`Point` objects ``p1`` to ``p2``.

      :arg p1: starting point
      :type p1: :ref:`Point`

      :arg p2: end point
      :type p2: :ref:`Point`

      :rtype: :ref:`Point`
      :returns: the end point, ``p2``.

   .. method:: drawSquiggle(p1, p2, breadth = 2)

      Draw a squiggly (wavy, undulated) line from :ref:`Point` objects ``p1`` to ``p2``. An integer number of full wave periods will always be drawn, one period having a length of ``4 * breadth``. The breadth parameter will be adjusted as necessary to meet this condition. The drawn line will always turn "left" when leaving ``p1`` and always join ``p2`` from the "right".

      :arg p1: starting point
      :type p1: :ref:`Point`

      :arg p2: end point
      :type p2: :ref:`Point`

      :arg float breadth: the amplitude of each wave. The condition ``2 * breadth < abs(p2 - p1)`` must be true to fit in at least one wave. See the following picture, which shows two points connected by one full period.

      .. image:: breadth.png

      :rtype: :ref:`Point`
      :returns: the end point, ``p2``.

      Here is an example of three connected lines, forming a closed, filled triangle. Little arrows indicate the stroking direction.

      .. image:: squiggly.png

      .. note:: Waves drawn are **not** trigonometric (sine / cosine). If you need that, have a look at `draw-sines.py <https://github.com/rk700/PyMuPDF/blob/master/demo/draw-sines.py>`_.

   .. method:: drawZigzag(p1, p2, breadth = 2)

      Draw a zigzag line from :ref:`Point` objects ``p1`` to ``p2``. An integer number of full zigzag periods will always be drawn, one period having a length of ``4 * breadth``. The breadth parameter will be adjusted to meet this condition. The drawn line will always turn "left" when leaving ``p1`` and always join ``p2`` from the "right".

      :arg p1: starting point
      :type p1: :ref:`Point`

      :arg p2: end point
      :type p2: :ref:`Point`

      :arg float breadth: the amplitude of the movement. The condition ``2 * breadth < abs(p2 - p1)`` must be true to fit in at least one period.

      :rtype: :ref:`Point`
      :returns: the end point, ``p2``.

   .. method:: drawPolyline(points)

      Draw several connected lines between points contained in the sequence ``points``. This can be used for creating arbitrary polygons by setting the last item equal to the first one.

      :arg sequence points: a sequence of :ref:`Point` objects. Its length must at least be 2 (in which case it is equivalent to ``drawLine()``).

      :rtype: :ref:`Point`
      :returns: ``points[-1]`` - the last point in the argument sequence.

   .. method:: drawBezier(p1, p2, p3, p4)

      Draw a standard cubic Bézier curve from ``p1`` to ``p4``, using ``p2`` and ``p3`` as control points.

      :arg p1: starting point
      :type p1: :ref:`Point`

      :arg p2: control point 1
      :type p2: :ref:`Point`

      :arg p3: control point 2
      :type p3: :ref:`Point`

      :arg p4: end point
      :type p4: :ref:`Point`

      :rtype: :ref:`Point`
      :returns: the end point, ``p4``.

      Example:

      .. image:: drawbezier.png

   .. method:: drawOval(rect)

      Draw an ellipse inside the given rectangle. If ``rect`` is a square, a standard circle is drawn. The drawing starts and ends at the middle point of the left rectangle side in a counter-clockwise movement.

      :arg rect: rectangle, must be finite and not empty.
      :type rect: :ref:`Rect`

      :rtype: :ref:`Point`
      :returns: the middle point of the left rectangle side.

   .. method:: drawCircle(center, radius)

      Draw a circle given its center and radius. The drawing starts and ends at point ``start = center - (radius, 0)`` in a counter-clockwise movement. ``start`` corresponds to the middle point of the enclosing square's left border.

      The method is a shortcut for ``drawSector(center, start, 360, fullSector = False)``. To draw a circle in a clockwise movement, change the sign of the degree.

      :arg center: the center of the circle.
      :type center: :ref:`Point`

      :arg float radius: the radius of the circle. Must be positive.

      :rtype: :ref:`Point`
      :returns: ``center - (radius, 0)``.

   .. method:: drawCurve(p1, p2, p3)

      A special case of ``drawBezier()``: Draw a cubic Bézier curve from ``p1`` to ``p3``. On each of the two lines from ``p1`` to ``p2`` and from ``p2`` to ``p3`` one control point is generated. This guaranties that the curve's curvature does not change its sign. If these two connecting lines intersect with an angle of 90 degress, then the resulting curve is a quarter ellipse (or circle, if of same length) circumference.

      :arg p1: starting point.
      :type p1: :ref:`Point`

      :arg p2: helper point.
      :type p2: :ref:`Point`

      :arg p3: end point.
      :type p3: :ref:`Point`

      :rtype: :ref:`Point`
      :returns: the end point, ``p3``.

      Example: a filled quarter ellipse segment.

      .. image:: drawcurve.png

   .. method:: drawSector(center, point, angle, fullSector = True)

      Draw a circular sector, optionally connecting the arc to the circle's center (like a piece of pie).

      :arg center: the center of the circle.
      :type center: :ref:`Point`

      :arg point: one of the two end points of the pie's arc segment. The other one is calculated from the ``angle``.
      :type point: :ref:`Point`

      :arg float angle: the angle of the sector in degrees. Used to calculate the other end point of the arc. Depending on its sign, the arc is drawn counter-clockwise (postive) or clockwise.

      :arg bool fullSector: whether to draw connecting lines from the ends of the arc to the circle center. If a fill color is specified, the full "pie" is colored, otherwise just the sector.

      :returns: the other end point of the arc. Can be used as starting point for a following invocation to create logically connected pies charts.
      :rtype: :ref:`Point`

      Examples:

      .. image:: drawsector1.png

      .. image:: drawsector2.png


   .. method:: drawRect(rect)

      Draw a rectangle. The drawing starts and ends at the top-left corner in a counter-clockwise movement.
      
      :arg rect: where to put the rectangle on the page.
      :type rect: :ref:`Rect`

      :rtype: :ref:`Point`
      :returns: top-left corner of the rectangle.

   .. method:: insertText(point, text, fontsize = 11, fontname = "Helvetica", fontfile = None, color = (0, 0, 0), rotate = 0, morph = None)

      Insert text lines beginning at a :ref:`Point` ``point``.

      :arg point: the bottom-left position of the first ``text`` character in pixels. ``point.x`` specifies the distance from left border, ``point.y`` the distance from top of page. This is independent from text orientation as requested by ``rotate``. However, there must always be sufficient room "above", which can mean the distance from any of the four page borders.
      :type point: :ref:`Point`

      :arg text: the text to be inserted. May be specified as either a string type or as a sequence type. For sequences, or strings containing line breaks ``\n``, several lines will be inserted. No care will be taken if lines are too wide, but the number of inserted lines will be limited by "vertical" space on the page (in the sense of reading direction as established by the ``rotate`` parameter). Any rest of ``text`` is discarded - the return code however contains the number of inserted lines. Only single byte character codes are currently supported.
      :type text: str or sequence

      :arg int rotate: determines whether to rotate the text. Acceptable values are multiples of 90 degrees. Default is 0 (no rotation), meaning horizontal text lines oriented from left to right. 180 means text is shown upside down from **right to left**. 90 means counter-clockwise rotation, text running **upwards**. 270 (or -90) means clockwise rotation, text running **downwards**. In any case, ``point`` specifies the bottom-left coordinates of the first character's rectangle. Multiple lines, if present, always follow the reading direction established by this parameter. So line 2 is located **above** line 1 in case of ``rotate = 180``, etc.

      :rtype: int
      :returns: number of lines inserted.

      For a description of the other parameters see :ref:`CommonParms`.

   .. method:: insertTextbox(rect, buffer, fontsize = 11, fontname = "Helvetica", fontfile = None, color = (0, 0, 0), expandtabs = 8, align = TEXT_ALIGN_LEFT, charwidths = None, rotate = 0, morph = None)

      PDF only: Insert text into the specified rectangle. The text will be split into lines and words and then filled into the available space, starting from one of the four rectangle corners, depending on ``rotate``. Line feeds will be respected as well as multiple spaces will be.

      :arg rect: the area to use. It must be finite, not empty and completely contained in the page.
      :type rect: :ref:`Rect`

      :arg buffer: the text to be inserted. Must be specified as a string or a sequence of strings. Line breaks are respected also when occurring in a sequence entry.
      :type text: str or sequence

      :arg int align: align each text line. Default is 0 (left). Centered, right and justified are the other supported options, see :ref:`TextAlign`.

      :arg int expandtabs: controls handling of tab characters ``\t`` using the ``string.expandtabs()`` method **per each line**.

      :arg sequence charwidths: specify a sequence of character widths (floats) for the specified font. If omitted, it will be created by the function on **each invocation**. For performance, provide this parameter if you insert several text boxes with the same font. Use low-level function :meth:`Document._getCharWidths` to create one. Only single byte character codes are currently supported. Results are unpredictable, if larger codes occur (and the ``charwidths`` sequence is longer than 256).

      :arg int rotate: requests text to be rotated in the rectangle. This value must be a multiple of 90 degrees. Default is 0 (no rotation). Effectively, four different values are processed: 0, 90, 180 and 270 (= -90), each causing the text to start in a different rectangle corner. Bottom-left is 90, bottom-right is 180, and -90 / 270 is top-right. See the example how text is filled in a rectangle.

      :rtype: float
      :returns:
          **If positive or zero**: successful execution. The value returned is the unused rectangle space in pixels. This may safely be ignored - or be used to optimize the rectangle, position subsequent items, etc.

          **If negative**: no execution. The value returned is the space deficit to store the text. Enlarge rectangle, decrease ``fontsize``, decrease text amount, etc.

      .. image:: rotate.png

      For a description of the other parameters see :ref:`CommonParms`.

   .. method:: finish(width = 1, color = (0, 0, 0), fill = None, roundCap = True, dashes = None, closePath = True, even_odd = False, morph = (pivot, matrix))

      Finish a set of ``draw*()`` methods by applying :ref:`CommonParms` to all of them. This method also supports morphing the resulting compound drawing using a pivotal :ref:`Point`.

      :arg sequence morph: morph the compound drawing around some arbitrary pivotal :ref:`Point` ``pivot`` by applying :ref:`Matrix` ``matrix`` to it. Default is no morphing (``None``). The matrix can contain any values in its first 4 components, ``matrix.e == matrix.f == 0`` must be true, however. This means that any combination of scaling, shearing, rotating, flipping, etc. is possible, but translations are not.

      :arg bool even_odd: request the **"even-odd rule"** for filling operations. Default is ``False``, so that the **"nonzero winding number rule"** is used. These rules are alternative methods to determine, whether a pixel is considered inside or outside the area to receive the fill color. Only with fairly complex shapes a different behavior is to be expected with these rules. For an in-depth explanation, see :ref:`AdobeManual`, pp. 232 ff. Here is an example to demonstrate the difference.

      .. image:: even-odd.png

      .. note:: Method **"even-odd"** counts the overlaps of areas. Pixels in areas overlapping an odd number of times, are regarded as being inside, otherwise outside. In contrast, the default method **"nonzero winding"** counts the number of times a pixel lies in areas orientated clockwise or counter-clockwise, respectively. If the difference of these counters is zero,the pixel is regarded outside, otherwise inside. In the top two shapes, circles are drawn in standard, counter-clockwise manner (look at the arrows). The lower two shapes contain one (top-left) circle drawn clockwise. As can be seen, area orientation is irrelevant for the even-odd rule.

   .. method:: commit(overlay = True)

      Update the page's `/Contents` with the accumulated drawing commands. If a ``Shape`` is not committed, the page will not be changed. The method must be preceeded with at least one ``finish()`` method.

      :arg bool overlay: determine whether to put the drawing in foreground (default) or background. Relevant only, if the page is not empty.

   .. attribute:: doc

      For reference only: the page's document.

      :type: :ref:`Document`

   .. attribute:: page

      For reference only: the owning page.

      :type: :ref:`Page`

   .. attribute:: height

      Copy of the page's height

      :type: float

   .. attribute:: width

      Copy of the page's width.

      :type: float

   .. attribute:: contents

      Drawing commands accumulated since last finish.

      :type: str

   .. attribute:: totalcont

      Total contents about to be added to e page's ``/Contents`` object (via ``commit``).

      :type: str

   .. attribute:: lastPoint

      For reference only: the current point of the drawing path. It is ``None`` at ``Shape`` creation and after each ``finish()`` and ``commit()``.

      :type: :ref:`Point`

Usage
------
A drawing object is constructed by ``img = page.newShape()``. After this, as many draw and finish methods as required may follow. Each sequence of draws must be finished before the drawing is committed. The overall coding pattern looks like this:

>>> img = page.newShape()
>>> img.draw1(...)
>>> img.draw2(...)
>>> ...
>>> img.finish(width=..., color = ..., fill = ..., morph = ...)
>>> img.draw3(...)
>>> img.draw4(...)
>>> ...
>>> img.finish(width=..., color = ..., fill = ..., morph = ...)
>>> ...
>>> img.commit(overlay = True)
>>> ....

Notes
~~~~~~
1. Each ``finish()`` combines the preceding "elementary" draws into one logical shape, giving it common colors, line width, morphing, etc. Likewise, if ``closePath`` is specified, it will connect the end point of the last draw method with the starting point of the first one.

2. For successfully creating compound graphics, it is important that each draw method uses the end point of the previous one as its starting point. In the above pseudo code, ``draw2`` should hence use the returned :ref:`Point` of ``draw1`` as its starting point. Failing to do so, would automatically start a new path and ``finish()`` may not work as expected.

3. Each ``commit`` takes all text insertions and shapes and places them in foreground or background on the page - thus providing a way to control graphical layers.

4. Text insertions may occur anywhere before the commit. Please take into account, that text inserts are appended to ``Shape.totalcont`` directly, whereas draws will be appended by ``Shape.finish``. 

5. Only ``commit`` will update the page's contents, the other methods are basically string manipulations. With many draw / text operations, this will result in a much better performance, than issuing the corresponding page methods separately (they each do their own commit).

Examples
---------
1. Create a full circle of pieces of pie in different colors.

>>> img  = page.newShape()       # start a new shape
>>> cols = (...)                 # a sequence of RGB color triples
>>> pieces = len(cols)           # number of pieces to draw
>>> beta = 360. / pieces         # angle of each piece of pie
>>> center = fitz.Point(...)     # center of the pie
>>> p0     = fitz.Point(...)     # starting point
>>> for i in range(pieces):
        p0 = img.drawSector(center, p0, beta,
                            fullSector = True) # draw piece
        # now fill it but do not connect ends of the arc
        img.finish(fill = cols[i], closePath = False)
>>> img.commit()                 # update the page

2. Create a regular n-edged polygon (fill yellow, red border). We use ``drawSector()`` only to calculate the points on the circle. We will empty the PDF command buffer before actually drawing the polygon.

>>> img  = page.newShape()       # start a new shape
>>> beta = -360.0 / n            # our angle, drawn clockwise
>>> center = fitz.Point(...)     # center of circle
>>> p0     = fitz.Point(...)     # start here (1st edge)
>>> points = [p0]                # store polygon edges
>>> for i in range(n):           # calculate the edges
        p0 = img.drawSector(center, p0, beta)
        points.append(p0)
>>> img.contents = ""            # do not draw the circle sectors
>>> img.drawPolyline(points)     # draw the polygon
>>> img.finish(color = (1,0,0), fill = (1,1,0), closePath = False)
>>> img.commit()