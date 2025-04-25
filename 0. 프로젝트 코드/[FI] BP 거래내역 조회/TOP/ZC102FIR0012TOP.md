``` abap
*&---------------------------------------------------------------------*
*& Include ZC102FIR0012TOP                          - Report ZC102FIR0012
*&---------------------------------------------------------------------*
REPORT zc102fir0012 MESSAGE-ID zc102msg.

**********************************************************************
* TABLES
**********************************************************************
TABLES : zc102bpt0001, zc102fit0009, zc102fit0010.

**********************************************************************
* Macro
**********************************************************************
DEFINE _init.

  REFRESH &1.
  CLEAR &1.

END-OF-DEFINITION.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_custom_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and work area
**********************************************************************
*-- 메인 데이터 테이블
DATA : BEGIN OF gs_trade,
         belnr       TYPE zc102fit0009-belnr,
         gjahr       TYPE zc102fit0009-gjahr,
         bktxt       TYPE zc102fit0009-bktxt,
         blart       TYPE zc102fit0009-blart,
         bldat       TYPE zc102fit0009-bldat,
         duedt       TYPE zc102fit0010-duedt,
         wrbtr       TYPE zc102fit0010-wrbtr,
         waers       TYPE zc102fit0010-waers,
         augbl       TYPE zc102fit0010-augbl,
         sgtxt       TYPE zc102fit0010-sgtxt,
         rebzg       TYPE zc102fit0016-rebzg,
*         dstat  TYPE icon-id,
         status      TYPE icon-id,
         remain_date TYPE string,    " 남은 납기일 필드
         calc_date   TYPE p,         " 계산용 필드 -> 출력X 단순 날짜 차이 계산
         color       TYPE lvc_t_scol,
       END OF gs_trade,
       gt_trade LIKE TABLE OF gs_trade.
*
*DATA : BEGIN OF gs_store_belnr,
*         belnr TYPE zc102fit0010-belnr,
*       END OF gs_store_belnr,
*       gt_store_belnr LIKE TABLE OF gs_store_belnr.

*-- BP 마스터 테이블
DATA : BEGIN OF gs_partner,
         partner      TYPE zc102bpt0001-partner,
         name1        TYPE zc102sdt0001-name1,
         bp_role      TYPE zc102bpt0001-bp_role,
         bpno        TYPE zc102sdt0001-cusno,
         venno        TYPE zc102mmt0002-venno,
         bukrs        TYPE zc102fit0009-bukrs,
         stras        TYPE zc102sdt0001-stras,
         total        TYPE zc102fit0008-credit_limit,
         used         TYPE zc102fit0008-credit_used,
         remain       TYPE zc102fit0008-credit_limit,
         waers        TYPE zc102fit0008-waers,
         waers_used   TYPE zc102fit0008-waers,
         waers_remain TYPE zc102fit0008-waers,
         status       TYPE icon-id,
       END OF gs_partner,
       gt_partner LIKE TABLE OF gs_partner.

DATA : gt_fcat    TYPE lvc_t_fcat,
       gs_fcat    TYPE lvc_s_fcat,
       gs_layout  TYPE lvc_s_layo,
       gs_variant TYPE disvariant,
       gt_sort    TYPE lvc_t_sort,
       gs_sort    TYPE lvc_s_sort.

**********************************************************************
* Global variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm,
       gv_sum_s  TYPE zc102fit0010-wrbtr,
       gv_sum_h  TYPE zc102fit0010-wrbtr,
       gv_text   TYPE string,
       gv_datum  TYPE sy-datum.
