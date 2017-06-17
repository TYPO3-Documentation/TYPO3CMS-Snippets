
.. include:: ../Includes.txt

===============
2008 1-25
===============

.. index:: email, spam protection
.. _s2008-25:
.. _s2008-25-Spam-Protection:

2008-25 Spam Protection
=======================

by **Emiliano Kamppeter**, 2008-07-06 00:09:44, old #25, typoscript

Keywords:
   email, spam protection

Description:
   Place in base TS setup or bind as external ressource template.
   Further you have to place an at.gif and dot.gif in the fileadmin folder.
   Make sure it has the same color as the <a> links.
   The alt attribute "@" and "." makes it possible to correctly copy the email
   address to clipboard.
   config.spamProtectEmailAddresses encrypts the email address. The value has a
   range from -10 to 10.

Code:
   .. code-block:: typoscript

      config.spamProtectEmailAddresses = ascii
      config.spamProtectEmailAddresses = 3
      config.spamProtectEmailAddresses_atSubst = <img src="fileadmin/at.gif" alt="@" border="0" />
      config.spamProtectEmailAddresses_lastDotSubst = <img src="fileadmin/dot.gif" alt="." border="0" />


.. index:: BE, columns
.. _s2008-24:
.. _s2008-24-Columns-for-Content-in-BE---rename--add:

2008-24 Columns for Content in BE - rename / add
================================================

by **Steffen Kamper**, 2008-07-05 16:36:58, old #24, php

Keywords:
   BE, columns

Description:
   If you need more than 4 columns or you need specific names for the columns
   according to your design, you can do it with the following snippet.
   Place this code in the extTables.php, ensure you included this script in
   your LocalConfiguration.php with following line:
   $typo\_db\_extTableDef\_script = 'extTables.php';

Code:
   .. code-block:: php

      t3lib_extMgm::addPageTSConfig('
      mod.SHARED.colPos_list = 0,1,2,3,4,5
      ');

      $TCA["tt_content"]["columns"]["colPos"]["config"]["items"] = array (
      "1" => array ("Left||Left||||||||","1"),
      "0" => array ("Main||Main||||||||","0"),
      "3" => array ("Right||Right||||||||","3"),
      "2" => array ("Border left||Border left||||||||","2"),
      "4" => array ("Border right||Border right||||||||","4"),
      "5" => array ("Footer||Footer||||||||","5")
      );


.. index:: menu, typoscript
.. _s2008-23:
.. _s2008-23-Rootline-Menu:

2008-23 Rootline Menu
=====================

by **Steffen Kamper**, 2008-07-05 16:16:17, old #23, typoscript

Keywords:
   menu, typoscript

Description:
   Simple rootline-Menu (also known as "breadcrumb menu")
   range is [begin level] \| [end level] (-1 = unlimited levels)

Code:
   .. code-block:: typoscript

      lib.rootline=HMENU
      lib.rootline.special=rootline
      lib.rootline.special.range= 0 | -1
      lib.rootline.1=TMENU
      lib.rootline.1.NO.allWrap= You are here: | &nbsp;&gt;&nbsp; |*| | &nbsp;&gt;&nbsp; |*| |
      lib.rootline.1.CUR=1
      lib.rootline.1.CUR.doNotLinkIt=1


.. index:: FE, stdWrap
.. _s2008-22:
.. _s2008-22-Clean-FE-content-rendering:

2008-22 Clean FE content rendering
==================================

by **Stefano Cecere**, 2008-07-04 03:29:04, old #22, typoscript

Keywords:
   FE, stdWrap

Code:
   .. code-block:: typoscript

      lib.stdheader.stdWrap.dataWrap >
      tt_content.stdWrap.dataWrap >
      config.disablePrefixComment = 1


.. index:: typoscript, dataWrap, wrap3
.. _s2008-21:
.. _s2008-21-Show-page-title-of-default-language:

2008-21 Show page title of default language
===========================================

by **Steffen Kamper**, 2008-07-03 13:34:45, old #21, typoscript

Keywords:
   typoscript, dataWrap, wrap3

Description:
   very tricky usage of wrap3. It's needed here because wrap3 is performed after
   dataWrap.

Code:
   .. code-block:: typoscript

      temp.pagetitle_default = TEXT
      temp.pagetitle_default {
         dataWrap = DB:pages:{TSFE:id}:title
         wrap3 = <h5>PAGE&nbsp;{|}</h5>
         insertData = 1
      }


.. index:: typoscript
.. _s2008-20:
.. _s2008-20-Multilevel-CSS-Dropdown-enu:

2008-20 Multilevel CSS Dropdown enu
===================================

by **Gerd Kolbe**, 2008-07-03 09:31:48, old #20, typoscript

Keywords:
   typoscript

Description:
   A Multilevel CSS Drop Downmenu without javascript. Based on Stu Nichols CSS
   Menus

Code:
   .. code-block:: typoscript

      lib.mainMenu = HMENU
      lib.mainMenu {
       entryLevel = 0
       excludeUidList =
              wrap =  |
              # Hauptmenue
              1 = TMENU
              1 {
                  wrap = <ul class="level1"> | </ul>
                      expAll = 1

                      NO.ATagTitle.field = subtitle//title
                     NO.wrapItemAndSub = <li class="link">|</li>


                      IFSUB = 1
                      IFSUB {
                            wrapItemAndSub = <li class="link">|</li>
                              allWrap = | <!--<![endif]-->
                              linkWrap = |<!--[if IE 7]><!-->
                        ATagBeforeWrap = 1
                              ATagParams = class=drop
                      }
              }

              2 = TMENU
              2 {
                      wrap = <!--[if IE 6]><table><tr><td><![endif]--><ul> | </ul><!--[if IE 6]></td></tr></table></a><![endif]-->
                      expAll = 1

                      NO.ATagTitle.field = subtitle//title
                      NO.wrapItemAndSub = <li> | </li>

                      IFSUB = 1
                      IFSUB {
                              wrapItemAndSub = <li> | </li>
                              allWrap = | <!--<![endif]-->
                              linkWrap = |<!--[if IE 7]><!-->
                        ATagBeforeWrap = 1
                              ATagParams = class=fly
                      }

              }


             3 < .2
               4 < .2
               }


      /*The CSS */

      #menu {width:750px;margin-left:auto;margin-right:auto;margin-top:1px;padding:1px;background:#ddd;z-index:1000;height:37px;text-align:center;}
      #menu ul {padding:0;margin:0;list-style-type:none;z-index:200;}
      .level1 {padding:0; margin:0; list-style-type: none;}
      .level1 ul {padding:0; margin:0; list-style-type: none;}
      .level1 a, .level1 a:visited {display:block;width:102px; font-size:12px;  font-weight:bold;height:36px; line-height:36px; text-decoration:none;border-right:1px solid #666; border-left:1px solid #eee;}
      .level1 li ul li a, .level1 li ul li a:visited {background:#ddd;width:185px;border-top:1px solid #eee;text-align:left;text-indent:5px;}
      .level1 li ul li a:hover {background:#ddd;}
      li.link {border-right:1px solid #eee;}
      .level1 li {float:left;}
      .level1 li:hover {position:relative;background:#ddd; }
      .level1 li:hover > a {background:#ddd; }
      .level1 li ul {display:none;}
      .level1 li:hover > ul {display:block; position:absolute; top:0px; left:187px;width:185px;}
      .level1 > li:hover > ul {left:-1px; top:36px;}
      .level1 table {position:absolute; border-collapse:collapse; top:0; left:0; z-index:100; font-size:1em;}
      .level1 li a:active, .level1 li a:focus {background:#ddd}
      .level1 li a:hover ul ul{visibility:hidden;}
      .level1 li a:hover ul a:hover ul ul{visibility:hidden;}
      .level1 li a:hover ul a:hover ul a:hover ul ul{visibility:hidden;}
      .level1 li a:hover ul a:hover ul a:hover ul a:hover ul ul {visibility:hidden;}
      .level1 li a:hover ul {visibility:visible; left:-30px; top:14px;}
      .level1 li a:hover ul a:hover ul { visibility:visible; top:-12px; left:186px;}
      .level1 li a:hover ul a:hover ul a:hover ul { visibility:visible; }
      .level1 li a:hover ul a:hover ul a:hover ul a:hover ul { visibility:visible;}
      .level1 li a:hover ul a:hover ul a:hover ul a:hover ul a:hover ul { visibility:visible;}

      /* AND THE IE HACKS */

      *html .level1 li a:hover {position:relative;}
      *html .level1 li ul {visibility:hidden; display:block; position:absolute; top:36px; left:185px;width:185px;}
      *html .level1 a, .level1 a:visited {height:37px;}
      *html .level1 li a:hover ul {left:-1px; top:35px;}
      *html .level1 li a:hover ul a:hover ul {top:-1px; left:186px;}


.. index:: debugging, log
.. _s2008-19:
.. _s2008-19-using-devLog:

2008-19 using devLog
====================

by **Jonas Dübi**, 2008-07-02 17:39:51, old #19, php

Keywords:
   debugging, log

Description:
   TYPO3 serves a great way to log errors and infos during development or
   debugging: devLog

   It's realy easy to use and it's amazing! You can add data (arrays,
   objects...) to the message which you than can browse within the great backend
   extension of Francois Suter:
   http://typo3.org/extensions/repository/view/devlog/2.3.0/

Code:
   .. code-block:: php

      // ... your source wherever (BE/FE/CLI) ...

      // this line adds a log message for extension "extkey" with the severity 'info / -1' and some data.

      if(TYPO3_DLOG) t3lib_div::devLog('title of the info', 'extensionkey', -1, $somearray);

      // you can filter those values within the devlog extension in the backend

      // ... localconf.php ...

      // to enable devlog you have to add this line in the localconf.php - but remember to disable it again!
      $TYPO3_CONF_VARS['SYS']['enable_DLOG'] = 1;

      // we use this in our extensions very often - we always do this for the typoscript ($this->conf) so you can easyly enable devLog and get the real typoscript/config you have in your pi


.. index:: extension, typoscript
.. _s2008-18:
.. _s2008-18-override-locallang-values-with-typoscript:

2008-18 override locallang-values with typoscript
=================================================

by **Bernd Wilke**, 2008-07-02 17:13:55, old #18, typoscript

Keywords:
   extension, typoscript

Description:
   First part:
   for each plugin there are typoscript entries
   (\_LOCAL\_LANG.[lang-key].[label-key) which override values from locallang.xml

   Second part:
   for each TEXT-definition in typoscript you don't need to use conditions to
   have the text in differnt languages

Code:
   .. code-block:: typoscript

      plugin.tx_my_extension_pi1._LOCAL_LANG.de.marker = neuer Text
      plugin.tx_my_extension_pi1._LOCAL_LANG.en.marker = new text


      temp.bspltext = TEXT
      temp.bspltext.value = deutscher Text
      temp.bspltext.lang.en = english text


.. index:: BE, typoscript
.. _s2008-17:
.. _s2008-17-helpfull-BE-user-configuration:

2008-17 helpfull BE-user configuration
======================================

by **Bernd Wilke**, 2008-07-02 17:04:00, old #17, typoscript

Keywords:
   BE, typoscript

Description:
   This configuration is usefull to BE-editors
   it has to be inserted in the TSConfig of
   BE-user
   BE-usergroup

Code:
   .. code-block:: typoscript

      // show page-uid within pagetree
      options.pageTree.showPageIdWithTitle = 1

      mod.web_list {
        // edit record just by clicking on title in list-view
        clickTitleMode = edit
        // alternating colors for records in list-view
        alternateBgColors = 1
      }

      // make framesize of pagetree resizable
      setup.override.navFrameResizable = 1


.. index:: cObject, extension, locallang
.. _s2008-16:
.. _s2008-16-use-more-than-one-locallang:

2008-16 use more than one locallang
===================================

by **Bernd Wilke**, 2008-07-02 16:48:56, old #16, php

Keywords:
   cObject, extension, locallang

Description:
   overlay pi\_loadLL from pibase to include more than one locallang (e.g.
   extension-global locallang\_db.xml)

   first the original-function is called,
   then a descision wether the additional locallang is loaded,
   if not loaded yet:
   build up path to global locallang\_db (or insert other locallang)
   load additional array in default language as base
   merge arrays
   load additional array in alternative language
   merge array
   set marker additioanl locallang loaded

Code:
   .. code-block:: php

      function pi_loadLL()   {
              parent::pi_loadLL();

              if (!$this->additional_locallang_include) {
            $basePath = t3lib_extMgm::extPath($this->extKey).'locallang_db.xml';
            $tempLOCAL_LANG = t3lib_div::readLLfile($basePath,$this->LLkey);
            //array_merge with new array first, so a value in locallang (or typoscript) can overwrite values from ../locallang_db
            $this->LOCAL_LANG = array_merge_recursive($tempLOCAL_LANG,is_array($this->LOCAL_LANG) ? $this->LOCAL_LANG : array());
            if ($this->altLLkey)   {
               $tempLOCAL_LANG = t3lib_div::readLLfile($basePath,$this->altLLkey);
               $this->LOCAL_LANG=array_merge_recursive($tempLOCAL_LANG,is_array($this->LOCAL_LANG) ? $this->LOCAL_LANG : array());
            }
            $this->additional_locallang_include=true;
         }
      }


.. index:: coding, extension, record, prototype
.. _s2008-15:
.. _s2008-15-update-record-prototype:

2008-15 update record (prototype)
=================================

by **Bernd Wilke**, 2008-07-02 16:37:54, old #15, php

Keywords:
   coding, extension, record, prototype

Code:
   .. code-block:: php

      $into_table  ='';
      $where_clause='';
      $field_values=array('fieldname1' => $value1
                         ,'fieldname2' => $value2
                         ,'fieldname3' => $value3
                         ,'fieldname4' => $value4
                         ,'fieldname5' => $value5
                         );

      $res=$GLOBALS['TYPO3_DB']->exec_UPDATEquery( $into_table
                                                 , $where_clause
                                                 , $field_values
                                                 );


.. index:: coding, extension, record, prototype
.. _s2008-14:
.. _s2008-14-store-data-to-record-prototype:

2008-14 store data to record (prototype)
========================================

by **Bernd Wilke**, 2008-07-02 16:36:48, old #14, php

Keywords:
   coding, extension, record, prototype

Code:
   .. code-block:: php

      $into_table  ='';
      $field_values=array('fieldname1' => $value1
                         ,'fieldname2' => $value2
                         ,'fieldname3' => $value3
                         ,'fieldname4' => $value4
                         ,'fieldname5' => $value5
                         );

      $res=$GLOBALS['TYPO3_DB']->exec_INSERTquery( $into_table
                                                 , $field_values
                                                 );


.. index:: coding, extension, record
.. _s2008-13:
.. _s2008-13-get-all-records-for-a-query-in-an-array-prototype:

2008-13 get all records for a query in an array (prototype)
===========================================================

by **Bernd Wilke**, 2008-07-02 16:33:19, old #13, php

Keywords:
   coding, extension, record

Description:
   build up a query with single parameters.
   do the query and have a stub to do further actions.
   includes commented debug-call

Code:
   .. code-block:: php

      $select_fields='*';
      $from_table='';
      $where_clause='';
      $groupBy='';
      $orderBy='';
      $limit='';
      $sammel=$GLOBALS['TYPO3_DB']->exec_SELECTgetRows( $select_fields
                                                      , $from_table
                                                      , $where_clause
                                                       .$this->cObj->enableFields($from_table)
                                                      , $groupBy
                                                      , $orderBy
                                                      , $limit
                                                      );
      //debug(array('fields'=>$select_fields
      //           ,'from'  =>$from_table
      //           ,'where' =>$where_clause
      //           ,'enable'=>$this->cObj->enableFields($from_table)
      //           ,'group' =>$groupBy
      //           ,'order' =>$orderBy
      //           ,'limit' =>$limit
      //           ,'sammel'=>$sammel
      //           ),'SELECT',__LINE__,__FILE__);

      // immidiate processing for each record:
      foreach ((array)$sammel as $record) {
         //$record['uid']
      }


.. index:: FE, typoscript, cObject
.. _s2008-12:
.. _s2008-12-excute--render-single-TypoScript-Snippet--Object:

2008-12 excute / render single TypoScript Snippet / Object
==========================================================

by **Jonas Dübi**, 2008-07-02 09:02:15, old #12, php

Keywords:
   FE, typoscript, cObject

Description:
   With this script you can render any typoscript with your record data. You can
   do in example images or typolinks with this very very easy.

Code:
   .. code-block:: php

      // clone the cObj so the data you writh into it does not influence the cObj which is used by the whole pi, otherwise you'll get strange phenomens
      $localCObj = clone $this->cObj;

      // add your record data
      $localCObj->data = $yourrecord;

      // execute your typoscript
      $content = $localCObj->cObjGetSingle($this->conf['coolobject'], $this->conf['coolobject.']);


      /*

      You could render the record title as linked text very easy:

      coolobject = TEXT
      coolobject {
        field = title
        typolink = 1
        typolink.parameter.field = url
      }

      */


.. index:: FE, typoscript, cObject
.. _s2008-11:
.. _s2008-11-excute--render-single-TypoScript-Snippet--Object:

2008-11 excute / render single TypoScript Snippet / Object
==========================================================

by **Jonas Dübi**, 2008-07-02 09:00:14, old #11, php

Keywords:
   FE, typoscript, cObject

Description:
   With this script you can render any typoscript with your record data. You can
   do in example images or typolinks with this very very easy.

Code:
   .. code-block:: php

      // clone the cObj so the data you writh into it does not influence the cObj which is used by the whole pi, otherwise you'll get strange phenomens
      $localCObj = clone $this->cObj;

      // add your record data
      $localCObj->data = $yourrecord;

      // execute your typoscript
      $content = $localCObj->cObjGetSingle($this->conf['coolobject'], $this->conf['coolobject.']);


      /*

      You could render the record title as linked text very easy:

      coolobject = TEXT
      coolobject {
        field = title
        typolink = 1
        typolink.parameter.field = url
      }

      */


.. index:: backend, admin, BE
.. _s2008-10:
.. _s2008-10-hide-tables-from-static_info_tables:

2008-10 hide tables from static_info_tables
===========================================

by **Krystian Szymukowicz**, 2008-07-01 21:22:05, old #10, typoscript

Keywords:
   backend, admin, BE

Description:
   After you install extension "static\_info\_tables"; and go at root page (id=0)
   then a lots of tables will be shown with countries, currencies etc. It slows
   down working with other tables like sys\_action, groups etc. With snippet below
   you can hide this tables. Put this snippet in user Admin TS.

Code:
   .. code-block:: typoscript

      mod.web_list.hideTables=static_template,static_countries,static_country_zones,static_currencies,static_languages,static_territories,static_taxes,static_markets


.. index:: FE, Cache, coding, extension
.. _s2008-9:
.. _s2008-9-Clear-Cache-of-specific-page-in-FE-extensions:

2008-9 Clear Cache of specific page in FE extensions
====================================================

by **Steffen Kamper**, 2008-07-01 18:40:59, old #9, php

Keywords:
   FE, Cache, coding, extension

Description:
   with this function you can clear page cache, eg. after submitting new data with
   a form
   Usage:
   $this->clearSpecificCache($GLOBALS['TSFE']->id);

Code:
   .. code-block:: php

      function clearSpecificCache($pid, $cHash=false) {
         if(is_array($pid)) {
            $GLOBALS['TYPO3_DB']->exec_DELETEquery('cache_pages', 'page_id IN (' . implode(',', $pid) . ')');
            $GLOBALS['TYPO3_DB']->exec_DELETEquery('cache_pagesection', 'page_id IN (' . implode(',', $pid) .')');
         } else {
            $addWhere = $cHash ? ' and cHash = "' . $cHash . '"' : '';
            $GLOBALS['TYPO3_DB']->exec_DELETEquery('cache_pages', 'page_id = ' . $pid . $addWhere);
            $GLOBALS['TYPO3_DB']->exec_DELETEquery('cache_pagesection', 'page_id = ' . $pid . $addWhere);
         }
      }


.. index:: core, Workspaces, email
.. _s2008-8:
.. _s2008-8-Email-handling-in-workspaces:

2008-8 Email handling in workspaces
===================================

by **Steffen Kamper**, 2008-07-01 18:20:23, old #8, php

Keywords:
   core, Workspaces, email

Description:
   looking to the code the recipients are not flexible, there is no real flow
   control who recieves mails on which stage

Code:
   .. code-block:: php

      switch((int)$stageId)   {
         case 1:
            $newStage = 'Ready for review';
         break;
         case 10:
            $newStage = 'Ready for publishing';
         break;
         case -1:
            $newStage = 'Element was rejected!';
         break;
         case 0:
            $newStage = 'Rejected element was noticed and edited';
         break;
         default:
            $newStage = 'Unknown state change!?';
         break;
      }

         // Compile list of recipients:
      $emails = array();
      switch((int)$stat['stagechg_notification'])   {
         case 1:
            switch((int)$stageId)   {
               case 1:
                  $emails = $this->notifyStageChange_getEmails($workspaceRec['reviewers']);
               break;
               case 10:
                  $emails = $this->notifyStageChange_getEmails($workspaceRec['adminusers'], TRUE);
               break;
               case -1:
                  $emails = $this->notifyStageChange_getEmails($workspaceRec['reviewers']);
                  $emails = array_merge($emails,$this->notifyStageChange_getEmails($workspaceRec['members']));
               break;
               case 0:
                  $emails = $this->notifyStageChange_getEmails($workspaceRec['members']);
               break;
               default:
                  $emails = $this->notifyStageChange_getEmails($workspaceRec['adminusers'], TRUE);
               break;
            }
         break;
         case 10:
            $emails = $this->notifyStageChange_getEmails($workspaceRec['adminusers'], TRUE);
            $emails = array_merge($emails,$this->notifyStageChange_getEmails($workspaceRec['reviewers']));
            $emails = array_merge($emails,$this->notifyStageChange_getEmails($workspaceRec['members']));
         break;
      }


.. index:: menu, last updated, pages, JSMENU
.. _s2008-7:
.. _s2008-7-Menu-of-the-last-updated-pages:

2008-7 Menu of the last updated pages
=====================================

by **Steffen Kamper**, 2008-07-01 12:46:57, old #7, typoscript

Keywords:
   menu, last updated, pages, JSMENU

Description:
   This menu shows the  of the last updated pages

Code:
   .. code-block:: typoscript

      tempJumpNav= HMENU
      tempJumpNav {
        special=updated
        # pid of parent page where to start the search
        special.value=1
        special.mode= tstamp
        #last 7 days
        special.maxAge=3600*24*7
        limit=10
        1=JSMENU
        1.target=_top
        1.firstLabelGeneral = Jump to page
      }


.. index:: email, subject, coding
.. _s2008-6:
.. _s2008-6-email-link-with-parameter:

2008-6 email link with parameter
================================

by **Steffen Kamper**, 2008-06-29 18:19:24, old #6, php

Keywords:
   email, subject, coding

Description:
   this example adds a subject

Code:
   .. code-block:: php

      $this->cObj->typolink('mail',array(
           'parameter' => 'mail@me.de?subject=bla&cc=you@me.de',
      ));


.. index:: RECORDS, typoscript, COA, tt_news
.. _s2008-5:
.. _s2008-5-Display-title-of-news-record:

2008-5 Display title of news record
===================================

by **Steffen Kamper**, 2008-06-29 18:11:57, old #5, typoscript

Keywords:
   RECORDS, typoscript, COA, tt\_news

Description:
   this is used for displaying the news title anywhere on the page

Code:
   .. code-block:: typoscript

      temp.newsTitel=COA
      temp.newsTitel{
        wrap=Das ist der Titel:|
        10=RECORDS
        10 {
          # id des template-records
          source = {GPvar:tx_ttnews|tt_news}
          source.insertData = 1
          tables = tt_news
          conf.tt_news >
          conf.tt_news = TEXT
          conf.tt_news.field=title
        }
      }


.. index:: typoscript, menu, stdWrap, cObject
.. _s2008-4:
.. _s2008-4-format-first-char-in-menu:

2008-4 format first char in menu
================================

by **Steffen Kamper**, 2008-06-29 17:45:06, old #4, typoscript

Keywords:
   typoscript, menu, stdWrap, cObject

Description:
   crop = 1\|  cuts all after first char
   substring = 1  starts with second char (count starts from 0)

Code:
   .. code-block:: typoscript

      lib.firstLetterMenu = HMENU
      lib.firstLetterMenu {
         special = directory
         special.value=14
         wrap=<div>|</div>

         1 = TMENU
         1 {
            NO.stdWrap.cObject = COA
            NO.stdWrap.cObject {
                    wrap=<p>|</p>

                    10 = TEXT
               10.field=title
                     10.crop = 1|
                     10.wrap=<span style="color:red;font-size:140%;">|</span>

                    20 = TEXT
                    20.field = title
                     20.substring=1
                  }
         }
      }


.. index:: coding, BE, record, Workspaces
.. _s2008-3:
.. _s2008-3-t3lib_beFuncgetRecordWSOL:

2008-3 t3lib_beFunc::getRecordWSOL
==================================

by **Steffen Kamper**, 2008-06-29 15:08:09, old #3, php

Keywords:
   coding, BE, record, Workspaces

Description:
   Like getRecord(), but overlays workspace version if any

Code:
   .. code-block:: php

      /**
       * Like getRecord(), but overlays workspace version if any.
       *
       * @param   string      Table name present in $TCA
       * @param   integer      UID of record
       * @param   string      List of fields to select
       * @param   string      Additional WHERE clause, eg. &quot; AND blablabla = 0&quot;
       * @param   boolean      Use the deleteClause to check if a record is deleted (default true)
       * @return   array      Returns the row if found, otherwise nothing
       */
      public static function getRecordWSOL($table, $uid, $fields = '*', $where = '', $useDeleteClause = true) {
         if ($fields !== '*') {
            $internalFields = t3lib_div::uniqueList($fields.',uid,pid'.($table == 'pages' ? ',t3ver_swapmode' : ''));
            $row = t3lib_BEfunc::getRecord($table, $uid, $internalFields, $where, $useDeleteClause);
            t3lib_BEfunc::workspaceOL($table, $row);

            if (is_array ($row)) {
               foreach (array_keys($row) as $key) {
                  if (!t3lib_div::inList($fields, $key) &amp;&amp; $key{0} !== '_') {
                     unset ($row[$key]);
                  }
               }
            }
         } else {
            $row = t3lib_BEfunc::getRecord($table, $uid, $fields, $where);
            t3lib_BEfunc::workspaceOL($table, $row);
         }
         return $row;
      }


.. index:: BE, coding, record
.. _s2008-2:
.. _s2008-2-t3lib_befuncgetRecord:

2008-2 t3lib_befunc::getRecord
==============================

by **Steffen Kamper**, 2008-06-29 14:40:21, old #2, php

Keywords:
   BE, coding, record

Description:
   Gets record with uid = $uid from $table

Code:
   .. code-block:: php

      /**
       * Gets record with uid = $uid from $table
       * You can set $field to a list of fields (default is '*')
       * Additional WHERE clauses can be added by $where (fx. ' AND blabla = 1')
       * Will automatically check if records has been deleted and if so, not return anything.
       * $table must be found in $TCA
       * Usage: 99
       *
       * @param   string      Table name present in $TCA
       * @param   integer      UID of record
       * @param   string      List of fields to select
       * @param   string      Additional WHERE clause, eg. &quot; AND blablabla = 0&quot;
       * @param   boolean      Use the deleteClause to check if a record is deleted (default true)
       * @return   array      Returns the row if found, otherwise nothing
       */
      public static function getRecord($table, $uid, $fields = '*', $where = '', $useDeleteClause = true) {
         if ($GLOBALS['TCA'][$table]) {
            $res = $GLOBALS['TYPO3_DB']-&gt;exec_SELECTquery(
               $fields,
               $table,
               'uid='.intval($uid).($useDeleteClause ? t3lib_BEfunc::deleteClause($table) : '').$where
            );
            $row = $GLOBALS['TYPO3_DB']-&gt;sql_fetch_assoc($res);
            $GLOBALS['TYPO3_DB']-&gt;sql_free_result($res);
            if ($row) {
               return $row;
            }
         }
      }

.. index:: coding, CGL
.. _s2008-1:
.. _s2008-1-Copyright-notice:

2008-1 Copyright notice
=======================

by **Steffen Kamper**, 2008-06-29 14:19:20, old #1, php

Keywords:
   coding, CGL

Description:
   used at the beginning of TYPO3 php-files

Code:
   .. code-block:: php

      /***************************************************************
      *  Copyright notice
      *
      *  (c) 1999-2008 Kasper Skaarhoj (kasperYYYY@typo3.com)
      *  All rights reserved
      *
      *  This script is part of the TYPO3 project. The TYPO3 project is
      *  free software; you can redistribute it and/or modify
      *  it under the terms of the GNU General Public License as published by
      *  the Free Software Foundation; either version 2 of the License, or
      *  (at your option) any later version.
      *
      *  The GNU General Public License can be found at
      *  http://www.gnu.org/copyleft/gpl.html.
      *  A copy is found in the textfile GPL.txt and important notices to the license
      *  from the author is found in LICENSE.txt distributed with these scripts.
      *
      *
      *  This script is distributed in the hope that it will be useful,
      *  but WITHOUT ANY WARRANTY; without even the implied warranty of
      *  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
      *  GNU General Public License for more details.
      *
      *  This copyright notice MUST APPEAR in all copies of the script!
      ***************************************************************/

