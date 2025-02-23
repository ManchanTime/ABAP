``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16046TOP                            - Report ZCL1R16046
*&---------------------------------------------------------------------*
REPORT zcl1r16046 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : zcl1mara16.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_body.
         INCLUDE STRUCTURE zcl1mara16.
DATA :   cell_tab  TYPE lvc_t_styl,
         modi_yn,
       END OF gs_body,
       gt_body   LIKE TABLE OF gs_body,
       gt_body_d TYPE TABLE OF zcl1mara16.

DATA : gs_layout  TYPE lvc_s_layo,
       gs_fcat    TYPE lvc_s_fcat,
       gt_fcat    TYPE lvc_t_fcat,
       gs_variant TYPE disvariant.

DATA : gs_button       TYPE stb_button,
       gt_ui_functions TYPE ui_functions.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm,
       gv_mode   TYPE i VALUE 0.
