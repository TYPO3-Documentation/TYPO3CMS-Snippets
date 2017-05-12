
.. include:: ../Includes.txt

===============
2017
===============

.. rst-class:: horizbuttons-tip-xxl

- Please contribute!



.. index:: TypoScript; extract
.. _s2017-1:
.. _s2017-1-Extract-TypoScript-templates-from-database-and-save-as-files:

2017-1 Extract TypoScript templates from database and save as files
===================================================================

by **Martin Bless**, 2017-04-05 21:15:00

Keywords:
   Extract TypoScript

Task:
   You are dealing with an older TYPO3 installation that has TypoScript
   templates in the database. You prefer to have the templates stored
   as files in the filesystem.

   How can you extract the templates easily and store them as files?

Solution:
   Download this extension and follow the instructions:
   https://github.com/marketing-factory/mfc_tsextract

Credits:
   Thank you, Marketing Factory, for making your solution publicly available.




.. index:: Fluid, inline-syntax, Extbase, viewhelper
.. _s2017-2:
.. _s2017-2-Fluid-inline-syntax:

2017-2 Fluid inline syntax: How to pass arguments to viewhelpers
================================================================

by **Riccardo De Contardi**, 2017-05-12 10:07:00

.. highlight:: xml

Keywords:
   Fluid, inline-syntax, Extbase, viewhelper

Problem:
   People tend to translate viewhelper calls badly to inline form.

   Consider this example::

      <my:viewhelper string="{myVar}" />

Bad:
   Very often you can find this translated like so::

      {my:viewhelper(string: '{myVar}')}

   What's wrong with this? While this *may* work it creates
   an extra, superfluous TextNode around the variable accessor.
   And it can easily lead to warnings and errors.

Good:
   The *clean and correct solution* is to avoid the single quotes
   that create the TextNode. The curly braces are then dropped as well::

      {my:viewhelper(string: myVar)}

   Pretty easy and well readable!

References:
   - `Claus Due <https://vimeo.com/user20720051>`__ has created a
     `Vimeo video <https://vimeo.com/167666466>`__ about this with
     an in-depth explanation. Thank you very much!

   - The problem is for example being discussed in :issue:`79694`.

Credits:
   `Claus Due <https://vimeo.com/user20720051>`__

.. highlight:: php


.. index:: abc, bcd, cde
.. _s2017-3:
.. _s2017-3-The-Title:

2017-3 ... ((template for the next snippet))
===================================================================

by **Your Name**, 2017-mm-dd hh:mm:ss

Keywords:
   abc, bcd, cde

Abc:
   ...

Bcd:
   ...

and:
   ...

so:
   ...

on:
   ...
