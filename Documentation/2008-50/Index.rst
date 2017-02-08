
.. include:: ../Includes.txt

===============
2008 26-50
===============


.. index:: HMENU, TMENU, numRows, if, stdWrap, hide, user, login, pages
.. _s2008-50:
.. _s2008-50-Hide-Pages-if-Empty:

2008-50 Hide Pages if Empty
===========================

by **Martin Holtz**, 2008-07-28 21:56:16, old #50, typoscript

Keywords:
   HMENU, TMENU, numRows, if, stdWrap, hide, user, login, pages

Description:
   This snippet renders a menu where every page is hidden, which has no content
   for the current user.

   If on an page is only content for specific groups, the menuitem will only be
   shown for an user from that group.

Code:
   .. code-block:: typoscript

      lib.menu = HMENU
      lib.menu.1 = TMENU
      lib.menu.1.NO.stdWrap {
              # if numRows returns 0
              # the menu item will not be renderd
         if.isTrue.numRows {
            table = tt_content
            select.languageField = sys_language_uid
                      # field = uid
                      # returns the id of the page, which should
                      # be renderd at this point
            select.pidInList.field = uid
         }
      }


.. index:: BE, skin
.. _s2008-49:
.. _s2008-49-Adjust-the-left-menu-in-BE-version-42:

2008-49 Adjust the left menu in BE (version >=4.2)
==================================================

by **Steffen Kamper**, 2008-07-28 10:39:05, old #49, php

Keywords:
   BE, skin

Description:
   In some translations the menu is too small. To adjust, enter following in your
   extTables.php

Code:
   .. code-block:: php

      $TBE_STYLES['dims']['leftMenuFrameW'] = 180;


.. index:: BE, typoscript, condition
.. _s2008-48:
.. _s2008-48-Condition-if-BE-User-is-logged-in:

2008-48 Condition if BE-User is logged in
=========================================

by **Steffen Kamper**, 2008-07-28 10:15:18, old #48, typoscript

Keywords:
   BE, typoscript, condition

Description:
   This condition checks if user is logged in in BE.
   Very useful to disable realurl or simulateStatic.

Code:
   .. code-block:: typoscript

      [globalVar = TSFE : beUserLogin > 0]
      config.simulateStaticDocuments = 0
      [global]


.. index:: BE, CType
.. _s2008-47:
.. _s2008-47-Add-Icon-to-your-CType:

2008-47 Add Icon to your CType
==============================

by **Steffen Kamper**, 2008-07-27 02:33:13, old #47, php

Keywords:
   BE, CType

Description:
   When your extension adds a new CType, use 3rd parameter to add the icon for the
   type select.

Code:
   .. code-block:: php

      t3lib_extMgm::addPlugin(
        array(
          'LLL:EXT:yourext/locallang_db.xml:tt_content.CType_pi1',
          $_EXTKEY.'_pi1',
          t3lib_extMgm::extRelPath($_EXTKEY).'ext_icon.gif'
        ),
        'CType'
      );


.. index:: ext_tables.php, plugin, allowTableOnStandardPages, attempt to insert record on page, not allowed, page, sysfolder
.. _s2008-46:
.. _s2008-46-Attempt-to-insert-record-on-page-xy-pid-where-this-table-tx_yourtable-is-not-allowed:

2008-46 Attempt to insert record on page 'xy' (pid) where this table, tx_yourtable is not allowed
=================================================================================================

by **Martin Holtz**, 2008-07-25 09:35:13, old #46, php

Keywords:
   ext\_tables.php, plugin, allowTableOnStandardPages, attempt to insert record on
   page, not allowed, page, sysfolder

Description:
   Per default plugin-records are only allowed to be insered into sysfolder.

   With one line of code in ext\_tables.php you can enable your plugin-records
   to be inserted on any type of pages.

Code:
   .. code-block:: php

      # add in ext_tables.php:

      t3lib_extMgm::allowTableOnStandardPages('tx_yourtable');


.. index:: typoscript, IMAGE, GIFBUILDER, stdheader
.. _s2008-45:
.. _s2008-45-graphical-multi-line-stdHeader:

2008-45 graphical multi line stdHeader
======================================

by **Stefan Berger**, 2008-07-24 20:08:31, old #45, typoscript

Keywords:
   typoscript, IMAGE, GIFBUILDER, stdheader

Description:
   This typoscript will render a graphical multi line stdHeader headline seperated
   by the \| char if exist. Dividing it in a COA of images will make it possible
   to set a fix height of each line and no space more after it.

Code:
   .. code-block:: typoscript

      lib.stdheader.10.1 >
      lib.stdheader.10.1 = COA
      lib.stdheader.10.1 {

         wrap = <div class="imageHeadlines">|</div>

         10 = IMAGE
         10 {
            wrap = <div class="line">|</div>

            file = GIFBUILDER
            file {

               XY = [10.w] + 4, [10.h] + 8
               backColor = #FFFFFF

               10 = TEXT
               10.text.field = header
               10.fontFile = fileadmin/fonts/Arial.ttf
               10.fontSize = 20
               10.fontColor = #000000
               10.offset = 0,16

               10.text.listNum = 0
               10.text.listNum.splitChar = |
            }
         }

         # check if a second line should be displayed
         20 = COA
         20.if {
            value.field = header

            isTrue.cObject = TEXT
            isTrue.cObject.field = header
            isTrue.cObject.listNum.splitChar = |
            isTrue.cObject.listNum = 1
         }

         # get the config from the first line
         20.10 < .10
         20.10 {
            file {
               10.text.listNum = 1
               10.text.listNum.splitChar = |
            }
         }
      }

      /* here the css, to handle the different browsers and set the fix height for the image .line */
      .imageHeadlines { line-height: 0px; font-size: 1px; }
      .imageHeadlines img { display: block; line-height: 0px; }
      .imageHeadlines .line { height: 26px; }


.. index:: typoscript, menu, new until, page properties, secondary options
.. _s2008-44:
.. _s2008-44-Prepend-menu-item-until-New-until-date-has-been-reached:

2008-44 Prepend menu item until "New until" date has been reached.
==================================================================

by **Tsoots**, 2008-07-24 17:21:08, old #44, typoscript

Keywords:
   typoscript, menu, new until, page properties, secondary options

Description:
   In the page properties one can make a page "new until" (page type's secondary
   options). With this snippet it's possible to prepend a menu item with something
   based on the "New until" date value.

Code:
   .. code-block:: typoscript

      lib.menu = TMENU
      lib.menu {
        NO = 1
        NO {
          before = [new]
          before.if.value.data = date:U
          before.if.isGreaterThan.data = field:newUntil


.. index:: strftime, dataWrap, wrap, TEXT, typolink, returnLast, parameter, form, timestamp, tool
.. _s2008-43:
.. _s2008-43-convert-Timestamp-into-Date-Time:

2008-43 convert Timestamp into Date Time
========================================

by **Martin Holtz**, 2008-07-23 22:11:40, old #43, typoscript

Keywords:
   strftime, dataWrap, wrap, TEXT, typolink, returnLast, parameter, form,
   timestamp, tool

Description:
   I needed a little tool to convert a MySQL Timestamp into Date/Time format. So i
   wrote it.

   Object 10 renders a simple Form where you can put the timestamp into. The
   timestamp will be submitted to the same page and Object 5 outputs the timestamp
   formated with strftime.

   It is important, that Object 5 is defined as COA\_INT, so it is not cached!

   Example:

   http://www.martinholtz.de/MySQL-Timestamp-in-normale-Zeit-umrechnen.220.0.html

Code:
   .. code-block:: typoscript

      5 = COA_INT
      5.10 = TEXT
      5.10 {
        wrap = <h2>Ergebnis: |</h2>
        dataWrap = | ({GPvar:timestamp})
        data = GPvar:timestamp
        strftime = %d.%m.%Y - %H.%M.%S
        if.isTrue.data = GPvar:timestamp
      }

      10 = TEXT
      10.typolink.parameter.data = TSFE:id
      10.typolink.returnLast = url
      10.wrap (
       <form method="post" action="|">
      <label for="timestamp">Timestamp eingeben:</label>
      <input id="timestamp" type="text" value="" name="timestamp" />
      <br />
      <input type="submit" value="anzeigen" />
      </form>

      )


.. index:: typoscript, mailform
.. _s2008-42:
.. _s2008-42-Frontend-user-name-and-email-in-mailform:

2008-42 Frontend user name and email in mailform
================================================

by **Dmitry Dulepov**, 2008-07-23 20:27:07, old #42, typoscript

Keywords:
   typoscript, mailform

Description:
   This code adds Frontend user name and email to the mailform. For full
   description and importanmt tips, visit
   http://typo3bloke.net/post-details/howto\_use\_frontend\_user\_data\_in\_a\_typo3\_mailform/

Code:
   .. code-block:: typoscript

      tt_content.mailform = COA_INT
      tt_content.mailform.20.hiddenFields {
          name = TEXT
          name.data = TSFE:fe_user|user|username
          email = TEXT
          email.data = TSFE:fe_user|user|email
      }


.. _s2008-41:
.. _s2008-41-username-in-menu:

2008-41 username in menu
========================

by **PanTerra**, 2008-07-23 14:27:12, old #41, typoscript

Description:
   This code puts a username in the menu to personalize a page

Code:
   .. code-block:: typoscript

      temp.username = COA_INT
      temp.username {
      10 = TEXT
      10.data = TSFE:fe_user|user|username
      10.noTrimWrap = |your prefix here | your postfix here
      }


      lib.nav.20.1.NO = 1
      lib.nav.20.1.NO{

             stdWrap.override {
               override < temp.username.10
               override.if {
                 value = 10 //id of menu page
                 equals.field = uid
               }
             }
             allWrap = <span><li>|</li></span>
           }



       lib.nav.20.1.ACT < lib.nav.20.1.NO


.. index:: menu
.. _s2008-40:
.. _s2008-40-pulldown-menu:

2008-40 pulldown menu
=====================

by **larsmessmer**, 2008-07-22 18:37:01, old #40, typoscript

Keywords:
   menu

Description:
   simple pulldown menu (old fashion)

Code:
   .. code-block:: typoscript

      temp.pulldownmenu = COA
            temp.pulldownmenu {
            10 = HMENU
            10.special = directory
            10.special.value = 1 // your rootpage pid
            10.wrap = <form name="pulldownmenu" action=""><select name="pulldownmenu" onChange="window.location='index.php?id='+value" class="pulldownmenu">|</select></form>
            10.1 = TMENU
            10.1 {
                expAll = 1
                NO {
                  allWrap = <option value="{elementUid}">|</option>
                  subst_elementUid = 1
                  doNotLinkIt = 1
                }
                CUR < .NO
                CUR = 1
                CUR.allWrap = <option value="{elementUid}" "selected="selected">|</option>
              }
            10.2 = TMENU
            10.2 {
                expAll = 1
                NO {
                  allWrap = <option value="{elementUid}">&nbsp;&nbsp;&nbsp;&nbsp;|</option>
                  subst_elementUid = 1
                  doNotLinkIt = 1
                }
                CUR < .NO
                CUR = 1
                CUR.allWrap = <option value="{elementUid}" "selected="selected">&nbsp;&nbsp;&nbsp;&nbsp;|</option>
              }
      }


.. index:: clean, content, tt_content, stdheader, wrap, rendering, fe, CONTENT, stdWrap, FE
.. _s2008-39:
.. _s2008-39-clean-content-rendering-2:

2008-39 clean content rendering 2
=================================

by **larsmessmer**, 2008-07-22 18:21:46, old #39, typoscript

Keywords:
   clean, content, tt\_content, stdheader, wrap, rendering, fe, CONTENT, stdWrap,
   FE

Description:
   formating and clean content rendering how you like.

Code:
   .. code-block:: typoscript

      #### clean / config Content Header
      lib.stdheader {
         stdWrap {
            space = 0|0
            dataWrap =
         }
         10.1.fontTag = <h1>|</h1>
         10.2.fontTag = <h2>|</h2>
         10.3.fontTag = <h3>|</h3>
         10.4.fontTag = <h4>|</h4>
         3 = LOAD_REGISTER
         3.headerClass =
         3.headerClass.noTrimWrap =
         10.stdWrap.wrap =
      }

      #### clean / config Content
      content {
         wrap.bodytext = <p>|</p>
         headerSpace = 0|0
         space = 0|0
      }
      tt_content {
         header.stdWrap.space = 0|0
         stdWrap {
            space = 0|0
            spaceBefore = 0
            spaceAfter = 0
         }
         image.20.noStretchAndMarginCells = 1
         image.20.spaceBelowAbove = 0
         image.20.1.file.width.ifEmpty=400
         textpic.20.spaceBelowAbove = 0
         textpic.20.noStretchAndMarginCells = 1
         textpic.20.1.file.width.ifEmpty=400
      }
      styles.content {
         imgtext {
            colSpace = 0
            rowSpace = 0
            textMargin = 5
            caption.1.wrap = 0
            caption.1.spaceBefore = 0
            caption.1.br = 0
         }
      }


.. index:: section, frame, replace, custom, border, element, typoscript
.. _s2008-38:
.. _s2008-38-Custom-Section-Frame:

2008-38 Custom Section Frame
============================

by **larsmessmer**, 2008-07-21 10:01:18, old #38, typoscript

Keywords:
   section, frame, replace, custom, border, element, typoscript

Description:
   Replace of the standard section frame at content elements to your own element.

Code:
   .. code-block:: typoscript

      ## In Setup

      tt_content.stdWrap.innerWrap.cObject {
         100 = TEXT
         100.value = <div class="class">|</div>
         101 = TEXT
         101.value = <div class="class">|</div>
         102 = TEXT
         ...
      }


      ## In TSconfig

      TCEFORM.tt_content.section_frame {
           addItems.100 = Your coustom frame one
           addItems.101 = Your coustom frame two
           addItems.102 = Your coustom frame ...
      }


.. index:: IMAGE, IMG_RESOURCE, typo3temp, img, background-url
.. _s2008-37:
.. _s2008-37-Background-Images-via-TypoScript:

2008-37 Background-Images via TypoScript
========================================

by **Martin Holtz**, 2008-07-20 23:49:25, old #37, typoscript

Keywords:
   IMAGE, IMG\_RESOURCE, typo3temp, img, background-url

Description:
   With IMG\_RESOURCE you can manipulate an IMAGE via TYPO3 without outputting the
   whole img-Tag. IMG\_RESOURCE creates an Image in typotemp (f.e.
   typo3temp/pics/134e7c7dd2.jpg). So you can use it then as an background-image.

Code:
   .. code-block:: typoscript

      page.90 = IMG_RESOURCE
      page.90.file = fileadmin/Dog.jpg
      page.90.file.width = 100
      page.90.file.height = 100
      page.90.stdWrap.wrap (
       <div style="background-image:url(|); width:100px; height:100px;">Headline</div>
      )


      This is the result:

      <div style="background-image:url(typo3temp/pics/134e7c7dd2.jpg); width:100px; height:100px;">
          Headline
        </div>


.. index:: tt_address, RECORDS, COA, TEXT, typolink
.. _s2008-36:
.. _s2008-36-Show-address-information-somewhere-but-edit-in-an-address-element:

2008-36 Show address information somewhere, but edit in an address element
==========================================================================

by **Martin Holtz**, 2008-07-20 23:35:26, old #36, typoscript

Keywords:
   tt\_address, RECORDS, COA, TEXT, typolink

Description:
   If you want to show an address information on your site on every page, you can
   easily use tt\_address for that.
   You define an RECORDS Object and put it into an marker.
   With "source" you define the id of the address element, with
   conf.tt\_address how it should be rendered.

   In this case, "send us an email" will be linked with an mailto link.

Code:
   .. code-block:: typoscript

      page.80 = RECORDS
      page.80 {
         source = 12
         tables = tt_address
              conf.tt_address >
         conf.tt_address = COA
         conf.tt_address {
            20 = TEXT
            20.value = send us an email
            20.typolink.parameter.field = email
         }
      }


.. index:: TCA, BE, CONTENT, RECORDS, backend
.. _s2008-35:
.. _s2008-35-Extra-content-columns-or-rename-columns:

2008-35 Extra content columns or rename columns
===============================================

by **Gijs Epping / Bas van der Togt**, 2008-07-14 10:35:54, old #35, php

Keywords:
   TCA, BE, CONTENT, RECORDS, backend

Description:
   If you need extra content columns, even more then border,right,left and normal
   or rename the default columns

Code:
   .. code-block:: php

      $TCA["tt_content"]["columns"]["colPos"]["config"]["items"] = array (
            "3" => array ("NEWS||||||||||","3"), // rename BORDER to NEWS
            "2" => array ("RIGHT||||||||||","2"),
            "1" => array ("LEFT||||||||||","1"),
            "0" => array ("NORMAL||||||||||","0"),
            "4" => array ("BORDER2||||||||||","4") // add extra column
      );


.. index:: typoscript, COA, tt_news
.. _s2008-34:
.. _s2008-34-Display-news-list-using-Insert-Records:

2008-34 Display news list using Insert Records
==============================================

by **Roger Bunyan**, 2008-07-11 00:37:51, old #34, typoscript

Keywords:
   typoscript, COA, tt\_news

Description:
   This TS snippet enables tt\_news records to be selected in Insert Records and
   display on a page with links to single view

Code:
   .. code-block:: typoscript

      tt_content.shortcut.20.0.conf.tt_news=COA
      tt_content.shortcut.20.0.conf.tt_news {
         wrap=<div class="newslist">|</div>
             10=TEXT
             10.field= title
             10.wrap=<strong>|</strong><br>
             10.typolink {
            #PID of single view page
                      parameter={$shortcut.conf.tt_news.pid}
                      #PID of the page for the back pid link
                      additionalParams=&tx_ttnews[backPid]={$shortcut.conf.tt_news.backPid}&tx_ttnews[tt_news]={field:uid}
                      additionalParams.insertData = 1
                      #no_cache=1
                      useCacheHash=1
                    }
              20=TEXT
              20 {
                      field=short
                      ifEmpty.field = bodytext
                      crop=150
                      wrap=|&nbsp;
                  }
              25=TEXT
              25.value=[more]
              25.stdWrap.typolink < .10.typolink

              50=IMAGE
              50{
                      # only output it if not empty
                      stdWrap.required=1
                      wrap=|
                      stdWrap.typolink < .10.typolink
                  }

                       file.import=uploads/pics/
                       file.import.field=image
                       file.maxW=100
                       file.maxH=100
                       file.import.listNum=0
              }

      }


.. index:: FE, Hook
.. _s2008-33:
.. _s2008-33-FE-Hook-for-check-class-inclusion:

2008-33 FE-Hook for check class-inclusion
=========================================

by **Steffen Kamper**, 2008-07-09 23:29:29, old #33, php

Keywords:
   FE, Hook

Description:
   with this hook you can do some internal checks.
   Remark: this hook is called twice: beginning of page render and end of page
   render.

Code:
   .. code-block:: php

      // put that in your ext_localconf.php
      $TYPO3_CONF_VARS['SC_OPTIONS']['tslib/class.tslib_fe.php']['isOutputting'][] = 'EXT:yourextension/class.yourextensionfile.php:classname->checkInclusion';

      // put that in your class
      public function checkInclusion($params, &$pObj) {
        if (!class_exists('classname_to_check_for')) {
         echo '<p style="border:1px solid black;background-color:yellow;padding:20px;font-size:18px;font-weight:bold;">
         Please add the Template for extension to your template [Include static (from extensions)]!
         </p>';
        }
       }


.. _s2008-32:
.. _s2008-32-Working-with-Frontend-Session-Data:

2008-32 Working with Frontend Session Data
==========================================

by **Jeff Segars**, 2008-07-08 20:55:26, old #32, php

Description:
   This allows any data to be temporarily saved in a user session (for both
   anonymous visitors and frontend users). The data is temporary though and will
   be destroyed once the user's cookie has expired.

Code:
   .. code-block:: php

      /* Save preference to user session */
      $GLOBALS["TSFE"]->fe_user->setKey("ses","tx_cfcampus_api", $campusUID);
      $GLOBALS["TSFE"]->fe_user->sesData_change = true;
      $GLOBALS["TSFE"]->fe_user->storeSessionData();

      /* Retrieve preference from user session */
      $campusUID = $GLOBALS["TSFE"]->fe_user->getKey("ses","tx_cfcampus_api");


.. _s2008-31:
.. _s2008-31-Extracting-Data-from-a-T3D-File:

2008-31 Extracting Data from a T3D File
=======================================

by **Jeff Segars**, 2008-07-08 20:53:01, old #31, php

Description:
   This snippet grabs the metadata from a single T3D file for display on a
   frontend page.

Code:
   .. code-block:: php

      global $TYPO3_CONF_VARS;

      require_once(t3lib_extMgm::extPath('impexp').'class.tx_impexp.php');

      $import = t3lib_div::makeInstance('tx_impexp');
      $import->init(0,'import');
      $import->enableLogging = TRUE;

      if (@is_file($filename) && $import->loadFile($filename,0))   {
         $data = $import->dat;

         $title = $data['header']['meta']['title'];
         $description = $data['header']['meta']['description'];
         $notes = $data['header']['meta']['notes'];
         $packager_username = $data['header']['meta']['packager_username'];
         $packager_name = $data['header']['meta']['packager_name'];
         $packager_email = $data['header']['meta']['packager_email'];
      }


.. _s2008-30:
.. _s2008-30-Saving-Data-to-a-T3D-File:

2008-30 Saving Data to a T3D File
=================================

by **Jeff Segars**, 2008-07-08 20:51:16, old #30, php

Description:
   This snippet exports all TemplaVoila Template Objects and Data Structures
   associated with a specific template package to a T3D file.

Code:
   .. code-block:: php

      $packageRow = t3lib_BEfunc::getRecord("tx_wectemplateadmin_package", $this->packageID);

      $this->export = t3lib_div::makeInstance('tx_wectemplateadmin_impexp');
      $this->export->init(0, 'export');
      $this->export->setCharset($GLOBALS['LANG']->charSet);
      $this->export->extensionDependencies = 'templavoila';
      $this->export->showStaticRelations = 0;
      $this->export->includeExtFileResources = 0;
      $this->export->relStaticTables[]="tx_templavoila_datastructure";
      $this->export->includeExtFileResources = 0;

      if ($packageRow['thumbnail']) {
         $this->export->addThumbnail(PATH_site.'uploads/tx_wectemplateadmin/'.$packageRow['thumbnail']);
      }

      t3lib_div::loadTCA('tx_templavoila_tmplobj');
      unset($GLOBALS['TCA']['tx_templavoila_tmplobj']['columns']['fileref']['config']['softref']);

      $uids = explode(',', $packageRow['records']);
      foreach($uids as $uid) {
         if($uid > 0) {
            $table = 'tx_templavoila_tmplobj';
         } else {
            $table = 'tx_templavoila_datastructure';
            $uid = abs($uid);
         }

         $recordRow = t3lib_BEfunc::getRecord($table, $uid);
         $this->export->export_addRecord($table, $recordRow);

         $this->export->export_addFilesFromRelations();   // MUST be after the DBrelations are set so that file from ALL added records are included!
         $templateFolder = dirname(PATH_site.$recordRow['fileref']);
      }

      $this->export->export_addDBRelations();
      $this->export->setHeaderBasics();

      // Meta data setting:
      $this->export->setMetaData(
         $packageRow['title'],
         $packageRow['description'],
         '', // Notes are empty
         $GLOBALS['BE_USER']->user['username'],
         $GLOBALS['BE_USER']->user['realName'],
         $GLOBALS['BE_USER']->user['email']
      );

      if($packageRow['filename']) {
         $filename = 'uploads/tx_wectemplateadmin/'.$packageRow['filename'];
      } else {
         $filename = 'uploads/tx_wectemplateadmin/templateexport_'.date('YmdHi').'t3d';
      }

      // Now the internal DAT array is ready to export:

      // Write export
      $out = $this->export->compileMemoryToFileContent('t3d');
      t3lib_div::writeFile(PATH_site.$filename, $out);

      return $filename;


.. _s2008-29:
.. _s2008-29-Merge-External-Typoscript-with-Default-Typoscript:

2008-29 Merge External Typoscript with Default Typoscript
=========================================================

by **Jeff Segars**, 2008-07-08 20:49:31, old #29, php

Description:
   This can be used if you have a Flexform field where a backend user enters
   Typoscript that only pertains to that single instance of the plugin, not all
   instances on the page.

Code:
   .. code-block:: php

      $flexformTyposcript = $this->pi_getFFvalue($piFlexForm, 'myTS','s_TS_View');
      if($flexformTyposcript) {
         require_once(PATH_t3lib.'class.t3lib_tsparser.php');
         $tsparser = t3lib_div::makeInstance('t3lib_tsparser');
              // Copy conf into existing setup
         $tsparser->setup = $this->conf;
         // Parse the new Typoscript
         $tsparser->parse($flexformTyposcript);
         // Copy the resulting setup back into conf
         $this->conf = $tsparser->setup;
      }


.. index:: COA_INT, CONTENT, rand(), renderObj, pages, orderBy, pidInList, typoscript, random
.. _s2008-28:
.. _s2008-28-Random-Content:

2008-28 Random Content
======================

by **Martin Holtz**, 2008-07-08 15:36:58, old #28, php

Keywords:
   COA\_INT, CONTENT, rand(), renderObj, pages, orderBy, pidInList, typoscript,
   random

Description:
   Selects one Sub-Page from page 63 randomly.

   Renders a link to that page.

Code:
   .. code-block:: php

      1 = CONTENT
      1.table = pages
      1.select {
         pidInList = 63
         max = 1
         orderBy = rand()
      }
      1.renderObj = COA_INT
      1.renderObj {
         10 = TEXT
         10.field = header
         10.typolink.parameter.field = uid
      }


.. index:: ATagParams append
.. _s2008-27:
.. _s2008-27-Add-relligthbox-to-a-TMENU-item-of-a-specific-UID:

2008-27 Add rel="ligthbox" to a TMENU item of a specific UID
============================================================

by **Søren Andersen**, 2008-07-07 13:28:04, old #27, typoscript

Keywords:
   ATagParams append

Description:
   With this snippet you can add params to 1 specific link in your menu, in this
   example rel="lightbox" is added to the link to page UID 77, without disturbing
   other params

Code:
   .. code-block:: typoscript

      NO.ATagParams =
      NO.ATagParams.append = TEXT
      NO.ATagParams.append.value = rel="lightbox"
      NO.ATagParams.append.if {
        value.field = uid
        equals = 77
      }


.. index:: DAM, typoscript, IMAGE, cObject
.. _s2008-26:
.. _s2008-26-IMAGE-object-with-DAM:

2008-26 IMAGE object with DAM
=============================

by **Martin Holtz**, 2008-07-07 09:32:43, old #26, typoscript

Keywords:
   DAM, typoscript, IMAGE, cObject

Description:
   If you have an Picture-Content-Element where the Images are managed by DAM you
   can render it with this typoscript snippet.

Links:
   http://wiki.typo3.org/De:TSref/IMAGE

Code:
   .. code-block:: typoscript

      # Verwendung des DAM zusammen mit einem IMAGE-Objekt:
      # Using an IMAGE which is managed via DAM
      bild = IMAGE
      bild.file.import.cObject = USER
      bild.file.import.cObject {
           userFunc=tx_dam_tsfe->fetchFileList
           refField=tx_damttcontent_files
           refTable=tt_content
           additional.fileList.field=image
           additional.filePath=uploads/pics/
      }


