
.. include:: ../Includes.txt

===============
2012
===============

.. index:: Menu Bootstrap
.. _s2012-16:
.. _s2012-16-Responsive-Twitter-Bootstrap-Menu:

2012-16 Responsive Twitter Bootstrap Menu
=========================================

by **Max-Milan Stoyanov**, 2012-12-22 19:05:55, old #444, typoscript

Keywords:
   Menu Bootstrap

Description:
   TypoScript code for Twitter Bootstrap CSS Framework. This menu comes with
   support for in-navigation headings. Just name the spacer.

   See http://blog.mmslab.de/typo3/bootstrap-menus-in-typo3/ for further
   information.

Code:
   .. code-block:: typoscript

      lib.menu = HMENU
      lib.menu {

        1 = TMENU
        1 {
          expAll = 1

          NO.allWrap = <li>|</li>
          NO.ATagTitle.field = abstract // description // title

          ACT = 1
          ACT.wrapItemAndSub = <li class="active">|</li>
          ACT.ATagTitle.field = abstract // description // title

          IFSUB = 1
          IFSUB.before = <a href="#" class="dropdown-toggle" data-toggle="dropdown">
          IFSUB.after =  <b class="caret"></b></a>
          IFSUB.doNotLinkIt = 1
          IFSUB.wrapItemAndSub = <li class="dropdown">|</li>
          IFSUB.ATagTitle.field = abstract // description // title

          ACTIFSUB = 1
          ACTIFSUB.before = <a href="#" class="dropdown-toggle" data-toggle="dropdown">
          ACTIFSUB.after =  <b class="caret"></b></a>
          ACTIFSUB.doNotLinkIt = 1
          ACTIFSUB.wrapItemAndSub = <li class="dropdown active">|</li>
          ACTIFSUB.ATagTitle.field = abstract // description // title

          wrap = <ul class="nav">|</ul>
        }

        2 = TMENU
        2 {
          expAll = 1

          ACT = 1
          ACT.wrapItemAndSub = <li class="active">|</li>
          ACT.ATagTitle.field = abstract // description // title

          ACTIFSUB = 1
          ACTIFSUB.wrapItemAndSub = |
          ACTIFSUB.before = <li class="divider"></li><li class="nav-header">
          ACTIFSUB.after = </li>
          ACTIFSUB.doNotLinkIt = 1
          ACTIFSUB.ATagTitle.field = abstract // description // title

          NO.allWrap = <li>|</li>
          NO.ATagTitle.field = abstract // description // title

          IFSUB = 1
          IFSUB.before = <li class="divider"></li><li class="nav-header">
          IFSUB.after = </li>
          IFSUB.doNotLinkIt = 1
          IFSUB.ATagTitle.field = abstract // description // title

          SPC = 1
          SPC.allWrap = <li class="divider"></li><li class="nav-header">|</li>

          wrap = <ul class="dropdown-menu">|</ul>
        }

        3 = TMENU
        3 {
          NO.allWrap = <li>|</li>
          NO.ATagTitle.field = abstract // description // title

          ACT = 1
          ACT.wrapItemAndSub = <li class="active">|</li>
          ACT.ATagTitle.field = abstract // description // title
        }
      }


.. index:: Development
.. _s2012-15:
.. _s2012-15-SopheWare:

2012-15 SopheWare
=================

by **Romaszewski**, 2012-10-20 16:10:22, old #443, php

Keywords:
   Development

Description:
   Non-profit organization for development of CMS Quality system.
   Build web site for hight school in france and for aerospace organization

Code:
   .. code-block:: php

      Sopheware non-profit organization - For French High school & aerospace


.. index:: typolink, page
.. _s2012-14:
.. _s2012-14-create-typolink-with-external-php:

2012-14 create typolink with external php
=========================================

by **Matthias Schmid**, 2012-10-16 16:42:04, old #442, php

Keywords:
   typolink, page

Description:
   create a typolink with external php scrip

Code:
   .. code-block:: php

      // load typo3 lib
      include_once(PATH_site.'typo3/sysext/cms/tslib/class.tslib_content.php');
      $cObj = t3lib_div::makeInstance('tslib_cObj');

      // get page id of this page
      $pageid = intval($GLOBALS['TSFE']->id);

      // create link for current page
      $pagelink = $cObj->typoLink_URL(array('parameter' => $pageid));

      // create link for parent page
      $pid = intval($GLOBALS['TSFE']->page['pid']);

      $parentlink = $cObj->typoLink_URL(array('parameter' => $pid));


.. index:: page
.. _s2012-13:
.. _s2012-13-get-page-id:

2012-13 get page id
===================

by **Matthias Schmid**, 2012-10-16 16:37:28, old #441, php

Keywords:
   page

Description:
   get the current page id with php script

Code:
   .. code-block:: php

      <?php

      // get page id of this page
      $pageid = intval($GLOBALS['TSFE']->id);

      ?>


.. index:: page, Menu, sitemap
.. _s2012-12:
.. _s2012-12-Defining-a-sitemap-with-a-title-abstract-and-image:

2012-12 Defining a sitemap with a title, abstract and image
===========================================================

by **Jean-Sébastien Gervais**, 2012-10-11 23:20:46, old #440, typoscript

Keywords:
   page, Menu, sitemap

Description:
   Using nothing else than default page fields (title, abstract, image), you can
   create a more complex sitemap with some basic html, css, and typoscript.

   Just add a normal "Menu/Sitemap" as a page content, set the type to sitemap.
   And add the typoscript below :

   The basic requirement was a two column block style  of subpages with a
   title, and image, and a short description of this page.  And it had to be for a
   separate section of the website. And, of course, two columns meant left and
   right alternating blocks for proper display.  Typoscript's optionsplit to the
   rescue !

Code:
   .. code-block:: typoscript

      #Only for the first page for this section of the website.
      [globalVar = TSFE:id = 19]

      #SITEMAP avec image  V1
      tt_content.menu.20.2 >
      tt_content.menu.20.2 < tt_content.menu.20.1

      #No need for the default wrap <ul class=csc-sitemap>|</ul>
      tt_content.menu.20.2.wrap = |

      #no need for the default tt_content.menu.20.2.1.wrap = <div class="page-teaser">|</div>
      tt_content.menu.20.2.1.wrap = |

      tt_content.menu.20.2.1.NO {
         #not using link on the page title, therefore, remove the default <a> link wrap
         linkWrap = |
         ATagBeforeWrap = 0
         doNotLinkIt = 1
         stdWrap.htmlSpecialChars = 0

          #Option split to have left and right blocks in alternance
          wrapItemAndSub=  |*|<div class="block left"><div class="container">|</div></div>||<div class="block right"><div class="container">|</div></div>|*|
          ATagBeforeWrap = 0


          # Define an array of items to display for each pages in the sitemap below
          stdWrap.cObject = CASE
          stdWrap.cObject {
            key.field = doktype

            # array of element.  Title, link, image, abstract..
            default = COA
            default {
               wrap = |

              # page title
              10 = TEXT
              10 {
               field = title
               wrap = <div class="header"><div class="title">|</div></div><div class="clear"></div><div class="shadow"></div>  <div class="img-bloc-santepublique">
               typolink.parameter.field = uid
              }

              # use images from the field media in the page table.
              #It saves the file name and uploads the file to uploads/media (from the website root)
              20 = IMAGE
              20.file {
               import.override.field = media
               import = uploads/media/
               import.listNum = 0
               #width = 150m
               width = 275m
               height = 102m

              }

              # show the Abstract
              30 = TEXT
              30 {
               field = abstract
               htmlSpecialChars = 1
               wrap =</div> <p>|</p>

              }

              # Read more link
              40 = TEXT
              40 {
               value = Read more...
               htmlSpecialChars = 1
               wrap = |
               typolink.parameter.field = uid
              }
              #REMOVED (customer wanted to remove it.  Kept the code just in case ;)
              40 >
            }
        }
      }



      # Add additional css styles for this page  if you dont want to add those styles for the whole site

      page.headerData.10530 = TEXT
      page.headerData.10530.value = <link rel="StyleSheet" type="text/css" href="fileadmin/templates/css/graphic_sitemap.css" />

      [end]


.. index:: logo, typolink, image link
.. _s2012-11:
.. _s2012-11-Add-Logo-to-your-TYPO3-template:

2012-11 Add Logo to your TYPO3 template
=======================================

by **Tanel Põld**, 2012-10-01 12:35:28, old #439, typoscript

Keywords:
   logo, typolink, image link

Description:
   Use your root page "Alternative Navigation Title" and "Title" to add link title
   and image ALT text. First could say "Home" as it's a link title and shown in
   browsers with hover. "Title" is used as image ATL text.

Code:
   .. code-block:: typoscript

      lib.logo {
        10 = IMAGE
        10 {
      # path to your logo file, make sure to have templatePath constant defined,
      # or just add full path to the file
          file={$templatePath}img/logo.png
      # combining alt text from root page title and it's tq_seo extension prefix
          altText.cObject = COA
          altText.cObject {
      # getting page title prefix from tq_seo extension field
            10 = TEXT
            10.data = levelfield:0,tx_tqseo_pagetitle_prefix
      # getting root page title adding it to title prefix
            20 = TEXT
            20.data = levelfield:0,title
      # using notrimwrap to add space before root page title
            20.noTrimWrap = | |
          }
          stdWrap.typolink {
      # links logo to the root page
            parameter.data = leveluid:0
      # getting link title text from nav_title field or page title if empty
            title.data = levelfield:0,nav_title // levelfield:0,title
          }
      # wrap it all as you like
          wrap = <div id="LOGO">|</div>
        }
      }


.. index:: FAL
.. _s2012-10:
.. _s2012-10-list-images-from-pagesmedia-in-TypoScript-using-FAL:

2012-10 list images from pages:media in TypoScript using FAL
============================================================

by **Anja Leichsenring**, 2012-09-16 11:50:51, old #438, typoscript

Keywords:
   FAL

Description:
   Task: have all images from pages media field listed in TypoScript, for example
   to use in a slideshow.

   Note:
   - This snippet will work in TYPO3 6.0 only!
   - I used the FAL documentation draft found at
   http://forge.typo3.org/issues/34137

   Description:
   - the Top Level Object to use is FILES (line 1)
   - the references part as shown reads all files from page uid 1 media field
   (line 3-7)
   - the data line retrieves the attribute publicUrl from every file found
   (line 10)

Code:
   .. code-block:: typoscript

      temp.slideImages = FILES
      temp.slideImages {
        references {
          table = pages
          uid = 1
          fieldName = media
        }
        renderObj = TEXT
        renderObj {
          data = file:current:publicUrl
          wrap = {image : '|', title : ' ', thumb : ' ', url : ''},
        }
      }


.. index:: displayCond
.. _s2012-9:
.. _s2012-9-Super-displayCond:

2012-9 Super displayCond
========================

by **huangbo**, 2012-08-30 12:56:02, old #437, php

Keywords:
   displayCond

Description:
   Normally we can use "displayCond" like this: 'displayCond' =>
   'FIELD:sys\_language\_uid:>:0', but it's not enough when I want to filter with
   multiple fields. So I make a small change to implement this featue.

   How to use:
   displayCond =>
   FIELD:sys\_language\_uid:=:1&&FIELD:t\_type:=:0\|\|FIELD:sys\_language\_uid:=:0&&FIELD:t\_type:=:1
   this means if sys\_language\_uid=1 and t\_type=0, or sys\_language\_uid=0
   and t\_type=1, this field will be shown.

Code:
   .. code-block:: php

      t3lib/class.t3lib_tceforms.php
      function isDisplayCondition($displayCond, $row, $ffValueKey = '') {
            $output = FALSE;
            $tempOr = FALSE;
            $tempArrayOr = explode("||", $displayCond);
            foreach($tempArrayOr as $tempArrayAnd){
               //add by bobo
               $tempArray = explode("&&", $tempArrayAnd);
               $tempAnd = TRUE;
               foreach($tempArray as $displayCondSingle){
                  $parts = explode(':', $displayCondSingle);
                  switch ((string) $parts[0]){ // Type of condition:
                     case 'FIELD':
                        if ($ffValueKey) {
                           if (strpos($parts[1], 'parentRec.') !== FALSE) {
                              $fParts = explode('.', $parts[1]);
                              $theFieldValue = $row['parentRec'][$fParts[1]];
                           } else {
                              $theFieldValue = $row[$parts[1]][$ffValueKey];
                           }
                        } else {
                           $theFieldValue = $row[$parts[1]];
                        }

                        switch ((string) $parts[2]) {
                           case 'REQ':
                              if (strtolower($parts[3]) == 'true') {
                                 $output = $theFieldValue ? TRUE : FALSE;
                              } elseif (strtolower($parts[3]) == 'false') {
                                 $output = !$theFieldValue ? TRUE : FALSE;
                              }
                           break;
                           case '>':
                              $output = $theFieldValue > $parts[3];
                           break;
                           case '<':
                              $output = $theFieldValue < $parts[3];
                           break;
                           case '>=':
                              $output = $theFieldValue >= $parts[3];
                           break;
                           case '<=':
                              $output = $theFieldValue <= $parts[3];
                           break;
                           case '-':
                           case '!-':
                              $cmpParts = explode('-', $parts[3]);
                              $output = $theFieldValue >= $cmpParts[0] && $theFieldValue <= $cmpParts[1];
                              if ($parts[2]{0} == '!') {
                                 $output = !$output;
                              }
                           break;
                           case 'IN':
                           case '!IN':
                              $output = t3lib_div::inList($parts[3], $theFieldValue);
                              if ($parts[2]{0} == '!') {
                                 $output = !$output;
                              }
                           break;
                           case '=':
                           case '!=':
                              $output = t3lib_div::inList($parts[3], $theFieldValue);
                              if ($parts[2]{0} == '!') {
                                 $output = !$output;
                              }
                           break;
                        }
                     break;
                     case 'EXT':
                        switch ((string) $parts[2]) {
                           case 'LOADED':
                              if (strtolower($parts[3]) == 'true') {
                                 $output = t3lib_extMgm::isLoaded($parts[1]) ? TRUE : FALSE;
                              } elseif (strtolower($parts[3]) == 'false') {
                                 $output = !t3lib_extMgm::isLoaded($parts[1]) ? TRUE : FALSE;
                              }
                           break;
                        }
                     break;
                     case 'REC':
                        switch ((string) $parts[1]) {
                           case 'NEW':
                              if (strtolower($parts[2]) == 'true') {
                                 $output = !(intval($row['uid']) > 0) ? TRUE : FALSE;
                              } elseif (strtolower($parts[2]) == 'false') {
                                 $output = (intval($row['uid']) > 0) ? TRUE : FALSE;
                              }
                           break;
                        }
                     break;
                     case 'HIDE_L10N_SIBLINGS':
                        if ($ffValueKey === 'vDEF') {
                           $output = TRUE;
                        } elseif ($parts[1] === 'except_admin' && $GLOBALS['BE_USER']->isAdmin()) {
                           $output = TRUE;
                        }
                     break;
                     case 'HIDE_FOR_NON_ADMINS':
                        $output = $GLOBALS['BE_USER']->isAdmin() ? TRUE : FALSE;
                     break;
                     case 'VERSION':
                        switch ((string) $parts[1]) {
                           case 'IS':
                              $isNewRecord = (intval($row['uid']) > 0 ? FALSE : TRUE);

                                 // detection of version can be done be detecting the workspace of the user
                              $isUserInWorkspace = ($GLOBALS['BE_USER']->workspace > 0 ? TRUE : FALSE);
                              if (intval($row['pid']) == -1 || intval($row['_ORIG_pid']) == -1) {
                                 $isRecordDetectedAsVersion = TRUE;
                              } else {
                                 $isRecordDetectedAsVersion = FALSE;
                              }

                                 // New records in a workspace are not handled as a version record
                                 // if it's no new version, we detect versions like this:
                                 // -- if user is in workspace: always true
                                 // -- if editor is in live ws: only true if pid == -1
                              $isVersion = ($isUserInWorkspace || $isRecordDetectedAsVersion) && !$isNewRecord;

                              if (strtolower($parts[2]) == 'true') {
                                 $output = $isVersion;
                              } else {
                                 if (strtolower($parts[2]) == 'false') {
                                    $output = !$isVersion;
                                 }
                              }
                           break;
                        }
                     break;
                  }
                  if(!$output){
                     $tempAnd = FALSE;
                  }
               }
               if($tempAnd){
                  $tempOr = TRUE;
               }
            }

            $output = $tempOr?TRUE:FALSE;

            return $output;
         }


.. index:: forceTemplateParsing
.. _s2012-8:
.. _s2012-8-force-Template-parsing:

2012-8 force Template parsing
=============================

by **Roland Brödel**, 2012-07-09 09:49:22, old #436, typoscript

Keywords:
   forceTemplateParsing

Code:
   .. code-block:: typoscript

      admPanel.override.tsdebug.forceTemplateParsing = 1


.. index:: HMENU, ATagParams append, COA, MENU
.. _s2012-7:
.. _s2012-7-W3C-valide-HTML5-Navigation:

2012-7 W3C valide HTML5 Navigation
==================================

by **larsmessmer**, 2012-07-03 20:07:10, old #435, typoscript

Keywords:
   HMENU, ATagParams append, COA, MENU

Description:
   very simple and W3C valide HTML5 Navigation

Code:
   .. code-block:: typoscript

      lib.navigation = COA
      lib.navigation {
         10 = TEXT
         10.value = <nav>

         20 = HMENU
         20 {
         stdWrap {
            dataWrap = <ul> | </ul>
            insertData = 1
            required = 1
         }
         # seiten ausschliessen (PID)
         excludeUidList =
         special = directory
         special.value = 1
         entryLevel = 0
         expAll = 0

         1 = TMENU
         1.expAll = 1
         1 {
            noBlur = 1
            NO {
               wrapItemAndSub = <li class="first">|</li>|*|<li>|</li>|*|<li class="last">|</li>
               ATagParams =
               ATagBeforeWrap = 1
               linkWrap =  |
             }

            ACT = 1
            ACT {
               wrapItemAndSub = <li class="first">|</li>|*|<li class="active">|</li>|*|<li class="last">|</li>
               ATagParams = class="active"
               ATagBeforeWrap = 1
               linkWrap =  |
            }
           }
         }

         30 = TEXT
         30.value = </nav>
      }


.. index:: language, default, domain, realurl, typoscript, namics
.. _s2012-6:
.. _s2012-6-Different-default-language-for-domains:

2012-6 Different default language for domains
=============================================

by **Noel Bossart**, 2012-07-02 17:29:25, old #434, typoscript

Keywords:
   language, default, domain, realurl, typoscript, namics

Description:
   This is how you can configure your domains for a default language with pure
   TypoScript and Conditions

Code:
   .. code-block:: typoscript

      # Default language, sys_language.uid = 0
      config {
         sys_language_uid = 0
         language = en
         htmlTag_langKey = en
         locale_all = en_US.UTF-8
      }

      [globalVar= GP:L=1] || [globalString= IENV:HTTP_HOST=*.de, IENV:HTTP_HOST=*.ch, IENV:HTTP_HOST=*.at] && [globalVar = GP:L<1]
      config {
         sys_language_uid = 1
         language = de
         htmlTag_langKey = de
         locale_all = de_CH.UTF-8
         language_alt = en
      }
      [global]

      [globalVar = GP:L = 2] || [globalString = IENV:HTTP_HOST = *.fr] && [globalVar = GP:L<1]
       config {
         sys_language_uid = 2
         language = fr
         htmlTag_langKey = fr
         locale_all = fr_FR.UTF-8
         language_alt = en
      }
      [global]

      [globalVar = GP:L = 3] || [globalString= IENV:HTTP_HOST=*.it] && [globalVar = GP:L<1]
      config {
         sys_language_uid = 3
         language = it
         htmlTag_langKey = it
         locale_all = it_IT.UTF-8
         language_alt = en
      }
      [global]

      [globalVar = GP:L = 4] || [globalString= IENV:HTTP_HOST=*.es] && [globalVar = GP:L<1]
      config {
         sys_language_uid = 4
         language = es
         htmlTag_langKey = es
         locale_all = es_ES.UTF-8
         language_alt = en
      }
      [global]


.. index:: language, default, domain, realurl, typoscript, namics
.. _s2012-5:
.. _s2012-5-Different-default-language-for-domains:

2012-5 Different default language for domains
=============================================

by **Noel Bossart**, 2012-07-02 17:29:03, old #433, typoscript

Keywords:
   language, default, domain, realurl, typoscript, namics

Description:
   This is how you can configure your domains for a default language with pure
   TypoScript and Conditions

Code:
   .. code-block:: typoscript

      # Default language, sys_language.uid = 0
      config {
         sys_language_uid = 0
         language = en
         htmlTag_langKey = en
         locale_all = en_US.UTF-8
      }

      [globalVar= GP:L=1] || [globalString= IENV:HTTP_HOST=*.de, IENV:HTTP_HOST=*.ch, IENV:HTTP_HOST=*.at] && [globalVar = GP:L<1]
      config {
         sys_language_uid = 1
         language = de
         htmlTag_langKey = de
         locale_all = de_CH.UTF-8
         language_alt = en
      }
      [global]

      [globalVar = GP:L = 2] || [globalString = IENV:HTTP_HOST = *.fr] && [globalVar = GP:L<1]
       config {
         sys_language_uid = 2
         language = fr
         htmlTag_langKey = fr
         locale_all = fr_FR.UTF-8
         language_alt = en
      }
      [global]

      [globalVar = GP:L = 3] || [globalString= IENV:HTTP_HOST=*.it] && [globalVar = GP:L<1]
      config {
         sys_language_uid = 3
         language = it
         htmlTag_langKey = it
         locale_all = it_IT.UTF-8
         language_alt = en
      }
      [global]

      [globalVar = GP:L = 4] || [globalString= IENV:HTTP_HOST=*.es] && [globalVar = GP:L<1]
      config {
         sys_language_uid = 4
         language = es
         htmlTag_langKey = es
         locale_all = es_ES.UTF-8
         language_alt = en
      }
      [global]


.. _s2012-4:
.. _s2012-4-Adding-canoncial-tags:

2012-4 Adding canoncial tags
============================

by **Sebastian Iffland**, 2012-04-19 13:20:32, old #432, typoscript

Description:
   in order to avoid duplicate content caused by realURL at multiple language
   sites, add TS below and reference them in additionalHeaderData object.

Code:
   .. code-block:: typoscript

      temp.canonical = COA
      temp.canonical.10 = TEXT
      temp.canonical.10 {
        typolink {
          parameter.data = TSFE:id
          addQueryString = 1
          addQueryString {
            method = GET
            // vermeide doppelte id:
            exclude = id
          }
          returnLast = url
        }
        wrap = <link rel="canonical" href="http://www.meineDomain.de/|" />
      }


.. index:: FLUIDTEMPLATE, Fluid, Backend, Layout, dynamic, change, namics
.. _s2012-3:
.. _s2012-3-Dynamic-FLUIDTEMPLATE-based-on-Backend-Layout:

2012-3 Dynamic FLUIDTEMPLATE based on Backend Layout
====================================================

by **Noel Bossart**, 2012-04-17 10:25:09, old #431, typoscript

Keywords:
   FLUIDTEMPLATE, Fluid, Backend, Layout, dynamic, change, namics

Description:
   Add a Fluid Template based on the selected Backend Layout. without additional
   extensions required.

Code:
   .. code-block:: typoscript

      # setup.txt
      # Assign the Template files with the Fluid Backend-Template
      page.10 = FLUIDTEMPLATE
      page.10{
         # Create a Fluid Template
         # Set the Template Pathes, we collect all settings in a base extension: nxbase
         partialRootPath = EXT:nxbase/Resources/Private/Templates/partials/
         layoutRootPath = EXT:nxbase/Resources/Private/Templates/layouts/
         variables {

         }

         file.stdWrap.cObject = CASE
         file.stdWrap.cObject {
            key.data = levelfield:-1, backend_layout_next_level, slide
            key.override.field = backend_layout

            # Default Template
            default = TEXT
            default.value = EXT:nxbase/Resources/Private/Templates/content.html

            # The number equals the UID of the backend Layout:
            1 = TEXT
            1.value = EXT:nxbase/Resources/Private/Templates/index.html

            # The number equals the UID of the backend Layout:
            2 = TEXT
            2.value = EXT:nxbase/Resources/Private/Templates/content.html

            # The number equals the UID of the backend Layout:
            3 = TEXT
            3.value = EXT:nxbase/Resources/Private/Templates/index-nocontent.html
         }
      }


.. index:: google, analytics, domains, multiple, namics, condition, data, http_host, getIndpEnv
.. _s2012-2:
.. _s2012-2-Google-Analytics-with-multiple-Domains:

2012-2 Google Analytics with multiple Domains
=============================================

by **Noel Bossart**, 2012-04-17 10:17:52, old #430, typoscript

Keywords:
   google, analytics, domains, multiple, namics, condition, data, http\_host,
   getIndpEnv

Description:
   Add Google Analytics with multiple Domains

Code:
   .. code-block:: typoscript

      # constants.txt
      [globalString = IENV:HTTP_HOST=*domain.com*]
         library.analyticsCode = UA-1234567-1
      [end]
      [globalString = IENV:HTTP_HOST=*domain.ch*]
         library.analyticsCode = UA-1234567-2
      [end]
      [globalString = IENV:HTTP_HOST=*domain.de*]
         library.analyticsCode = UA-1234567-3
      [end]

      # setup.txt
      page {
         jsFooterInline.10 = TEXT
         jsFooterInline.10.insertData = 1
         jsFooterInline.10.value (
            var _gaq = _gaq || [];
            _gaq.push(['_setAccount', '{$library.analyticsCode}']);
            _gaq.push(['_setDomainName', '{getIndpEnv:HTTP_HOST}']);
            _gaq.push(['_trackPageview']);

            (function() {
              var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
              ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
              var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
            })();
         )
      }


.. index:: typoscript
.. _s2012-1:
.. _s2012-1-Use-strong-and-em-instead-of-b-and-i-Tags:

2012-1 Use strong and em instead of b and i Tags
================================================

by **Pascal Collins**, 2012-03-14 17:22:42, old #286, typoscript

Keywords:
   typoscript

Description:
   By default the RTE in TYPO3 creates <b> and <i> tags which have no semantic
   meaning but just represent a visual style. Therefore these tags are considered
   deprecated in modern web design, just like the font-tag. People use CSS to
   decide if something should be bold or italic. Therefore web developers have
   switched to strong and em tags which do have a semantic meaning.
   Put the following in your TS Setup to convert <b> and <i> tags to <strong>
   and <em>

Code:
   .. code-block:: typoscript

      lib.parseFunc_RTE.tags{
        b=TEXT
        b{
          current=1
          wrap= <strong>|</strong>
        }
        i=TEXT
        i{
          current=1
          wrap= <em>|</em>
        }
      }


