``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16037TOP                            - Report ZCL1R16037
*&---------------------------------------------------------------------*
REPORT zcl1r16037 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf, bseg.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_body,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         gjahr TYPE bkpf-gjahr,
         wrbtr TYPE bseg-wrbtr, " 금액
         waers TYPE bkpf-waers, " 통화키
         menge TYPE bseg-menge, " 수량
         meins TYPE bseg-meins, " 단위
       END OF gs_body,
       gt_body LIKE TABLE OF gs_body.

DATA : gt_fcat   TYPE lvc_t_fcat,
       gs_fcat   TYPE lvc_s_fcat,
       gs_layout TYPE lvc_s_layo.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
