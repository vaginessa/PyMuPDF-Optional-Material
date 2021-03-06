.. _Link:

================
Link
================
Represents a pointer to somewhere (this document, other documents, the internet). Links exist per document page, and they are forward-chained to each other, starting from an initial link which is accessible by the :attr:`Page.firstLink` property.

There is a parent-child relationship between a link and its page. If the page object becomes unusable (closed document, any document structure change, etc.), then so does every of its existing link objects - an exception is raised saying that the object is "orphaned", whenever a link property or method is accessed.

========================= ============================================
**Attribute**             **Short Description**
========================= ============================================
:attr:`Link.rect`         clickable area in untransformed coordinates.
:attr:`Link.uri`          link destination
:attr:`Link.isExternal`   external link destination?
:attr:`Link.next`         points to next link
:attr:`Link.dest`         points to link destination details
========================= ============================================

**Class API**

.. class:: Link

   .. attribute:: rect

      The area that can be clicked in untransformed coordinates.

      :type: :ref:`Rect`

   .. attribute:: isExternal

      A bool specifying whether the link target is outside of the current document.

      :type: bool

   .. attribute:: uri

      A string specifying the link target. The meaning of this property should be evaluated in conjunction with property ``isExternal``. The value may be ``None``, in which case ``isExternal == False``. If ``uri`` starts with ``file://``, ``mailto:``, or an internet resource name, ``isExternal`` is ``True``. In all other cases ``isExternal == False`` and ``uri`` points to an internal location. In case of PDF documents, this should either be ``#nnnn`` to indicate a 1-based (!) page number ``nnnn``, or a named location. The format varies for other document types, e.g. ``uri = '../FixedDoc.fdoc#PG_2_LNK_1'`` for page number 2 (1-based) in an XPS document.

      :type: str

   .. attribute:: next

      The next ``Link`` or ``None``

      :type: ``Link``

   .. attribute:: dest

      The link destination details object.

      :type: :ref:`linkDest`
