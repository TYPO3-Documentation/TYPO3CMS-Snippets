
.. include:: ../Includes.txt

===============
2008 51-75
===============

.. index:: typoscript, page, title
.. _s2008-75:
.. _s2008-75-change-order-of-default-title-in-browser:

2008-75 change order of default title (in browser)
==================================================

by **Cyrill Helg**, 2008-09-09 20:10:00, old #77, typoscript

Keywords:
   typoscript, page, title

Description:
   This TS option lets you change the order of the browser title, from:
   Static site title: pagetitle -> pagetitle: Static site title

Code:
   .. code-block:: typoscript

      //small but maybe helpful
      config.pageTitleFirst = 1


.. index:: typoscript, page, css
.. _s2008-74:
.. _s2008-74-dynamic-css-file-with-markers:

2008-74 dynamic css file with markers
=====================================

by **Cyrill Helg**, 2008-09-09 20:04:48, old #76, typoscript

Keywords:
   typoscript, page, css

Description:
   The following TS let you have markers in your css file. This is parsed and then
   included in your site.

   So you CSS file can contain entries like:
   color:###color###
   font-size: ###font\_size###

   The CSS File in parsed with a PAGE object.

Code:
   .. code-block:: typoscript

      dynamicCssFile = PAGE
      dynamicCssFile {
        //the same typeNum is used to include the file, see below
        typeNum = 108
        config {
          disableAllHeaderCode = 1
          additionalHeaders = Content-Type:text/css
        }
        10 = TEMPLATE
        10 {
          template = FILE
          template.file = fileadmin/yourDynamicCSSFile.css
          marks.color = TEXT
          //uses a constant for example, you can get the value from everywhere
          marks.color.dataWrap = {$css.bg_color}
        }
        //you could even add more than one file here
      }

      //now the parsed object is added to your page
      page.headerData = COA
      //use any "free" number
      page.headerData.809 = TEXT
      page.headerData.809.value = <link rel="stylesheet" type="text/css" href="./index.php?type=108" />


.. index:: tt_news
.. _s2008-73:
.. _s2008-73-read-more-link-inline-with-text-of-the-news-abstract:

2008-73 "read more" link inline with text of the news abstract
==============================================================

by **Ben van 't Ende**, 2008-09-09 10:48:19, old #75, typoscript

Keywords:
   tt\_news

Description:
   Default the "read more" link is diplayed on a new line. This piece of
   typoscript provides a means to show that link right after the text. tHNx Rupert

Code:
   .. code-block:: typoscript

      subheader_stdWrap {
        stripHtml = 1
        crop = 230 | ... | 1
        ifEmpty.field = bodytext

        # the "more" link is directly appended to the subheader
        append = TEXT
        append.data = register:newsMoreLink
        append.wrap = <span class="news-list-morelink">|</span>

        # display the "more" link only if the field bodytext contains something
        append.if.isTrue.field = bodytext

        outerWrap = <p>|</p>
      }


.. index:: date, today's date
.. _s2008-72:
.. _s2008-72-Get-Todays-date:

2008-72 Get Today's date
========================

by **Jarnail Singh Dhillon**, 2008-09-08 12:42:48, old #74, typoscript

Keywords:
   date, today's date

Description:
   Displaying today's date on the webpage

Code:
   .. code-block:: typoscript

      #COA_INT to prevent cache
      lib.date = COA_INT
      lib.date {
         data = date:U
         strftime= %e. %B %Y
      }


.. index:: localconf.php, extTables.php, typo_db_extTableDef_script, TCA, fields, standard, advanced, pages
.. _s2008-71:
.. _s2008-71-Fields-keyword-and-description-should-be-shown-on-an-standard-Page:

2008-71 Fields keyword and description should be shown on an standard Page
==========================================================================

by **Martin Holtz**, 2008-09-04 16:14:18, old #72, php

Keywords:
   localconf.php, extTables.php, typo\_db\_extTableDef\_script, TCA, fields,
   standard, advanced, pages

Description:
   In TYPO3 4.2. the page "Advanced" has been removed.

   You do not need to use it in TYPO3 < 4.2 because you can add the fields you
   need into the Standard Page-View.

   Simple change via ext\_Tables.php the TCA for Page "Standard" and hide Page
   "Advanced" for Non-Admins.

Code:
   .. code-block:: php

      In your localconf.php you need a line:
      $typo_db_extTableDef_script = 'extTables.php';

      Than you can use extTables.php to overwrite TCA Settings (and more;)

      extTables.php :


      $TCA['pages']['types']['1']['showitem'] = 'hidden;;;;1-1T-1, doktype;;2;button, title;;3;;2-2-2, subtitle, nav_hide,  --div--, keywords, description, Sconfig;;6;nowrap;5-5-5, storage_pid;;7, l18n_cfg, tx_rlmptmplselector_main_tmpl;;;;1-1-1, tx_rlmptmplselector_ca_tmpl, tx_mcgooglesitemap_priority;;;;1-1-1, tx_mcgooglesitemap_changefreq, tx_cssselect_stylesheets;;;;1-1-1';

      # "--div--, keywords, description" has been added to page "standard".


.. index:: t3lib_extMgm, ext_tables, addToInsertRecords, insert record, typoscript
.. _s2008-70:
.. _s2008-70-insert-record-from-your-plugin:

2008-70 insert record from your plugin
======================================

by **Martin Holtz**, 2008-09-02 10:25:40, old #71, php

Keywords:
   t3lib\_extMgm, ext\_tables, addToInsertRecords, insert record, typoscript

Description:
   If you want to allow your table to be inserted via "insert record" content
   element, you have to allow it via addToInsertRecords in ext\_tables.php.

   Keep in Mind, that you should then define via TypoScript how your record
   should be renderd.

Code:
   .. code-block:: php

      # Add to ext_tables.php
      t3lib_extMgm::addToInsertRecords('tx_yourtable');


      # define how your record should be rendered:
      tx_yourtable = COA
      tx_yourtable {
        10 = TEXT
        10.field = uid
        10.wrap = Uid:|<br />
        20 = TEXT
        20.field = title
        20.wrap = Title:|<br />
      }


      # add definition to the typoscript defintion of
      # your insert record (shortcut) content element
      tt_content.shortcut.20.0.conf.tx_yourtable < tx_yourtable


.. _s2008-69:
.. _s2008-69-typolink-problem-with-double-id:

2008-69 typolink problem with double id
=======================================

by **Steffen Kamper**, 2008-08-29 11:24:44, old #69, typoscript

Description:
   check it out with this code as root template

Links:
   n360551

Code:
   .. code-block:: typoscript

      lib.fontLinks = COA
      lib.fontLinks {
         wrap = <p>|</p><hr />
         10 = TEXT
         10.value = A
         10.wrap = |&nbsp;&nbsp;&nbsp;
         10.typolink {
            parameter.dataWrap = {TSFE:id}
            additionalParams = &S=0
            addQueryString = 1
         }

         20 < .10
         20.value = A+
         20.typolink.additionalParams = &S=1

         30 < .10
         30.value = A++
         30.typolink.additionalParams = &S=2
      }


      config {
         linkVars = S
         uniqueLinkVars = 1
      }

      page = PAGE
      page.5 < lib.fontLinks
      page.10 = TEXT
      page.10.value = <h1>HELLO WORLD!</h1><p>saf sdf adsf asfd fdsafaf afd fa  e tplerk opir jea jja rt </p>

      page.CSS_inlineStyle = body {font-size: 100%;background-color: #eee;}

      [globalVar = GP:S = 1]
      page.CSS_inlineStyle = body {font-size: 120%;background-color: #ddd;}
      [globalVar = GP:S = 2]
      page.CSS_inlineStyle = body {font-size: 150%;background-color: #aaa;}
      [end]


.. index:: typoscript, GIFBUILDER, IMAGE
.. _s2008-68:
.. _s2008-68-multiline-GIFBUILDER-subtitle-text:

2008-68 multiline GIFBUILDER subtitle text
==========================================

by **Cyrill Helg**, 2008-08-29 00:41:48, old #68, typoscript

Keywords:
   typoscript, GIFBUILDER, IMAGE

Description:
   This snippet generates a multiline IMAGE from a text (here the subtitle of the
   page is used). You can se the splitchar (here \|). Maybe you need to adjust
   some of the pixel values to make look it good.

Code:
   .. code-block:: typoscript

      lib.subTitle = COA
      lib.subTitle.7 = IMAGE
      lib.subTitle.7.file = GIFBUILDER
      lib.subTitle.7.file{
        XY=[10.w]+200,[10.h]+[20.h]+[30.h]+[40.h]+30

        backColor = #A3A3A3
        transparentColor= #A3A3A3
        transparentBackground = 1
        10 = TEXT
        10{
          textMaxLength = 200
          offset = 0,24
          text.listNum.splitChar=|
          text.listNum=0
          text.data = page:subtitle
          fontColor = #ffffff
          fontSize = 24
          niceText = 1
          fontFile = fileadmin/templates/main/res/YOURFONT.ttf
        }

        20 < .10
        20.text.listNum=1
        20.offset=0,24+[10.h]+6
        30 < .10
        30.text.listNum=2
        30.offset=0,24+[10.h]+[20.h]+12
        40 < .10
        40.text.listNum=3
        40.offset=0,24+[10.h]+[20.h]+[30.h]+18
      }


.. index:: typoscript, page, title
.. _s2008-67:
.. _s2008-67-custom-browser-title:

2008-67 custom browser title
============================

by **Cyrill Helg**, 2008-08-29 00:37:44, old #67, typoscript

Keywords:
   typoscript, page, title

Description:
   With the following TS you can customize the title tag and thus the text of your
   site in the browsers title window.

   Here first the nav\_title is used, if that is empty the title and then -
   yoursite.

Code:
   .. code-block:: typoscript

      #disable normal title
      config.noPageTitle = 1

      page{
          # custom titles
          headerData = COA
          headerData{
           5 = TEXT
           5.wrap = <title> | &nbsp;-&nbsp;yoursite </title>
           5.field = nav_title // title
          }
      }


.. index:: typoscript, menu, locallang
.. _s2008-66:
.. _s2008-66-rootline-menu-with-constants-for-different-languages:

2008-66 rootline menu with constants for different languages
============================================================

by **Cyrill Helg**, 2008-08-29 00:10:24, old #66, typoscript

Keywords:
   typoscript, menu, locallang

Description:
   This rootline menu uses constants to set the different languages

Code:
   .. code-block:: typoscript

      ###CONSTANTS###
      # cat=rootline/ctext/a; type=string; label=rootline class
      rootlineClass = navigation

      # cat=rootline/ctext/b; type=string; label=rootline symbole
      rootlineSym = -

      # cat=rootline/ctext/c; type=string; label=rootline texte lang0
      rootlineMsg_0 = vous etes sur la page:

      # cat=rootline/ctext/d; type=string; label=rootline texte lang1
      rootlineMsg_1 = you are on page:

      # cat=rootline/ctext/e; type=string; label=rootline texte lang2
      rootlineMsg_2 = si sind auf der Seite:

      ###SETUP###
      ## Rootline [BEGIN]
      lib.rootline = HMENU
      # Setting the special property to "rootline" - this will produce a "Path-menu"

      # Rootline type
      lib.rootline.special = rootline
      lib.rootline.special.range = 1

      # First level menu-object, textual
      lib.rootline.1 = TMENU

      ## DEFINE LANGUAGE [BEGIN]

      ## DEFAULT LANGUAGE
              lib.rootline.1.wrap = {$rootlineMsg_0} |

      ## LANGUAGE 1
          [globalVar = GP:L = 1]
              lib.rootline.1.wrap = {$rootlineMsg_1} |
          [global]
      ## LANGUAGE 2
          [globalVar = GP:L = 2]
              lib.rootline.1.wrap = {$rootlineMsg_2} |
          [global]


      ## DEFINE LANGUAGE [END]

      lib.rootline.1.NO.linkWrap = | {$rootlineSym} |*||*| <strong>|</strong>
      lib.rootline.1.NO.subst_elementUid = 1
      lib.rootline.1.NO.ATagParams = class="{$rootlineClass}"

      lib.rootline.1.CUR = 1
      lib.rootline.1.CUR.linkWrap = <i>|</i>
      lib.rootline.1.CUR.doNotLinkIt = 1

      ## Rootline [END]


.. index:: typoscript, menu
.. _s2008-65:
.. _s2008-65-Accessible-Rootline--Breadcrumb-Menu:

2008-65 Accessible Rootline / Breadcrumb Menu
=============================================

by **Cyrill Helg**, 2008-08-29 00:07:15, old #65, typoscript

Keywords:
   typoscript, menu

Description:
   Homepage

   \* Subpage 1
   o Subpage 1.1
   + Subpage 1.1.1

Code:
   .. code-block:: typoscript

      lib.rootline = HMENU
      lib.rootline {
          entryLevel = 0

          1 = TMENU
          1 {
              noBlur=1
              wrap = <ul>|</ul>

              NO {
                  doNotLinkIt = 1
                  doNotShowLink = 1
              }

              ACT = 1
              ACT {
                  wrapItemAndSub = <li>|</li>
                  stdWrap.field = nav_title // title
                  ATagTitle.field = nav_title // title
              }

              CUR = 1
              CUR {
                  allWrap = <li>|</li>
                  stdWrap.field = nav_title // title
                  doNotLinkIt=1
              }
          }

          2 < .1
          3 < .2
          4 < .3
          5 < .4
          6 < .5
          7 < .6
          8 < .7
      }


.. index:: typoscript, menu, locallang
.. _s2008-64:
.. _s2008-64-graphical-language-menu:

2008-64 graphical language menu
===============================

by **Cyrill Helg**, 2008-08-27 22:04:04, old #64, typoscript

Keywords:
   typoscript, menu, locallang

Description:
   This code creates a language menu with flags.

Code:
   .. code-block:: typoscript

      temp.langMenu = HMENU
      temp.langMenu.special = language
      temp.langMenu.special.value = 0,1,2
      temp.langMenu.special.normalWhenNoLanguage = 0
      temp.langMenu.1 = GMENU
      temp.langMenu.1.NO {
        9 = IMAGE
        9.file = fileadmin/template/img/uk_d.gif || fileadmin/template/img/de_d.gif || fileadmin/template/img/fr_d.gif
      }

      temp.langMenu.1.ACT < temp.langMenu.1.NO
      temp.langMenu.1.ACT = 1
      temp.langMenu.1.ACT.9.file = fileadmin/template/img/uk.gif || fileadmin/template/img/de.gif || fileadmin/template/img/fr.gif


.. index:: typoscript, image, import, page
.. _s2008-63:
.. _s2008-63-import-image-from-page-properties:

2008-63 import image from page properties
=========================================

by **Cyrill Helg**, 2008-08-27 19:35:54, old #63, typoscript

Keywords:
   typoscript, image, import, page

Description:
   With this TS you can use an image from the page properties

Code:
   .. code-block:: typoscript

      # IMPORT IMG
      temp.headerimg = IMAGE
      temp.headerimg {
        //default image
        file = fileadmin/template/img/headerimg/darts.jpg

        // override it with an image inserted in page properties in field
        file.import.data = levelmedia: -1, "slide"
        file.import = uploads/media/
        file.import.listNum = 0
        file.import.override.field = media

        border = 0
        altText = xy
        titleText = xy

      }


.. index:: typoscript, menu
.. _s2008-62:
.. _s2008-62-rootline-style-navigation:

2008-62 rootline style navigation
=================================

by **Cyrill Helg**, 2008-08-27 19:31:16, old #62, typoscript

Keywords:
   typoscript, menu

Description:
   Renders a page navigation like:

   Home : About : Page1 : Page2

Code:
   .. code-block:: typoscript

      temp.nav_bc = HMENU
      temp.nav_bc.special = rootline
      temp.nav_bc.special.range = 1|8
      #temp.nav_bc.excludeUidList = 177
      temp.nav_bc.1 = TMENU
      temp.nav_bc.1 {
        NO.linkWrap = |&nbsp;:&nbsp;|*||*| |
        NO.stdWrap.htmlSpecialChars = 1
        NO.stdWrap.field = nav_title // title
        CUR < NO
        CUR = 1
        #CUR.doNotLinkIt = 1
        CUR.stdWrap.htmlSpecialChars = 1
      }


.. index:: typoscript, page
.. _s2008-61:
.. _s2008-61-get-useful-page-information:

2008-61 get useful page information
===================================

by **Cyrill Helg**, 2008-08-27 19:27:28, old #61, typoscript

Keywords:
   typoscript, page

Description:
   Gets various information about your site

Code:
   .. code-block:: typoscript

      # INFOBOX
      temp.author = TEXT
      temp.author.data = page:author

      temp.author_mail = TEXT
      temp.author_mail.data = page:author_email

      temp.crdate = TEXT
      temp.crdate.data = page:crdate
      temp.crdate.strftime = %d. %b, %Y
      #temp.crdate.date = d. M, Y

      temp.lastUpdate = TEXT
      temp.lastUpdate.data = page:SYS_LASTCHANGED
      temp.lastUpdate.strftime = %d. %b, %Y
      #temp.lastUpdate.date = d. M, Y

      styles.content.lastUpdate = TEXT
      styles.content.lastUpdate {
        data = page:SYS_LASTCHANGED
        if.isTrue.data = page:SYS_LASTCHANGED
        date >
        wrap = <span class="text_b">Last update of this page:</span>&nbsp;|
        strftime = %d. %b, %Y
        data = register:SYS_LASTCHANGED
        if >
      }


.. index:: typoscript, if, content
.. _s2008-60:
.. _s2008-60-show-custom-message-when-empty-content:

2008-60 show custom message when empty content
==============================================

by **Cyrill Helg**, 2008-08-27 19:24:56, old #60, typoscript

Keywords:
   typoscript, if, content

Description:
   Lets you show a message on empty pages.

Code:
   .. code-block:: typoscript

      temp.contentSwitch = COA
      temp.contentSwitch {
       10 = TEXT
       10.value = <div class="error"><span class="bold">*No contentelements*</span>  on this page!</div>
       10.if.isFalse.numRows < styles.content.get
       20 < styles.content.get
      }

      # insert the right path to your template here (look in TSOB):
      page.10.xy.subparts.content < temp.contentSwitch


.. index:: typoscript, GIFBUILDER
.. _s2008-59:
.. _s2008-59-vertically-aligned-title-in-TS:

2008-59 vertically aligned title in TS
======================================

by **Cyrill Helg**, 2008-08-27 19:16:36, old #59, typoscript

Keywords:
   typoscript, GIFBUILDER

Description:
   This TS creates a title vertically.

Code:
   .. code-block:: typoscript

      temp.menu = IMAGE
      temp.menu {
      wrap = |
      file = GIFBUILDER
      file {
      XY = 140,[20.h]+80
      backColor = #EEEEEE
      20 = TEXT
      20 {
      case = upper
      angle = 90
      text.field = title
      fontSize = 39,26
      fontFile = fileadmin/fonts/xy.TTF
      fontColor = #CCCCCC
      offset = 80,[20.h]+60
      }
      }
      }


.. index:: cal, hook
.. _s2008-58:
.. _s2008-58-cal---using-hook-extraMonthDayMarkerProcessor:

2008-58 cal - using hook extraMonthDayMarkerProcessor
=====================================================

by **Steffen Kamper**, 2008-08-27 13:00:35, old #58, php

Keywords:
   cal, hook

Description:
   I needed to colorize days in month view if they have an event. Because there
   are more than one event possible, i use the first event for colorize

Code:
   .. code-block:: php

      // enable hook in ext_localconf
      $GLOBALS['TYPO3_CONF_VARS']['EXTCONF']['tx_cal_controller']['extraMonthDayMarkerHook'][]='tx_hooks';

      // jook in tx_hooks
      function extraMonthDayMarkerProcessor($parentObject,$daylink,$switch) {
        $formatedGetdate = $daylink->format('%Y%m%d');
        if($parentObject->viewarray[$formatedGetdate]) {
         $key = array_shift($parentObject->viewarray[$formatedGetdate]);
         $event = $parentObject->eventArray[$key[0]];
         $style = $event->getHeaderStyle();
         $switch['###CATHEADERSTYLE###'] = $style;
        }

        return  $switch;
      }


.. index:: background, image, css
.. _s2008-57:
.. _s2008-57-Insert-background-image-on-pr-page-basis:

2008-57 Insert background image on pr. page basis
=================================================

by **Søren Andersen**, 2008-08-22 13:05:18, old #57, typoscript

Keywords:
   background, image, css

Description:
   Want to change a background image for a section of pages? This snippet can
   enable you to do exactly that.

Code:
   .. code-block:: typoscript

      temp.headImage = TEXT
      temp.headImage {
         value = path/to/default/header.jpg
         wrap = <style type="text/css"> .header {background: url('|') no-repeat;}</style>
         stdWrap.override.data = levelmedia : -1, slide
         stdWrap.override.wrap = uploads/media/|
         stdWrap.override.required = 1
      }

      # Insert this into the pageheader somewhere
      page.headerData.49 < temp.headImage


.. index:: typolink
.. _s2008-56:
.. _s2008-56-Print-link-using-popup-window:

2008-56 Print link using popup window
=====================================

by **Steffen Kamper**, 2008-08-21 20:37:49, old #56, php

Keywords:
   typolink

Description:
   This TS produces a printlink, type=1 and opens current page in popup window. It
   uses cHash and will contain all URL vars

Code:
   .. code-block:: php

      PRINTLINK = TEXT
      PRINTLINK.value = print
      PRINTLINK.typolink {
         parameter.dataWrap = {TSFE:id},1 800x600
         addQueryString = 1
         useCacheHash = 1
         ATagParams = class="print-link"
         title = Print this page
         JSwindow_params = scrollbars=0,resizable=0
      }


.. index:: mysql, sql, TYPO3_DB, globals, store_lastBuildQuery, debug_lastBuildQuery, exec_SELECTquery, debug
.. _s2008-55:
.. _s2008-55-Get-SQL-Query-from-exec_SELECTquery:

2008-55 Get SQL-Query from exec_SELECTquery
===========================================

by **Martin Holtz**, 2008-08-11 10:03:01, old #55, php

Keywords:
   mysql, sql, TYPO3\_DB, globals, store\_lastBuildQuery, debug\_lastBuildQuery,
   exec\_SELECTquery, debug

Description:
   There is a build in function which stores the last SQL-Statement which is
   created via $GLOBALS['TYPO3\_DB']->exec\_\*query

Code:
   .. code-block:: php

      $GLOBALS['TYPO3_DB']->store_lastBuiltQuery = 1;

      // process query
      $res = $GLOBALS['TYPO3_DB']->exec_SELECTquery($select_fields,$from_table,$where_clause,$groupBy,$orderBy,$limit);

      // the complete SQL-Statement
      echo $GLOBALS['TYPO3_DB']->debug_lastBuiltQuery;


.. index:: typoscript, random, CE
.. _s2008-54:
.. _s2008-54-Random-Content:

2008-54 Random Content
======================

by **Steffen Kamper**, 2008-08-10 17:20:14, old #54, typoscript

Keywords:
   typoscript, random, CE

Description:
   This is a typical 4-liner for presenting random content, there is no need for
   an extension.
   This code was originally posted by Jan Bednarik

Links:
   n357696

Code:
   .. code-block:: typoscript

      lib.random < styles.content.get
      lib.random.select.pidInList = {$pageWhereRandCEareStored}
      lib.random.select.orderBy = rand()
      lib.random.select.max = 1


.. index:: API, copy, page
.. _s2008-53:
.. _s2008-53-API-call-to-copy-a-page:

2008-53 API call to copy a page
===============================

by **Steffen Müller**, 2008-08-09 15:58:45, old #53, php

Keywords:
   API, copy, page

Description:
   For example, this snippet can be used in a backend module to copy a page and
   paste it somewhere else in the pagetree.

   $destinationPid can be either positive, which means "insert as the last
   subpage of this page" or negative, which means "insert on the same level after
   the page with uid=abs($destinationPid)".

   Originally posted on 2008/08/09 in typo3-english by Dmitry. Dulepov.

Links:
   n357621

Code:
   .. code-block:: php

      require_once(PATH_t3lib . 'class.t3lib_tcemain.php');

      $cmdmap = array(
          'pages' => array(
              $your_page_id => array(
                  'copy' => $destinationPid,
              ),
          ),
      );
      $tce = t3lib_div::makeInstance('t3lib_TCEmain');
      $tce->start(false, cmdmap);
      $tce->process_cmdmap();
      if (count($tce->errorLog) > 0) {
          // Handle error here
      }


.. index:: BE, module, docheader
.. _s2008-52:
.. _s2008-52-Use-Docheader-in-your-BE-Modul:

2008-52 Use Docheader in your BE-Modul
======================================

by **Steffen Kamper**, 2008-08-04 12:15:25, old #52, php

Keywords:
   BE, module, docheader

Description:
   Nearly every module in 4.2 uses docheader. Here i show how to modify a module
   created from kickstarter using docheader.
   For a template file copy one of the typo3/templates to your mod1-directory.

Code:
   .. code-block:: php

      //initialize  (in function main)
      $this->doc = t3lib_div::makeInstance('template');
      $this->doc->backPath = $GLOBALS['BACK_PATH'];
      $GLOBALS['TBE_STYLES']['htmlTemplates']['mymodule.html'] = t3lib_extMgm::extRelPath('mymodule') . 'mod1/mymodule.html';
      $this->doc->setModuleTemplate('mymodule.html');
      #additional js-files
      $this->doc->loadJavascriptLib('contrib/scriptaculous/scriptaculous.js?load=builder,effects,dragdrop,controls,slider');
      $this->doc->docType='xhtml_trans';

      function printContent()   {
         // Setting up the buttons and markers for docheader
         $docHeaderButtons = $this->getButtons();
         $markers = array(
            'CSH' => $docHeaderButtons['csh'],
            'FUNC_MENU' => $this->getFuncMenu(),
            'CONTENT' => $this->content
         );

            // Build the <body> for the module
         $this->content = $this->doc->startPage($GLOBALS['LANG']->getLL('title'));
         $this->content.= $this->doc->moduleBody($this->pageinfo, $docHeaderButtons, $markers);
         $this->content.= $this->doc->endPage();
         $this->content = $this->doc->insertStylesAndJS($this->content);
         echo $this->content;
      }

      protected function getButtons()   {

         $buttons = array(
            'csh' => '',
            'shortcut' => ''
         );
            // CSH
         //$buttons['csh'] = t3lib_BEfunc::cshItem('_MOD_web_func', '', $GLOBALS['BACK_PATH']);

            // Shortcut
         if ($GLOBALS['BE_USER']->mayMakeShortcut())   {
            $buttons['shortcut'] = $this->doc->makeShortcutIcon('','function',$this->MCONF['name']);
         }
         return $buttons;
      }

      protected function getFuncMenu() {
         $funcMenu = t3lib_BEfunc::getFuncMenu(0, 'SET[function]', $this->MOD_SETTINGS['function'], $this->MOD_MENU['function']);
         return $funcMenu;
      }


.. index:: FE, Menu, stdWrap, if, numRows
.. _s2008-51:
.. _s2008-51-Submenu-of-actual-page-respecting-existence-of-subpages:

2008-51 Submenu of actual page respecting existence of subpages
===============================================================

by **Steffen Kamper**, 2008-07-29 12:11:25, old #51, typoscript

Keywords:
   FE, Menu, stdWrap, if, numRows

Description:
   since 4.2 entryLevel has stdWrap, so this is possible now!
   Thanx to Olly for his great support :-)

Code:
   .. code-block:: typoscript

      #submenu
      temp.submenu < temp.menu
      # default = -2 if page has no subpages
      temp.submenu.entryLevel = -2
      # use -1 if page has subpages
      temp.submenu.entryLevel.override = -1
      temp.submenu.entryLevel.override {
        if.isTrue.numRows {
          table = pages
        }
      }



