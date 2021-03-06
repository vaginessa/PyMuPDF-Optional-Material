Introduction
==============

.. image:: img-pymupdf.jpg
   :align: center

**PyMuPDF** is a Python binding for `MuPDF <http://www.mupdf.com/>`_ - "a lightweight PDF and XPS viewer".

MuPDF can access files in PDF, XPS, OpenXPS, CBZ (comic book archive), FB2 and EPUB (e-book) formats.

These are files with extensions ``*.pdf``, ``*.xps``, ``*.oxps``, ``*.cbz``, ``*.fb2``  or ``*.epub`` (so in essence, with this binding you can develop **e-book viewers in Python** ...).

PyMuPDF provides access to many important functions of MuPDF from within a Python environment, and we are continuously seeking to expand this function set.

MuPDF stands out among all similar products for its top rendering capability and unsurpassed processing speed. At the same time, its "light weight" makes it an excellent choice for platforms where resources are typically limited, like smartphones.

Check this out yourself and compare the various free PDF-viewers. In terms of speed and rendering quality `SumatraPDF <http://www.sumatrapdfreader.org/>`_ ranges at the top (apart from MuPDF's own standalone viewer) - since it has changed its library basis to  MuPDF!

While PyMuPDF has been available since several years for an earlier version of MuPDF (v1.2, called **fitz-python** then), it was until only mid May 2015, that its creator and a few co-workers decided to elevate it to support current releases of MuPDF (first v1.7a, up to v1.12.0 as of this writing).

PyMuPDF runs and has been tested on Mac, Linux, Windows XP SP2 and up, Python 2.7 through Python 3.6 (note that Python supports Windows XP only up to v3.4), 32bit and 64bit versions. Other platforms should work too, as long as MuPDF and Python support them.

PyMuPDF is hosted on `GitHub <https://github.com/rk700/PyMuPDF>`_. Because we rely on MuPDF's C library, installation consists of two separate steps for all platforms except for MS Windows:

1. Installation of MuPDF: this involves downloading the source from their website and then compiling it on your machine.

2. Installation of PyMuPDF: this step is normal Python procedure. Usually you will have to adapt the ``setup.py`` to point to correct ``include`` and ``lib`` directories of your generated MuPDF.

For the Windows platform we have however combined these steps and offer binaries, available in ZIP and wheel formats. This installation material is contained in a separate GitHub `repository <https://github.com/JorjMcKie/PyMuPDF-optional-material>`_ and obsoletes all other download and generation work. You only need to choose which Python version and bitness you want and then download the respective zip or wheel file (less than 3 MB).

For installation details check out the respective chapter.

We also are registered on `PyPI <https://pypi.org/project/PyMuPDF/>`_.

There exist several `demo <https://github.com/rk700/PyMuPDF/tree/master/demo>`_ and `example <https://github.com/rk700/PyMuPDF/tree/master/examples>`_ programs in the main repository, ranging from simple code snippets to full-featured utilities, like text extraction, PDF joiners and bookmark maintenance.

Interesting **PDF manipulation and generation** functions have been added over time, including metadata and bookmark maintenance, document restructuring, annotation / link handling and document or page creation.

Note on the Name ``fitz``
--------------------------
The standard Python import statement for this library is ``import fitz``. This has a historical reason:

The original rendering library for MuPDF was called ``Libart``.

*"After Artifex Software acquired the MuPDF project, the development focus shifted on writing a new modern graphics library called ``Fitz``. Fitz was originally intended as an R&D project to replace the aging Ghostscript graphics library, but has instead become the rendering engine powering MuPDF."* (Quoted from `Wikipedia <https://en.wikipedia.org/wiki/MuPDF>`_).

License
--------
PyMuPDF is distributed under GNU GPL V3 (or later, at your choice).

MuPDF is distributed under a separate license, the **GNU AFFERO GPL V3**.

Both licenses apply, when you use PyMuPDF.

.. note:: Version 3 of the GNU AFFERO GPL is a lot less restrictive than its earlier versions used to be. It basically is an open source freeware license, that obliges your software to also being open source and freeware. Consult `this website <http://artifex.com/licensing/>`_, if you want to create a commercial product with PyMuPDF.

.. include:: version.rst
