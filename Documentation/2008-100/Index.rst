
.. include:: ../Includes.txt

===============
2008 76-100
===============


.. index:: CONTENT, TypoScript, condition, content, renderObj, if
.. _s2008-97:
.. _s2008-97-hide-some-specific-content-elements:

2008-97 hide some specific content elements
===========================================

by **Martin Holtz**, 2008-12-19 10:25:57, old #100, typoscript

Keywords:
   CONTENT, TypoScript, condition, content, renderObj, if

Description:
   if you want to enable or disable some content elements f.e. within an
   condition, you can use the if-statement for that.

   It should be esay to adapt that for your needs. Read TSref for that.

Links:
   http://documentation.typo3.org/documentation/tsref/functions/if/

Code:
   .. code-block:: typoscript

      [globalVar = GP:test = 1]
      # Shows only content elements, which have not
      # uid 23,24,25
      page.10 < styles.content.get
      page.10.renderObj.stdWrap.if {
        isInList.field = uid
        value = 23,24,25
        negate = 1
      }
      [end]


.. index:: 30, page_is_being_generated, CONFIG, message
.. _s2008-96:
.. _s2008-96-Change-Page-is-being-generated-Message:

2008-96 Change: "Page is being generated." Message
==================================================

by **Martin Holtz**, 2008-12-10 12:40:47, old #99, typoscript

Keywords:
   30, page\_is\_being\_generated, CONFIG, message

Description:
   It is possible via TypoScript.

Code:
   .. code-block:: typoscript

      config.message_page_is_being_generated (
                 Page is being generated.
                 If you don't see the page in 30 seconds, please reload
      )


.. index:: additionalHeaders, Location, redirect, TypoScript
.. _s2008-95:
.. _s2008-95-Header-redirect-Location-httptypo3org:

2008-95 Header redirect Location: http://typo3.org
==================================================

by **Erwin**, 2008-12-05 09:01:27, old #97, typoscript

Keywords:
   additionalHeaders, Location, redirect, TypoScript

Description:
   You can do header redirects via TypoScript.

   Thanks to gerards who told me:)

Code:
   .. code-block:: typoscript

      config >
      config.additionalHeaders = Location: http://www.martinholtz.de
      page >
      page = PAGE
      page.10 = TEXT


.. index:: CONTENT, renderObj, SQL, mm, mm-relation, DAM, TypoScript
.. _s2008-94:
.. _s2008-94-Read--values-of-MM-Tables-via-TypoScript-fe-DAM:

2008-94 Read  values of MM-Tables via TypoScript f.e. DAM
=========================================================

by **Dennis Grote**, 2008-12-03 11:01:41, old #96, typoscript

Keywords:
   CONTENT, renderObj, SQL, mm, mm-relation, DAM, TypoScript

Description:
   This snippets helps you to read via TypoScript the Categorys of an DAM-Record.

Code:
   .. code-block:: typoscript

      category = CONTENT
      category {
         table =  tx_dam_cat
         select {
            # the pid where the records are stored
            pidInList = 38
            selectFields = *
            join = tx_dam_mm_cat
            where = tx_dam_cat.uid = tx_dam_mm_cat.uid_foreign
            # field:uid should contain the uid of
            # the dam-record
            andWhere = tx_dam_mm_cat.uid_local = {field:uid}
            andWhere.insertData = 1
            languageField = sys_language_uid
         }
         renderObj = TEXT
         renderObj.field = title
         renderObj.wrap = |<br />
      }


.. index:: TV, FCE, tt_content
.. _s2008-93:
.. _s2008-93-Generate-Link-Lists-via-plain-text:

2008-93 Generate Link-Lists via plain-text
==========================================

by **Niels Fröhling**, 2008-11-21 23:34:24, old #95, php

Keywords:
   TV, FCE, tt\_content

Description:
   Could actually be officially integrated into css\_syled\_content.
   Generates Link-lists out of:

   Title \| http://google.com - - Google

Links:
   http://blog.frohling.biz/2008/11/typoscript-insane/

Code:
   .. code-block:: php

      10 = TEXT
      10.field = field_links
      10.wrap = <dl class="links">|</dl>
      10.if.value.current = 1
      10.if.equals =
      10.if.negate = 1
      10 {
         trim = 1
         split {
            token.char = 10
            cObjNum = 1

            1.parseFunc >

            # the negative match has to be done first, because the
            # following split actually changes the current-value
            1.1 = TEXT
            1.1.current = 1
            1.1.wrap = <dt>|</dt>
            1.1.if.value.current = 1
            1.1.if.matches = /\|/
            1.1.if.negate = 1

            # continue with destructive splitting
            1.2 = TEXT
            1.2.current = 1
            1.2.wrap = <dd>|</dd>
            1.2.if.value.current = 1
            1.2.if.matches = /\|/

            1.2.trim = 1
            1.2.split {
                  token = |
                  cObjNum = |*|1||2|*|

                  # we don't put the string into value, so we would not need to delete it
                  # if we would delete it with '>', the LOAD_REGISTER will not work
                  1.parseFunc >
                  1.1 = LOAD_REGISTER
                  1.1.title.current = 1

                  # we don't put the string into value, so we don't overwrite 2.1
                  # if we would overwrite 2.1, the RESTORE_REGISTER will kill all data
                  2.parseFunc >
                  2.2 = RESTORE_REGISTER
                  2.trim = 1

                  # so this is it, first fragment into value, second directly into typolink
                  # the order of execution is sorted, so don't worry about the strange order here
                  2.1 = TEXT
                  2.1.data = register:title
                  2.typolink {
                     parameter.current = 1
                     ATagParams = rel="shadowbox"
                     target =
                     extTarget =
                  }
            }
         }
      }


.. index:: icon
.. _s2008-92:
.. _s2008-92-Add-a-new-pageicon-under-different-conditions:

2008-92 Add a new pageicon under different conditions
=====================================================

by **Stefano Cecere**, 2008-11-21 14:19:47, old #94, php

Keywords:
   icon

Description:
   I needed to show different pageicons for pages with layout = 99. I solved it
   looking into the code of kb\_page\_icon and found a solution.

Code:
   .. code-block:: php

      /*
      First create a new extension, in this extension you hook into tcemain to check when pages get layout=99 set. When that happens you assign "ICON:cat" to the field "Module". When any other layout is set, you empty the field "Module".

      Then you define which image "ICON:cat" should point to, in ext_tables.php

      Of course you need to create some icons for this to work.
      */

      // FILE: class.user_pageicon_t3libtcemain.php:
      class user_pageicon_t3libtcemain {

      function processDatamap_postProcessFieldArray($status, $table, $id, &$fieldArray, &$pObj) {
      if ($table=='pages') {
      if ($fieldArray['layout'] == 99) {
      $fieldArray['module'] = 'ICON:cat';
      } elseif (isset($fieldArray['layout'])) {
      $fieldArray['module'] = '';
      }
      }
      }

      }

      //FILE: ext_tables.php
      if (!defined ('TYPO3_MODE')) die ('Access denied.');

      $data = array('ICON:cat' => array('icon' => '../'.t3lib_extMgm::siteRelPath('user_pageicon').'category_icons/pages.gif'));
      $ICON_TYPES = t3lib_div::array_merge_recursive_overrule($ICON_TYPES, $data);

      //FILE: ext_localconf.php
      if (!defined ('TYPO3_MODE')) die ('Access denied.');

      $GLOBALS['TYPO3_CONF_VARS']['SC_OPTIONS']['t3lib/class.t3lib_tcemain.php']['processDatamapClass'][] = 'EXT:user_pageicon/class.user_pageicon_t3libtcemain.php:&user_pageicon_t3libtcemain';


.. _s2008-91:
.. _s2008-91-Social-Bookmark-links-with-Typoscript:

2008-91 Social Bookmark links with Typoscript.
==============================================

by **Erik Svendsen**, 2008-11-13 20:49:41, old #93, typoscript

Description:
   An easy way to make links to social bookmarks on a page/website. Code could
   possible be cleaned up. Working example on http://home.linnearad.no/.

Code:
   .. code-block:: typoscript

      # get URL of the page
      temp.getLink = COA
      temp.getLink {
         10 = TEXT
              10.wrap = http://|/
              10.data = getIndpEnv: HTTP_HOST
              10.rawUrlEncode = 1
              20 = TEXT
              20.typolink.parameter = {page:uid}
              20.typolink.parameter.insertData = 1
              20.typolink.addQueryString = 1
              20.typolink.returnLast = url
              20.rawUrlEncode = 1
      }

      # get subtitle/title of the page
      temp.getTitle = COA
      temp.getTitle {
              10 = TEXT
              10.data = page : subtitle // page : title
              10.insertData = 1
              10.rawUrlEncode = 1
      }

      # make the share link with icon and text
      temp.socialBookmarks = COA
      temp.socialBookmarks {
          # Facebook
          10 = COA
          10 {
              10 = COA
              10.htmlSpecialChars = 1
              10.wrap = <li><a href="|" target="_blank">
              10 {
                  10 = TEXT
                  10.value = http://www.facebook.com/share.php
                  20 = COA
                  20.wrap = ?u=|
                  20 {
                     10 < temp.getLink
                  }
                  30 < temp.getTitle
                  30.wrap = &title=|
              }

              20 = IMAGE
              20.file = fileadmin/templates/eriksverden/icons/art-facebook.png
              20.altText = Del pÃ¥ Facebook
              20.titleText = Del pÃ¥ Facebook
              20.params = class="facebook"

              30 = TEXT
              30.value = <span>Facebook</span></a></li>
          }
          #Just repeat for Technorati etc.
      }


.. index:: HMENU, MENU, CASE, override, stdWrap, typoscript, LOAD_REGISTER, condition
.. _s2008-90:
.. _s2008-90-Some-menuitems-with-special-classes:

2008-90 Some menuitems with special classes
===========================================

by **Martin Holtz**, 2008-11-03 21:08:08, old #92, typoscript

Keywords:
   HMENU, MENU, CASE, override, stdWrap, typoscript, LOAD\_REGISTER, condition

Description:
   This is an example which describes, how to define an special class for some
   pages.

   I show two different methods, take that which is more usefull for you. The
   first method gives you the possibility to define some special classes for some
   pages, the second method only works, if you have only one different class.

   And for better debugging i put the class into the title.

   The main idea is, to put the class into an register, which can than inserted
   into the class or tilte-tag.

   While wrapItemAndSub has no stdWrap property, you have to do that insertData
   on allWrap.

Code:
   .. code-block:: typoscript

      lib.navigation = HMENU
      lib.navigation {
        1 = TMENU
        1 {
          noBlur = 1
          NO.allWrap = <li title="{register:class1} {register:class2}">|</li>
          NO.allWrap {
             prepend = LOAD_REGISTER
             prepend {
                      # first method
                class1.cObject = CASE
                class1.cObject {
                   key.field = uid
                   default = TEXT
                   default.value = defaultclass
                   458 = TEXT
                   458.value = classforpage458
                              459 = TEXT
                              459.value = classforpage459
            }
                      # second method
                      class2 = defaultclass
                      class2.override = classforpage458and459
                      class2.override.if.isInList.field = uid
                      class2.override.if.value = 458,459
         }
             insertData = 1
          }
          NO.stdWrap.htmlSpecialChars = 1
          ACT = 1
          ACT.allWrap < .NO.allWrap
          ACT.allWrap = <li title="{register:class1} {register:class2}">|
          ACT.wrapItemAndSub = |</li>
          wrap = <ul>|</ul>
        }
        2 < .1
        3 < .1
        4 < .1
        5 < .1
       }


.. index:: TMENU, css, if
.. _s2008-89:
.. _s2008-89-Extra-classes-for-linkitems-if-special-pages-are-inside:

2008-89 Extra classes for linkitems if special pages are inside
===============================================================

by **Georg Ringer**, 2008-10-19 14:25:39, old #91, typoscript

Keywords:
   TMENU, css, if

Description:
   Use this snippet to set extra classes to the link items by knowing the id or
   modify the if and use another field to compare

Code:
   .. code-block:: typoscript

      NO {
         ATagParams.stdWrap.cObject = COA
         ATagParams.stdWrap.cObject {
            10 = COA
            10 {
               10 = TEXT
               10.value = class="important"

               if {
                  value = 11
                  isInList.field = uid
               }
            }
            20 <.10
            20 {
               10.value = class="important2"
               if.value = 9,8
            }

         }
      }


.. index:: colPos, IMAGE, image
.. _s2008-88:
.. _s2008-88-Image-width-based-on-colPos:

2008-88 Image width based on colPos
===================================

by **Ben van 't Ende**, 2008-10-16 15:46:16, old #90, typoscript

Keywords:
   colPos, IMAGE, image

Description:
   This snippet provides different imagewidths for the different columns for the
   IMAGE element.

Code:
   .. code-block:: typoscript

      temp.tt_content.image < tt_content.image

      tt_content.image = CASE

      tt_content.image {

        key.field=colPos





        1 < temp.tt_content.image

        2 < temp.tt_content.image

        3 < temp.tt_content.image

        default < temp.tt_content.image





        1.20.maxW= 145 #col-02

        2.20.maxW= 190 #col-04

        3.20.maxW= 100

        default.20.maxW= 345

      }

      temp.tt_content.image >


.. index:: tt_news, title, page, FE
.. _s2008-87:
.. _s2008-87-News-title-as-page-title:

2008-87 News title as page title
================================

by **Thomas Loeffler**, 2008-10-13 14:44:01, old #89, php

Keywords:
   tt\_news, title, page, FE

Description:
   In the detail view of tt\_news, you can overwrite the page title to the title
   of the news record. Multilanguage support.

Code:
   .. code-block:: php

      [globalVar = TSFE:id = {$newsSinglePid}]
      temp.newsTitle = RECORDS
      temp.newsTitle {
      source = {GPvar:tx_ttnews|tt_news}
      source.insertData = 1
      tables = tt_news
      conf.tt_news >
      conf.tt_news = TEXT
      conf.tt_news.field=title
      wrap = <title>|</title>
      }
      page.config.noPageTitle = 2
      page.headerData.10 >
      page.headerData.10 < temp.newsTitle
      [global]


.. index:: tt_news, title, page, FE
.. _s2008-86:
.. _s2008-86-News-title-as-page-title:

2008-86 News title as page title
================================

by **Thomas**, 2008-10-13 14:42:42, old #88, typoscript

Keywords:
   tt\_news, title, page, FE

Description:
   In the detail view of tt\_news, you can overwrite the page title to the title
   of the news record. Multilanguage support.

Code:
   .. code-block:: typoscript

      [globalVar = TSFE:id = {$newsSinglePid}]
      temp.newsTitle = RECORDS
      temp.newsTitle {
      source = {GPvar:tx_ttnews|tt_news}
      source.insertData = 1
      tables = tt_news
      conf.tt_news >
      conf.tt_news = TEXT
      conf.tt_news.field=title
      wrap = <title>|</title>
      }
      page.config.noPageTitle = 2
      page.headerData.10 >
      page.headerData.10 < temp.newsTitle
      [global]


.. index:: typoscript, menu, coa, content
.. _s2008-85:
.. _s2008-85-Page-index--Content-elements-index--counter:

2008-85 Page index + Content elements index + counter
=====================================================

by **nubla**, 2008-10-10 16:17:24, old #87, typoscript

Keywords:
   typoscript, menu, coa, content

Description:
   This snippet will generate a list of pages with a counter next to it indicating
   how many active content elements there are on that page. It then lists the
   headers of those content elements. Useful if you don't want to create a whole
   tree of pages to do this and want to work with content elements.

Code:
   .. code-block:: typoscript

      // Page 1: 24010
      // Page 2: 24012
      // Page 3: 24013
      // Page 4: 24015
      temp.PageListIndex = COA
      temp.PageListIndex {
         // Function for 'Page1'
         10 = COA
         10 {
            // Get the title
            10 = CONTENT
            10 {
               table = pages
               select.uidInList = 24010
               renderObj = TEXT
               renderObj.field = title
               renderObj.wrap = <h6>|
               renderObj.typolink.parameter.field=uid
            }
            // Count how many active content elements with colPos=0
            20 = TEXT
            20 {
               numRows {
                  table = tt_content
                  select {
                     pidInList = 24010
                     where = colPos=0 AND hidden=0 AND deleted=0
                  }
               }
               noTrimWrap=| (active |)</h6>|
            }
            // List active 'headers'
            30 = CONTENT
            30 {
               table = tt_content
               select {
                  pidInList = 24010
                  orderBy = sorting
                  where = colPos=0 AND hidden=0 AND deleted=0
               }
               renderObj = TEXT
               renderObj {
                  field = header
                  wrap=<li>|</li>
                  typolink.parameter.field=pid
                  typolink.parameter.dataWrap=|#{field:uid}
                  if.isTrue.field=header
               }
               wrap = <ul>|</ul>
            }
         }

         // Copy the function for 'Page 2'
         20 < .10
         20.10.select.uidInList = 24012
         20.20.numRows.select.pidInList = 24012
         20.30.select.pidInList = 24012

         // Copy the function for 'Page 3'
         30 < .10
         30.10.select.uidInList = 24013
         30.20.numRows.select.pidInList = 24013
         30.30.select.pidInList = 24013

         // Copy the function for 'Page 4'
         40 < .10
         40.10.select.uidInList = 24015
         40.20.numRows.select.pidInList = 24015
         40.30.select.pidInList = 24015
      }


.. index:: link, extension, wizard
.. _s2008-84:
.. _s2008-84-Link-to-custom-records:

2008-84 Link to custom records
==============================

by **Stefano Cecere**, 2008-09-25 16:53:45, old #86, typoscript

Keywords:
   link, extension, wizard

Description:
   to make the link browser (both "normal" and "RTE") select custom records (say
   tt\_news for example) for , you can use extension "linkhandler"

   you can find it at TER.

   woks natively with TYPO3 4.2, but it also works with 4.1 (with XCLASSES)

   you can configure easily it to add custom tabs to the link browsers.
   at the following link there is the tutorial

Links:
   http://www.typo3-media.com/blog/article/extension-linkhandler.html

Code:
   .. code-block:: typoscript

      TSConfig:
      --------
      RTE.default.tx_linkhandler {
          tt_news {
              label=News
              listTables=tt_news
          }
      }

      mod.tx_linkhandler {
          tt_news {
              label=News
              listTables=tt_news
          }
      }

      TSSetup:
      -------
      plugin.tx_linkhandler {
          tt_news {
                  parameter={$linkhandler.newsSinglePid}
                  additionalParams=&tx_ttnews[tt_news]={field:uid}
                  additionalParams.insertData=1
                  useCacheHash=1
          }
      }


.. index:: fullRootLine, storage_pid
.. _s2008-83:
.. _s2008-83-Finding-the-UID-of-the-General-Record-Storage-page-using-TypoScript:

2008-83 Finding the UID of the General Record Storage page using TypoScript
===========================================================================

by **Peter Klein**, 2008-09-24 17:45:05, old #85, typoscript

Keywords:
   fullRootLine, storage\_pid

Description:
   Returns the UID of the "General Record Storage page"

   NOTE: If your site has MORE than 9 levels, you 'll have to change the
   "fullRootLine:9,storage\_pid,slide" to reflect that.

Code:
   .. code-block:: typoscript

      page.10 = TEXT
      page.10.data = fullRootLine:9,storage_pid,slide
      page.10.wrap = UID of <b>General Record Storage page:</b>&nbsp;|<br>


.. index:: COA_INT, date, cObject, prioriCalc, strftime
.. _s2008-82:
.. _s2008-82-Date-countdown:

2008-82 Date countdown
======================

by **Peter Klein**, 2008-09-24 17:43:03, old #84, typoscript

Keywords:
   COA\_INT, date, cObject, prioriCalc, strftime

Description:
   Create a countdown that displays the days until a specified date.
   This typoscript snippet simply outputs "It is now xx days until xxx" where
   xx is the days until the xxx date.

Code:
   .. code-block:: typoscript

      # Put following in the constants field:
      # The countdown date (In UNIX time format)
      lib.countdown.date = 1142095231

      # And put the following into the SETUP field:
      lib.countdown = COA_INT
      lib.countdown {
        10 = TEXT
        10.cObject = TEXT
        10.cObject.data = date:U
        10.cObject.wrap = ({$lib.countdown.date} -|)/86400
        10.prioriCalc = intval
        10.wrap = It is now&nbsp;|&nbsp;days until&nbsp;
        20 = TEXT
        20.value = {$lib.countdown.date}
        20.strftime = %d %B %y
      }


.. index:: search, htmlSpecialChars, HTMLparser
.. _s2008-81:
.. _s2008-81-Getting-rid-of-nbsp-shown-in-search-results-of-the-standard-search:

2008-81 Getting rid of &nbsp; shown in search results of the standard search
============================================================================

by **Peter Klein**, 2008-09-24 17:34:33, old #83, typoscript

Keywords:
   search, htmlSpecialChars, HTMLparser

Description:
   The standard TYPO3 search functions displays &nbsp; entities in the search
   result.
   This will fix the problem and display the &nbsp; entity as a space.

Code:
   .. code-block:: typoscript

      tt_content.search.20.renderObj.20.stdWrap.HTMLparser = 1
      tt_content.search.20.renderObj.20.stdWrap.HTMLparser.htmlSpecialChars = -1


.. index:: TCA, wizard, element browser
.. _s2008-80:
.. _s2008-80-Link-wizard-for-filemounts:

2008-80 Link wizard for filemounts
==================================

by **Steffen Kamper**, 2008-09-23 13:35:10, old #82, php

Keywords:
   TCA, wizard, element browser

Description:
   annoyed by typing foldernames to filemounts?
   So simply use the wizard!

   Copy this to your extTables.php and clear cache.

Code:
   .. code-block:: php

      $TCA['sys_filemounts']['columns']['path']['config']['wizards'] = array(
         '_PADDING' => 2,
         'link' => array (
            'type' => 'popup',
            'title' => 'Link',
            'icon' => 'link_popup.gif',
            'script' => 'browse_links.php?mode=wizard&amp;act=folder',
            'params' => array (
              'blindLinkOptions' => 'page,url,mail,spec,file',
            ),
            'JSopenParams' => 'height=300,width=500,status=0,menubar=0,scrollbars=1'
         )
      )


.. index:: special.rootline, COA, HMENU, headerData, title
.. _s2008-79:
.. _s2008-79-Rootline-Menu-in-title-Tag:

2008-79 Rootline Menu in <title>-Tag
====================================

by **Stefan Lang**, 2008-09-22 10:43:15, old #81, typoscript

Keywords:
   special.rootline, COA, HMENU, headerData, title

Description:
   This Snippet shows how to get a complete rootline-menu in your website head
   into the title-Tag

Code:
   .. code-block:: typoscript

      page {
              headerData {
                      5 = COA
                      5 {
                              wrap = <title>|</title>
                              10 = TEXT
                              10 {
                                      stdWrap.noTrimWrap = || - |
                                      value = Websitetitle
                              }
                              20 = HMENU
                              20 {
                                      special = rootline
                                      special.range = 1|-1
                                      1 = TMENU
                                      1 {
                                              NO {
                                                      stdWrap.field = subtitle // title
                                                      linkWrap = | - ||*|| - ||*||
                                                      doNotLinkIt = 1
                                              }
                                      }
                              }
                      }
              }
      }


.. index:: tt_news
.. _s2008-78:
.. _s2008-78-read-more-link-inline-with-text-of-the-news-abstract-v2:

2008-78 "read more" link inline with text of the news abstract v2
=================================================================

by **Peter Klein**, 2008-09-17 20:32:51, old #80, typoscript

Keywords:
   tt\_news

Description:
   Couldn't find a way to comment on the original snippet, so I had to create a
   new snippet.

   This is juss a more simple way of getting the morelink shown directly after
   the cropped text.

Code:
   .. code-block:: typoscript

      plugin.tt_news.displayList.subheader_stdWrap {
        stripHtml = 1
        crop = 230 | ... {register:newsMoreLink} | 1
        ifEmpty.field = bodytext
        insertData = 1
      }


.. index:: IMG_RESOURCE, IMAGE, png
.. _s2008-77:
.. _s2008-77-Using-Transparent-PNGs-in-IE56-without-JS:

2008-77 Using Transparent PNG's in IE5/6 without JS
===================================================

by **Peter Klein**, 2008-09-17 20:19:32, old #79, typoscript

Keywords:
   IMG\_RESOURCE, IMAGE, png

Description:
   Sometimes you need to make a site compatible with the old crappy IE5/6 when
   using transparent PNGs.

   People often resort to various Javascripts to convert the IMG tags into
   filters for IE.

   But the same can be done directly by Typoscript!
   Below is an example.

Code:
   .. code-block:: typoscript

      # This is the normal IMG tag shown in good browsers
      lib.pngImage = COA
      lib.pngImage {
        10 = IMAGE
        10.file = fileadmin/img/car.png
        10.file.import = uploads/tx_templavoila/
        10.file.import.field = field_image
        10.file.import.listNum = 0
        10.file.maxW = 600
      }

      # If the browser is IE, and the version lower than 7,
      # then we modify the lib.pngImage object
      [browser = msie] && [version=  <7]
      lib.pngImage = COA
      lib.pngImage {

        # first we create a dummy register value, in order to fill
        # the TSFE:lastImgResourceInfo array.
        20 = LOAD_REGISTER
        # The dummy object is just a copy of the original IMAGE object
        20.dummy.cObject < .10
        # But we change the type from IMAGE to IMG_RESOURCE
        20.dummy.cObject = IMG_RESOURCE

        # And unset the original 10 object
        10 >

        # Then we create a SPAN tag instead of the IMG tag.
        # The span tag uses the AlphaImageLoader filter
        # to display the transparent PNG correctly in IE.
        30 = TEXT
        30.value = <span style="width:{TSFE:lastImgResourceInfo|0}px;height:{TSFE:lastImgResourceInfo|1}px;display:inline-block;filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='{TSFE:lastImgResourceInfo|3}', sizingMethod='scale');"></span>
        30.insertData = 1
      }
      [GLOBAL]
      # Done! - Back to normal..


.. index:: image, css, menu, typoscript, GMENU
.. _s2008-76:
.. _s2008-76-noBlur-for-other-browsers-remove-dashed-line-around-linked-images:

2008-76 noBlur for other browsers, remove dashed line around linked images
==========================================================================

by **Cyrill Helg**, 2008-09-09 20:16:10, old #78, typoscript

Keywords:
   image, css, menu, typoscript, GMENU

Description:
   When you add the TS below, the dashed lines on a clicked link will disappear in
   all common browsers.

   Remember that this is bad for accessibility, because you can't select the
   image links with the tab key any longer. (You won't find out which on is
   selected)

Code:
   .. code-block:: typoscript

      //...
      1 = GMENU
      1.NO = 1
      1.NO {
      ATagParams = style="outline: none;"
      //...
      }


