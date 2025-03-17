``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16067TOP                            - Report ZCL1R16067
*&---------------------------------------------------------------------*
REPORT zcl1r16067 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : ztbox_16_01.

**********************************************************************
* TAB Strip
**********************************************************************
CONTROLS : gc_tab TYPE TABSTRIP.
DATA : gv_subscreen TYPE sy-dynnr.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_cont_1 TYPE REF TO cl_gui_custom_container,
       go_grid_1 TYPE REF TO cl_gui_alv_grid,
       go_cont_2 TYPE REF TO cl_gui_custom_container,
       go_grid_2 TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_cskt,
         kokrs TYPE cskt-kokrs,
         kostl TYPE cskt-kostl,
         datbi TYPE cskt-datbi,
         ktext TYPE cskt-ktext,
       END OF gs_cskt,
       gt_cskt LIKE TABLE OF gs_cskt.

DATA : BEGIN OF gs_cepct,
         prctr TYPE cepct-prctr,
         datbi TYPE cepct-datbi,
         kokrs TYPE cepct-kokrs,
         ktext TYPE cepct-ktext,
       END OF gs_cepct,
       gt_cepct LIKE TABLE OF gs_cepct.

DATA : gs_layout  TYPE lvc_s_layo,
       gs_variant TYPE disvariant,
       gs_fcat1   TYPE lvc_s_fcat,
       gt_fcat1   TYPE lvc_t_fcat,
       gs_fcat2   TYPE lvc_s_fcat,
       gt_fcat2   TYPE lvc_t_fcat.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
