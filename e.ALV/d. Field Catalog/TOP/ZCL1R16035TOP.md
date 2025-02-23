``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16035TOP                            - Report ZCL1R16035
*&---------------------------------------------------------------------*
REPORT zcl1r16035 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, spfli.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and work area
**********************************************************************
DATA : BEGIN OF gs_body,
         status      TYPE icon-id,
         carrid      TYPE scarr-carrid,
         connid      TYPE spfli-connid,
         carrname    TYPE scarr-carrname,
         cityfrom    TYPE spfli-cityfrom,
         cityto      TYPE spfli-cityto,
         url         TYPE scarr-url,
         country(10),
       END OF gs_body,
       gt_body LIKE TABLE OF gs_body.

DATA : gt_sflight TYPE TABLE OF sflight,
       gs_sflight TYPE sflight.

DATA : gs_layout TYPE lvc_s_layo,
       gt_fcat   TYPE lvc_t_fcat,
       gs_fcat   TYPE lvc_s_fcat.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
