
.. include:: ../Includes.txt

===============
2009
===============


.. index:: TV, TypoScript
.. _s2009-32:
.. _s2009-32-Dynamic-image-header-per-page-with-default-as-Background-Image-for-TV:

2009-32 Dynamic image header per page with default (as Background Image for TV)
===============================================================================

by **Guido Unger**, 2009-12-22 17:12:00, old #234, typoscript

Keywords:
   TV, TypoScript

Description:
   Similar to the snippet "Dynamic image header per page with default" by Pim
   Broens. (http://snippets.typo3.org/c/231/)

   Modified to insert the image as a backround graphic via inline style.
   Workz as shown below only with TemplaVoila.

   Create an empty style attribute in yr html template on the appropriate html
   tag (e.g. <div id="myheader" style="">) and map lib.myBGImage to the empty
   style attribute not the element itself.

Code:
   .. code-block:: typoscript

      lib.myBGImage = IMG_RESOURCE
      lib.myBGImage {
         file.import = uploads/media/
         file.import.data = levelmedia: -1, slide
         file.import.listNum = 0
         file.import.override.field = media
         stdWrap.wrap = background: url( | ) no-repeat top left;
         # stdWrap.ifEmpty = fileadmin/<yrPath>/<yrImage> // fallback IMG
      }


.. index:: stdWrap, typoscript
.. _s2009-31:
.. _s2009-31-Wrap-content-elements-of-a-column:

2009-31 Wrap content elements of a column
=========================================

by **Pim Broens**, 2009-12-22 10:18:02, old #233, typoscript

Keywords:
   stdWrap, typoscript

Description:
   To wrap every content element in a certain column for example for CSS use. Do
   the following

Code:
   .. code-block:: typoscript

      subparts{
        RIGHT_COLUMN < styles.content.getRight
        RIGHT_COLUMN.renderObj.stdWrap.wrap=<div class="rightColumnBox">|</div>
      }


.. index:: conf
.. _s2009-30:
.. _s2009-30-HeaderComment:

2009-30 HeaderComment
=====================

by **Masod Mohmand**, 2009-12-17 23:06:16, old #232, typoscript

Keywords:
   conf

Description:
   HeaderComment in html source code above the
   "This website is powered by TYPO3 - inspiring people to share!
   ...."

Code:
   .. code-block:: typoscript

      config.headerComment (
             ###############
             Name - Company
             ###############
      }


.. index:: typoscript
.. _s2009-29:
.. _s2009-29-Dynamic-image-header-per-page-with-default:

2009-29 Dynamic image header per page with default
==================================================

by **Pim Broens**, 2009-12-16 12:59:21, old #231, typoscript

Keywords:
   typoscript

Description:
   Create dynamic header images per page with a fall back to default image in the
   root of the site.

   use the resourcetab, files field of the page properties to upload images

Code:
   .. code-block:: typoscript

      lib.headerImage = COA
      lib.headerImage {
         10 = IMAGE
         10 {
            file {
               import = uploads/media/
               import.data = levelmedia:-1, slide
               import.listNum = 0
               width = 720c #height of the image
               height = 140c #width of the image
            }
         }
      }


.. index:: typoscript
.. _s2009-28:
.. _s2009-28-_gpvars-in-typoscript:

2009-28 _gpvars in typoscript
=============================

by **Pim Broens**, 2009-12-16 11:02:06, old #230, typoscript

Keywords:
   typoscript

Description:
   Sometimes it's usefull to get the \_GPvars into your typoscript for displayment

Code:
   .. code-block:: typoscript

      temp.gpvar = TEXT
      temp.gpvar.data = GPvar:tx_pivarsprefix_pi1|pivar
      temp.gpvar.insertData = 1


.. index:: typoscript
.. _s2009-27:
.. _s2009-27-Default-piVars-when-no-piVar-is-set:

2009-27 Default piVars when no piVar is set
===========================================

by **Pim Broens**, 2009-12-16 10:46:52, old #229, typoscript

Keywords:
   typoscript

Description:
   Set a default piVars just in case there is none.
   If one is set it uses this as a fallback.

Code:
   .. code-block:: typoscript

      plugin.tt_news._DEFAULT_PI_VARS.tt_news = 905

      ## in general:
      plugin.[extensionName]._DEFAULT_PI_VARS.[piVar] = [new_value]


.. _s2009-26:
.. _s2009-26-Basic-counter:

2009-26 Basic counter
=====================

by **Nikolas Hagelstein (pulponair)**, 2009-12-14 15:10:14, old #228, typoscript

Code:
   .. code-block:: typoscript

      10 = LOAD_REGISTER
      10.myCounter.cObject = TEXT
      10.myCounter.cObject.data = register:myCounter
      10.myCounter.cObject.wrap = |+1
      10.myCounter.prioriCalc = intval


.. _s2009-25:
.. _s2009-25-Authentificate-a-frontend-user-manually:

2009-25 Authentificate a frontend user manually
===============================================

by **Nikolas Hagelstein (pulponair)**, 2009-12-14 15:04:27, old #227, php

.. warning::
   The following code will not work in newer TYPO3 versions (>= TYPO3 9). 

Code:
   .. code-block:: php

      $loginData=array(
          'uname' => 'johndoe', //usernmae
          'uident'=> 'mypassword', //password
          'status' =>'login'
      );

       //do not use a particular pid
      $GLOBALS['TSFE']->fe_user->checkPid=0;
      $info = $GLOBALS['TSFE']->fe_user->getAuthInfoArray();
      $user = $GLOBALS['TSFE']->fe_user->fetchUserRecord(
         $info['db_user'] ,$loginData['uname']);
                      $ok=$GLOBALS['TSFE']->fe_user->compareUident($user, $loginData);
      if($ok) {
        //login successfull
        $GLOBALS['TSFE']->fe_user->createUserSession($user);
      } else {
        //login failed
      }


.. _s2009-24:
.. _s2009-24-Convert-CMYK-to-RGB-using-a-color-profile:

2009-24 Convert CMYK to RGB using a color profile
=================================================

by **Nikolas Hagelstein (pulponair)**, 2009-12-14 15:02:21, old #226, php

Code:
   .. code-block:: php

      $imageConfig['file'] = fileadmin/cmykimg.tiff;              $imageConfig['file.']['params']="-profile /somewhere/srgb.icc";
      $imageResource=$this->cObj->IMG_RESOURCE($imageConfig);


.. index:: IMG_RESOURCE
.. _s2009-23:
.. _s2009-23-Convert-CMYK-to-RGB-using-a-color-profile:

2009-23 Convert CMYK to RGB using a color profile.
==================================================

by **Nikolas Hagelstein (pulponair)**, 2009-12-14 15:01:18, old #225, typoscript

Keywords:
   IMG\_RESOURCE

Code:
   .. code-block:: typoscript

      10=IMG_RESOURCE
      10.file=fileadmin/cmykimg.tiff
      10.params=-profile /somewhere/srgb.icc


.. index:: wizard, find-as-you-type, suggest, BE, TCA, backend, extension, fields, autocomplete
.. _s2009-22:
.. _s2009-22-Adding-the-TCA-wizard-suggest-find-as-you-type-to-BE-fields-of-extensions-43-only:

2009-22 Adding the TCA wizard "suggest" (find-as-you-type) to BE fields of extensions (>=4.3 only!)
===================================================================================================

by **Steffen Müller**, 2009-10-16 15:01:11, old #224, php

Keywords:
   wizard, find-as-you-type, suggest, BE, TCA, backend, extension, fields,
   autocomplete

Description:
   There's a new type of wizard in the TYPO3 core called "suggest", which has been
   added to TCA with 4.3beta-1. This wizard adds a magic input field for
   autocompletion to fields of type "group" or "select", also called
   find-as-you-type.

   For a more detailed description, see:
   http://www.t3node.com/blog/using-the-new-tca-wizard-suggest-autocomplete-in-extensions-of-typo3-43/

Links:

   http://www.t3node.com/blog/using-the-new-tca-wizard-suggest-autocomplete-in-extensions-of-typo3-43/

Code:
   .. code-block:: php

      # Example: Adding the wizard to related news field of tt_news
      # TCA (tca.php)

      $TCA['tt_news'] = array(
      # (...)
        'columns' => array(
      # (...)
          'related' => array (
            'exclude' => 1,
            'l10n_mode' => 'exclude',
            'label' => 'LLL:EXT:tt_news/locallang_tca.php:tt_news.related',
            'config' => array (
              'type' => 'group',
              'internal_type' => 'db',
              'allowed' => 'tt_news,pages',
              'MM' => 'tt_news_related_mm',
              'size' => '3',
              'autoSizeMax' => 10,
              'maxitems' => '200',
              'minitems' => '0',
              'show_thumbs' => '1',

              # Here comes the magic
              'wizards' => array(
                'suggest' => array(
                  'type' => 'suggest',
                ),
              ),
            ),
          ),
        ),
      );

      # For more configuration options, see official docs TYPO3 Core API (once 4.3 is released).


.. index:: infocenter, slideshow, typoscript, numrows, select, meta, refresh, HMENU
.. _s2009-21:
.. _s2009-21-Creating-a-simple-Infocenter-using-TypoScript:

2009-21 Creating a simple Infocenter using TypoScript
=====================================================

by **Peter Klein**, 2009-10-16 13:50:05, old #223, typoscript

Keywords:
   infocenter, slideshow, typoscript, numrows, select, meta, refresh, HMENU

Description:
   If you need to create a simple Infocenter (An endless slideshow of pages), you
   can do it with this little Typoscript snippet.

   The TS will display all subpages to a given ID in an endless loop, by adding
   a META refresh tag to every page, pointing to the next in line.
   When the last subpage is displayed, it will start over with the 1st subpage
   again.

   There's also an additional Typoscript snippet (lib.pagecount) that can be
   used if you want to display a pagecounter on the pages.

Code:
   .. code-block:: typoscript

      lib.refresh = COA
      lib.refresh {
         10 = CONTENT
         10.table = pages
         10.select {
            pidInList = {$slideroot}
            orderBy = sorting
            andWhere.dataWrap = sorting>{field:sorting}
            max = 1
         }
         10.renderObj = COA
         10.renderObj {
            10 = TEXT
            10.typolink.parameter.field = uid
            10.typolink.returnLast = url
            wrap = {$refreshdelay};|
         }

      }
      page.meta.refresh.cObject < lib.refresh
      page.meta.refresh.cObject.10.select.andWhere >
      page.meta.refresh.override.cObject < lib.refresh


      lib.pagecount = COA
      lib.pagecount {
         10 = TEXT
         10.numRows.table = pages
         10.numRows.select.pidInList = {$slideroot}
         10.numRows.select.orderBy = sorting
         10.numRows.select.andWhere.dataWrap = sorting<={page:sorting}
         10.wrap = |/

         20 = TEXT
         20.numRows.table = pages
         20.numRows.select.pidInList = {$slideroot}
         20.numRows.select.orderBy = sorting
      }


.. index:: templavoila, TV, typoscript
.. _s2009-20:
.. _s2009-20-KB-TV-Content-Slide---excluding-root-page:

2009-20 KB TV Content Slide - excluding root page
=================================================

by **Stig Nørgaard Færch**, 2009-06-24 14:31:36, old #222, php

Keywords:
   templavoila, TV, typoscript

Description:
   This tip is for TemplaVoila and KB TV Content Slide.
   If you don't want KB TV Content Slide to go all the way down to the root,
   you can make it go only to the level above root.
   Using the stdWrap property data to get the current page level is the key.

Links:
   http://typo3.org/extensions/repository/view/kb\_tv\_cont\_slide/current/

Code:
   .. code-block:: php

      10= RECORDS
      10.source.postUserFunc = tx_kbtvcontslide_pi1->main
      10.source.postUserFunc.field = field_openings
      10.source.postUserFunc.table = tt_content
      10.source.postUserFunc.slide.data = level:1
      10.tables = tt_content


.. index:: BE, custom, tempalte
.. _s2009-19:
.. _s2009-19-BE-Login-Template:

2009-19 BE Login Template
=========================

by **Dan Osipov**, 2009-06-08 17:43:40, old #221, php

Keywords:
   BE, custom, tempalte

Description:
   Replace default BE template with a custom one. Works in 4.3. Place code in
   ext\_tables.php

Links:
   http://danosipov.com/blog/?p=100

Code:
   .. code-block:: php

      $GLOBALS['TBE_STYLES']['htmlTemplates']['templates/login.html'] = 'path_to_login_template';


.. index:: numRows, table, select, pidInList
.. _s2009-18:
.. _s2009-18-Show-Section-Title-on-Subpages:

2009-18 Show Section Title on Subpages
======================================

by **Peter Gallagher**, 2009-06-03 18:16:11, old #220, typoscript

Keywords:
   numRows, table, select, pidInList

Description:
   I wanted to show the section title above all the subpages using a TEXT element.
   This snippet will only render the TEXT element if the root page in the tree has
   subpages.

Code:
   .. code-block:: typoscript

      lib.sectionTitle = COA
      lib.sectionTitle.10 = TEXT
      lib.sectionTitle.10.data = leveltitle:1
      lib.sectionTitle.10.wrap = <h1>|</h1>
      lib.sectionTitle.10.if.isTrue.numRows{
        table = pages
        select.pidInList = {leveluid:1}
        select.pidInList.insertData = 1
      }


.. index:: numRows, isTrue, if
.. _s2009-17:
.. _s2009-17-Show-content-only-if-there-are-subpages:

2009-17 Show content only if there are subpages
===============================================

by **Peter Gallagher**, 2009-06-03 17:11:48, old #219, typoscript

Keywords:
   numRows, isTrue, if

Description:
   This will check the PAGES table for subpages to the current page. You can then
   choose to show/render the content based on the results.

Code:
   .. code-block:: typoscript

      lib.something = TEXT
       lib.something.value = Text to show if there are subpages
       lib.something.if.isTrue.numRows {
          table = pages
          where = pid=this
       }

       #if you want to show something if there are no subpages

       lib.something = TEXT
       lib.something.value = Text to show if there are no subpages
       lib.something.if.negate = 1
       lib.something.if.isTrue.numRows {
          table = pages
          where = pid=this
       }


.. index:: be, CONFIG, FCE, ext_tables.php, ext_tables
.. _s2009-16:
.. _s2009-16-extra-required-field:

2009-16 extra required field
============================

by **larsmessmer**, 2009-04-24 14:15:11, old #218, php

Keywords:
   be, CONFIG, FCE, ext\_tables.php, ext\_tables

Description:
   Example: for Accessability we make the Alt Tag Field as required field by
   default.

   Put following Code into extTables.php

Code:
   .. code-block:: php

      $TCA['tt_content']['columns']['altText']['config']['eval'] = 'required';


.. _s2009-15:
.. _s2009-15-RSS-XML-generating-COA:

2009-15 RSS XML generating COA
==============================

by **Manuel Stofer**, 2009-04-15 20:06:10, old #217, typoscript

Description:
   i used the extension kk\_downloader to have its downloads generated into an rss
   xml

Code:
   .. code-block:: typoscript

      lib.downloader-feed = COA
      lib.downloader-feed {
         wrap (
            <?xml version="1.0"?>
            <rss version="2.0">
               <channel>
               |
               </channel>
            </rss>
         )
         10 = COA
         10 {
            10 = TEXT
            10.value = Test xml Document
            10.wrap = <title>|</title>

            20 = TEXT
            20.value = http://neu.glabischnig.at
            20.wrap = <link>|</link>

            30 = TEXT
            30.value = This is a description
            30.wrap = <description>|</description>
         }

         20 = CONTENT
         20 {
            table = tx_kkdownloader_images
            select {
               pidInList = put the sysfolder with the downloads here
            }
            renderObj = COA
            renderObj {
               wrap = <item>|</item>
               10 = TEXT
               10.field = name
               10.wrap = <title>|</title>

         20 = COA
         20 {
                wrap = <link>|</link>
          10 = TEXT
          10.data = TSFE:baseUrl

                   20 = TEXT
                  20.typolink.parameter.dataWrap = /uploads/tx_kkdownloader/{field:image}
                  20.typolink.returnLast = url
               }

               30 = TEXT
               30.field = longdescription // description
               30.required = 1
               30.wrap = <description>|</description>

               40 = TEXT
               40.field = tstamp
               40.strftime = %d/%m-%Y %H:%M
               40.wrap = <pubDate>|</pubDate>
            }
         }
      }


      xmltest = PAGE
      xmltest {
       typeNum = 80
       10 < lib.downloader-feed

       config {
       disableAllHeaderCode = 1
       additionalHeaders = Content-type:application/xml
       xhtml_cleaning = 0
       admPanel = 0
       baseURL = put your baseurl here
       }
      }


.. index:: cObj, filelink, TypoScript, extension, FE
.. _s2009-14:
.. _s2009-14-Link-to-a-file-in-a-FE-plugin:

2009-14 Link to a file in a FE plugin
=====================================

by **Steffen Müller**, 2009-04-01 17:51:59, old #216, php

.. warning:: 
   `filelink` is deprecated since version 9 and will be removed in version 10. 
   Use DataProcessors or Fluid Styled Content instead.

Keywords:
   cObj, filelink, TypoScript, extension, FE

Description:
   The idea is to have a FE plugin with a link to a file, which is set in the
   media field ("Files") of the page.

   Once you have selected the file in the backend, it gets copied to the
   uploads/media/ directory. All we need to do then in the PHP part of a FE plugin
   is
   1) fetch the path to the filename
   2) build a link to it using the cObject filelink function and some TS.

Links:

   http://typo3.org/documentation/document-library/references/doc\_core\_tsref/4.2.0/view/1/5/#id4198294

Code:
   .. code-block:: php

      /* PHP part */

      // 1. fetch the path to the file
      $filePath = 'uploads/media/'.$GLOBALS['TSFE']->page['media'];

      // 2. use the filelink function to create a rich feature link
      $content = $this->cObj->filelink(
        $filePath,
        $this->conf['fileLink.']
      );


      /* TypoScript part */

      plugin.tx_myplugin_pi1 {
        fileLink {
          file.noTrimWrap = | | |
          labelStdWrap.data = page:media
          icon = 1
          icon_link = 1
          size = 1
          size.wrap = (|)
          size.bytes = 1
          size.bytes.labels = " B| KB| MB| GB"
        }
      }


.. index:: CSH, FlexForms, extension
.. _s2009-13:
.. _s2009-13-Add-context-sensitive-help-CSH-to-FlexForm-fields:

2009-13 Add context sensitive help (CSH) to FlexForm fields
===========================================================

by **Steffen Müller**, 2009-04-01 17:39:28, old #215, xml

Keywords:
   CSH, FlexForms, extension

Description:
   Do you like to have CSH items in your extensions FlexForm? Two steps are
   enough:
   1) Create a file with the CSH contents:
   your\_extkey/pi1/locallang\_csh\_flexform.xml
   2) Add a reference to this file in your FlexForm file by adding the line
   <cshFile>...</cshFile>.
   See the example code below.

   This is supported in TYPO3 4.2 and supposed to be free of bugs in 4.3

Code:
   .. code-block:: guess

      1. FILE: your_extkey/pi1/flexform_ds.xml

      <T3DataStructure>
        <meta>
          <langDisable>1</langDisable>
        </meta>
        <sheets>
          <sDEF>
            <ROOT>
              <TCEforms>
                <sheetTitle>The title of the sheet</sheetTitle>
                <cshFile>LLL:EXT:your_extkey/pi1/locallang_csh_flexform.xml</cshFile>
              </TCEforms>
              <type>array</type>
              <el>
                <yourField>
                  <TCEforms>
                    <label>Label of the the field</label>
                    <config>
                      ... field configuration ...
                    </config>
                  </TCEforms>
                </yourField>
              </el>
            </ROOT>
          </sDEF>
        </sheets>
      </T3DataStructure>


      2. FILE: your_extkey/pi1/locallang_csh_flexform.xml

      <?xml version="1.0" encoding="utf-8" standalone="yes" ?>
      <T3locallang>
        <meta type="array">
          <description>CSH labels for FlexForm fields</description>
          <type>CSH</type>
          <csh_table></csh_table>
          <fileId>EXT:your_extkey/pi1/locallang_csh_flexform.xml</fileId>
          <labelContext type="array"></labelContext>
        </meta>
        <data type="array">
          <languageKey index="default" type="array">
            <label index="yourField.alttitle">alternative title</label>
            <label index="yourField.description">description</label>
            <label index="yourField.details">details</label>
            <label index="yourField.syntax">syntax</label>
            <label index="yourField.image_descr">image caption</label>
            <label index="yourField.image">gfx/i/pages.gif</label>
            <label index="yourField.seeAlso">pages:starttime</label>
          </languageKey>
        </data>
      </T3locallang>


.. index:: users, fe_user, online
.. _s2009-12:
.. _s2009-12-Who-Is-Online:

2009-12 Who Is Online
=====================

by **Miroslav Monkevic**, 2009-04-01 09:53:13, old #214, typoscript

Keywords:
   users, fe\_user, online

Description:
   Simple WhoIsOline script, supports templates. Was designed to be easily
   customizable not speedy.

Code:
   .. code-block:: typoscript

      ###############
      ## CONSTANTS ##
      ###############

      lib.contentLeft.20 {
        template (
      ###COUNT_LABEL### ###COUNT###
      <!-- ###USERLIST### -->
      <ol>
      <!-- ###USER### -->
      <li>###UIMAGE### ###ULINK###</li>
      <!-- ###USER### -->
      </ol>
      <!--  ###USERLIST### -->
      )

        feusersPid = 38
        feusersProfileUid = 36
        feusersImageWidth = 30
        feusersProfileParams = action=getviewprofile&uid=
        feusersImageDir = uploads/tx_srfeuserregister/
        timeout = 3600
        orderby = ses_tstamp
        membersonline_message {
          en = member(s) are online.
        }
      }

      ###########
      ## SETUP ##
      ###########
      lib.contentLeft.20 = COA_INT
      lib.contentLeft.20 {

        10 = TEMPLATE
        10 {
          template = TEXT
          template.value (
          {$lib.contentLeft.20.template}
          )

          marks {
            COUNT = TEXT
            COUNT.numRows {
              table = fe_users
              select {
                pidInList = {$lib.contentLeft.20.feusersPid}
                selectFields = DISTINCT ses_userid, username, name, image
                where = ses_tstamp+{$lib.contentLeft.20.timeout} > unix_timestamp() OR is_online+{$lib.contentLeft.20.timeout} > unix_timestamp()
                orderBy = {$lib.contentLeft.20.orderby} DESC
                join = fe_sessions ON fe_sessions.ses_userid = fe_users.uid
              }
            }

            COUNT_LABEL = TEXT
            COUNT_LABEL.lang {
              en = {$lib.contentLeft.20.membersonline_message.en}
            }
          }

          subparts.USERLIST.subparts.USER < .marks.COUNT.numRows
          subparts {
            USERLIST = TEMPLATE
            USERLIST {
              template = TEXT
              template.current = 1
              subparts {
                USER = CONTENT
                USER {
                  renderObj = TEMPLATE
                  renderObj {
                    template = TEXT
                    template.data= register:SUBPART_USER
                    marks {
                      UIMAGE = IMAGE
                      UIMAGE {
                        file {
                          import = {$lib.contentLeft.20.feusersImageDir}
                          import.field = image
                          import.listNum = 0
                          width = {$lib.contentLeft.20.feusersImageWidth}
                        }
                      }

                      ULINK = TEXT
                      ULINK {
                        field = username
                        typolink {
                          parameter = {$lib.contentLeft.20.feusersProfileUid}
                          additionalParams = &{$lib.contentLeft.20.feusersProfileParams}{field:ses_userid}
                          additionalParams.insertData = 1
                        }
                        required = 1
                      }
                    #marks
                    }
                  # renderObj
                  }
                  stdWrap.required = 1
                # USER
                }
              }
            }
          # subparts
          }
        }
      }


.. index:: condition, userFunc, includeLibs, loadFile
.. _s2009-11:
.. _s2009-11-simple-userfunction-used-in-condition:

2009-11 simple userfunction used in condition
=============================================

by **Martin Holtz**, 2009-03-26 14:47:32, old #213, php

Keywords:
   condition, userFunc, includeLibs, loadFile

Description:
   Original Code posted by pmk65 in #typo3 IRC Channel - thanks:)

Code:
   .. code-block:: php

      // Userfunction Start. (Put this in
      // fileadmin/example_userfunc.php)
      <?php

      function user_mycheck() {
        $GP = $_GET('tx_yourdata_pi1');
        if ($GP['detailId'] > 0) {
           return true;
        }
        return false;
      }
      ?>
      // Userfunction End

      Put
      require_once('fileadmin/example_userfunc.php');
      in your AdditionalConfiguration.php


      // Typoscript Setup start
      [userFunc = user_mycheck()]
      .... do something if there is a
      GET/POST-Parameter like tx_yourdata_pi1[detailId]=123
      [global]
      // Typoscript Setup end


.. index:: TEXT, printlink, typolink, typoscript, Realurl, tt_news
.. _s2009-10:
.. _s2009-10-Print-link-with-all-URL-Parameters:

2009-10 Print link with all URL Parameters
==========================================

by **Bernhard Gschwantner**, 2009-02-23 12:29:58, old #131, typoscript

Keywords:
   TEXT, printlink, typolink, typoscript, Realurl, tt\_news

Description:
   This Typoscript snippet creates a print link with all GET Parameters, not just
   the Page ID. So this should also work with tt\_news single view and other
   extensions.

   Of course you also have to define a page template with the page type 98.

Code:
   .. code-block:: typoscript

      temp.print-link = TEXT
      temp.print-link {
         value = Print this page
         wrap = <p class="print-link">|</p>
         typolink {
            # link to the current page id with type 98
            parameter = {page:uid},98
            parameter.insertData = 1
            useCacheHash = 1
            # add all get parameters from the current URL
            addQueryString = 1
            addQueryString.method = GET
            # remove the page id from the parameters so it is not inserted twice
            addQueryString.exclude = id
         }
      }

      # Add the print link to your page (you will have to adapt this to your setup)
      page.10.subparts.PRINT-LINK < temp.print-link


.. index:: Realurl, to top, fix
.. _s2009-9:
.. _s2009-9-Fix-for-to-top-Link-with-Realurl:

2009-9 Fix for to top Link with Realurl
=======================================

by **Manuel Stofer**, 2009-02-10 23:16:32, old #112, typoscript

Keywords:
   Realurl, to top, fix

Description:
   if you use realurl the "to top" link does not work.
   this ts will fix it.

Code:
   .. code-block:: typoscript

      tt_content.stdWrap.innerWrap2 = {LLL:EXT:css_styled_content/pi1/locallang.php:label.toTop}
      tt_content.stdWrap.innerWrap2.stdWrap {
        typolink.parameter.data = TSFE:id
        wrap = <p class="csc-linkToTop">|</p>
      }


.. index:: sql comm seperated join left where
.. _s2009-8:
.. _s2009-8-SQL-Statement-with-join-and-comma-seperated-value:

2009-8 SQL Statement with join and comma seperated value
========================================================

by **dexcs**, 2009-02-10 16:22:34, old #111, typoscript

Keywords:
   sql comm seperated join left where

Description:
   And SQL over tt\_news and an costom pages column with an comma seperated value

Code:
   .. code-block:: typoscript

      lib.matrix = COA
      lib.matrix {
         10 = CONTENT
         10 {
            table = tt_news
            select {
               selectFields = *
               join = tt_news_cat_mm
               where=1=1
               andWhere (
                  tt_news_cat_mm.uid_local=tt_news.uid AND
                  tt_news_cat_mm.uid_foreign IN ({page:tx_ttnewsextension_tt_news_group})
               )
               andWhere.insertData=1
               pidInList = 9
               orderBy = datetime desc
               max = 8
            }
            renderObj = COA
            renderObj {
                  10 = IMAGE
                  10 {
                     file.import = uploads/pics/
                     file.import.field = image
                     file.import.listNum = 0
                     file.width = 150
                     #params = style="border: 1px #164E70 solid; border-top: none;"
                  }
                  20 = IMAGE
                  20 {
                     file.import = uploads/pics/
                     file.import.field = image
                     file.import.listNum = 1
                     file.width = 150
                     #params = style="border: 1px #164E70 solid; border-top: none;"
                  }
            }
         }
      }


.. index:: templavoila, BE, LOAD_REGISTER, TV
.. _s2009-7:
.. _s2009-7-Automatic-hiding-of-unused-content-area:

2009-7 Automatic hiding of unused content area...
=================================================

by **Stig Nørgaard Færch**, 2009-02-04 09:52:32, old #110, typoscript

Keywords:
   templavoila, BE, LOAD\_REGISTER, TV

Description:
   When you have a site layout which is 3 columns on the frontpage and 2/3 columns
   on the subpages, you could create two datastructures.

   The problem is that it is not very intuitive for the user to change from 2
   to 3 columns on a subpage, as he has to both change a datastructure and the
   template.

   So instead of going for two datastructures, I have managed to hide the 3rd
   column if it is empty.
   So the thing that decides if the 3rd column is shown, is if I have put any
   content in it.

Code:
   .. code-block:: typoscript

      This is the typoscript in the datastructure on the field/area which I want to hide if it is empty:
          <TypoScript><![CDATA[
          10= RECORDS
          10.source.current=1
          10.tables = tt_content
          10.stdWrap.ifEmpty{
              cObject = LOAD_REGISTER
              cObject.rightContentEmpty = 1
          }
          10.stdWrap {
              required = 1
              wrap = <!--TYPO3SEARCH_begin--> | <!--TYPO3SEARCH_end-->
          }
          ]]></TypoScript>
      So here I say that if there is no content the register should add rightContentEmpty with a value of 1.

      In my typoscript template (setup field), I add the following code:
      page.headerData.1001 = TEXT
      page.headerData.1001 {
        value (
        <style type="text/css">
        /*<![CDATA[*/
        #right {display:none;} #normal {width: 501px; margin-right: 33px;}
        /*]]>*/
        </style>
        )
        if.isTrue.data = register:rightContentEmpty
      }
      So if rightContentEmpty is true, then the css will be added with the headerData.


.. index:: HMENU, updated, special, TMENU, SYS_LASTCHANGED, COA_INT
.. _s2009-6:
.. _s2009-6-Show-last-updated-pages:

2009-6 Show last updated pages
==============================

by **Martin Holtz**, 2009-02-03 09:25:54, old #109, typoscript

Keywords:
   HMENU, updated, special, TMENU, SYS\_LASTCHANGED, COA\_INT

Description:
   This is an simple typoscript to show the last updated pages. Read TSref for
   Details.

   I put it into an COA\_INT, so it is not cached.

Code:
   .. code-block:: typoscript

      lib.lastUpdated = COA_INT
      lib.lastUpdated {
        10 = HMENU
        10.wrap = <h3>Last updated:</h3><p>|</p>
        10 {
           special = updated
           special.value = 1
           special.depth = 20
         1 = TMENU
         1 {
            NO.allWrap = |
            NO.allWrap.append = TEXT
            NO.allWrap.append {
               field = SYS_LASTCHANGED
               date = d.m.
               noTrimWrap = | &nbsp;(|)<br />|
            }
         }
        }
      }


.. index:: templavoila, case, to, ds
.. _s2009-5:
.. _s2009-5-Finding-the-right-TemplaVoila-Template-Object-TO-with-Typoscript:

2009-5 Finding the right TemplaVoila Template Object (TO) with Typoscript
=========================================================================

by **Patrick Broens**, 2009-02-02 11:37:07, old #108, typoscript

Keywords:
   templavoila, case, to, ds

Description:
   If you need to set a part of Typoscript, according to a TemplaVoila Template
   Object, you can use CASE.

   In the example below I add a certain stylesheet according to a Template
   Object (TO). You can use this for all kinds of Typoscript settings.

   The numbers 1-4 represent the uid's of the Template Objects.

Code:
   .. code-block:: typoscript

      page.headerData {
         10 = CASE
         10 {
            // Get TO on current page
            key.data = page:tx_templavoila_to

            // default: No TO defined on current page
            default = CASE
            default {
               // Find page back to root where TO for subpages has been defined
               key.cObject = TEXT
               key.cObject.data = levelfield:-1, tx_templavoila_next_to, slide

               // default: No page with TO for subpages defined
               default = CASE
               default {
                  // Find page back to root with TO defined
                  key.cObject = TEXT
                  key.cObject.data = levelfield:-1, tx_templavoila_to, slide

                  // default: You should never end up here, because this means no template has been set which should result in an error
                  default = TEXT
                  default {
                     value = <link href="fileadmin/site/css/layoutdefault.css" rel="stylesheet" type="text/css" />
                  }

                  1 = TEXT
                  1 {
                     value  = <link href="fileadmin/site/css/layout1.css" rel="stylesheet" type="text/css" />
                  }
                  2 = TEXT
                  2 {
                     value = <link href="fileadmin/site/css/layout2.css" rel="stylesheet" type="text/css" />
                  }
                  3 = TEXT
                  3 {
                     value = <link href="fileadmin/site/css/layout3.css" rel="stylesheet" type="text/css" />
                  }
                  4 = TEXT
                  4 {
                     value = CSS for TO with uid=4
                  }
               }

               1 < .default.1
               2 < .default.2
               3 < .default.3
               4 < .default.4
            }

            1 < .default.1
            2 < .default.2
            3 < .default.3
            4 < .default.4
         }
      }


.. index:: TypoScript, TSFE, extension
.. _s2009-4:
.. _s2009-4-Read-TypoScript-from-Extension:

2009-4 Read TypoScript from Extension
=====================================

by **Riccardo De Contardi**, 2009-01-26 13:42:50, old #107, php

Keywords:
   TypoScript, TSFE, extension

Description:
   Get the whole TypoScript Configuration in your Extension - or an special
   value...

Code:
   .. code-block:: php

      The Array for the Top-Level elements:
      $GLOBALS['TSFE']->tmpl->setup

      f.e.:
      $GLOBALS['TSFE']->tmpl->setup['plugin.']['tt_news']['templateFile']


.. index:: FE, TypoScript, cObject, exec_SELECTquery, typoscript, makeInstance, tslib_cObj, cObj, cObjGetSingle, conf
.. _s2009-3:
.. _s2009-3-Using-TypoScript-in-Extension:

2009-3 Using TypoScript in Extension
====================================

by **Miroslav Monkevic**, 2009-01-20 13:52:41, old #106, php

Keywords:
   FE, TypoScript, cObject, exec\_SELECTquery, typoscript, makeInstance,
   tslib\_cObj, cObj, cObjGetSingle, conf

Description:
   If you want to use TypoScript in your frontend-extension, you should not
   overwrite $cObj->data

Code:
   .. code-block:: php

      // Make your local cObject
      $cObj = t3lib_div::makeInstance('tslib_cObj');
      while ($row = ...) {
        $cObj->start($row, $tableName);
        $markerArray['###fieldname###'] = $cObj->cObjGetSingle($this->conf['fieldname'],$this->conf['fieldname.']);
        ...
      }

      // Then you can use TypoScript like that:
      plugin.tx_yourplugin_pi1.fieldname = TEXT
      plugin.tx_yourplugin_pi1.fieldname {
         field = fieldname
         strftime = %A
         // this field will be available if it is in $row
         append = TEXT
         append.field = anotherfield
      }


.. index:: TMENU, doktype, external url
.. _s2009-2:
.. _s2009-2-TMENU-with-external-links:

2009-2 TMENU with external links
================================

by **Christopher**, 2009-01-11 18:45:58, old #105, typoscript

Keywords:
   TMENU, doktype, external url

Description:
   This menu alters the default TMENU behaviour with respect to pages of the type
   "External URL" (doktype 3); here, instead of linking to an internal TYPO3 page
   which then redirects to the external url, the href attribute of "External URL"
   pages just contains the destination url.

Code:
   .. code-block:: typoscript

      lib.extUrl = HMENU
      lib.extUrl {
        1 = TMENU
        1 {
          expAll = 1
          noBlur = 1
          wrap = <ul>|</ul>

          NO {
            stdWrap.cObject = CASE
            stdWrap.cObject {
              key.field = doktype
              default = TEXT
              default {
                field = title
                typolink {
                  parameter.data = field:uid
                }
              }

              3 < .default
              3 {
                stdWrap.htmlSpecialChars = 1
                typolink {
                  parameter {
                    data >
                    dataWrap = http://{field:url}
                  }
                }
              }
            }
            doNotLinkIt = 1
            wrapItemAndSub = <li>|</li>
          }
        }
      }


.. index:: fe, API, extension
.. _s2009-1:
.. _s2009-1-include-additional-header-data-in-plugins-like-JS-or-CSS:

2009-1 include additional header data in plugins (like JS or CSS)
=================================================================

by **Tomaz Zaman**, 2009-01-08 17:28:45, old #104, php

Keywords:
   fe, API, extension

Description:
   How to include the JavaScript or CSS link in the <head> section of the output
   page, put the following snippet into the main() function of the plugin.

Code:
   .. code-block:: php

      $GLOBALS['TSFE']->additionalHeaderData[$this->extKey] = '<script src="'.t3lib_extMgm::siteRelPath($this->extKey).'pi1/res/my.js" type="text/javascript"></script>';


