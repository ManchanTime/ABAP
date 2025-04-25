``` abap
*&---------------------------------------------------------------------*
*& Include ZC102FIR0021TOP                          - Report ZC102FIR0021
*&---------------------------------------------------------------------*
REPORT zc102fir0021 MESSAGE-ID zc102msg.

**********************************************************************
* Class
**********************************************************************
CLASS cl_gui_column_tree DEFINITION LOAD.
CLASS cl_gui_cfw DEFINITION LOAD.

**********************************************************************
* ICON
**********************************************************************
INCLUDE <icon>.

**********************************************************************
* TABLES
**********************************************************************
TABLES : zbut000, zc102bpt0001.

**********************************************************************
* TYPES
**********************************************************************
TYPES : BEGIN OF ts_tr_item,
          belnr TYPE zc102fit0010-belnr,
          count TYPE i,
        END OF ts_tr_item,
        tt_tr_item TYPE TABLE OF ts_tr_item.

TYPES: BEGIN OF ty_tree,
*         partner TYPE zc102bpt0001-partner,
         name1 TYPE zc102bpt0001-name1, " BP 이름
         belnr TYPE zc102fit0010-belnr, " 전표 번호
         buzei(3),                      " 개별항목
         shkzg TYPE zc102fit0010-shkzg, " 차/대구분
         saknr TYPE zc102fit0010-saknr, " G/L Account
         wrbtr TYPE zc102fit0010-wrbtr, " 금액
         waers TYPE waers,              " 통화키
         sgtxt TYPE zc102fit0010-sgtxt, " Item Text
         bldat TYPE string,             " 증빙일
         budat TYPE string,             " 전기일
*         bktxt   TYPE zc102fit0009-bktxt, "
       END OF ty_tree.

**********************************************************************
* Class instance
**********************************************************************
DATA: go_tree        TYPE REF TO cl_gui_alv_tree,
      go_container   TYPE REF TO cl_gui_docking_container,
      go_change_menu TYPE REF TO cl_ctmenu.

**********************************************************************
* Internal table and Work area
**********************************************************************
*-- 트리 구조 생성 데이터
DATA: gt_tree          TYPE STANDARD TABLE OF ty_tree,
      gs_tree          TYPE ty_tree,
      gt_tree_download LIKE TABLE OF gs_tree.

*-- BP Data
DATA : BEGIN OF gs_partner,
         partner TYPE zc102bpt0001-partner,
         name1   TYPE zc102bpt0001-name1,
         wrbtr   TYPE zc102fit0010-wrbtr,
         waers   TYPE zc102fit0010-waers,
       END OF gs_partner,
       gt_partner LIKE TABLE OF gs_partner.

*-- 미결 Header Data
DATA : BEGIN OF gs_open_h,
         belnr   TYPE zc102fit0010-belnr,
         partner TYPE zc102fit0010-partner,
         bktxt   TYPE zc102fit0009-bktxt,
         bldat   TYPE zc102fit0009-bldat,
         budat   TYPE zc102fit0009-budat,
         wrbtr   TYPE zc102fit0010-wrbtr,
         waers   TYPE zc102fit0010-waers,
       END OF gs_open_h,
       gt_open_h LIKE TABLE OF gs_open_h.

*-- 미결 Item Data
DATA : BEGIN OF gs_open,
         belnr   TYPE zc102fit0010-belnr,
         buzei   TYPE zc102fit0010-buzei,
         saknr   TYPE zc102fit0010-saknr,
         shkzg   TYPE zc102fit0010-shkzg,
         partner TYPE zc102fit0010-partner,
         wrbtr   TYPE zc102fit0010-wrbtr,
         waers   TYPE zc102fit0010-waers,
         sgtxt   TYPE zc102fit0010-sgtxt,
       END OF gs_open,
       gt_open LIKE TABLE OF gs_open.

*-- 반제 Header Data
DATA : BEGIN OF gs_clear_h,
         belnr   TYPE zc102fit0010-belnr,
         bldat   TYPE zc102fit0009-bldat,
         budat   TYPE zc102fit0009-budat,
         bktxt   TYPE zc102fit0009-bktxt,
         rebzg   TYPE zc102fit0016-rebzg,
         partner TYPE zc102fit0010-partner,
         wrbtr   TYPE zc102fit0010-wrbtr,
         waers   TYPE zc102fit0010-waers,
       END OF gs_clear_h,
       gt_clear_h LIKE TABLE OF gs_clear_h.

*-- 반제 Data
DATA : BEGIN OF gs_clear,
         belnr TYPE zc102fit0016-belnr,
         buzei TYPE zc102fit0016-buzei,
         saknr TYPE zc102fit0016-saknr,
         shkzg TYPE zc102fit0016-shkzg,
         wrbtr TYPE zc102fit0016-wrbtr,
         waers TYPE zc102fit0016-waers,
         sgtxt TYPE zc102fit0016-sgtxt,
       END OF gs_clear,
       gt_clear LIKE TABLE OF gs_clear.

DATA: gs_hierhdr         TYPE treev_hhdr,
      gs_variant         TYPE disvariant,
      gt_list_commentary TYPE slis_t_listheader.

DATA: gt_events TYPE cntl_simple_events,
      gs_event  TYPE cntl_simple_event.

DATA : gt_fcat TYPE lvc_t_fcat,
       gs_fcat TYPE lvc_s_fcat.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm,
       save_ok   TYPE sy-ucomm.

DATA : gv_logo TYPE sdydo_value.

*--------------------------------------------------------------------*
* 엑셀 다운로드 용
*--------------------------------------------------------------------*
*-- File Browser
DATA : objfile       TYPE REF TO cl_gui_frontend_services,
       pickedfolder  TYPE string,
       initialfolder TYPE string,
       fullinfo      TYPE string,
       pfolder       TYPE rlgrap-filename.  " Memory ID mfolder.

*-- For Excel
DATA : gv_tot_page   LIKE sy-pagno,         " Total page
       gv_percent(3) TYPE n,                " Reading percent
       gv_file       LIKE rlgrap-filename.  " File name

DATA : gv_temp_filename     LIKE rlgrap-filename,
       gv_temp_filename_pdf LIKE rlgrap-filename,
       gv_form(40).

DATA: excel       TYPE ole2_object,
      workbook    TYPE ole2_object,
      books       TYPE ole2_object,
      book        TYPE ole2_object,
      sheets      TYPE ole2_object,
      sheet       TYPE ole2_object,
      activesheet TYPE ole2_object,
      application TYPE ole2_object,
      pagesetup   TYPE ole2_object,
      cells       TYPE ole2_object,
      cell        TYPE ole2_object,
      row         TYPE ole2_object,
      buffer      TYPE ole2_object,
      font        TYPE ole2_object,
      range       TYPE ole2_object,  " Range
      borders     TYPE ole2_object,
      worksheet   TYPE ole2_object,
      columns     TYPE ole2_object.

DATA: cell1 TYPE ole2_object,
      cell2 TYPE ole2_object.
