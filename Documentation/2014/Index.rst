
.. include:: ../Includes.txt

===============
2014
===============


.. index:: rss date
.. _s2014-11:
.. _s2014-11-Display-date-of-pages-as-RFC822-compatible-date:

2014-11 Display date of pages as RFC822 compatible date
=======================================================

by **Sacha Vorbeck**, 2014-12-23 11:47:00, old #478, typoscript

Keywords:
   rss date

Description:
   I created an RSS feed out of a selection of pages with the help of a HMENU
   object. The date of the pages has to be convertetd to RFC822.

   Date is read from the lastUpdated field. If this field is empty, we use the
   date when the page has been modified for the last time.

Code:
   .. code-block:: typoscript

      10 = COA
      10 {
         10 = TEXT
         10 {
            field = lastUpdated
            stdWrap.date = r
            if.isTrue.field = lastUpdated
         }
         20 = TEXT
         20 {
            data = page:SYS_LASTCHANGED
            stdWrap.date = r
            if.isFalse.field = lastUpdated
         }
         stdWrap {
            wrap = <pubDate>|</pubDate>
         }
      }


.. _s2014-10:
.. _s2014-10-Language-switcher-with-HMENU-special-language:

2014-10 Language switcher with HMENU special language
=====================================================

by **Sacha Vorbeck**, 2014-12-09 18:20:55, old #477, typoscript

Description:
   This snippets creates a language switcher with a HMENU special language. The
   trick is to get the TMENUITEM to render only the URL without the a -element and
   wrap it with the HTML necessary to create a select  box.

Code:
   .. code-block:: typoscript

      lib.nav.lang = COA
      lib.nav.lang {
         10 = HMENU
         10 {
            special = language
            special {
               value = 0,2,3,4,5,6
               normalWhenNoLanguage = 0
            }
            1 = TMENU
            1 {
               noBlur = 1
               NO = 1
               ACT = 1
               USERDEF1 = 1
               NO {
                  doNotLinkIt = 1
                  linkWrap = <option value="|</option>
                  stdWrap {
                     setCurrent = Deutsch || English || Fran&ccedil;ais || ??????? || Espa&ntilde;ol || Italiano
                     current = 1
                     typolink {
                        parameter.field = uid
                        returnLast = url
                     }
                  }
               }
               ACT {
                  doNotShowLink = 1
               }
               USERDEF1 {
                  doNotShowLink = 1
               }
            }
         }
         stdWrap {
            wrap = <form id="languageSwitch" class="languageSwitch" name="langSwitch" action="#" method="get" onChange="window.location=document.langSwitch.languageLink.options[document.langSwitch.languageLink.selectedIndex].value;"><select id="language" name="languageLink"><option>{$portal.langlabels.switchLang}</option>|</select></form>
            required = 1
         }
      }


.. index:: TMENU, menu, wrapping, wrap, stdWrap, linkWrap
.. _s2014-9:
.. _s2014-9-Wrapping-of-TMENU:

2014-9 Wrapping of TMENU
========================

by **Sven Burkert**, 2014-08-26 16:27:19, old #476, typoscript

Keywords:
   TMENU, menu, wrapping, wrap, stdWrap, linkWrap

Description:
   There are many configuration options to wrap a menu item. This is an overview
   of all "wraps", valid for TYPO3 versions 4.5 to 6.2.

   If you use a wrap, please be aware of the data types you have to use:

   wrapItemAndSub: wrap/stdWrap
   allStdWrap: stdWrap
   allWrap: wrap/stdWrap
   before: mixed
   stdWrap2: wrap/stdWrap
   linkWrap: wrap
   stdWrap: stdWrap
   after: mixed

Code:
   .. code-block:: typoscript

      <wrapItemAndSub>
         <allStdWrap>
            <allWrap>
               <before />
               <stdWrap2>
                  <linkWrap>
                     <a href=... ATagParams title=ATagTitle>
                        <stdWrap>
                           menu item 1
                        </stdWrap>
                     </a>
                  </linkWrap>
               </stdWrap2>
               <after />
            </allWrap>
         </allStdWrap>
         submenu of menu item 1
      </wrapItemAndSub>
      menu item 2
      submenu of menu item 2


.. _s2014-8:
.. _s2014-8-Make-sg_gallery-10793-compatible-with-TYPO3-62:

2014-8 Make sg_gallery 1.0.793 compatible with TYPO3 6.2
========================================================

by **filigivuji filigivuji**, 2014-07-22 21:42:41, old #475, php

Description:
   Even though sg\_gallery 1.0.793 advertises to be compatible with TYPO3 6.2,
   links are completely missing from the rendered pages, as according to the TYPO3
   documentation the TypoScript "USER" object has been removed in TYPO3 6.0.
   The patch here is a workaround to be used until the extension is updated.

Code:
   .. code-block:: php

      diff -urN sg_glossary_1.0.793/ext_emconf.php sg_glossary/ext_emconf.php
      --- sg_glossary_1.0.793/ext_emconf.php   2014-07-14 18:24:14.000000000 +0200
      +++ sg_glossary/ext_emconf.php   2014-07-18 23:01:37.000000000 +0200
      @@ -37,7 +37,7 @@
             'depends' =>
             array (
                'php' => '5.2.0-5.4.999',
      -         'typo3' => '4.5.0-6.1.999',
      +         'typo3' => '4.5.0-6.2.999',
             ),
             'conflicts' =>
             array (
      @@ -48,4 +48,4 @@
          ),
       );

      -?>
      \ No newline at end of file
      +?>
      diff -urN sg_glossary_1.0.793/ext_typoscript_setup.txt sg_glossary/ext_typoscript_setup.txt
      --- sg_glossary_1.0.793/ext_typoscript_setup.txt   2014-07-14 18:24:14.000000000 +0200
      +++ sg_glossary/ext_typoscript_setup.txt   2014-07-22 21:34:22.000000000 +0200
      @@ -1,10 +1,10 @@
       #Static Setup

       includeLibs.tx_sgglossary_LinkClass =  EXT:sg_glossary/prg/class.tx_sgglossary_LinkClass.inc
      +includeLibs.tx_sgglossary_lexlink = EXT:sg_glossary/prg/lexlink.inc
       lib.tx_sgglossary_lextag {
      -    lex = PHP_SCRIPT
      -    lex.stripNL = 0
      -    lex.file = EXT:sg_glossary/prg/lexlink.inc
      +    lex = USER
      +    lex.userFunc = user_tx_sgglossary->generateLink
       }

       # comment out next 6 lines, if you dont want to use the lex-tags
      diff -urN sg_glossary_1.0.793/prg/lexlink.inc sg_glossary/prg/lexlink.inc
      --- sg_glossary_1.0.793/prg/lexlink.inc   2014-07-14 18:24:14.000000000 +0200
      +++ sg_glossary/prg/lexlink.inc   2014-07-22 21:33:24.000000000 +0200
      @@ -1,5 +1,10 @@
       <?php
      -   $lexlink = new tx_sgglossary_LinkClass;
      -   $contentOfTag = $this->cObj->getCurrentVal();
      -   $content = $lexlink->doLexLink($contentOfTag,$this->cObj->parameters);
      -?>
      \ No newline at end of file
      +class user_tx_sgglossary {
      +   function generateLink() {
      +      $lexlink = new tx_sgglossary_LinkClass;
      +      $contentOfTag = $this->cObj->getCurrentVal();
      +      $content = $lexlink->doLexLink($contentOfTag, $this->cObj->parameters);
      +      return $content;
      +   }
      +}
      +?>


.. index:: sitemap, csv, changed
.. _s2014-7:
.. _s2014-7-Export-Sitemap-as-CSV:

2014-7 Export Sitemap as CSV
============================

by **Markus**, 2014-07-01 18:01:33, old #474, typoscript

Keywords:
   sitemap, csv, changed

Description:
   Snippet to generate a CSV-Sitemap. After adding the code, you can call
   /?type=888 on your site to get the file as sitemap.csv with a complete overview
   of all pages. Useful for reference e.g. if you need to check all meta tags or
   want to perform a task on all pages and need a checklist.
   For security reasons, the sitemap is only generated when you're logged into
   the backend.
   Also contains info on latest changes of pages or content elements in pages
   (!).

Code:
   .. code-block:: typoscript

      lib.tue_sitemapcsv = COA_INT
      lib.tue_sitemapcsv {
         5 = TEXT
         5 {
            value (
      "URL";"Latest change";"Doktype";"Title";"Nav Title";"Description"

            )
         }
         10 = COA
         10 {
            10 = TEXT
            10 {
               typolink {
                  forceAbsoluteUrl = 1
                  parameter.field = uid
                  returnLast = url
               }
               wrap = "|";
               replacement.10 {
                  search = "
                  replace = ""
               }
            }
            20 = CONTENT
            20 {
               table = tt_content
               select {
                  pidInList.field = uid
                  orderBy = tstamp DESC
                  languageField = sys_language_uid
                  max = 1
               }
               renderObj = COA
               renderObj {
                  10 = TEXT
                  10 {
                     field = tstamp
                     date = c
                     postCObject = TEXT
                     postCObject {
                        field = tstamp
                        age = " Minutes| Hours| Days| Years| Minute| Hour| Day| Year"
                        noTrimWrap = | (vor |)|
                     }
                     if {
                        value.field = tstamp
                        isLessThan.data = DB:pages:{field:uid}:tstamp
                        isLessThan.data.insertData = 1
                     }
                  }
               }
               stdWrap {
                  ifEmpty.cObject < lib.tue_sitemapcsv.10.20.renderObj.10
                  ifEmpty.cObject.if >
                  replacement < lib.tue_sitemapcsv.10.10.replacement
                  wrap < lib.tue_sitemapcsv.10.10.wrap
               }
            }
            30 = CASE
            30 {
               key.field = doktype
               1 = TEXT
               1.value = Standard
               3 = TEXT
               3.value = External URL
               4 = TEXT
               4.value = Shortcut
               6 = TEXT
               6.value = Backend User Section
               stdWrap {
                  replacement < lib.tue_sitemapcsv.10.10.replacement
                  wrap < lib.tue_sitemapcsv.10.10.wrap
               }
            }
            40 = TEXT
            40 {
               field = title
               replacement < lib.tue_sitemapcsv.10.10.replacement
               wrap < lib.tue_sitemapcsv.10.10.wrap
            }
            50 = TEXT
            50 {
               field = nav_title
               replacement < lib.tue_sitemapcsv.10.10.replacement
               wrap < lib.tue_sitemapcsv.10.10.wrap
            }
            60 = TEXT
            60 {
               field = description
               replacement < lib.tue_sitemapcsv.10.10.replacement
               wrap < lib.tue_sitemapcsv.10.10.wrap
            }
            100 = TEXT
            100 {
               value (


               )
            }
         }
         20 = HMENU
         20 {
            includeNotInMenu = 1
            1 = TMENU
            1 {
               noBlur = 1
               expAll = 1
               NO {
                  doNotShowLink = 1
                  allStdWrap.cObject < lib.tue_sitemapcsv.10
               }
            }
            2 < .1
            3 < .1
            4 < .1
            5 < .1
            6 < .1
         }
      }


      [globalVar = TSFE : beUserLogin > 0]
      sitemapcsv = PAGE
      sitemapcsv {
         typeNum = 888
         config {
            disableAllHeaderCode = 1
            xhtml_cleaning = 0
            admPanel = 0
            debug = 0
            no_cache = 1
            additionalHeaders = Content-type:text/csv;charset=utf-8|Content-Disposition:attachment; filename="sitemap.csv"
         }
         10 < lib.tue_sitemapcsv
      }
      [global]


.. index:: TypoScript, if, elseif
.. _s2014-6:
.. _s2014-6-Chained-Conditions-elseif:

2014-6 Chained Conditions (elseif)
==================================

by **Eric Bode**, 2014-06-25 17:58:29, old #473, typoscript

Keywords:
   TypoScript, if, elseif

Description:
   This snippet allows to create conditional chaining like this:

   if (cond1) {  ..out1... }
   else if (cond2) {  ..out2... }
   else if (cond3) {  ..out3... }
   else {  ..out4... }

   In the following Code temp.condOutput will output "!cond1 && !cond2 &&
   cond3" since temp.cond3 returns true while temp.cond2 and temp.cond1 return
   false.

   The code can be easily changed to support more or less conditions. Just
   follow the typoscript logic.

Code:
   .. code-block:: typoscript

      temp.cond1.isTrue = 0
      temp.cond2.isTrue = 0
      temp.cond3.isTrue = 1
      temp.cond4.isTrue = 0

      temp.out1 = TEXT
      temp.out1.value = cond1
      temp.out2 = TEXT
      temp.out2.value = !cond1 && cond2
      temp.out3 = TEXT
      temp.out3.value = !cond1 && !cond2 && cond3
      temp.out4 = TEXT
      temp.out4.value = !cond1 && !cond2 && !cond3 && cond4

      temp.condOutput = CASE
      temp.condOutput {
        key.cObject = COA
        key.cObject {
          10 = TEXT
          10 {
            value = 1
            if < temp.cond1
          }

          20 = COA
          20.10 < .10
          20 {
            10.value = 2
            10.if.negate = 1
            if < temp.cond2
          }

          30 = COA
          30.20 < .20
          30 {
            20 {
              10.value = 3
              if.negate = 1
            }
            if < temp.cond3
          }

          40 = COA
          40.30 < .30
          40.30 {
            20.10.value = 4
            if.negate = 1
          }
        }
        1 < temp.out1
        2 < temp.out2
        3 < temp.out3
        4 < temp.out4
      }


.. index:: meta Tag, Unavaiable after
.. _s2014-5:
.. _s2014-5-meta-Tag-unavaiable_after:

2014-5 meta-Tag unavaiable_after
================================

by **Dieter Porth**, 2014-05-15 21:56:05, old #472, typoscript

Keywords:
   meta Tag, Unavaiable after

Description:
   Invite the unavaibale\_after-Tag into your meta-tag for your normal
   Content-Elements

Code:
   .. code-block:: typoscript

      page = PAGE
      page {
          headerData {
              537 = LOAD_REGISTER
         537 {
             unavaiableDate = TEXT
             unavaiableDate {
                      current = 1
            setCurrent.data = DB:pages:2:SYS_LASTCHANGED
            setCurrent.ifEmpty.field = crdate
            # calculation with Typoscript http://typo3blog.at/blog/artikel/date-via-typoscript/
      #= 3*365,25*24*3600 three Years = german statute of limitations for debt
            setCurrent.wrap = |+94672800
            prioriCalc = 1
            date = l, d-M-y H:i:s T
                  }
              }
              539 = RESTORE_REGISTER
          }
      }


.. index:: content, sys_category, typoscript
.. _s2014-4:
.. _s2014-4-Display-tt_content-categories-in-frontend:

2014-4 Display tt_content categories in frontend
================================================

by **Sabine Deeken**, 2014-05-15 12:26:12, old #471, typoscript

Keywords:
   content, sys\_category, typoscript

Description:
   You might want to display your content-sys\_categories in frontend, so this is
   how you can add them to the Content title using typoscript

Code:
   .. code-block:: typoscript

      lib.stdheader.50 = CONTENT
      lib.stdheader.50 {
          wrap = <p class="categories">|</p>
          if.isTrue.field = categories
          table = tt_content
          select {
            uidInList.field = uid
            join = sys_category_record_mm ON tt_content.uid = sys_category_record_mm.uid_foreign JOIN sys_category ON sys_category.uid = sys_category_record_mm.uid_local
            orderBy = sys_category.sorting
          }
          renderObj = TEXT
          renderObj {
            field = title
            wrap = |&nbsp;
          }
        }


.. index:: FAL, open graph, meta, facebook, image
.. _s2014-3:
.. _s2014-3-Add-Facebook-Open-Graph-ogimage-metatags-for-content-elements-with-TYPO3-60-and-FAL:

2014-3 Add Facebook Open Graph (og:image) metatags for content elements with TYPO3 6.0 and FAL
==============================================================================================

by **Dirk Klimpel**, 2014-05-14 08:57:44, old #470, typoscript

Keywords:
   FAL, open graph, meta, facebook, image

Description:
   This script generates metatags for og:image for any tt\_content element of a
   page. It runs with TYPO3 CMS 6.0 and newer and FAL.

Code:
   .. code-block:: typoscript

      page.headerData.200 = CONTENT
      page.headerData.200 {
        table = tt_content
        select {
          where = colPos = 0
          selectFields = uid
        }
        renderObj = COA
        renderObj {
          1 = TEXT
          1 {
            cObject = FILES
            cObject {
              references {
                table = tt_content
                uid.field = uid
                fieldName = image
              }
              renderObj = TEXT
              renderObj {
                data = file:current:publicUrl
                stdWrap {
                  wrap = <meta name="og:image" content="{TSFE:baseUrl}|" />
                  wrap {
                    override = <meta property="og:image" content="{TSFE:baseUrl}|" />
                    override.if {
                      value = html5
                      equals.data = TSFE:config|config|doctype
                    }
                  }
                }
              }
            }
          }
        }
      }


.. index:: image, caption
.. _s2014-2:
.. _s2014-2-Add-Link-to-Image-Caption:

2014-2 Add Link to Image Caption
================================

by **Markus Quaritsch**, 2014-04-26 21:49:58, old #469, typoscript

Keywords:
   image, caption

Code:
   .. code-block:: typoscript

      tt_content.image.20.default.caption.1.stdWrap.typolink.parameter.data = file:current:link

.. index:: tt_news, typoscript, teasertext
.. _s2014-1:
.. _s2014-1-Teasertext-from-bodytext-with-morelink:

2014-1 Teasertext from bodytext with morelink
=============================================

by **Reinhard Wegmann**, 2014-03-24 17:01:22, old #468, typoscript

Keywords:
   tt\_news, typoscript, teasertext

Description:
   Make a teasertext from bodytext (f.e. the first 250 chars) to insert in latest-
   or list view and add morelink, only if there is more bodytext.

   # The latest-template need a marker ###CONTENT###,
   # the div with the marker ###MORE### should be deleted.

Code:
   .. code-block:: typoscript

      # if you want text without formats, you need  as a first line: stripHtml = 1

      plugin.tt_news.displayLatest.content_stdWrap {
        crop = 250 | ... | 1
        append = TEXT
        append.data = register:newsMoreLink
        append.wrap = <span class = "news-latest-morelink">|</span>
        append.if {
          isTrue.data = field : bodytext
          isTrue.substring = 251, 10
        }
      }



