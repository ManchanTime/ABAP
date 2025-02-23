``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16038TOP                            - Report ZCL1R16038
*&---------------------------------------------------------------------*
REPORT zcl1r16038 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : sflight, sbook.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_body,
         carrid    TYPE sflight-carrid,
         connid    TYPE sflight-connid,
         fldate    TYPE sflight-fldate,
         planetype TYPE sflight-planetype,
         currency  TYPE sflight-currency,
         bookid    TYPE sbook-bookid,
         customid  TYPE sbook-customid,
         custtype  TYPE sbook-custtype,
         class     TYPE sbook-class,
         agencynum TYPE sbook-agencynum,
       END OF gs_body,
       gt_body LIKE TABLE OF gs_body.

DATA : BEGIN OF gs_display,
         carrid    TYPE sflight-carrid,
         connid    TYPE sflight-connid,
         fldate    TYPE sflight-fldate,
         bookid    TYPE sbook-bookid,
         customid  TYPE sbook-customid,
         custtype  TYPE sbook-custtype,
         agencynum TYPE sbook-agencynum,
       END OF gs_display,
       gt_display LIKE TABLE OF gs_display.

DATA : gs_layout TYPE lvc_s_layo,
       gs_fcat   TYPE lvc_s_fcat,
       gt_fcat   TYPE lvc_t_fcat.

**********************************************************************
* Common varidable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
