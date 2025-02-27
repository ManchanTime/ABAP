``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16057TOP                            - Report ZCL1R16057
*&---------------------------------------------------------------------*
REPORT zcl1r16057 MESSAGE-ID zcl1msg16.

**********************************************************************
* Internal table And Work area
**********************************************************************
DATA : gs_body TYPE zfaglflext,
       gt_body TYPE TABLE OF zfaglflext.

DATA : BEGIN OF gs_month,
         value TYPE zfaglflext-hsl01,
       END OF gs_month,
       gt_month LIKE TABLE OF gs_month.

DATA : BEGIN OF gs_result,
         month TYPE i,
         sum   TYPE zfaglflext-hsl01,
       END OF gs_result,
       gt_result LIKE TABLE OF gs_result.

**********************************************************************
* Common variable
**********************************************************************
FIELD-SYMBOLS <gs_fs> TYPE ANY.

DATA : gv_result TYPE zfaglflext-hsl01,
       gv_amount(20).
