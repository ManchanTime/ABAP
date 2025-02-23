``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16048TOP                            - Report ZCL1R16048
*&---------------------------------------------------------------------*
REPORT zcl1r16048 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : ztcl116_01, bkpf.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

DATA : go_pop_cont TYPE REF TO cl_gui_custom_container,
       go_pop_grid TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_body.
         INCLUDE STRUCTURE ztcl116_01.
DATA :   cell_tab TYPE lvc_t_styl,
         modi_yn,
       END OF gs_body,
       gt_body LIKE TABLE OF gs_body.

DATA : gt_body_d TYPE TABLE OF ztcl116_01,
       gs_body_d TYPE ztcl116_01.

DATA : BEGIN OF gs_text,
         saknr TYPE skat-saknr,
         txt50 TYPE skat-txt50,
       END OF gs_text,
       gt_text LIKE TABLE OF gs_text.

DATA : BEGIN OF gs_bseg,
         bukrs TYPE bseg-bukrs,
         belnr TYPE bseg-belnr,
         gjahr TYPE bseg-gjahr,
         buzei TYPE bseg-buzei,
         hkont TYPE bseg-hkont,
         txt50 TYPE skat-txt50,
         bschl TYPE bseg-bschl,
         koart TYPE bseg-koart,
         sgtxt TYPE bseg-sgtxt,
         dmbtr TYPE bseg-dmbtr,
         pswsl TYPE bseg-pswsl,
       END OF gs_bseg,
       gt_bseg LIKE TABLE OF gs_bseg.

DATA : gs_layout       TYPE lvc_s_layo,
       gt_fcat         TYPE lvc_t_fcat,
       gs_fcat         TYPE lvc_s_fcat,
       gs_variant      TYPE disvariant,
       gt_ui_functions TYPE ui_functions,
       gs_button       TYPE stb_button.

DATA : gs_playout TYPE lvc_s_layo,
       gt_pfcat   TYPE lvc_t_fcat,
       gs_pfcat   TYPE lvc_s_fcat.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm,
       gv_mode   TYPE i VALUE 0.
