``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16027TOP                            - Report ZCL1R16027
*&---------------------------------------------------------------------*
REPORT zcl1r16027 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf, bseg.

**********************************************************************
* Internal table & Work area
**********************************************************************
DATA : BEGIN OF gs_data,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         gjahr TYPE bkpf-gjahr,
         buzei TYPE bseg-buzei,
         bktxt TYPE bkpf-bktxt,
         hkont TYPE bseg-hkont,
         txt50 TYPE skat-txt50,
         gsber TYPE bseg-gsber,
         gtext TYPE tgsbt-gtext,
         shkzg TYPE bseg-shkzg,
         sgtxt TYPE bseg-sgtxt,
       END OF gs_data,
       gt_data LIKE TABLE OF gs_data.

DATA : BEGIN OF gs_hkont,
         saknr TYPE bseg-saknr,
         txt50 TYPE skat-txt50,
       END OF gs_hkont,
       gt_hkont LIKE TABLE OF gs_hkont.

DATA : BEGIN OF gs_gsber,
         gsber TYPE bseg-gsber,
         gtext TYPE tgsbt-gtext,
       END OF gs_gsber,
       gt_gsber LIKE TABLE OF gs_gsber.
