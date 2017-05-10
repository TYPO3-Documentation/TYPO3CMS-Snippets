
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



by **Riccardo De Contardi**, 2017-04-05 21:15:00

Keywords:
   Fluid, Inline Syntax, Extbase

Problem:
   When dealing with the inline fluid syntax, a common mistake is to translate a viewhelper that contains an attribute that accepts 
   a string value like:

.. code-block:: xml
    <my:viewhelper string="{string}" />

into:
     
.. code-block:: xml
   {my:viewhelper(string:'{string}')}
   
This must be avoided as it uselessly wraps the value in a TextNode and could even lead to errors

Solution:
  Avoid the syntax with the single quotes and curly brackets and simply write:  

.. code-block:: xml
    {my:viewhelper(string:string)}

References:
   https://vimeo.com/167666466    
   https://forge.typo3.org/issues/79694

Credits:
   Claus Due.


.. index:: abc, bcd, cde
.. _s2017-2:
.. _s2017-2-The-Title:

2017-2 ... ((template for the next snippet))
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
