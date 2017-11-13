
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
   Add custom FAL overlay palettes, to get custom configuration on any FAL
   related TCA fields.

Custom palette:
   Add your custom definition of palette to existing sys_file_reference
   palettes (default: basicoverlayPalette, imageoverlayPalette,
   audioOverlayPalette, videoOverlayPalette).

   :file:`my_sitepackage/Configuration/TCA/Overrides/sys_file_reference.php`::

      <?php
      if (!defined('TYPO3_MODE')) die ('Access denied.');

      $GLOBALS['TCA']['sys_file_reference']['palettes']['imageOverlayPaletteWithoutLink'] = array(
          'showitem' => '
                  title,alternative,--linebreak--,
                  description,--linebreak--,crop'
      );

Apply palette:
   Use *columnsOverrides* to apply custom palette to any existing FAL related
   TCA field, for example, tt_content.image
   *my_sitepackage/Configuration/TCA/Overrides/tt_content.php*.

   *foreign_types* 3 & 4 are for audio and video file types::

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



.. index:: TypoScript; without_DB
.. _s2017-5:
.. _s2017-5-Root-TypoScript-without-database:

2017-5 Root TypoScript without database
=======================================

by **Jörg Kummer**, 2017-07-16 20:00:00

Keywords:
   TypoScript, without_DB

.. highlight:: php

Since TYPO3 CMS 8.6 there's a new hook in :php:`TemplateService` that allows to
add or modify TypoScript templates. This means that you can create even the
root TypoScript template of your site programmatically. Not a single database
record is needed. That way *all* TypoScript can be stored in files
which is perfect for version control.

The following recipe shows how to do it. It is inspired by Jigal Hemert's talk
*New little gems in TYPO3 v8* at the T3DD17.


Register a hook
---------------
Add a hook registration to your sitepackage extension configuration
file :file:`mysitepackage/Classes/Hooks/ext_localconf.php`::

   // Add default Typoscript
   $GLOBALS['TYPO3_CONF_VARS']['SC_OPTIONS']['Core/TypoScript/TemplateService']
      ['runThroughTemplatesPostProcessing'][] =
      \Vendor\Mysitepackage\Hooks\TypoScriptHook::class . '->addCustomTypoScriptTemplate';


Provide a TypoScriptHook class
------------------------------
Add a hook class to your sitepackage extension classes
to add the custom typoscript template
:file:`mysitepackage/Classes/Hooks/TypoScriptHook.php`::

   <?php
   namespace Vendor\Mysitepackage\Hooks;

   /**
    * Class TypoScriptHook
    *
    * @package TYPO3
    * @subpackage tx_mysitepackage
    */
   class TypoScriptHook
   {

      /**
       * Hook into the default TypoScript to add custom typoscript template
       *
       * @param array $parameters
       * @param \TYPO3\CMS\Core\TypoScript\TemplateService $parentObject
       * @return void
       */
      public function addCustomTypoScriptTemplate($parameters, $parentObject)
      {
         // Add a custom "fake" sys_template record, if no template was found in rootline
         if ($parentObject->outermostRootlineIndexWithTemplate === 0) {
            $row = [
               'uid' => 'mysitepackage',
               'constants' => '<INCLUDE_TYPOSCRIPT: source="FILE:EXT:mysitepackage/Configuration/TypoScript/constants.txt">' . LF,
               'config' => '<INCLUDE_TYPOSCRIPT: source="FILE:EXT:mysitepackage/Configuration/TypoScript/setup.txt">' . LF,
               'root' => 1,
               'clear' => 3,
               'nextlevel' => 0,
               'static_file_mode' => 1,
               'title' => 'Root template',
            ];
            $parentObject->processTemplate($row, 'sys_' . $row['uid'], $parameters['absoluteRootLine'][0]['uid'], 'sys_' . $row['uid']);
            $parentObject->rootId = $parameters['absoluteRootLine'][0]['uid'];
            $parentObject->rootLine[] = $parameters['absoluteRootLine'][0];
         }
      }
   }

.. highlight:: typoscript

Use TypoScript Constants
------------------------
Add TypoScript constants as usual to your sitepackage extension configuration
file :file:`mysitepackage/Configuration/TypoScript/constants.txt`::

   example_text = This is a page example text


Use the TypoScript Setup
------------------------
Add the TypoScript setup as usual to your sitepackage
extension configuration file
:file:`mysitepackage/Configuration/TypoScript/setup.txt`::

   page = PAGE
   page.10 = TEXT
   page.10.data = {$example_text}


Include other TypoScript templates
----------------------------------
To add static typoscript templates from 3rd party extensions like
:file:`fluid_styled_content` use includes::

   # CONSTANTS in
   #    mysitepackage/Configuration/TypoScript/constants.txt
   # ...
   <INCLUDE_TYPOSCRIPT: source="FILE:EXT:fluid_styled_content/Configuration/TypoScript/constants.txt">
   # ...

   # SETUP code in
   #    mysitepackage/Configuration/TypoScript/setup.txt
   # ...
   <INCLUDE_TYPOSCRIPT: source="FILE:EXT:fluid_styled_content/Configuration/TypoScript/setup.txt">
   # ...

*Note:* Extensions may use different file endings like :file:`.ts`,
:file:`.t3s`, :file:`.ts.txt` or whatever.

Related hint
   Adding TypoScript from 3rd party extensions using
   :php:`\TYPO3\CMS\Core\Utility\ExtensionManagementUtility::addTypoScript()`
   doesn't work. In such case add the TypoScript of other extensions directly
   to your TypoScript files instead.

Tips
   - The technique is used in the `TYPO3 extension 'bolt'
     <https://github.com/CMSExperts/bolt>`__ which is used in production.

References:
   - :issue:`79140`

   - Changelog: `Add hook to add custom typoscript templates
     <https://docs.typo3.org/typo3cms/extensions/core/Changelog/8.6/Feature-79140-AddHookToAddCustomTypoScriptTemplates.html>`__




.. index:: TypoScript; menu; shortcut
.. _s2017-6:
.. _s2017-6-TypoScript-Menu-with-active-or-current-class-for-shortcuts:

2017-6 TypoScript Menu with active or current class for shortcuts
=================================================================

by Loek Hilgersom, 2017-07-18 14:45:00

Keywords:
   TypoScript, Menu, TMENU, active, current, shortcut

This is an improved version of :ref:`s2013-20`.

.. highlight:: TypoScript

::

   /****
   * Conditions which allow Shortcut pages to show active in menu's, works for shortcuts to specific pages
   * (shortcut_mode=0) and shortcuts to parent page (shortcut_mode=3). Options 1 and 2 (shortcut to first
   * child page and to random page) are nearly impossible to resolve.
   *
   * Example usage (ATTENTION: always use on NO-state, others make no sense!):
   *
   *   NO.ATagParams = class="link"
   *   NO.ATagParams.override = class="link active"
   *   NO.ATagParams.override.if < tmp.activeShortCutPages.if
   */

   tmp.activeShortCutPages.if {
      // If shortcut page
      value = 4
      equals.field = doktype
      // Now check different shortcut modes
      isTrue.cObject = CASE
      isTrue.cObject {
         key.field = shortcut_mode
         // Shortcut to selected page
         0 = TEXT
         0.value = 1
         0.if {
            value.data = TSFE:id
            equals.field = shortcut
         }
         // Shortcut to parent page
         3 < .0
         3.if.equals.field = pid
      }
   }

   lib.subMenu = HMENU
   lib.subMenu {
      1 = TMENU
      1 {
         wrap = <ul>|</ul>
         NO = 1
         NO {
            wrapItemAndSub = <li class="nav-item">|</li>
            ATagParams = class="nav-link"
            // Set active class for shortcut pages
            ATagParams {
               override = class="nav-link active"
               override.if < tmp.activeShortCutPages.if
            }
         }
         CUR < .NO
         CUR.ATagParams = class="nav-link active"
      }
   }

.. index:: signal; slot
.. _s2017-7:
.. _s2017-7-Signals-and-Slots:

2017-7 Signals and Slots – Extend TYPO3 Functionality
=====================================================

by **Marcus Schwemer**, 2017-07-25

Keywords:
   Development, Extension, Tutorial

.. highlight:: php

About signals and slots
   Signals and slots are a possibility within TYPO3 to extend the functionality
   of an object. The blogpost explains the theoretical basics and shows the
   practical details.

Find the blogpost
   https://typo3worx.eu/2017/07/signals-and-slots-in-typo3/



.. index:: abc, bcd, cde
.. _s2017-99:
.. _s2017-99-The-Title:

2017-99 The Title ((template for the next snippet))
===================================================

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

