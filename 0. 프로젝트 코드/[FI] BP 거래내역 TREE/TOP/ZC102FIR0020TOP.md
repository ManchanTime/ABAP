``` abap
*&---------------------------------------------------------------------*
*& Include ZC102FIR0020TOP                          - Report ZC102FIR0020
*&---------------------------------------------------------------------*
REPORT zc102fir0020 MESSAGE-ID zc102msg.

**********************************************************************
* TABLES
**********************************************************************
TABLES : sflight, zc102bpt0001.

**********************************************************************
* Declaration area for NODE
**********************************************************************
TYPES: node_table_type LIKE STANDARD TABLE OF mtreesnode
                       WITH DEFAULT KEY.
DATA: node_table TYPE node_table_type.

**********************************************************************
* Declaration area for GUI Class instance
**********************************************************************
DATA: go_container  TYPE REF TO cl_gui_docking_container,
      go_base_cont  TYPE REF TO cl_gui_easy_splitter_container,
      go_left_cont  TYPE REF TO cl_gui_container,
      go_right_cont TYPE REF TO cl_gui_container,
      go_alv_grid   TYPE REF TO cl_gui_alv_grid,
      go_tree       TYPE REF TO cl_gui_simple_tree.

**********************************************************************
* Declaration area for Tree event
**********************************************************************
DATA: events TYPE cntl_simple_events,
      event  TYPE cntl_simple_event.

**********************************************************************
* Internal table and work area
**********************************************************************
DATA : BEGIN OF gs_partner,
         partner TYPE zc102bpt0001-partner,
         name1   TYPE zc102bpt0001-name1,
         belnr   TYPE zc102fit0009-belnr,
         augbl   TYPE zc102fit0016-augbl,
       END OF gs_partner,
       gt_partner LIKE TABLE OF gs_partner.

*-- 메인 데이터 테이블
DATA : BEGIN OF gs_trade,
         belnr   TYPE zc102fit0009-belnr,
         gjahr   TYPE zc102fit0009-gjahr,
         bktxt   TYPE zc102fit0009-bktxt,
         blart   TYPE zc102fit0009-blart,
         bldat   TYPE zc102fit0009-bldat,
         duedt   TYPE zc102fit0010-duedt,
         wrbtr   TYPE zc102fit0010-wrbtr,
         waers   TYPE zc102fit0010-waers,
         augbl   TYPE zc102fit0010-augbl,
         sgtxt   TYPE zc102fit0010-sgtxt,
         partner TYPE zc102bpt0001-partner,
         name1   TYPE zc102bpt0001-name1,
         dstat   TYPE icon-id,
         status  TYPE icon-id,
       END OF gs_trade,
       gt_trade LIKE TABLE OF gs_trade.

DATA : gt_fcat    TYPE lvc_t_fcat,
       gs_fcat    TYPE lvc_s_fcat,
       gs_layout  TYPE lvc_s_layo,
       gs_variant TYPE disvariant.

**********************************************************************
* Common variable
**********************************************************************
DATA: gv_okcode TYPE sy-ucomm,
      gv_cprog  TYPE sy-cprog,
      gv_dynnr  TYPE sy-dynnr.
