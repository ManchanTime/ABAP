``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16029TOP                            - Report ZCL1R16029
*&---------------------------------------------------------------------*
REPORT zcl1r16029 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : gt_spfli   TYPE TABLE OF spfli,
       gs_scarr   TYPE scarr.

DATA : gs_spfli   TYPE spfli,
       gt_sflight TYPE TABLE OF sflight,
       gt_airline TYPE TABLE OF ZSCL1_AIR_16.

DATA : gs_bkpf TYPE bkpf,
       gt_bseg TYPE TABLE OF bseg.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_message(50).
