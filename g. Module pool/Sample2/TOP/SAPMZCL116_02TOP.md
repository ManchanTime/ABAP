``` abap
*&---------------------------------------------------------------------*
*& Include SAPMZCL116_02TOP                         - Module Pool      SAPMZCL116_02
*&---------------------------------------------------------------------*
PROGRAM sapmzcl116_02 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf, bseg.

**********************************************************************
* Macro
**********************************************************************
DEFINE _init.

  REFRESH &1.
  CLEAR &1.

END-OF-DEFINITION.
**********************************************************************
* Screen field
**********************************************************************
DATA : gv_bukrs    TYPE bkpf-bukrs,
       gv_gjahr    TYPE bkpf-gjahr,
       gv_budat_fr TYPE bkpf-budat,
       gv_budat_to TYPE bkpf-budat.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_custom_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_body,
         bukrs TYPE bseg-bukrs,
         belnr TYPE bseg-belnr,
         gjahr TYPE bseg-gjahr,
         buzei TYPE bseg-buzei,
         hkont TYPE bseg-hkont,
         txt50 TYPE skat-txt50,
         bschl TYPE bseg-bschl,
         koart TYPE bseg-koart,
         sgtxt TYPE bseg-sgtxt,
       END OF gs_body,
       gt_body LIKE TABLE OF gs_body.

DATA : BEGIN OF gs_text,
         saknr TYPE skat-saknr,
         txt50 TYPE skat-txt50,
       END OF gs_text,
       gt_text LIKE TABLE OF gs_text.

DATA : gs_layout  TYPE lvc_s_layo,
       gs_fcat    TYPE lvc_s_fcat,
       gt_fcat    TYPE lvc_t_fcat,
       gs_variant TYPE disvariant.

**********************************************************************
* Common variable
**********************************************************************
RANGES gr_budat FOR bkpf-budat.

DATA : gv_okcode TYPE sy-ucomm.
