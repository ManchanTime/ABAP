``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16026TOP                            - Report ZCL1R16026
*&---------------------------------------------------------------------*
REPORT zcl1r16026 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf.

**********************************************************************
* Internal table & Work area
**********************************************************************
DATA : BEGIN OF gs_data,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         gjahr TYPE bkpf-gjahr,
         buzei TYPE bseg-buzei,
         bktxt TYPE bkpf-bktxt,
         koart TYPE bseg-koart,
         kostl TYPE bseg-kostl,
         ktext TYPE cskt-ktext,
         sgtxt TYPE bseg-sgtxt,
         dmbtr TYPE bseg-dmbtr,
         waers TYPE bkpf-waers,
       END OF gs_data,
       gt_data LIKE TABLE OF gs_data.

DATA : BEGIN OF gs_text,
         kostl TYPE bseg-kostl,
         ktext TYPE cskt-ktext,
       END OF gs_text,
       gt_text LIKE TABLE OF gs_text.
