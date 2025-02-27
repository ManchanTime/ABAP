``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16062TOP                            - Report ZCL1R16062
*&---------------------------------------------------------------------*
REPORT zcl1r16062 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scustom.

**********************************************************************
* Internal table and Work area
**********************************************************************
*-- For BDC
DATA : gt_bdcdata TYPE TABLE OF bdcdata,    " BDC Data
       gs_bdcdata TYPE bdcdata,
       gs_params  TYPE ctu_params.          " BDC Option

*-- BDC Message
DATA : gt_messtab TYPE TABLE OF bdcmsgcoll WITH HEADER LINE.
