---
layout: post
title: ABAP to JSON and JSON to ABAP 
description: ABAP <=> JSON Serializer and Deserializer.
date: 2018-02-12 21:47:13+0100
date_modified: 2018-02-12 21:47:13+0100
categories: [sap,abap,json]
tags:
  - sap
  - abap
  - json
author: wechris
image:
  path: /img/2018-02-12-ABAP-to-JSON-and-JSON-to-ABAP/image000.jpg
  width: 629
  height: 600
---

# ABAP <=> JSON Serializer and Deserializer #

There are a lot of other implementations of the ABAP to JSON Serializer and Deserializer on SDN.

Importante for me: Support from SAP!

So, I have written my own example with JSON serializer/deserializer which has some differences:
- Class <code>/UI2/CL_JSON</code> delivered with UI2 Add-on (can be applied on SAP_BASIS 700 – 76X).
- The statement <code>CALL TRANSFORMATION</code> avaiable available in Release 7.40 and downported to 7.02 and 7.31 (Kernelpatch 116) (SAP Notes 1648418 and 1650141) 
- Classes <code>CL_TREX_JSON_SERIALIZER</code> and <code>CL_TREX_JSON_DESERIALIZER</code>

Below you can find the source code of the class with my example.

Clone the Repo [wechris/abapJSONTransformation](https://github.com/wechris/abapJSONTransformation) with [abapGit](https://github.com/larshp/abapGit)


## JSON with /UI2/CL_JSON ##

```abap
* serialize table lt_flight into JSON, skipping initial fields and converting ABAP field names into camelCase
    LV_JSON = /UI2/CL_JSON=>SERIALIZE( DATA = LT_FLIGHT COMPRESS = ABAP_TRUE PRETTY_NAME = /UI2/CL_JSON=>PRETTY_MODE-CAMEL_CASE ).

    " Display JSON in ABAP
    CALL TRANSFORMATION SJSON2HTML SOURCE XML LV_JSON
                               RESULT XML DATA(LVC_HTML).
    CL_ABAP_BROWSER=>SHOW_HTML( TITLE = 'ABAP (iTab) -> JSON: /ui2/cl_json=>serialize' HTML_STRING = CL_ABAP_CODEPAGE=>CONVERT_FROM( LVC_HTML ) ).

    CLEAR LT_FLIGHT.

* deserialize JSON string json into internal table lt_flight doing camelCase to ABAP like field name mapping
    /UI2/CL_JSON=>DESERIALIZE( EXPORTING JSON = LV_JSON PRETTY_NAME = /UI2/CL_JSON=>PRETTY_MODE-CAMEL_CASE CHANGING DATA = LT_FLIGHT ).

    CL_SALV_TABLE=>FACTORY(
       EXPORTING
          LIST_DISPLAY = 'X'
        IMPORTING
          R_SALV_TABLE = GR_TABLE
        CHANGING
          T_TABLE      = LT_FLIGHT ).
```

## JSON with CALL TRANSFORMATION ##

```abap
* abap (itab) -> json
    DATA(O_WRITER_ITAB) = CL_SXML_STRING_WRITER=>CREATE( TYPE = IF_SXML=>CO_XT_JSON ).
    CALL TRANSFORMATION ID SOURCE VALUES = LT_FLIGHT RESULT XML O_WRITER_ITAB.
    DATA: LV_RESULT TYPE STRING.
    CL_ABAP_CONV_IN_CE=>CREATE( )->CONVERT( EXPORTING
                                              INPUT = O_WRITER_ITAB->GET_OUTPUT( )
                                            IMPORTING
                                              DATA = LV_RESULT ).

* JSON -> ABAP (iTab)
    CLEAR LT_FLIGHT.
    CALL TRANSFORMATION ID SOURCE XML LV_RESULT RESULT VALUES = LT_FLIGHT.
```

## JSON with TREX ##
```abap
* ABAP (iTab) -> JSON (trex)
    DATA(O_TREX) = NEW CL_TREX_JSON_SERIALIZER( LT_FLIGHT ).
    O_TREX->SERIALIZE( ).
    LV_TREX_JSON = O_TREX->GET_DATA( ).
```

## Display JSON with HTML Browser Class: ##
{% include lightbox.html src="/img/2018-02-12-ABAP-to-JSON-and-JSON-to-ABAP/image001.jpg" title="abapGit" width="500" %}
```abap
" Display JSON in ABAP
    CALL TRANSFORMATION SJSON2HTML SOURCE XML LV_JSON
    RESULT XML LVC_HTML.
    CL_ABAP_BROWSER=>SHOW_HTML( TITLE = 'ABAP (iTab) -> JSON: TTREX JSON Serializer' HTML_STRING = CL_ABAP_CODEPAGE=>CONVERT_FROM( LVC_HTML ) ).
```


references:

https://wiki.scn.sap.com/wiki/display/Snippets/One+more+ABAP+to+JSON+Serializer+and+Deserializer

https://blogs.sap.com/2013/07/04/abap-news-for-release-740-abap-and-json/

https://help.sap.com/doc/abapdocu_740_index_htm/7.40/en-US/index.htm?file=abenabap_json_abexas.htm
