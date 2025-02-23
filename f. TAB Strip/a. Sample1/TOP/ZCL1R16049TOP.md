``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16049TOP                            - Report ZCL1R16049
*&---------------------------------------------------------------------*
REPORT zcl1r16049 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, spfli.

**********************************************************************
* TAB Strip controls
**********************************************************************
CONTROLS : gc_tab TYPE TABSTRIP.  " TAB Strip object

DATA : gv_subscreen TYPE sy-dynnr.  " 스크린 번호

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_custom_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
*-- For ALV in First tab
DATA : gt_spfli TYPE TABLE OF spfli,
       gs_spfli TYPE spfli.

*-- For ALV
DATA : gs_layout  TYPE lvc_s_layo,
       gs_fcat    TYPE lvc_s_fcat,
       gt_fcat    TYPE lvc_t_fcat,
       gs_variant TYPE disvariant.

**********************************************************************
* Common variable
**********************************************************************
*-- Screen element
DATA : gs_scarr TYPE scarr.

DATA : gv_okcode TYPE sy-ucomm.
