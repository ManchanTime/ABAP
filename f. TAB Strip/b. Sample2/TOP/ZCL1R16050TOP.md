``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16050TOP                            - Report ZCL1R16050
*&---------------------------------------------------------------------*
REPORT zcl1r16050 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, spfli, sflight, sbook.

**********************************************************************
* TAB strip
**********************************************************************
CONTROLS : gc_tab TYPE TABSTRIP.

DATA : gv_subscreen TYPE sy-dynnr.

**********************************************************************
* Macro
**********************************************************************
DEFINE _init.

  REFRESH &1.
  CLEAR &1.

END-OF-DEFINITION.

DEFINE _ranges.

*  so_conn-sign   = 'I'.
*  so_conn-option = 'EQ'.
  &1-sign   = &2.
  &1-option = &3.
  &1-low    = &4.
  &1-high   = &5.
  APPEND &1.

END-OF-DEFINITION.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_custom_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : gt_sflight TYPE TABLE OF sflight,
       gs_sflight TYPE sflight.

DATA : gs_layout  TYPE lvc_s_layo,
       gt_fcat    TYPE lvc_t_fcat,
       gs_fact    TYPE lvc_s_fcat,
       gs_variant TYPE disvariant.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
