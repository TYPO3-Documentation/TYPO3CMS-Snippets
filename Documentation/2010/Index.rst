
.. include:: ../Includes.txt

===============
2010
===============

.. index:: ratings, fluid
.. _s2010-25:
.. _s2010-25-EXTratings-ViewHelper:

2010-25 EXT:ratings ViewHelper
==============================

by **Søren Malling**, 2010-10-07 12:45:34, old #277, php

Keywords:
   ratings, fluid

Description:
   Fluid ViewHelper implementing EXT:ratings.

   Remember to rename class if it doesn't fit your needs :) But keep the author
   note, please!

Code:
   .. code-block:: php

      <?php

      /**
       * This class is a view helper for EXT:ratings
       *
       * @package TYPO3
       * @subpackage Fluid
       * @version
       */
      class Tx_Mediadatabase_ViewHelpers_RatingViewHelper extends Tx_Fluid_Core_ViewHelper_AbstractViewHelper {

          /**
           * Renders a star rating
           *
           * @param int Storage pid
           * @param string Record ref from url
           * @param string Variable from the url to get the uid from
           * @param string Filename of additional css file
           * @param int Size of rating images
           * @param array map a prefix to a table, to use for reference in tx_ratings_data table
           *
           * @author Soren Malling <sma@dkm.dk>
           */
          public function render($storagePid, $ref, $showUidMap = 'showUid', $additionalCSS = 'komodeomedia.css', $ratingImageWidth = 30, $prefixToTableMap = NULL) {

            require_once(t3lib_extMgm::extPath('ratings', 'class.tx_ratings_api.php'));

            $apiObj = t3lib_div::makeInstance('tx_ratings_api');

            $refVariables = t3lib_div::_GP(key($prefixToTableMap));
            $conf = $apiObj->getDefaultConfig();
            $conf['includeLibs'] = 'EXT:ratings/pi1/class.tx_ratings_pi1.php';
            $conf['ref'] = $prefixToTableMap[key($prefixToTableMap)] . '_' . $refVariables[$showUidMap];
            if($additionalCSS != NULL) {
               $conf['additionalCSS'] = 'EXT:ratings/res/' . $additionalCSS;
            }
            $conf['ratingImageWidth'] = $ratingImageWidth;


            $cObj = t3lib_div::makeInstance('tslib_cObj');
            /* @var $cObj tslib_cObj */
            $cObj->start(array());
            return $cObj->cObjGetSingle('USER_INT', $conf);
          }
      }

      ?>


.. index:: Development, tt_news, Development
.. _s2010-24:
.. _s2010-24-Create-different-tt_news-caption_stdWrapwrap:

2010-24 Create different tt_news caption_stdWrap.wrap
=====================================================

by **Sacha Vorbeck**, 2010-09-27 12:20:25, old #276, typoscript

Keywords:
   Development, tt\_news, Development

Description:
   I would like to render the news single image with a caption with a nice
   <dl><dt>###image###</dt><dd>###text###</dd></dl>

   structure.
   Problem is, with the normal wrap you get:
   <dl><dt>###image###<dd>###text###</dd></dt></dl>
   if a caption is there.

   Solution:

Code:
   .. code-block:: typoscript

      imageWrapIfAny =  <dl class="newsImage"><dt>|</dl>
      caption_stdWrap >
      caption_stdWrap.cObject = COA
      caption_stdWrap.cObject {
         10 = TEXT
         10 {
            field = imagecaption
            wrap = </dt><dd class="newsImageCaption">|</dd>
            if.isTrue.field = imagecaption
         }
         20 = TEXT
         20 {
            field = imagecaption
            wrap = </dt>|
            if.isFalse.field = imagecaption
         }
      }


.. index:: rte, htmlarea, tsconfig
.. _s2010-23:
.. _s2010-23-htmlarea-config:

2010-23 htmlarea config
=======================

by **Felix Nagel**, 2010-09-17 16:45:46, old #272, typoscript

Keywords:
   rte, htmlarea, tsconfig

Description:
   RTE Config for htmlarea delivered with TYPO3 4.2.2
   This is a extended version of the standard config.

   See link for latest version!

Links:
   # This is a modified version by Felix Nagel - FelixNagel.com # original file
   could be found under:
   \\typo3\\sysext\\rtehtmlarea\\res\\typical\\pageTSConfig.txt # based upon
   htmlare delivered with TYPO3 4.2.2 # latest version:
   http://www.felixnagel.com/blog/arti

Code:
   .. code-block:: typoscript

      # This is a modified version by Felix Nagel - FelixNagel.com
      # original file could be found under: \typo3\sysext\rtehtmlarea\res\typical\pageTSConfig.txt
      # based upon htmlarea delivered with TYPO3 4.2.2
      # latest version: http://www.felixnagel.com/blog/artikel/2010/09/16/konfiguration-fuer-htmlarea-rte-typo3-44x/

      # ***************************************************************************************
      # "Typical" Page TSconfig for htmlArea RTE and Classic RTE
      #
      # Sets Page TSConfig with most commonly used features representing a good start for typical sites.
      #
      # @author   Stanislas Rolland <stanislas.rolland(arobas)fructifor.ca>
      #
      # TYPO3 SVN ID: $Id: pageTSConfig.txt 5131 2009-03-06 17:53:40Z stan $
      # ***************************************************************************************


         # Define labels and styles to be applied to class selectors in the interface of the RTE
         # The examples included here make partial re-use of color scheme and frame scheme from CSS Styled Content extension
      RTE.classes >
      RTE.classes.ui-state-highlight {
         name = Hinweis
      }
      RTE.classes.ui-state-error {
         name = Warnung
      }
      RTE.classes.small {
         name = Kleine Schrift
      }
      RTE.classes.ui-helper-hidden-accessible {
         name = Barrierefrei verstecken
      }

         # Anchor classes configuration for use by the anchor accesibility feature (htmlArea RTE only)
         # removed anchor images via image >
      RTE.classesAnchor {
         externalLink {
            class = external-link
            type = url
            titleText = LLL:EXT:rtehtmlarea/res/accessibilityicons/locallang.xml:external_link_titleText
            image >
         }
         externalLinkInNewWindow {
            class = external-link-new-window
            type = url
            titleText = LLL:EXT:rtehtmlarea/res/accessibilityicons/locallang.xml:external_link_new_window_titleText
            image >
         }
         internalLink {
            class = internal-link
            type = page
            titleText = LLL:EXT:rtehtmlarea/res/accessibilityicons/locallang.xml:internal_link_titleText
            image >
         }
         internalLinkInNewWindow {
            class = internal-link-new-window
            type = page
            titleText = LLL:EXT:rtehtmlarea/res/accessibilityicons/locallang.xml:internal_link_new_window_titleText
            image >
         }
         download {
            class = download
            type = file
            titleText = LLL:EXT:rtehtmlarea/res/accessibilityicons/locallang.xml:download_titleText
            image >
         }
         mail {
            class = mail
            type = mail
            titleText = LLL:EXT:rtehtmlarea/res/accessibilityicons/locallang.xml:mail_titleText
            image >
         }
      }

         # Default RTE configuration
      RTE.default {

         # START added configs (not included in stdandard file)

            # add CSS file for rte
         contentCSS = fileadmin/templates/v3.0/css/rte.css

            # disable right click
         disableRightClick = 1

            # acronym sys folder (id, not needed for admins as webmounts is used)
         buttons.acronym.pages = 306
         buttons.lockBEUserToDBmounts = 1

         # END added configs


            # Markup options (htmlArea RTE only)
         enableWordClean = 1
         removeTrailingBR = 1
         removeComments = 1
         removeTags = center, font, o:p, sdfield, u
         removeTagsAndContents = link, meta, script, style, title

            # Toolbar options
            # The TCA configuration may add buttons to the toolbar
            # The following buttons are specific to Classic RTE: class
            # The following buttons are specific to htmlArea RTE: blockstylelabel, blockstyle, textstylelabel, textstyle,
            #      insertcharacter, findreplace, removeformat, toggleborders, tableproperties,
            #      rowproperties, rowinsertabove, rowinsertunder, rowdelete, rowsplit,
            #      columninsertbefore, columninsertafter, columndelete, columnsplit,
            #      cellproperties, cellinsertbefore, cellinsertafter, celldelete, cellsplit, cellmerge

            # Original showButtons
         # showButtons (
            # class, blockstylelabel, blockstyle, textstylelabel, textstyle,
            # formatblock, bold, italic, subscript, superscript,
            # orderedlist, unorderedlist, outdent, indent, textindicator,
            # insertcharacter, link, table, findreplace, chMode, removeformat, undo, redo, about,
            # toggleborders, tableproperties,
            # rowproperties, rowinsertabove, rowinsertunder, rowdelete, rowsplit,
            # columninsertbefore, columninsertafter, columndelete, columnsplit,
            # cellproperties, cellinsertbefore, cellinsertafter, celldelete, cellsplit, cellmerge
            # hMode, underline, strikethrough, superscript, lefttoright, righttoleft, left, center, right, justifyfull, inserttag,  removeformat, copy, cut, paste
         # )
             # modified showButtons array
         showButtons (
            blockstylelabel, blockstyle, formatblock,
            bold, italic, subscript, superscript,
            orderedlist, unorderedlist
            insertcharacter, link, acronym, table, findreplace, chMode, removeformat, undo, redo, about,
            toggleborders, tableproperties,
            rowproperties, rowinsertabove, rowinsertunder, rowdelete, rowsplit,
            columninsertbefore, columninsertafter, columndelete, columnsplit,
            cellproperties, cellinsertbefore, cellinsertafter, celldelete, cellsplit, cellmerge
            hMode, underline, strikethrough, superscript, removeformat, copy, cut, paste
         )

         toolbarOrder = formatblock, space, blockstyle, space, textstylelabel, textstyle, bar, linebreak, fontstyle, space, fontsize, space, , bar, bold, italic, underline, bar, strikethrough, subscript, superscript, bar, lefttoright, righttoleft, bar, left, center, right, justifyfull, bar, orderedlist, unorderedlist, outdent, indent, bar, textcolor, bgcolor, textindicator, bar, emoticon, insertcharacter, line, link, acronym, image, user, bar, findreplace, spellcheck, bar, chMode, inserttag, removeformat, bar, copy, cut, paste, bar, undo, redo, bar, showhelp, about, linebreak,  table, toggleborders, bar, tableproperties, bar, rowproperties, rowinsertabove, rowinsertunder, rowdelete, rowsplit, bar, columninsertbefore, columninsertafter, columndelete, columnsplit, bar, cellproperties, cellinsertbefore, cellinsertafter, celldelete, cellsplit, cellmerge


            # More toolbar options (htmlArea RTE only)
         keepButtonGroupTogether = 1

            # Enable status bar (htmlArea RTE only)
         showStatusBar =  1

            # Hide infrequently used paragraph types in the paragraph type selector (formatblock button)
         hidePStyleItems = h5,h6,pre,address,div

            # Add default example styles
            # Left, center, right and justify alignment of text in block elements
         # inlineStyle.text-alignment (
            # p.align-left, h1.align-left, h2.align-left, h3.align-left, h4.align-left, h5.align-left, h6.align-left, div.align-left, address.align-left { text-align: left; }
            # p.align-center, h1.align-center, h2.align-center, h3.align-center, h4.align-center, h5.align-center, h6.align-center, div.align-center, address.align-center { text-align: center; }
            # p.align-right, h1.align-right, h2.align-right, h3.align-right, h4.align-right, h5.align-right, h6.align-right, div.align-right, address.align-right { text-align: right; }
            # p.align-justify, h1.align-justify, h2.align-justify, h3.align-justify, h4.align-justify, h5.align-justify, h6.align-justify, div.align-justify, address.align-justify { text-align: justify; }
         # )

            # Two frame examples taken from the example CSS file of CSS Styled Content extension and applied to p and table block elements.
         # inlineStyle.frames (
            # p.csc-frame-frame1, table.csc-frame-frame1 { background-color: #EDEBF1; padding: 2px 4px 2px 4px; border: 1px solid #333333; }
            # p.csc-frame-frame2, table.csc-frame-frame2 { background-color: #F5FFAA; padding: 2px 4px 2px 4px; border: 1px solid #333333; }
         # )

            # Bullet styles for unordered lists.
         # inlineStyle.ul (
            # ul.component-items { color: #186900; list-style-type: circle; }
            # ul.action-items { color: #8A0020; list-style-image: url(img/red_arrow_bullet.gif); }
         # )

            # Numbering styles for ordered lists.
         # inlineStyle.ol (
            # ol.component-items-ordered { color: #10007B; list-style-type: lower-roman; }
            # ol.action-items-ordered { color: #8A0020; list-style-type: lower-greek; }
         # )

            # Three inline text colors taken from the color scheme of CSS Styled Content extension.
         # inlineStyle.inline-text (
            # span.important { color: #8A0020; }
            # span.name-of-person { color: #10007B; }
            # span.detail { color: #186900; }
         # )

            # Default selectors for the default configuration of the link accessibity feature.
         # inlineStyle.accessibility (
            # a.external-link {}
            # a.external-link-new-window {}
            # a.internal-link {}
            # a.internal-link-new-window {}
            # a.download {}
            # a.mail {}
         # )
            # Default selector for indentation.
         # inlineStyle.indentation (
            # div.indent { margin-left: 2em; }
         # )

            # Use stylesheet file rather than the above mainStyleOverride and inlineStyle properties to style the contents (htmlArea RTE only).
            # When RTE.default.contentCSS is not specified, file EXT:rtehtmlarea/res/contentcsss/default.css is used.
         ignoreMainStyleOverride = 1

            # List all class selectors that are allowed on the way to the database
         proc.allowedClasses (
            ui-state-highlight,
            ui-state-error,
            ui-helper-hidden-accessible,
            small,
            linkbutton,
            linkbutton-block
         )

            # classesParagraph, classesTable, classesTD, classesLinks, classesCharacter
            # Classic RTE: Specify the list of class selectors that should be presented in the RTE interface:
            # htmlArea RTE: Restrict the list of class selectors presented by the RTE to the following:
         # classesParagraph (
            # align-left, align-center, align-right,
            # csc-frame-frame1, csc-frame-frame2
         # )
         # classesTable = csc-frame-frame1, csc-frame-frame2
         # classesTD = align-left, align-center, align-right
         # classesLinks = external-link, external-link-new-window, internal-link, internal-link-new-window, download, mail
         # classesCharacter = important, name-of-person, detail

            # Configuration of the anchor accessibility feature (htmlArea RTE only)
            # These classes should also be in the list of allowedClasses.
         classesAnchor = linkbutton, linkbutton-block
         classesAnchor.default {
            page =
            url =
            file =
            mail =
         }

            # Configuration specific to the TableOperations feature (htmlArea RTE only)
            # Remove the following fieldsets from the table operations dialogs
         disableAlignmentFieldsetInTableOperations = 1
         disableSpacingFieldsetInTableOperations = 1
         disableColorFieldsetInTableOperations = 1
         disableLayoutFieldsetInTableOperations = 1
         disableBordersFieldsetInTableOperations = 1

            # Show borders on table creation
         buttons.toggleborders.setOnTableCreation = 1

            # Configuration specific to the bold and italic buttons (htmlArea RTE only)
            # Add hotkeys associated with bold and italic buttons
         buttons.bold.hotKey = b
         buttons.italic.hotKey = i
      }

         # front end RTE configuration for the general public (htmlArea RTE only)
      RTE.default.FE < RTE.default
      RTE.default.FE.showStatusBar = 0
      RTE.default.FE.hideButtons = chMode, blockstyle, textstyle, underline, strikethrough, subscript, superscript, lefttoright, righttoleft, left, center, right, justifyfull, table, inserttag, findreplace, removeformat, copy, cut, paste
      RTE.default.FE.FE >
      RTE.default.FE.userElements >
      RTE.default.FE.userLinks >


.. index:: news, tt_news, img_resource
.. _s2010-22:
.. _s2010-22-Render-tt_news-list-image-as-CSS-background-image:

2010-22 Render tt_news list image as CSS background image
=========================================================

by **Sacha Vorbeck**, 2010-09-16 09:16:02, old #268, typoscript

Keywords:
   news, tt\_news, img\_resource

Description:
   Fill the NEWS\_IMAGE marker with a path to the image instead of the complete
   image-tag. This way you can use the marker to create CSS-background images.

Code:
   .. code-block:: typoscript

      displayList {
        image >
        image.stdWrap.cObject = IMG_RESOURCE
        image.stdWrap.cObject {
        file {
          import.field = image
          import = uploads/pics/
          listNum = 0
          maxW = 303
          maxH = 215
        }
      }
      }


.. index:: FE, TypoScript, FE
.. _s2010-21:
.. _s2010-21-Programmer:

2010-21 Programmer
==================

by **Duraisingh**, 2010-08-03 13:26:57, old #256, php

Keywords:
   FE, TypoScript, FE

Description:
   Learning Typo3

Code:
   .. code-block:: php

      TYPO3 RTE Table Classes in Table Properties

      Now I have the code in TS config

      RTE.default.contentCSS = fileadmin/default/templates/css/example.css
      RTE.default.showTagFreeClasses = 1

      It shows all the tag free classes in the body list, but not in the Table Classes list

      How can i add my css style classes to Table Classes list in Table Properties in RTE?


.. index:: image, caption
.. _s2010-20:
.. _s2010-20-Add-link-to-image-caption-V2:

2010-20 Add link to image caption V2.
=====================================

by **Gary Wong**, 2010-07-29 06:59:28, old #255, typoscript

Keywords:
   image, caption

Description:
   Updated snippet to accommodate multiple links when there are multiple images
   (comma separated)
   Add link to an image caption. The image in Text with Image and Image content
   elements can be assigned a link. But you might want the caption to be linked to
   the same location. This line of code will add the link to the caption if a link
   is set.

Code:
   .. code-block:: typoscript

      # Adds link to image caption, if there is one
      tt_content.image.20.caption.1.typolink.parameter.field = image_link
      tt_content.image.20.caption.1.typolink.parameter.listNum.stdWrap.data = register:IMAGE_NUM_CURRENT


.. index:: image, caption
.. _s2010-19:
.. _s2010-19-Add-link-to-image-caption:

2010-19 Add link to image caption.
==================================

by **Gary Wong**, 2010-07-28 22:40:27, old #254, typoscript

Keywords:
   image, caption

Description:
   Add link to an image caption. The image in Text with Image and Image content
   elements can be assigned a link.  But you might want the caption to be linked
   to the same location.  This line of code will add the link to the caption if a
   link is set.

Code:
   .. code-block:: typoscript

      # Add to Page TS Setup
      # Adds link to image caption, if there is one
      tt_content.image.20.caption.1.typolink.parameter.field = image_link


.. index:: TypoScript, fe_user, login, logout, condition
.. _s2010-18:
.. _s2010-18-Login--logout-link-for-FE-users-depending-on-session-state:

2010-18 Login / logout link for FE users depending on session state
===================================================================

by **Steffen Müller**, 2010-03-22 14:12:06, old #252, typoscript

Keywords:
   TypoScript, fe\_user, login, logout, condition

Description:
   Creates login or logout link for FE users.
   - If FE user is NOT logged in, the link text is LOGIN and points to the
   login form page.
   - If FE user IS logged in, the links text is LOGOUT and directly triggers a
   logout.

Code:
   .. code-block:: typoscript

      page.10 = COA_INT
      page.10 {
        ## Login link (shown if FE user is NOT logged in)
        10 = TEXT
        10 {
          value = LOGIN
          typolink {
            parameter = 123 // pid of page with login form
          }
          if.isFalse.data = TSFE:fe_user|user|username
        }

        ## Logout link (shown if FE user IS logged in)
        20 < .10
        20.if.negate = 1
        20 {
          value = LOGOUT
          typolink {
            additionalParams = &logintype=logout
          }
        }
      }


.. index:: Realurl, tt_news, includeLibs, localize
.. _s2010-17:
.. _s2010-17-tt_news-cat--localize-cat-title-with-Realurl:

2010-17 tt_news cat : localize cat title with Realurl
=====================================================

by **Laurent Cherpit**, 2010-03-22 12:55:14, old #251, php

Keywords:
   Realurl, tt\_news, includeLibs, localize

Description:
   If like me you need to translate the tt\_news category title with realurl, the
   next code snippet should do it.

   Attention: before realurl 1.7.1, the next patch is required :
   http://bugs.typo3.org/view.php?id=13721

Links:
   #13721

Code:
   .. code-block:: php

      // addthis to your TS setup:
      // include lib to handle tt_news cat title in alternative lang
      page.includeLibs.user_realurlEncTitle = fileadmin/scripts/user_realurlEncTitle.php

      // In realUrl php config file,add to the postVarSets array in the lookUpTable > useUniqueCache_conf key, add the next key=val:
      // 'encodeTitle_userProc' => 'user_realurlEncTitle->process'


      // next the content of the user_realurlEncTitle.php

      <?php

      /**
       *  @author     Laurent cherpit
       *  @version    0.1.0
       *  <2010-03-22>
       *  little tricks to set cat title to realurl in alternative language title
       *  Attention: before realurl 1.7.1, the next path is required : http://bugs.typo3.org/view.php?id=13721
       */
      class user_realurlEncTitle {

          protected $_keys;
          protected $_cfg;

          public function process( &$params ) {

              $titlesOl = '';
              $this->_keys = $params['pObj']->orig_paramKeyValues;
              $this->_cfg  = $params['encodingConfiguration'];

              // work on tt_newscat and syslang > 0
              if( isset( $this->_keys['tx_ttnews[cat]'] ) && $this->_keys['L'] > 0 ) {

                  $res = $GLOBALS['TYPO3_DB']->exec_SELECTquery(
                      'title_lang_ol',
                      'tt_news_cat',
                      'uid=' . (int)$this->_keys['tx_ttnews[cat]']
                  );

                  if( ( $row = $GLOBALS['TYPO3_DB']->sql_fetch_assoc( $res ) ) ) {
                      $titlesOl = $row['title_lang_ol'];
                  }
                  $GLOBALS['TYPO3_DB']->sql_free_result( $res );

                  if( !empty( $titlesOl ) ) {

                      $titlesOl = t3lib_div::trimExplode( '|', $titlesOl );

                      // get alt label in order of lang id declaration
                      $processedTitle = $titlesOl[((int)$this->_keys['L']-1)] ? $titlesOl[((int)$this->_keys['L']-1)] : $params['processedTitle'];

                      unset( $titlesOl );

                      // clean title
                      $this->_cleanTitle( $processedTitle );

                      $params['processedTitle'] = $processedTitle;

                      unset( $processedTitle );
                  }
              }

              return $params['processedTitle'];
          }

          /**
           * Code snippet from realurl
           * @param string $processedTitle
           */
          protected function _cleanTitle( &$processedTitle ) {

              // Fetch character set:
              $charset = $GLOBALS['TYPO3_CONF_VARS']['BE']['forceCharset'] ? $GLOBALS['TYPO3_CONF_VARS']['BE']['forceCharset'] : $GLOBALS['TSFE']->defaultCharSet;

              // Convert to lowercase:
              if( $this->_cfg['strtolower']) {
                  $processedTitle = $GLOBALS['TSFE']->csConvObj->conv_case( $charset, $processedTitle, 'toLower');
              }

              // Convert some special tokens to the space character:
              $space = $this->_cfg['spaceCharacter'] ? substr( $this->_cfg['spaceCharacter'], 0, 1 ) : '_';
              $processedTitle = strtr($processedTitle, ' -+_', $space . $space . $space . $space); // convert spaces

              // Convert extended letters to ascii equivalents:
              $processedTitle = $GLOBALS['TSFE']->csConvObj->specCharsToASCII($charset, $processedTitle);

              // Strip the rest...:
              $processedTitle = preg_replace( '/[^a-zA-Z0-9\\' . $space . ']/', '', $processedTitle ); // strip the rest
              $processedTitle = preg_replace( '/\\' . $space . '+/', $space, $processedTitle ); // Convert multiple 'spaces' to a single one
              $processedTitle = trim( $processedTitle, $space );

          }
      }


.. index:: TypoScript, media, video, youtube
.. _s2010-16:
.. _s2010-16-YouTube-Video-with-MediaCE:

2010-16 YouTube Video with MediaCE
==================================

by **Steffen Kamper**, 2010-03-01 12:28:43, old #250, typoscript

Keywords:
   TypoScript, media, video, youtube

Description:
   this snippet shows a youtube video defined in typoscript

Code:
   .. code-block:: typoscript

      temp.ytvideo < tt_content.media.20
      temp.ytvideo {
        file = http://www.youtube.com/v/1uKoBqDwSiM&fs=1&enablejsapi=1
        forcePlayer = 0
        params.allowFullScreen = true
      }


.. index:: userTS, cache, TSconfig
.. _s2010-15:
.. _s2010-15-Allow-clearing-the-cache-for-BEusers:

2010-15 Allow clearing the cache for BEusers
============================================

by **Marcus Biesioroff**, 2010-02-22 22:40:10, old #249, typoscript

Keywords:
   userTS, cache, TSconfig

Description:
   You can enable "flash icon" clear cache command for common BEuser, just add
   this code to user's and/or user's group TSconfig

Code:
   .. code-block:: typoscript

      options.clearCache.pages = 1
      options.clearCache.all = 1


.. index:: CONTENT
.. _s2010-14:
.. _s2010-14-Latest-comments:

2010-14 Latest comments
=======================

by **Ben van 't Ende**, 2010-02-21 00:48:05, old #248, typoscript

Keywords:
   CONTENT

Description:
   This snippet by Dmitry Dulepov (tHNx) shows a list of latest comments anywhere
   on your site.

Code:
   .. code-block:: typoscript

      lib.latestComments = CONTENT
      lib.latestComments {
         select {
            pidInList = 7
            max = 5
            recursive = 5
            orderBy = crdate DESC
         }
         table = tx_comments_comments
         renderObj = COA
         renderObj {
            10 = TEXT
            10.field = crdate
            10.date = Y/m/d H:i
            10.noTrimWrap = |<h4>Date: ||

            20 = TEXT
            20.dataWrap =  &nbsp;- From: {field:firstname} {field:lastname}</h4> {field:content}<br /><br />
         }
      }


.. index:: search, box, css, typoscript, fe
.. _s2010-13:
.. _s2010-13-Searchbox:

2010-13 Searchbox
=================

by **larsmessmer**, 2010-02-15 21:18:20, old #247, typoscript

Keywords:
   search, box, css, typoscript, fe

Description:
   Add nice Searchbox at any page via marker or TV only with TypoScript and CSS.

Code:
   .. code-block:: typoscript

      #TypoScript for Setup

      lib.searchbox = COA_INT
      lib.searchbox {
        10 = TEXT
        10.typolink.parameter = {$plugin.tx_indexedsearch.searchUID}
        10.typolink.returnLast = url
        10.wrap = <!-- searchbox begin --><form name="search" id="search" class="search" action="|" method="post"><label id="searchLabel" for="searchQuery">suchen</label><input type="text" size="15" name="tx_indexedsearch[sword]" id="searchQuery" value="" /><input class="hidden" type="hidden" name="tx_indexedsearch[_sections]" value="0" /><input type="hidden" name="tx_indexedsearch[pointer]" value="0" /><input type="hidden" name="tx_indexedsearch[ext]" value="0" /><input type="hidden" name="tx_indexedsearch[lang]" value="0" /><button class="submit" name="tx_indexedsearch[submit_button]" type="submit" value="search">search</button></form><!-- searchbox end -->
        # if you need here comes userFunc for exampe ajax search
        20 = USER
        20.userFunc = user_functionname
        20.userFunc < plugin.name
        20.userFunc.cssId = searchQuery
      }

      #TypoScript Constants
      plugin.tx_indexedsearch.searchUID = 25

      #CSS Code for box styling
      /************* search box begin *******************/

      .search {
         clear: right;
         width: 163px;
         margin: 5px 0px 0 0;
         padding: 0;
         border: 1px solid #DDD;
         position: relative;
         line-height: 125%;
         height: 1.75em;
         background: #fff url(../img/bg_search.jpg) top right no-repeat;
      }

      .searchfocus {
         background: #fff url(../img/bg_search_over.jpg) top right no-repeat;
      }

      .search label {
           position: relative;
           display: block;
           width: 117px;
           color: #AAA;
           margin: 0;
           padding: .25em 4px .25em 20px;
           background: transparent url(../img/bg_searchlabel.gif) 4px 50% no-repeat;
           overflow: hidden;
         font-weight: normal;
      }

      .search label.hidden {
           text-indent: -5000px;
      }

      .search #searchQuery {
           margin: 0;
           padding: 0;
           border: none;
           background: none;
           position: absolute;
           top: .25em;
           left: 20px;
           width: 117px;
           height: 1.25em;
           font-family: Arial, Tahoma, Verdana, Helvetica, sans-serif;
           font-size: 100%;
           line-height: 125%;
           color: #000;
      }

      .search #searchQuery:focus {
           outline: none;
      }

      .search .submit {
           position: absolute;
           display: block;
          top: 0;
           right: 0;
           bottom: 0;
           left: 141px;
           width: 22px;
           height: 100%;
           border: none;
           margin: 0;
           padding: 0;
           background: transparent url(../img/bg_searchsubmit.gif) 50% 50% no-repeat;
           text-indent: -5000px;
           overflow: hidden;
      }
      /************* search box end *******************/


.. index:: extbase, controller, validation, error
.. _s2010-12:
.. _s2010-12-Raise-a-custom-validation-error-and-forward-back-to-the--triggering-action:

2010-12 Raise a custom validation error and forward back to the  triggering action.
===================================================================================

by **Nikolas Hagelstein (pulponair)**, 2010-02-15 14:55:16, old #246, php

Keywords:
   extbase, controller, validation, error

Code:
   .. code-block:: php

      $error = t3lib_div::makeInstance('Tx_Extbase_MVC_Controller_ArgumentError',
         '-- Adjust to your error container/property name --');

      $error->addErrors(array(t3lib_div::makeInstance('Tx_Extbase_Validation_Error',
         '-- Adjust to your error description --',
         '-- Adjust to your error code --')));

      $this->request->setErrors(array($error));
      $referrer = $this->request->getArgument('__referrer');

      $this->forward($referrer['actionName'], $referrer['controllerName'],
         $referrer['extensionName'], $this->request->getArguments());


.. index:: extbase
.. _s2010-11:
.. _s2010-11-Redirect-to-another-url-and-end-a-controller-action:

2010-11 Redirect to another url and end a controller action.
============================================================

by **Nikolas Hagelstein (pulponair)**, 2010-02-15 14:48:36, old #245, php

Keywords:
   extbase

Code:
   .. code-block:: php

      $this->response->setStatus(303);
      $this->response->setHeader('Location', $urlToRedirectTo);
      throw new Tx_Extbase_MVC_Exception_StopAction();


.. index:: extbase
.. _s2010-10:
.. _s2010-10-Enforce-persisting-of-pending-records:

2010-10 Enforce persisting of pending records.
==============================================

by **Nikolas Hagelstein (pulponair)**, 2010-02-15 14:44:35, old #244, php

Keywords:
   extbase

Code:
   .. code-block:: php

      $persistenceManager = t3lib_div::makeInstance('Tx_Extbase_Persistence_Manager');
      $persistenceManager->persistAll();


.. index:: Ajax, typenum, FE
.. _s2010-9:
.. _s2010-9-Simple-AJAX-responder-skeleton:

2010-9 Simple AJAX responder skeleton
=====================================

by **Nikolas Hagelstein (pulponair)**, 2010-02-15 14:39:00, old #243, typoscript

Keywords:
   Ajax, typenum, FE

Description:
   TypeNum based TS skeleton.

Links:
   http://www.trashboard.de/2008/11/04/typo3-typoscript-ajax-service-howto/

Code:
   .. code-block:: typoscript

      my_ajax_service = PAGE
      my_ajax_service {
          // typeNum needs to be adjust.
         typeNum = 4711
         config {
             // No html header code i.e. html/body tag.
            disableAllHeaderCode = 1
            xhtml_cleaning = 0
            admPanel = 0
         }
          // Usually we don't whant caching therefor COA_INT
         10 = COA_INT
         10 {
            // CONTENT, RECORDS do whatever you want
         }
      }


.. index:: tt_news, title, page, FE
.. _s2010-8:
.. _s2010-8-News-title-and-main-category-and-more-as-page-title:

2010-8 News title and main category (and more) as page title
============================================================

by **Marcus Biesioroff**, 2010-02-14 12:15:20, old #242, typoscript

Keywords:
   tt\_news, title, page, FE

Description:
   This snippet allows you to simulate common page title for news records, it
   includes service name, category and news title.

   Sample title:
   My News Portal: Extensions updates - New version of tt\_news brings more
   power into your TYPO3

Code:
   .. code-block:: typoscript

      // Notes for tt_news 3.x
      // 1. Below declaration MUST TO be placed after SINGLE view declaration (registers)
      // 2. SINGLE view template MUST TO contain one of these markers: (registers)
      //    ###NEWS_CATEGORY###
      //    ###NEWS_CATEGORY_ROOTLINE###
      //    ###NEWS_CATEGORY_IMAGE###
      // tip: If you don't want to show anywhere values of these markers use HTML comment to hide it in template file,
      //   however, you need to use one of them!
      // tip: Manipulate with sequence and wraps of the COA indexes to get proper title format
      // tip: make sure that you can use page.headerData.10 (is not used by other declaration)
      //   change it's index if required
      // tip: by changing lib.newsTitle.wrap you can also display this combination somewhere on the page.


      [globalVar = GP:tx_ttnews|tt_news > 0]
      lib.newsTitle = COA
      lib.newsTitle {

        10 = TEXT
        10.value = My News Portal
        10.noTrimWrap = ||: |

        20 = RECORDS
        20 {
          source.data = register:newsCategoryUid
          tables = tt_news_cat
          conf.tt_news_cat = TEXT
          conf.tt_news_cat.field = title
          conf.tt_news_cat.noTrimWrap = || - |
        }

        30 = RECORDS
        30 {
          source = {GPvar:tx_ttnews|tt_news}
          source.insertData = 1
          tables = tt_news
          conf.tt_news >
          conf.tt_news = TEXT
          conf.tt_news.field=title
        }
        wrap = <title>|</title>
      }

      page.config.noPageTitle = 2
      page.headerData.10 >
      page.headerData.10 < lib.newsTitle
      [global]


.. index:: userTS, Development, Cache, template, TypoScript
.. _s2010-7:
.. _s2010-7-Disable-Template-caching-when-logged-in-BE-for-development:

2010-7 Disable Template caching when logged in BE (for development)
===================================================================

by **Steffen Müller**, 2010-02-09 20:12:11, old #241, typoscript

Keywords:
   userTS, Development, Cache, template, TypoScript

Description:
   The tiny snippet is very useful when working on TypoScript templates. Normally
   templates are cached and you have to clear cache from time to time while
   developing. With the the snippet, template cache is disabled while you are
   logged in.
   The snippet code needs to be set as userTS in BE user properties.

Code:
   .. code-block:: typoscript

      admPanel.override.tsdebug.forceTemplateParsing = 1


.. index:: tt_news
.. _s2010-6:
.. _s2010-6-Link-tt_news-single-view-image-to-first-URL-in-links:

2010-6 Link tt_news single-view image to first URL in links
===========================================================

by **Gary Wong**, 2010-02-09 18:54:33, old #240, typoscript

Keywords:
   tt\_news

Description:
   This snippet will change/add a link to the tt\_news single-view image.  It will
   link it to the first URL listed in the "links" field of the news item.  If no
   links exist, there is no link.

Code:
   .. code-block:: typoscript

      plugin.tt_news.displaySingle {
          image {
              # turn off default popup anchor
              imageLinkWrap = 0
              # add our link to the first URL in links field
              stdWrap.typolink {
                  parameter = {current:1}
                  parameter {
                 setCurrent.field = links
                 setCurrent.listNum = 0
                      insertData = 1
             }
              }
          }
      }


.. index:: rootline, clickpath
.. _s2010-5:
.. _s2010-5-rootline-without-first-wrap:

2010-5 rootline without first wrap
==================================

by **Marcel Tannich**, 2010-02-02 00:49:30, old #239, typoscript

Keywords:
   rootline, clickpath

Description:
   Stanard rootline without a wrap in the first menu entry.

Code:
   .. code-block:: typoscript

      temp.clickpath =HMENU
         temp.clickpath.special=rootline
         temp.clickpath.special.range = 0 | -1
         temp.clickpath.1=TMENU
         temp.clickpath.1.NO.allWrap= |&nbsp;>&nbsp;
         temp.clickpath.1.NO.ATagParams.dataWrap = title = "{field:subtitle}"
         temp.clickpath.1.NO.allWrap=  | > |*| | > |*| |


.. index:: typoscript, frontend plugin
.. _s2010-4:
.. _s2010-4-Read-foreign-typoscript-configuration:

2010-4 Read foreign typoscript configuration
============================================

by **Christian Buelter**, 2010-01-12 14:38:37, old #238, php

Keywords:
   typoscript, frontend plugin

Description:
   Read the typoscript configuration of an other plugin.

Code:
   .. code-block:: php

      // read foreign typoscript configuration
      $value = $GLOBALS['TSFE']->tmpl->setup['plugin.']['tx_commerce_pi1.'];


.. _s2010-3:
.. _s2010-3-Get-Flexform-data-in-frontend-plugin:

2010-3 Get Flexform data in frontend plugin
===========================================

by **Christian Buelter**, 2010-01-12 14:36:29, old #237, php

Description:
   extract the flexform data entered in the backend and transfer it to
   $this->ffdata

Code:
   .. code-block:: php

      $this->pi_initPIflexForm();
      $piFlexForm = $this->cObj->data['pi_flexform'];
      if (is_array($piFlexForm['data'])) {
         foreach ( $piFlexForm['data'] as $sheet => $data ) {
            foreach ( $data as $lang => $value ) {
               foreach ( $value as $key => $val ) {
                  $this->ffdata[$key] = $this->pi_getFFvalue($piFlexForm, $key, $sheet);
               }
            }
         }
      }


.. index:: split, if, typoscript
.. _s2010-2:
.. _s2010-2-Alphabetical-link-list-using-split-and-if:

2010-2 Alphabetical link list using split and if
================================================

by **Sebastiaan de Jonge**, 2010-01-12 12:56:57, old #236, typoscript

Keywords:
   split, if, typoscript

Description:
   This menu was generated to display an alphanumeric list of links (used in
   combination with a modified tt\_products search, in order to list all products
   alphabetically).

   Displays the list, including an activestate.

Code:
   .. code-block:: typoscript

      lib.alphalist = HTML
      lib.alphalist {
        value = 0-9,a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z
        value {
          wrap = <div class="alphalist">|</div>
          split {
            token = ,
            cObjNum = 1
              1 {
                cObject = HTML
                cObject {
                  value {
                    typolink {
                      parameter = 256
                      additionalParams.current = 1
                      additionalParams.stdWrap.wrap = &tx_ttproducts_pi1[sword]=|
                      ATagParams = class="active"
                      ATagParams.if {
                        value.data = GPvar:tx_ttproducts_pi1|sword
                        equals.current = 1
                      }
                    }
                    current = 1
                    insertData = 1
                    case = upper
                    override = &#35;
                    override.if {
                      value = 0-9
                      equals.current = 1
                    }
                  }
                }
              }
            }
          }
        }
      }


.. index:: configuration, tabs, wizard
.. _s2010-1:
.. _s2010-1-Display-New-Content-Element-Wizard-with-tabs:

2010-1 Display New Content Element Wizard with tabs
===================================================

by **Steffen Kamper**, 2010-01-03 00:49:23, old #235, typoscript

Keywords:
   configuration, tabs, wizard

Description:
   this setting is for pageTS or userTS

Code:
   .. code-block:: typoscript

      #enable tabs for new content element wizard
      mod.wizards.newContentElement.renderMode = tabs

      #in templavoila >= 1.4 use this
      templavoila.wizards.newContentElement.renderMode = tabs

