``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16030TOP                            - Report ZCL1R16030
*&---------------------------------------------------------------------*
REPORT zcl1r16030 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_item,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         buzei TYPE bseg-buzei,
         budat TYPE bkpf-budat,
         bktxt TYPE bkpf-bktxt,
         sgtxt TYPE bseg-sgtxt,
         kostl TYPE bseg-kostl,
         ktext TYPE cskt-ktext,
         wrbtr TYPE bseg-wrbtr,
         waers TYPE bkpf-waers,
       END OF gs_item,
       gt_item LIKE TABLE OF gs_item.

DATA : gs_header TYPE ZSCL1_HEADER_16.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_message(50).
