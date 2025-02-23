``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16051TOP                            - Report ZCL1R16051
*&---------------------------------------------------------------------*
REPORT zcl1r16051 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf, bseg, ekko, t001.

**********************************************************************
* TAB Strip
**********************************************************************
CONTROLS : gc_tab TYPE TABSTRIP.

DATA : gv_subscreen TYPE sy-dynnr.

**********************************************************************
* Macro
**********************************************************************
DEFINE _ranges.

  &1-sign   = &2.
  &1-option = &3.
  &1-low    = &4.
  &1-high   = &5.
  APPEND &1.

END-OF-DEFINITION.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container1 TYPE REF TO cl_gui_custom_container,
       go_container2 TYPE REF TO cl_gui_custom_container,
       go_container3 TYPE REF TO cl_gui_custom_container,
       go_alv_grid1  TYPE REF TO cl_gui_alv_grid,
       go_alv_grid2  TYPE REF TO cl_gui_alv_grid,
       go_alv_grid3  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : gt_bkpf TYPE TABLE OF bkpf,
       gs_bkpf TYPE bkpf,
       gt_bseg TYPE TABLE OF bseg,
       gs_bseg TYPE bseg,
       gt_ekko TYPE TABLE OF ekko,
       gs_ekko TYPE ekko.

DATA : gs_layout  TYPE lvc_s_layo,
       gs_variant TYPE disvariant,
       gt_fcat1    TYPE lvc_t_fcat,
       gs_fcat1    TYPE lvc_s_fcat,
       gt_fcat2    TYPE lvc_t_fcat,
       gs_fcat2    TYPE lvc_s_fcat,
       gt_fcat3    TYPE lvc_t_fcat,
       gs_fcat3    TYPE lvc_s_fcat.

**********************************************************************
* Common variable
**********************************************************************
       data :     gv_okcode TYPE sy-ucomm.
