
.. include:: ../Includes.txt

===============
2013
===============

.. index:: extbase, namespace, GET/POST, FE, login
.. _s2013-23:
.. _s2013-23-Trigger-fe-logout-in-Extbase-with-namespace:

2013-23 Trigger fe-logout in Extbase (with namespace)
=====================================================

by **Sabine Deeken**, 2013-12-06 11:36:56, old #467, php

Keywords:
   extbase, namespace, GET/POST, FE, login

Description:
   Sometimes you might want no react on Frontend-Logout (or any other GP-variable
   which does not belong to your extension-namespace), so this is the Code to make
   it work

Code:
   .. code-block:: php

      $logintype = '' .  \TYPO3\CMS\Core\Utility\GeneralUtility::_GP('logintype');

      if($logintype == 'logout'){
               //do something
            }


.. _s2013-22:
.. _s2013-22-TYPO3-61-way-of-eID:

2013-22 TYPO3 6.1 way of eID
============================

by **Fabien Udriot**, 2013-11-19 15:25:56, old #466, php

Description:
   Based from http://typo3.org/documentation/snippets/sd/455/ but with TYPO3 6.1
   compatibility.

Code:
   .. code-block:: php

      /** @var \TYPO3\CMS\Frontend\Controller\TypoScriptFrontendController $TSFE */
      $TSFE = \TYPO3\CMS\Core\Utility\GeneralUtility::makeInstance('TYPO3\CMS\Frontend\Controller\TypoScriptFrontendController', $GLOBALS['TYPO3_CONF_VARS'], 0, 0);

      // Initialize Language
      \TYPO3\CMS\Frontend\Utility\EidUtility::initLanguage();

      // Initialize FE User.
      $TSFE->initFEuser();

      // Important: no Cache for Ajax stuff
      $TSFE->set_no_cache();
      $TSFE->checkAlternativeIdMethods();
      $TSFE->determineId();
      $TSFE->initTemplate();
      $TSFE->getConfigArray();
      \TYPO3\CMS\Core\Core\Bootstrap::getInstance()->loadCachedTca();
      $TSFE->cObj = \TYPO3\CMS\Core\Utility\GeneralUtility::makeInstance('TYPO3\CMS\Frontend\ContentObject\ContentObjectRenderer');
      $TSFE->settingLanguage();
      $TSFE->settingLocale();


.. index:: pagebrowser, pager, pagination
.. _s2013-21:
.. _s2013-21-pagebrowser-for-content-elements:

2013-21 pagebrowser for content-elements
========================================

by **Michael Stein**, 2013-10-15 16:13:21, old #465, typoscript

Keywords:
   pagebrowser, pager, pagination

Description:
   Create a pagebrowser on the number of content-elements on a specific page.
   On first page you get a list of n full elements rendert by tt\_content.
   On the following pages you get a listview created by cropped bodytext.
   As candy the is a singleview of content-elements with backlink.

Code:
   .. code-block:: typoscript

      # constants:
      site {
        global_id {
          news_list =
          news_archiv =
          news_single =
        }

        pager {
          elements_on_first = 5
          elements_on_page = 3
        }

        text {
          back = &larr; zurück
        }
      }


      # setup:
      lib.pager = COA
      lib.pager {
        # pager
        10 = COA
        10 {

          # get number of pages
          20 = CONTENT
          20 {
            table = tt_content
            select {
              pidInList = {$site.global_id.news_archiv}
              selectFields = count(*) as number
            }
            renderObj = LOAD_REGISTER
            renderObj.number_of_pages {
              field = number
              stdWrap.dataWrap = (| - {$site.pager.elements_on_first}) / {$site.pager.elements_on_page}  + 1
              prioriCalc = 1
            }
          }

          # create links in pagebrowser
          # loop about the number_of_pages by using CONTENT
          30 = CONTENT
          30 {
            wrap = <ol class="resultbrowser">|</ol>
            table = tt_content
            select {
              pidInList = {$site.global_id.news_archiv}
              max.data = register:number_of_pages
            }
            renderObj = COA
            renderObj {
              wrap = <li class="resultbrowser-active">|</li>

              # first page without GPvars
              10 = COA
              10 {
                if.equals.data = cObj:parentRecordNumber
                if.value = 1
                10 = TEXT
                10 {
                  if.isFalse.data = GP:page
                  data = cObj:parentRecordNumber
                  wrap = <span class="chosen">|</span>
                }
                20 = TEXT
                20 {
                  if.isTrue.data = GP:page
                  data = cObj:parentRecordNumber
                  typolink {
                    parameter.data = TSFE:id
                    useCacheHash = 1
                  }
                }
              }

              # following pages
              20 = COA
              20 {
                if.isGreaterThan.data = cObj:parentRecordNumber
                if.value = 1
                10 = TEXT
                10 {
                  if.equals.data = GP:page
                  if.value.data = cObj:parentRecordNumber
                  data = cObj:parentRecordNumber
                  wrap = <span class="choosen">|</span>
                }
                20 = TEXT
                20 {
                  if.equals.data = GP:page
                  if.value.data = cObj:parentRecordNumber
                  if.negate = 1
                  data = cObj:parentRecordNumber
                  typolink {
                    parameter.data = TSFE:id
                    useCacheHash = 1
                    additionalParams.cObject = TEXT
                    additionalParams.cObject {
                      data = cObj:parentRecordNumber
                      wrap = &page=|
                    }
                  }
                }
              }
            }
          }
        }

        # content
        20 = COA
        20 {

          # calculate SQL-limit
          10 = LOAD_REGISTER
          10 {
            archive_max.cObject = COA
            archive_max.cObject {
              stdWrap.intval = 1
              10 = TEXT
              10 {
                if.isFalse.data = GP:page
                value = {$site.pager.elements_on_first}
              }
              20 = TEXT
              20 {
                if.isTrue.data = GP:page
                value = {$site.pager.elements_on_page}
              }
            }
            archive_begin.cObject = COA
            archive_begin.cObject {
              stdWrap.intval = 1
              10 = TEXT
              10 {
                if.isFalse.data = GP:page
                value = 0
              }
              20 = TEXT
              20 {
                if.isTrue.data = GP:page
                data = GP:page
                stdWrap.dataWrap = ((| - 2) * {GP:page}) + {$site.pager.elements_on_first}
                prioriCalc = 1
              }
            }
          }
          40 = CONTENT
          40 {
            table = tt_content
            select {
              pidInList = {$site.global_id.news_archiv}
              begin.data = register:archive_begin
              max.data = register:archive_max
              orderBy = sorting ASC
              where = colPos=0
            }
            renderObj = COA
            renderObj {

              # display whole content on first page
              10 = COA
              10 {
                if.isFalse.data = GP:page
                10 < tt_content
              }

              # create listview on following pages
              20 = TEXT
              20 {
                if.isTrue.data = GP:page
                field = bodytext
                cropHTML = 100| &hellip;

                typolink {
                  parameter = {$site.global_id.news_single}
                  useCacheHash = 1
                  additionalParams.cObject = COA
                  additionalParams.cObject {
                    10 = TEXT
                    10 {
                      wrap = &content=|
                      field = uid
                    }
                    20 = TEXT
                    20 {
                      if.isTrue.data = GP:page
                      data = GP:page
                      wrap = &page=|
                      intval = 1
                    }
                  }
                }
              }
            }
          }
        }
      }

      lib.singleView = COA
      lib.singleView {
        5 = TEXT
        5 {
          value = {$site.text.back}
          typolink {
            parameter = {$site.global_id.news_list}
            useCacheHash = 1
            additionalParams.cObject = TEXT
            additionalParams.cObject {
              if.isTrue.data = GP:page
              data = GP:page
              intval = 1
              wrap = &page=|

            }
          }
        }


        10 = CONTENT
        10 {
          table = tt_content
          select {
            pidInList = {$site.global_id.news_archiv}
            andWhere.cObject = TEXT
            andWhere.cObject {
              data = GP:content
              intval = 1
              wrap = uid=|
            }
          }
        }
      }


.. index:: MENU, TMENU, Shortcut, typoscript, typolink, doktype, active
.. _s2013-20:
.. _s2013-20-Menu-with-active-class-for-shortcuts:

2013-20 Menu with active class for shortcuts
============================================

by **Goran Medakovic**, 2013-10-07 03:12:55, old #464, typoscript

See update:
   :ref:`s2017-6`

Keywords:
   MENU, TMENU, Shortcut, typoscript, typolink, doktype, active

Description:
   Menu that recognizes when the user is on target page of the shortcut and wraps
   it with "active" class. Additionally all of the menu items that have sub-menus
   are appended with arrow span.

Code:
   .. code-block:: typoscript

      ## MAIN Navigation [Begin]
      lib.Menu = HMENU
      lib.Menu {
          ## FIRST LEVEL ##
          1 = TMENU
          1 {
              wrap = <ul>|</ul>
              expAll = 1
              noBlur = 1

              NO.wrapItemAndSub = <li class="first">|</li>|*|<li>|</li>|*|<li class="last">|</li>

              # Now override class if the shortcut is pointing to the current page
              NO.wrapItemAndSub.override.cObject = COA
              NO.wrapItemAndSub.override.cObject {
                  if {
                      value = 4
                      equals.field = doktype
                      isTrue = 1
                      isTrue.if {
                          value.data = TSFE:page|uid
                          equals.field = shortcut
                      }
                  }
                  10 = TEXT
                  10.value = <li class="first active">|</li>|*|<li class="active">|</li>|*|<li class="last active">|</li>
              }

              ACT = 1
              ACT.wrapItemAndSub = <li class="first active">|</li>|*|<li class="active">|</li>|*|<li class="last active">|</li>

              CUR < .ACT

              IFSUB = 1
              IFSUB.wrapItemAndSub = <li class="first">|</li>|*|<li>|</li>|*|<li class="last">|</li>
              IFSUB.linkWrap = |<span class="arrow"></span>
              IFSUB.ATagBeforeWrap = 1

              ACTIFSUB = 1
              ACTIFSUB < .ACT
              ACTIFSUB.linkWrap = |<span class="arrow"></span>
              ACTIFSUB.ATagBeforeWrap = 1

              CURIFSUB = 1
              CURIFSUB < .ACTIFSUB

          }

          ## next LEVELs ##
          2 < .1
          3 < .1
          4 < .1
      }
      ## MAIN Navigation [End]


.. _s2013-19:
.. _s2013-19-Image-width-based-on-backend_layout-respecting-rootline-and-colPos:

2013-19 Image width based on backend_layout (respecting rootline) and colPos
============================================================================

by **Lorenz Ulrich**, 2013-10-02 17:15:11, old #463, typoscript

Description:
   When different backend layouts are in use, they sometimes share the same column
   positions, but these columns don't necessarily have the same dimensions. So the
   settings for the maximum image width must be defined respecting the backend
   layout and the current colPos. This can be done with nested CASE objects.

Code:
   .. code-block:: typoscript

      tt_content.image.20 {
        maxW >
        maxWInText >
        maxW.cObject = CASE
        maxW.cObject {
          key.data = levelfield:-1, backend_layout_next_level, slide
          key.override.field = backend_layout
          # Backend layout with id=1
          1 = CASE
          1 {
            key.field = colPos
            # colPos=0
            0 = TEXT
            0.value = 310
            1 = TEXT
            1.value = 310
            3 = TEXT
            3.value = 220
            2 = TEXT
            2.value = 680
          }
          # copy the values from backend layout 1
          2 < .1
          2 {
            # adjust where necessary
            1.value = 430
            2.value = 430
          }
        }

        # copy the same configuration to maxWInText; can be overridden afterwards
        maxWInText < .maxW

      }


.. index:: pageTree, contextMenu, tsconfig
.. _s2013-18:
.. _s2013-18-Hide-Branch-Actions-in-ContextMenu-PageTree:

2013-18 Hide Branch Actions in ContextMenu (PageTree)
=====================================================

by **Koen Van Nuffelen**, 2013-09-13 15:43:43, old #462, typoscript

Keywords:
   pageTree, contextMenu, tsconfig

Description:
   If you want to hide "Branch Actions" in the contecxt menu.(click menu in
   PageTree)
   Set in user TSConfig

   Watch out, if you use it in a group and you are as admin member of this
   group, you have to overrule this in the admin (group) TSConfig
   options.contextMenu.table.pages.disableItems =

Code:
   .. code-block:: typoscript

      options.contextMenu.table.pages.disableItems = exportT3d,importT3d,expandBranch,collapseBranch


.. index:: flexform, tx_news, page-TSconig
.. _s2013-17:
.. _s2013-17-disable-fields-and-sheets-in-flexform-of-tx_news:

2013-17 disable fields and sheets in flexform of tx_news
========================================================

by **Wolfgang Maschke**, 2013-08-29 16:53:48, old #461, typoscript

Keywords:
   flexform, tx\_news, page-TSconig

Description:
   You can disable single fields of flexform.
   When disabling all fields of a sheet the sheet is disabled completly.

Code:
   .. code-block:: typoscript

      TCEFORM.tx_news_domain_model_news {
         istopnews.disabled = 1
         teaser.disabled = 1
         bodytext.disabled = 1
         author.disabled = 0
         categories.disabled = 1
         related.disabled = 1
         relatedFrom.disabled = 1
         tags.disabled = 1
         keywords.disabled = 1
         related_from.disabled = 1
         media.disabled = 1
         media_records.disabled = 1
         related_files.disabled = 1
         related_links.disabled = 1
      }


.. index:: PID pass JS TYPO3 typoscript
.. _s2013-16:
.. _s2013-16-Fastest-way-to-pass-page-ID-to-JS:

2013-16 Fastest way to pass page ID to JS
=========================================

by **Michal Wojciul**, 2013-08-09 15:42:59, old #460, typoscript

Keywords:
   PID pass JS TYPO3 typoscript

Description:
   Just add a marker in your HTML template as below, and few more lines to
   template setup. Remember to launch you JS code on $(document).ready(function()
   {}) , then you have pageId variable available.
   Easy as that!

Code:
   .. code-block:: typoscript

      In HTML:

      <script language="JavaScript">
          var pageId = '###PID###';
      </script>


      In TypoScript:

      PID = TEXT
      PID.data = field:uid


.. index:: HTML, tt_content, wrap
.. _s2013-15:
.. _s2013-15-RemoveWrap-arount-HTML-Element:

2013-15 RemoveWrap arount HTML Element
======================================

by **Christian Wolff**, 2013-08-01 10:38:54, old #459, typoscript

Keywords:
   HTML, tt\_content, wrap

Description:
   This Snippet Removes the outer wrap around the HTML Content Element (<div
   id="c99">\|</div>) it is possible to add other content elements as well by
   expanding the isInList query.

   this is tested with typo3 4.5

Code:
   .. code-block:: typoscript

      /* only appy the wrap if the conditions is true */
      tt_content.stdWrap.innerWrap.if {
        /* compare the current elements Ctype */
        value.field = CType
        /* with elements in list */
        isInList = html
        /* negate the result as we want to wrap every element exept eleents in list */
        negate = 1
      }


.. index:: ce, wrap, last, first
.. _s2013-14:
.. _s2013-14-Wrap-Content-Elements:

2013-14 Wrap Content-Elements
=============================

by **Sven Lubenau**, 2013-07-26 15:19:18, old #458, typoscript

Keywords:
   ce, wrap, last, first

Description:
   A Short Snippet for wrap all Content-Elements on one Page.

Code:
   .. code-block:: typoscript

      tt_content.stdWrap.outerWrap = |###SPLIT###

      lib.content = COA
      lib.content {
        10 < styles.content.get
        stdWrap.split {
          token = ###SPLIT###
          cObjNum = 1 |*| 2 |*| 3 || 4
          1.current = 1
          1.wrap =   <div class="first-ce">|</div>

          2.current = 1
          2.wrap = |

          3.current = 1
          3.wrap =   <div class="last-ce">|</div>

          4.current = 1
        }
      }


.. index:: facebook, open graph, meta
.. _s2013-13:
.. _s2013-13-Add-Facebook-Open-Graph-metatags-for-tt_news-articles:

2013-13 Add Facebook Open Graph metatags for tt_news articles
=============================================================

by **Florian Seirer**, 2013-07-16 12:48:50, old #457, typoscript

Keywords:
   facebook, open graph, meta

Description:
   These tags are parsed by facebook when users post a link to a tt\_news article,
   see http://ogp.me/

   This script generates metatags for:
   - og:title
   - og:description
   - og:url
   - og:image

   You can test it here: http://developers.facebook.com/tools/debug

Code:
   .. code-block:: typoscript

      page.headerData {
        30 = RECORDS
        30 {
          source = {GP:tx_ttnews|tt_news}
          source.insertData = 1
          tables = tt_news
          conf.tt_news >
          conf.tt_news = TEXT
          conf.tt_news {
            field = title
            wrap = <meta property="og:title" content="|">
            htmlSpecialChars = 1
          }
        }
        31 = HTML
        31.value.char = 10
        32 = TEXT
        32 {
          data = register:newsSubheader
          wrap = <meta property="og:description" content="|">
          htmlSpecialChars = 1
        }
        33 < .31
        34 = RECORDS
        34 {
          source = {GP:tx_ttnews|tt_news}
          source.insertData = 1
          tables = tt_news
          conf.tt_news >
          conf.tt_news = TEXT
          conf.tt_news {
            typolink {
              parameter = {$tt_news-single-uid}
              additionalParams.cObject = TEXT
              additionalParams.cObject {
                field = uid
                wrap = &tx_ttnews[tt_news]=|
              }
              returnLast = url
            }
            wrap = <meta property="og:url" content="{TSFE:baseUrl}|">
            insertData = 1
          }
        }
        35 < .31
        36 = CONTENT
        36 {
          stdWrap.if.isTrue.data = GP:tx_ttnews|tt_news
          table = tt_news
          select {
            pidInList = {$tt_news-container-uid}
            recursive = {$tt_news-container-uid}
            where = CHAR_LENGTH(image) > 0
            andWhere.cObject = TEXT
            andWhere.cObject {
              data = GP:tx_ttnews|tt_news
              intval = 1
              wrap = uid = |
            }
          }
          renderObj = TEXT
          renderObj {
            field = image
            split {
              token = ,
              cObjNum = 1
              1.cObject = IMG_RESOURCE
              1.cObject {
                file {
                  import = uploads/pics/
                  import.current = 1
                  width = 400m
                  height = 400m
                }
                stdWrap.wrap = <meta property="og:image" content="{TSFE:baseUrl}|">
                stdWrap.insertData = 1
              }
            }
          }
        }
        37 < .31
      }


.. index:: bodyTagCObject
.. _s2013-12:
.. _s2013-12-Page-Media-FAL-image-as-body-tag-inline-css-background:

2013-12 Page Media (FAL) image as <body> tag inline css background
==================================================================

by **Tanel Põld**, 2013-07-16 09:06:31, old #456, typoscript

Keywords:
   bodyTagCObject

Code:
   .. code-block:: typoscript

      page.bodyTagCObject = COA
      page.bodyTagCObject {
        10 = FILES
        10 {
          references {
            data = levelmedia:-1, slide
          }
          renderObj = COA
          renderObj {
            10 = IMG_RESOURCE
            10 {
              file.import.data = file:current:publicUrl
              # file.maxW = 1600
              # file.maxH = 1600
            }
          }
        }
      }
      page.bodyTagCObject.wrap = <body style="background: url(|) no-repeat 0 30% fixed; -webkit-background-size: cover; -moz-background-size: cover; -o-background-size: cover; background-size: cover; background-color: #000;">


.. index:: Ajax, extbase, eid
.. _s2013-11:
.. _s2013-11-TYPO3-6X-way-of-eID:

2013-11 TYPO3 6.X way of eID
============================

by **Martin Muzatko**, 2013-06-19 12:08:02, old #455, php

Keywords:
   Ajax, extbase, eid

Description:
   in T3 6.x all the classes from 4.x still work! however, they moved around a lot
   and added a very important feature: namespaces!

   API can be found here: http://typo3.org/documentation/api/

   This is the new "Way" to get your eID working in your extension.

Code:
   .. code-block:: php

      <?php

      //Alias long namespaces to use shorter ones.
      use \TYPO3\CMS\Core as Core;
      use \TYPO3\CMS\Extbase\Utility as Utility;
      use \TYPO3\CMS\Frontend as Frontend;

      #$a = require_once(Core\Utility\ExtensionManagementUtility::extPath('np_viewer'));
      /* INITIALIZE */
      // Basic TSFE Setup - get all the Page data you may need.
      $TSFE = Core\Utility\GeneralUtility::makeInstance('TYPO3\CMS\Frontend\Controller\TypoScriptFrontendController', $TYPO3_CONF_VARS, 0, 0);
      Frontend\Utility\CoreUtility::initLanguage();

      // Get FE User Information
      $TSFE->initFEuser();
      // Important: no Cache for Ajax stuff
      $TSFE->set_no_cache();
      $TSFE->checkAlternativCoreMethods();
      $TSFE->determinCore();
      $TSFE->initTemplate();
      $TSFE->getConfigArray();
      Core\Core\Bootstrap::getInstance()->loadCachedTca();
      $TSFE->cObj = Core\Utility\GeneralUtility::makeInstance('TYPO3\CMS\Frontend\ContentObject\ContentObjectRenderer');
      $TSFE->settingLanguage();
      $TSFE->settingLocale();

      /* ACTUAL SCRIPT */

      //...

      ?>


.. index:: static_info_tables, sys_language
.. _s2013-10:
.. _s2013-10-Dynamic-Language-Selection:

2013-10 Dynamic Language Selection
==================================

by **Lars Peipmann**, 2013-05-31 11:16:05, old #454, typoscript

Keywords:
   static\_info\_tables, sys\_language

Description:
   Gets page tanslations and renders a language selection with the available
   languages for the current page.

   Depends: static\_info\_tables
   Output: de \| en
   (de is default language (constants: defaultLanguage = de), en is a
   translation)

Code:
   .. code-block:: typoscript

      lib.languageSelection = COA
      lib.languageSelection {
         wrap = <ul> | </ul>

         10 = CONTENT
         10 {
            table = static_languages
            select {
               selectFields (
                  pages.title AS page_title,
                  sys_language.uid AS sys_language_uid,
                  static_languages.lg_name_local,
                  static_languages.lg_iso_2,
                  static_languages.lg_name_en
               )
               leftjoin (
                  sys_language ON (sys_language.static_lang_isocode = static_languages.uid)
                  LEFT JOIN pages ON (pages.uid = '{page:uid}')
               )
               leftjoin.insertData = 1
               pidInList = 0
               where = static_languages.lg_iso_2=UPPER('{$defaultLanguage}')
            }
            renderObj = TEXT
            renderObj {
               wrap = <li> | </li>
               wrap.override = <li class="active"> | </li>
               wrap.override.if.isFalse.data = TSFE:sys_language_uid
               field = lg_iso_2
               typolink {
                  parameter.data = page:uid
                  additionalParams = &L=0
                  addQueryString = 1
                  addQueryString.exclude = L,id,cHash,no_cache
                  addQueryString.method = GET
                  useCacheHash = 1
                  title = {field:lg_name_local}: {field:page_title}
                  title.insertData = 1
               }
            }
         }

         20 = CONTENT
         20 {
            table = pages_language_overlay
            select {
               selectFields (
                  pages_language_overlay.title AS page_title,
                  sys_language.uid AS sys_language_uid,
                  static_languages.lg_name_local,
                  static_languages.lg_iso_2,
                  static_languages.lg_name_en
               )
               leftjoin (
                  sys_language ON (pages_language_overlay.sys_language_uid = sys_language.uid)
                  LEFT JOIN static_languages ON (sys_language.static_lang_isocode = static_languages.uid)
               )
            }
            renderObj = TEXT
            renderObj {
               wrap = <li> | </li>
               wrap.override = <li class="active"> | </li>
               wrap.override.if {
                  equals.data = TSFE:sys_language_uid
                  value.field = sys_language_uid
               }
               field = lg_iso_2
               typolink {
                  parameter.data = page:uid
                  additionalParams = &L={field:sys_language_uid}
                  additionalParams.insertData = 1
                  addQueryString = 1
                  addQueryString.exclude = L,id,cHash,no_cache
                  addQueryString.method = GET
                  useCacheHash = 1
                  title = {field:lg_name_local}: {field:page_title}
                  title.insertData = 1
               }
            }
         }
      }


.. index:: php hello
.. _s2013-9:
.. _s2013-9-Hello-World:

2013-9 Hello World
==================

by **Philippe COURT**, 2013-05-13 14:55:27, old #453, php

Keywords:
   php hello

Description:
   Description de mon Hello world

Code:
   .. code-block:: php

      <?php

      echo "Hello World !";

      ?>


.. _s2013-8:
.. _s2013-8-IE-Classes-in-HTML-Tag:

2013-8 IE-Classes in HTML-Tag
=============================

by **Lars Peipmann**, 2013-05-03 11:34:17, old #452, typoscript

Description:
   Output is:

   <!DOCTYPE html>
   <!--[if lt IE 7 ]><html lang="de" class="no-js ie6 oldie"><![endif]-->
   <!--[if IE 7 ]><html lang="de" class="no-js ie7 oldie"><![endif]-->
   <!--[if IE 8 ]><html lang="de" class="no-js ie8 oldie"><![endif]-->
   <!--[if IE 9 ]><html lang="de" class="no-js ie9"><![endif]-->
   <!--[if (gt IE 9)\|!(IE)]><!--><html lang="de"
   class="no-js"><!--<![endif]-->

Code:
   .. code-block:: typoscript

      config {
         htmlTag_stdWrap {
            setContentToCurrent = 1
            cObject = COA
            cObject {
               temp = TEXT
               temp.addParams.class = no-js
               temp.append = TEXT
               temp.append.char = 10
               temp.current = 1

               # Kleiner IE7
               10 < .temp
               10.addParams.class = no-js ie6 oldie
               10.wrap = <!--[if lt IE 7 ]>|<![endif]-->

               # IE7
               20 < .temp
               20.addParams.class = no-js ie7 oldie
               20.wrap = <!--[if IE 7 ]>|<![endif]-->

               # IE8
               30 < .temp
               30.addParams.class = no-js ie8 oldie
               30.wrap = <!--[if IE 8 ]>|<![endif]-->

               # IE9
               40 < .temp
               40.addParams.class = no-js ie9
               40.wrap = <!--[if IE 9 ]>|<![endif]-->

               # IE10
               #50 < .temp
               #50.addParams.class = no-js ie10
               #50.wrap = <!--[if IE 10 ]>|<![endif]-->

               # Kein IE oder größer IE9
               60 < .temp
               60.wrap = <!--[if (gt IE 9)|!(IE)]><!--> # <!--<![endif]-->
               60.wrap.splitChar = #
            }
         }
      }


.. index:: extbase, eid
.. _s2013-7:
.. _s2013-7-Extbase-repository-in-eID:

2013-7 Extbase repository in eID
================================

by **Felix Nagel**, 2013-03-19 19:54:19, old #451, php

Keywords:
   extbase, eid

Description:
   How to use an extbase repository within an eID with minimum footprint (but the
   need to do everything manually).

Code:
   .. code-block:: php

      <?php

      // get instance of object manager
      $objectManger = t3lib_div::makeInstance('Tx_Extbase_Object_ObjectManager');

      // get repository
      $repository = $objectManger->get('Tx_MyPlugin_Domain_Repository_DummyRepository');

      $newObject = new Tx_MyPlugin_Domain_Model_Dummy;

      $newObject->setPid(123);

      // hidden, deleted, crdate, tstamp, etc. need to be added as property (incl. setter / getter) in your model

      // add these in TCA as passthrough
      $newObject->setCrdate($GLOBALS['SIM_ACCESS_TIME']);
      $newObject->setTstamp($GLOBALS['SIM_ACCESS_TIME']);

      // add object
      $repository->add($newObject);

      // save object
      $objectManger->get('Tx_Extbase_Persistence_Manager')->persistAll();

      ?>


.. _s2013-6:
.. _s2013-6-Dynamic-header-image-for-TYPO3-6x:

2013-6 Dynamic header image for TYPO3 6.x
=========================================

by **Sacha Vorbeck**, 2013-03-13 14:53:09, old #450, typoscript

Description:
   Renders a CSS background image. The script either uses a default image defined
   by a constant or an image that is linked in the media field of the current page
   or any other page higher in the rootline.

Code:
   .. code-block:: typoscript

      lib.component.document.atmopic = COA
      lib.component.document.atmopic {
         10 = IMG_RESOURCE
         10 {
            file {
               import {
                  cObject = TEXT
                  cObject {
                     value = {$portal.component.atmopic.filename}
                     override {
                        required = 1
                        cObject = FILES
                        cObject {
                           references.data = levelmedia:-1, slide
                           references.listNum = 0
                           renderObj = TEXT
                           renderObj.data = file:current:publicUrl:url('|')";
                        }
                     }
                  }
               }
            }
         }
         stdWrap.wrap = style="background-image:url('|')";
      }


.. index:: page, replace, webperf
.. _s2013-5:
.. _s2013-5-HTML-render-optimization:

2013-5 HTML render optimization
===============================

by **Julien VITTE**, 2013-03-06 10:44:00, old #449, typoscript

Keywords:
   page, replace, webperf

Description:
   Optimize the page load by deleting unnecessary characters in the output  HTML
   content

Code:
   .. code-block:: typoscript

      ############
      # In setup #
      ############
      temp.html_minifier {
         10 {
            search = /[\n\r\t]+/
            useRegExp = 1
            replace.char = 32
         }
      }
      # Use it in your page render
      page = PAGE
      page.stdWrap.replacement < temp.html_minifier


.. index:: TypoScript
.. _s2013-4:
.. _s2013-4-Sort-tt_news-list-by-the-last-modified-date:

2013-4 Sort tt_news list by the last modified date
==================================================

by **Arun Chandran**, 2013-02-14 12:13:48, old #448, typoscript

Keywords:
   TypoScript

Description:
   By default the tt\_news extension shows the last added tt\_news first. For
   displaying the last modified news item first add this code in TS template
   setup.

Code:
   .. code-block:: typoscript

      plugin.tt_news.listOrderBy = tstamp desc


.. index:: extbase, boolean, checkbox
.. _s2013-3:
.. _s2013-3-Validate-checkbox-in-extbase:

2013-3 Validate checkbox in extbase
===================================

by **Felix Nagel**, 2013-02-13 16:56:34, old #447, php

Keywords:
   extbase, boolean, checkbox

Description:
   With this validation snippet you could validate if a checkbox is checked.

Code:
   .. code-block:: php

      @validate RegularExpression(regularExpression=/1/)


.. index:: extbase, boolean, checkbox
.. _s2013-2:
.. _s2013-2-Validate-checkbox-in-extbase:

2013-2 Validate checkbox in extbase
===================================

by **Felix Nagel**, 2013-02-13 16:56:10, old #446, php

Keywords:
   extbase, boolean, checkbox

Description:
   With this validation snippet you could validate if a checkbox is checked.

Code:
   .. code-block:: php

      @validate RegularExpression(regularExpression=/1/)


.. index:: TCA, showitem, dynaflex, override, TSconfig
.. _s2013-1:
.. _s2013-1-dynaflex-showitem-TSConfig-override:

2013-1 dynaflex showitem TSConfig override
==========================================

by **Gunther**, 2013-01-19 16:44:28, old #445, typoscript

Keywords:
   TCA, showitem, dynaflex, override, TSconfig

Description:
   overrides the TCA showitem collumn with TSConfig

Code:
   .. code-block:: typoscript

      dynaflex.tt_news.0{
       parseXML = 0
       modifications.0{
        method = remove
        element= showitem
        inside = tt_news/types/0
       }
      }
      dynaflex.tt_news.1{
       path = tt_news/types/0/showitem
       parseXML = 0
       modifications.0{
        method = add
        type = append
       }
      }

      dynaflex.tt_news.1.modifications.0.config.text(

      hidden, type;;;;1-1-1,title;;;;2-2-2,bodytext;;2;richtext:rte_transform[flag=rte_enabled|mode=ts];4-4-4,--div--;LLL:EXT:tt_news/locallang_tca.xml:tt_news.tabs.extended

      )

      localconf:

      $GLOBALS['T3_VAR']['ext']['dynaflex']['tt_news'][] = 'TS';


