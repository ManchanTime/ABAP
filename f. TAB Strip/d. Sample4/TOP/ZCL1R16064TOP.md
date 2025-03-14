``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16064TOP                            - Report ZCL1R16064
*&---------------------------------------------------------------------*
REPORT zcl1r16064 MESSAGE-ID zc1msg16.

**********************************************************************
* TAB Strip
**********************************************************************
CONTROLS : gc_tab TYPE TABSTRIP.

DATA : gv_subscreen TYPE sy-dynnr.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container_t1 TYPE REF TO cl_gui_custom_container,
       go_container_t2 TYPE REF TO cl_gui_custom_container,
       go_container_t3 TYPE REF TO cl_gui_custom_container,
       go_grid_t1      TYPE REF TO cl_gui_alv_grid,
       go_grid_t2      TYPE REF TO cl_gui_alv_grid,
       go_grid_t3      TYPE REF TO cl_gui_alv_grid.

DATA : go_pop_cont1 TYPE REF TO cl_gui_custom_container,
       go_pop_grid1 TYPE REF TO cl_gui_alv_grid.

DATA : go_pop_cont2 TYPE REF TO cl_gui_custom_container,
       go_pop_grid2 TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : gt_scarr TYPE TABLE OF scarr,
       gs_scarr TYPE scarr.

DATA : gt_flight TYPE TABLE OF sflight,
       gs_flight TYPE sflight.

DATA : BEGIN OF gs_sbook,
         carrid            TYPE sbook-carrid,
         connid            TYPE sbook-connid,
         fldate            TYPE sbook-fldate,
         bookid            TYPE sbook-bookid,
         customid          TYPE sbook-customid,
         custtype          TYPE sbook-custtype,
         custtype_desc(20),
         luggweight        TYPE sbook-luggweight,
         wunit             TYPE sbook-wunit,
         class             TYPE sbook-class,
         forcuram          TYPE sbook-forcuram,
         forcurkey         TYPE sbook-forcurkey,
       END OF gs_sbook,
       gt_sbook LIKE TABLE OF gs_sbook.

*-- Get Popup data
DATA : gt_spfli TYPE TABLE OF spfli,
       gs_spfli TYPE spfli.

DATA : gt_scustom_store TYPE TABLE OF scustom,
       gt_scustom       TYPE TABLE OF scustom,
       gs_scustom       TYPE scustom.

DATA : gs_layout  TYPE lvc_s_layo,
       gs_variant TYPE disvariant,
       gt_fcat_t1 TYPE lvc_t_fcat,
       gs_fcat_t1 TYPE lvc_s_fcat,
       gt_fcat_t2 TYPE lvc_t_fcat,
       gs_fcat_t2 TYPE lvc_s_fcat,
       gt_fcat_t3 TYPE lvc_t_fcat,
       gs_fcat_t3 TYPE lvc_s_fcat.

DATA : gt_fcat_grid1 TYPE lvc_t_fcat,
       gs_fcat_grid1 TYPE lvc_s_fcat,
       gt_fcat_grid2 TYPE lvc_t_fcat,
       gs_fcat_grid2 TYPE lvc_s_fcat.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
