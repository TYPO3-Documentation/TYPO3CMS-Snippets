
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


.. index:: image, FAL, TCA
.. _s2017-3:
.. _s2017-3-Custom-FAL-inline-overlay-palette-in-BE:

2017-3 Custom FAL inline overlay palette in BE
==============================================

by Joerg Kummer, 2017-05-15 13:30:00

.. highlight:: php

Keywords:
   image, FAL, TCA

Description:
   Add custom FAL overlay palettes, to get custom configuration on any FAL related TCA fields. 

Custom palette:
   Add your custom definition of palette to existing sys_file_reference palettes (default: basicoverlayPalette, imageoverlayPalette, audioOverlayPalette, videoOverlayPalette).
   *my_sitepackage/Configuration/TCA/Overrides/sys_file_reference.php*::

      <?php
      if (!defined('TYPO3_MODE')) die ('Access denied.');
      
      $GLOBALS['TCA']['sys_file_reference']['palettes']['imageOverlayPaletteWithoutLink'] = array(
          'showitem' => '
                  title,alternative,--linebreak--,
                  description,--linebreak--,crop'
      );

Apply palette:
   Use *columnsOverrides* to apply custom palette to any existing FAL related TCA field, for example, tt_content.image
   *my_sitepackage/Configuration/TCA/Overrides/tt_content.php*::

      <?php
      if (!defined('TYPO3_MODE')) die ('Access denied.');
      
      $GLOBALS['TCA']['tt_content']['types']['image']['columnsOverrides']['image']['config']['foreign_types'] = array(
          '0' => array(
              'showitem' => '
                  --palette--;LLL:EXT:lang/locallang_tca.xlf:sys_file_reference.imageoverlayPalette;imageOverlayPaletteWithoutLink,
                  --palette--;;filePalette'
          ),
          '1' => array(
              'showitem' => '
                  --palette--;LLL:EXT:lang/locallang_tca.xlf:sys_file_reference.imageoverlayPalette;imageOverlayPaletteWithoutLink,
                  --palette--;;filePalette'
          ),
          '2' => array(
              'showitem' => '
                  --palette--;LLL:EXT:lang/locallang_tca.xlf:sys_file_reference.imageoverlayPalette;imageOverlayPaletteWithoutLink,
                  --palette--;;filePalette'
          ),
          '5' => array(
              'showitem' => '
                  --palette--;LLL:EXT:lang/locallang_tca.xlf:sys_file_reference.imageoverlayPalette;imageOverlayPaletteWithoutLink,
                  --palette--;;filePalette'
          )
      );
   *foreign_types* 3 & 4 are for audio and video file types


.. index:: debugging, Extbase
.. _s2017-4:
.. _s2017-4-Using-the-TYPO3-API-for-debugging:

2017-4 Using the TYPO3 API for debugging
========================================

by **Franz Kugelmann**, 2017-06-19 09:21:00

Keywords:
   debugging, Extbase

.. highlight:: php

Task:
   You want to nicely display the contents of a PHP variable at
   runtime when using the TYPO3 API..

Solution:
   Make use of the class :php:`DebugUtility`.

   **DebugUtility::debug()**
   
   The :php:`DebugUtility::debug()` function works nicely in
   the frontend and backend. Example::

      // ((good example should be added here))
      \TYPO3\CMS\Core\Utility\DebugUtility::debug($yourVariable);  
   
   **DebugUtility::var_dump()**

   The DebugUtility::var_dump() function is specifically optimized 
   to reveal Extbase's object structures. Example::
   
      // ((good example should be added here))
      \TYPO3\CMS\Extbase\Utility\DebuggerUtility::var_dump($yourVariable,
         [checkForHelpfulAdditionalParameters])

References:
   - Look for the `DebugUtility Class Reference
     <https://api.typo3.org/typo3cms/current/html/class_t_y_p_o3_1_1_c_m_s_1_1_core_1_1_utility_1_1_debug_utility.html>`__
     that is part of the core.
   - Look for the `DebuggerUtility Class Reference
     <https://api.typo3.org/typo3cms/current/html/class_t_y_p_o3_1_1_c_m_s_1_1_extbase_1_1_utility_1_1_debugger_utility.html>`__.



.. index:: abc, bcd, cde
.. _s2017-5:
.. _s2017-5-The-Title:

2017-5 ... ((template for the next snippet))
============================================

by **Your Name**, 2017-mm-dd hh:mm:ss

Keywords:
   abc, bcd, cde

.. highlight:: php

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

