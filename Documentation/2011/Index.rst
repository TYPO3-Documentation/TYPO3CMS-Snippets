
.. include:: ../Includes.txt

===============
2011
===============

.. index:: fluid, extbase, HMENU, TypoScript, cObject
.. _s2011-2:
.. _s2011-2-TypoScript-and-Fluid-Render-a-breadcrumb-menu-in-a-Fluid-template:

2011-2 TypoScript and Fluid: Render a breadcrumb menu in a Fluid template
=========================================================================

by **Steffen Müller**, 2011-02-10 01:20:44, old #279, typoscript

Keywords:
   fluid, extbase, HMENU, TypoScript, cObject

Description:
   This snippets demonstrates the most basic combination of TypoScript and Fluid:
   Render content with TypoScript and show it in a Fluid template.
   In this example, a breadcrumb menu is rendered using a HMENU cObject of type
   rootline.

   The outcome looks like this:
   Home >> Learning TYPO3 >> Fluid examples

Links:

   http://www.t3node.com/blog/combining-fluid-viewhelpers-and-typoscript-in-typo3-5-basic-examples/

Code:
   .. code-block:: typoscript

      #Fluid template:
      <f:cObject typoscriptObjectPath="lib.breadcrumb" />


      #TypoScript:
      lib.breadcrumb = HMENU
      lib.breadcrumb {
        special = rootline
        special.range = 0|-1
        1 = TMENU
        1 {
          NO.linkWrap = | >>
          CUR = 1
          CUR.doNotLinkIt = 1
        }
      }


.. index:: fluid, extbase, css
.. _s2011-1:
.. _s2011-1-additional-header-data-to-extbasefluid:

2011-1 additional header data to extbase/fluid
==============================================

by **Søren Malling**, 2011-01-11 17:51:08, old #278, php

Keywords:
   fluid, extbase, css

Code:
   .. code-block:: php

      /**
          * Initialize view
          *
          * @return void
          */
         public function initializeView(Tx_Extbase_MVC_View_ViewInterface $view) {
            $this->response->addAdditionalHeaderData($this->wrapCssFile(t3lib_extMgm::siteRelPath('mediadatabase') . 'Resources/Public/tx_mediadatabase.css'));
            $this->response->addAdditionalHeaderData($this->wrapCssFile(t3lib_extMgm::siteRelPath('mediadatabase') . 'Resources/Public/CSS/Icons.css'));
            $this->response->addAdditionalHeaderData('<!-- crappy fluid/extbsae lack of pageRenderer -->');
         }

         /**
          * Wrap css files inside <link> tag
          *
          * @param string $cssFile Path to file
          * @return string <link.. string ready for <head> part
          */
          public function wrapCssFile($cssFile) {
             $cssFile = t3lib_div::resolveBackPath($cssFile);
            $cssFile = t3lib_div::createVersionNumberedFilename($cssFile);
            return '<link rel="stylesheet" type="text/css" href="' . htmlspecialchars($cssFile) . '" media="screen" />';
          }


