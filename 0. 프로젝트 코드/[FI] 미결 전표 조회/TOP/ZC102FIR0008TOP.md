``` abap
*&---------------------------------------------------------------------*
*& Include ZC102FIR0008TOP                          - Report ZC102FIR0008
*&---------------------------------------------------------------------*
REPORT zc102fir0008 MESSAGE-ID zc102msg.

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
DATA : go_container TYPE REF TO cl_gui_custom_container.
*       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

DATA : go_split_cont  TYPE REF TO cl_gui_splitter_container,
       go_bottom_cont TYPE REF TO cl_gui_container,
       go_bottom_grid TYPE REF TO cl_gui_alv_grid,
       go_top_cont    TYPE REF TO cl_gui_container,
       go_top_grid    TYPE REF TO cl_gui_alv_grid.

*-- Sort 팝업
DATA : go_pop_cont TYPE REF TO cl_gui_custom_container,
       go_pop_grid TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and work area
**********************************************************************
DATA : BEGIN OF gs_header,
         bukrs TYPE zc102fit0009-bukrs, " 회사코드
         belnr TYPE zc102fit0009-belnr, " 전표번호
         gjahr TYPE zc102fit0009-gjahr, " 회계연도
         blart TYPE zc102fit0009-blart, " 전표유형
         bldat TYPE zc102fit0009-bldat, " 증빙일
         budat TYPE zc102fit0009-budat, " 전기일
         bktxt TYPE zc102fit0009-bktxt, " 전표헤더텍스트
         stblg TYPE zc102fit0009-stblg, " 역분개전표번호
         stodt TYPE zc102fit0009-stodt, " 역분개전기계획일
         stgrd TYPE zc102fit0009-stgrd, " 역분개사유코드
*         augbl TYPE zc102fit0010-augbl, " 반제전표번호
         waers TYPE zc102fit0009-waers, " 통화
         rebzg TYPE zc102fit0016-rebzg,
         icon  TYPE icon-id,
       END OF gs_header,
       gt_header LIKE TABLE OF gs_header.

DATA : BEGIN OF gs_header_mm,
         bukrs TYPE zc102fit0009-bukrs, " 회사코드
         belnr TYPE zc102fit0009-belnr, " 전표번호
         gjahr TYPE zc102fit0009-gjahr, " 회계연도
         blart TYPE zc102fit0009-blart, " 전표유형
         bldat TYPE zc102fit0009-bldat, " 증빙일
         budat TYPE zc102fit0009-budat, " 전기일
         bktxt TYPE zc102fit0009-bktxt, " 전표헤더텍스트
         stblg TYPE zc102fit0009-stblg, " 역분개전표번호
         stodt TYPE zc102fit0009-stodt, " 역분개전기계획일
         stgrd TYPE zc102fit0009-stgrd, " 역분개사유코드
*         augbl TYPE zc102fit0010-augbl, " 반제전표번호
         waers TYPE zc102fit0009-waers, " 통화
       END OF gs_header_mm.

DATA : gs_fcat_header      TYPE lvc_s_fcat,
       gt_fcat_header      TYPE lvc_t_fcat,
       gs_layout           TYPE lvc_s_layo,
       gs_variant          TYPE disvariant,
       gt_sort_header      TYPE lvc_t_sort,
       gs_sort_header      TYPE lvc_s_sort,
       gt_filter_header    TYPE lvc_t_filt,
       gs_filter_header    TYPE lvc_s_filt,
       gv_alv_title_header TYPE lvc_title.

DATA : BEGIN OF gs_line,
         belnr   TYPE zc102fit0010-belnr,
         gjahr   TYPE zc102fit0010-gjahr,
         buzei   TYPE zc102fit0010-buzei,
         saknr   TYPE zc102fit0010-saknr,
         augdt   TYPE zc102fit0010-augdt,
         augbl   TYPE zc102fit0010-augbl,
         bschl   TYPE zc102fit0010-bschl,
         shkzg   TYPE zc102fit0010-shkzg,
         koart   TYPE zc102fit0010-koart,
         partner TYPE zc102fit0010-partner,
         sgtxt   TYPE zc102fit0010-sgtxt,
         ebeln   TYPE zc102fit0010-ebeln,
         vbeln   TYPE zc102fit0010-vbeln,
         wrbtr   TYPE zc102fit0010-wrbtr,
         waers   TYPE zc102fit0010-waers,
         duedt   TYPE zc102fit0010-duedt,
       END OF gs_line,
       gt_line LIKE TABLE OF gs_line.

DATA : gs_fcat_line   TYPE lvc_s_fcat,
       gt_fcat_line   TYPE lvc_t_fcat,
       gs_layout_line TYPE lvc_s_layo,
       gt_sort_line   TYPE lvc_t_sort,
       gs_sort_line   TYPE lvc_s_sort,
       gs_button      TYPE stb_button.

*--------------------------------------------------------------------*
* 필터링 ALV
*--------------------------------------------------------------------*
DATA : BEGIN OF gs_filter_pop,
         icon TYPE icon-id,
         name TYPE string,
       END OF gs_filter_pop,
       gt_filter_pop LIKE TABLE OF gs_filter_pop.

DATA : gs_fcat_pop        TYPE lvc_s_fcat,
       gt_fcat_pop        TYPE lvc_t_fcat,
       gs_layout_pop      TYPE lvc_s_layo,
       gt_exclude_toolbar TYPE ui_functions.

**********************************************************************
* Global variable
**********************************************************************
DATA : gv_okcode    TYPE sy-ucomm,
       gv_check1(1),
       gv_check2(1),
       gv_check3(1),
       gv_budat_fr  TYPE zc102fit0009-budat,
       gv_budat_to  TYPE zc102fit0009-budat,
       gv_bukrs     TYPE bkpf-bukrs VALUE '1000',
       gv_belnr     TYPE zc102fit0009-belnr.

*-- 필터 체킹
DATA : gv_filter TYPE icon-id.

*-- 반제 상태 체킹
DATA : gv_all,
       gv_entire,
       gv_part,
       gv_entire_cnt TYPE i,
       gv_part_cnt   TYPE i.

RANGES : gr_budat FOR bkpf-budat,
         gr_belnr FOR bseg-belnr.
