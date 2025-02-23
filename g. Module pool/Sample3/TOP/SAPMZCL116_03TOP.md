``` abap
*&---------------------------------------------------------------------*
*& Include SAPMZCL116_03TOP                         - Module Pool      SAPMZCL116_03
*&---------------------------------------------------------------------*
PROGRAM sapmzcl116_03 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, spfli, sflight.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_main_cont      TYPE REF TO cl_gui_custom_container,
       go_split_cont     TYPE REF TO cl_gui_splitter_container,
       go_split_bot_cont TYPE REF TO cl_gui_splitter_container,
       go_top_cont       TYPE REF TO cl_gui_container,
       go_top_grid       TYPE REF TO cl_gui_alv_grid,
       go_bot_cont       TYPE REF TO cl_gui_container,
       go_bot_lef_cont   TYPE REF TO cl_gui_container,
       go_bot_lef_grid   TYPE REF TO cl_gui_alv_grid,
       go_bot_rig_cont   TYPE REF TO cl_gui_container,
       go_bot_rig_grid   TYPE REF TO cl_gui_alv_grid.

DATA : gs_layout  TYPE lvc_s_layo,
       gs_variant TYPE disvariant.

DATA : gs_tfact  TYPE lvc_s_fcat,
       gt_tfcat  TYPE lvc_t_fcat,
       gs_blfcat TYPE lvc_s_fcat,
       gt_blfcat TYPE lvc_t_fcat,
       gs_brfcat TYPE lvc_s_fcat,
       gt_brfcat TYPE lvc_t_fcat.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : gt_scarr   TYPE TABLE OF scarr,
       gs_scarr   TYPE scarr,
       gt_spfli   TYPE TABLE OF spfli,
       gs_spfli   TYPE spfli,
       gt_sflight TYPE TABLE OF zvsflight16_cds,
       gs_sflight TYPE zvsflight16_cds.

DATA : gt_scarr_store   TYPE TABLE OF scarr,
       gs_scarr_store   TYPE scarr,
       gt_spfli_store   TYPE TABLE OF spfli,
       gs_spfli_store   TYPE spfli,
       gt_sflight_store TYPE TABLE OF zvsflight16_cds,
       gs_sflight_store TYPE zvsflight16_cds.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_carrid TYPE scarr-carrid,
       gv_okcode TYPE sy-ucomm.
