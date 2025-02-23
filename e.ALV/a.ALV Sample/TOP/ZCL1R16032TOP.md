``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16032TOP                            - Report ZCL1R16032
*&---------------------------------------------------------------------*
REPORT zcl1r16032 MESSAGE-ID zclmsg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, spfli.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_custom_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_scarr.
         INCLUDE STRUCTURE scarr.
DATA :   status TYPE icon-id,
         country(10),
       END OF gs_scarr,
       gt_scarr LIKE TABLE OF gs_scarr.

*-- Field catalog
DATA : gt_fcat TYPE lvc_t_fcat,
       gs_fcat TYPE lvc_s_fcat.

*-- For ALV
DATA : gs_layout TYPE lvc_s_layo.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm. " Get User-command
