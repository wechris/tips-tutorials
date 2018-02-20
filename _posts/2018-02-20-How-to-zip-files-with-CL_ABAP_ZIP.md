---
layout: post
title: How to zip files with CL_ABAP_ZIP 
description: Zip any file via ABAP using CL_ABAP_ZIP.
date: 2018-02-20 21:48:58+0100
date_modified: 2018-02-20 21:48:58+0100
categories: [sap,abap,zip]
tags:
  - sap
  - abap
  - zip
author: wechris
image:
  path: /img/2018-02-20-How-to-zip-files-with-CL_ABAP_ZIP/image000.jpg
  width: 629
  height: 600
---
# Example how to zip files via ABAP

As of Release 6.40 the class CL_ABAP_ZIP is available to read/write ZIP files in SAP.

How to zip a file using ABAP: The easy why it to use the standard class CL_ABAP_ZIP.

The repository includes a ready to run program, to multiselect files in input and converting it into Zip file which can be downloded to the client.

Clone the Repo [wechris/abapZIPCLASS_EXAMPLE](https://github.com/wechris/abapZIPCLASS_EXAMPLE) with [abapGit](https://github.com/larshp/abapGit)

### The different steps to zip a file using ABAP:###

__Uploading the file into memory__

```abap
  METHOD SELECT_FILES.

    DATA: LV_RET_CODE TYPE I,
          LV_USR_AXN  TYPE I.

    CL_GUI_FRONTEND_SERVICES=>FILE_OPEN_DIALOG(
      EXPORTING
        WINDOW_TITLE            = 'Select file'
        MULTISELECTION          = 'X'
      CHANGING
        FILE_TABLE              = ME->PT_FILETAB
        RC                      = LV_RET_CODE
        USER_ACTION             = LV_USR_AXN
      EXCEPTIONS
        FILE_OPEN_DIALOG_FAILED = 1
        CNTL_ERROR              = 2
        ERROR_NO_GUI            = 3
        NOT_SUPPORTED_BY_GUI    = 4
        OTHERS                  = 5
           ).
    IF SY-SUBRC <> 0 OR
       LV_USR_AXN = CL_GUI_FRONTEND_SERVICES=>ACTION_CANCEL.
      RAISE EX_FILE_SEL_ERR.
    ENDIF.

  ENDMETHOD.                    "select_files

```

{% include lightbox.html src="/img/2018-02-20-How-to-zip-files-with-CL_ABAP_ZIP/image001.jpg" title="abapGit" width="500" %}

__Convert Binary to XString__

```abap
*     Convert the data from Binary format to XSTRING

      CALL FUNCTION 'SCMS_BINARY_TO_XSTRING'
        EXPORTING
          INPUT_LENGTH = <LWA_BINDATA>-SIZE
        IMPORTING
          BUFFER       = LV_XSTRING
        TABLES
          BINARY_TAB   = <LWA_BINDATA>-DATA
        EXCEPTIONS
          FAILED       = 1
          OTHERS       = 2.

      IF SY-SUBRC <> 0.
        RAISE EX_BIN_CONV_ERROR.
      ENDIF.

```

__Create an instance of cl_abap_zip__

```abap
CREATE OBJECT LO_ZIP. "Create instance of Zip Class
```

__Add Binary File to__

```abap
*     Add file to the zip folder

      LO_ZIP->ADD(  NAME    = <LWA_BINDATA>-NAME
                    CONTENT = LV_XSTRING ).
```


__Get the XSTRING stream for ZIP file__

```abap
*   Get the stream for ZIP file

    LV_ZIP_XSTRING = LO_ZIP->SAVE( ).
```

__Convert XSTRING to Binary__

```abap
*   Convert the XSTRING to Binary table

    CALL FUNCTION 'SCMS_XSTRING_TO_BINARY'
      EXPORTING
        BUFFER        = LV_ZIP_XSTRING
      IMPORTING
        OUTPUT_LENGTH = ME->PV_ZIP_SIZE
      TABLES
        BINARY_TAB    = ME->PT_ZIP_BIN_DATA.
```

__Download ZIP file on Presentation server__

```abap
CL_GUI_FRONTEND_SERVICES=>GUI_DOWNLOAD(
      EXPORTING
        BIN_FILESIZE              = ME->PV_ZIP_SIZE
        FILENAME                  = ME->PV_DEST_FILEPATH
        FILETYPE                  = 'BIN'
      IMPORTING
        FILELENGTH                = Y_FILESIZE
      CHANGING
        DATA_TAB                  = ME->PT_ZIP_BIN_DATA
      EXCEPTIONS
```

{% include lightbox.html src="/img/2018-02-20-How-to-zip-files-with-CL_ABAP_ZIP/image002.jpg" title="abapGit" width="500" %}

{% include lightbox.html src="/img/2018-02-20-How-to-zip-files-with-CL_ABAP_ZIP/image003.jpg" title="abapGit" width="500" %}

__Other utility class for GZIP CL_ABAP_GZIP__

CL_ABAP_GZIP Class for (De)Compression (GZIP) is other alternative if you want to use rather GZIP compression.

References:

https://blogs.sap.com/2016/02/19/unzip-read-files-with-abap-class-clabapzip/