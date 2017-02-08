
.. include:: ../Includes.txt

===============
2015
===============


.. index:: TCA link field
.. _s2015-4:
.. _s2015-4-Link-Field-in-TCA-Typo3-7:

2015-4 Link Field in TCA Typo3 7
================================

by **Hassan Ali**, 2015-12-21 16:22:15, old #483, php

Keywords:
   TCA link field

Description:
   TCA fields are probably changed in Typo3 7.x.x
   So here is link field for TCA.

Code:
   .. code-block:: php

      'link_1' => array(
         'exclude' => 0,
         'label' => 'LLL:EXT:example/locallang_db.xml:tt_content.link',
         'config' => array(
            'type' => 'input',
            'size' => '30',
            'softref' => 'typolink',
            'wizards' => array(
               '_PADDING' => 2,
               'link' => array(
                   'type' => 'popup',
                   'title' => 'Link',
                   'icon' => 'EXT:example/Resources/Public/Images/FormFieldWizard/wizard_link.gif',
                   'module' => array(
                     'name' => 'wizard_element_browser',
                     'urlParameters' => array(
                        'mode' => 'wizard'
                     ) ,
                   ) ,
                   'JSopenParams' => 'height=600,width=500,status=0,menubar=0,scrollbars=1'
               )
            )
         )
      );


.. _s2015-3:
.. _s2015-3-Render-FAL-images-from-content-elements-and-give-them-different-css-Classes:

2015-3 Render FAL images from content elements and give them different css-Classes
==================================================================================

by **Sacha Vorbeck**, 2015-11-03 14:03:02, old #482, typoscript

Description:
   This snippets is designed to render one or multiple logos or headlines in the
   header section of a website. Editors can add the logos by creating content
   elements of type image/media in a certain page/folder. If they create a content
   elment of type TEXT, a headline will be added instead of a logo.
   One special requirement was that the first logo should be aligned left and
   the other logos right, so all logos after the first one must have an additional
   CSS-Class. As the params property of the IMAGE object doesn`t support
   optionSplit (which proably wouldn`t have worked anyway inside the
   render-Object), I used the optionSplit replace function.

Code:
   .. code-block:: typoscript

      lib.logo = COA
      lib.logo {
         10 < styles.content.get
         10 {
            select.pidInList = {$portal.pid.logos}
            renderObj = COA
            renderObj {
               10 = CASE
               10 {
                  key.field = CType
                  image = COA
                  image {
                     10 = FILES
                     10 {
                        references {
                           table = tt_content
                           uid.field = uid
                           fieldName = image
                        }
                        renderObj = IMAGE
                        renderObj {
                           file.import.data = file:current:publicUrl
                           altText.data = file:current:alternative
                           params = css_class
                        }
                     }
                  }
                  text = TEXT
                  text {
                     field = header
                     wrap = <span class="logoHeadline">|</span>
                  }
               }
            }
            stdWrap {
               replacement {
                  10 {
                     search = css_class
                     replace =  || class="right" |*| class="right"
                     useOptionSplitReplace = 1
                  }
               }
            }
         }
         stdWrap {
            wrap = <h1 class="clearfix">|</h1>
            typolink {
               parameter = {$portal.pid.root}
               addQueryString = 1
               ATagParams = class="startLink"
            }
         }
      }


.. index:: page
.. _s2015-2:
.. _s2015-2-Onepage-configuration:

2015-2 Onepage configuration
============================

by **Lars Messmer - comsolit AG**, 2015-10-16 17:23:28, old #481, typoscript

Keywords:
   page

Description:
   Example TypoScript for Onepage Website with TYPO3 based on pages

Code:
   .. code-block:: typoscript

      lib.onePage = CONTENT
      lib.onePage {
         table = pages
         select.orderBy = sorting

         renderObj = COA
         renderObj {

            10 = TEXT
            10.value = <section id="{field:subtitle}">
            10.insertData = 1

            100 = CONTENT
            100 {
               table = tt_content
               select {
                  pidInList.field = uid
                  orderBy = sorting
                  where = colPos = 0
               }
               wrap = <div class="content">|</div>
               wrap.insertData = 1
            }

            999 = TEXT
            999.value = </section>
         }
      }


.. _s2015-1:
.. _s2015-1-phone-view-helper-for-mobile-phone-link:

2015-1 phone view helper for mobile phone link
==============================================

by **Gert-jan Dikkescheij**, 2015-08-16 16:25:16, old #480, php

Description:
   save it with this name :
   PhoneViewHelper.php

   in this directory:
   typo3/sysext/fluid/Classes/ViewHelpers/Link/

   and use:
   <f:link.phone phone="0049 123 456 7890" />

Code:
   .. code-block:: php

      <?php
      namespace TYPO3\CMS\Fluid\ViewHelpers\Link;

      /*                                                                        *
       * This script is part of the TYPO3 project - inspiring people to share!  *
       *                                                                        *
       * TYPO3 is free software; you can redistribute it and/or modify it under *
       * the terms of the GNU General Public License version 2 as published by  *
       * the Free Software Foundation.                                          *
       *                                                                        *
       * This script is distributed in the hope that it will be useful, but     *
       * WITHOUT ANY WARRANTY; without even the implied warranty of MERCHAN-    *
       * TABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General      *
       * Public License for more details.                                       *
       *                                                                        */
      /**
       * Phone link view helper.
       * Generates a phone link
       *
       * = Examples
       *
       * <code title="basic phone link">
       * <f:link.phone phone="+49 123 456 7890" />
       * </code>
       * <output>
       * <a href="tel:+49 123 456 7890">+49 123 456 7890</a>
       * </output>
       *
       * <code title="Phone link with custom linktext">
       * <f:link.phone phone="+49 123 456 7890">some custom content</f:link.phone>
       * </code>
       * <output>
       * <a href="tel:+49 123 456 7890">some custom content</a>
       * </output>
       */
      class PhoneViewHelper extends \TYPO3\CMS\Fluid\Core\ViewHelper\AbstractTagBasedViewHelper {

         /**
          * @var string
          */
         protected $tagName = 'a';

         /**
          * Arguments initialization
          *
          * @return void
          */
         public function initializeArguments() {
            $this->registerUniversalTagAttributes();
            $this->registerTagAttribute('name', 'string', 'Specifies the name of an anchor');
            $this->registerTagAttribute('rel', 'string', 'Specifies the relationship between the current document and the linked document');
            $this->registerTagAttribute('rev', 'string', 'Specifies the relationship between the linked document and the current document');
            $this->registerTagAttribute('target', 'string', 'Specifies where to open the linked document');
         }

         /**
          * @param string $phone The phone number to be turned into a link.
          * @return string Rendered phone link
          */
         public function render($phone) {
            $linkHref = 'tel:' . $phone;
            $linkText = $phone;

            $tagContent = $this->renderChildren();
            if ($tagContent !== NULL) {
               $linkText = $tagContent;
            }
            $this->tag->setContent($linkText);
            $this->tag->addAttribute('href', $linkHref);
            $this->tag->forceClosingTag(TRUE);
            return $this->tag->render();
         }
      }


