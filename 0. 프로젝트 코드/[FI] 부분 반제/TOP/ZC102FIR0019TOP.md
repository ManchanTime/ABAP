``` abap
*&---------------------------------------------------------------------*
*& Include ZC102FIR0019TOP                          - Report ZC102FIR0019
*&---------------------------------------------------------------------*
REPORT zc102fir0019 MESSAGE-ID zc102msg.

**********************************************************************
* Macro
**********************************************************************
DEFINE _init.

  REFRESH &1.
  CLEAR &1.

END-OF-DEFINITION.

**********************************************************************
* TABLES
**********************************************************************
TABLES : zc102fit0009.

**********************************************************************
* Class instance
**********************************************************************
*-- Main ALV
DATA : go_container  TYPE REF TO cl_gui_custom_container,
       go_split_cont TYPE REF TO cl_gui_splitter_container.

DATA : go_left_cont  TYPE REF TO cl_gui_container,
       go_left_grid  TYPE REF TO cl_gui_alv_grid,
       go_right_cont TYPE REF TO cl_gui_container,
       go_right_grid TYPE REF TO cl_gui_alv_grid.

*-- Screen 110 ALV
DATA : go_pop_clear_cont TYPE REF TO cl_gui_custom_container,
       go_pop_clear_grid TYPE REF TO cl_gui_alv_grid.

*-- Screen 120 ALV
DATA : go_pop_acc_cont TYPE REF TO cl_gui_custom_container,
       go_pop_acc_grid TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and work area
**********************************************************************
*-- 메인 화면 미결전표 헤더
DATA : BEGIN OF gs_header,
         bukrs TYPE zc102fit0009-bukrs,
         belnr TYPE zc102fit0009-belnr,
         gjahr TYPE zc102fit0009-gjahr,
         blart TYPE zc102fit0009-blart,
         bldat TYPE zc102fit0009-bldat,
         budat TYPE zc102fit0009-budat,
         bktxt TYPE zc102fit0009-bktxt,
         stblg TYPE zc102fit0009-stblg,
         stodt TYPE zc102fit0009-stodt,
         stgrd TYPE zc102fit0009-stgrd,
*         augbl TYPE zc102fit0010-augbl,
         waers TYPE zc102fit0010-waers,
       END OF gs_header,
       gs_header_mm LIKE gs_header,
       gs_header_sh LIKE gs_header,
       gt_header    LIKE TABLE OF gs_header.

*-- 메인 화면 미결 전표 아이템
DATA : BEGIN OF gs_open,
         bukrs   TYPE zc102fit0010-bukrs,
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
         waers   TYPE zc102fit0010-waers.
         INCLUDE STRUCTURE zc102cms0001.
DATA : END OF gs_open.
DATA : gt_open LIKE TABLE OF gs_open.

*-- 메인 화면 반제 전표 아이템
DATA : BEGIN OF gs_clear,
         belnr    TYPE zc102fit0010-belnr,
         gjahr    TYPE zc102fit0010-gjahr,
         buzei    TYPE zc102fit0010-buzei,
         saknr    TYPE zc102fit0010-saknr,
         augdt    TYPE zc102fit0010-augdt,
         rebzg    TYPE zc102fit0016-rebzg,
         augbl    TYPE zc102fit0010-augbl,
         bschl    TYPE zc102fit0010-bschl,
         ltext    TYPE zc102fit0004-ltext,
         shkzg    TYPE zc102fit0010-shkzg,
         koart    TYPE zc102fit0010-koart,
         partner  TYPE zc102fit0010-partner,
         sgtxt    TYPE zc102fit0010-sgtxt,
         wrbtr    TYPE zc102fit0010-wrbtr,
         waers    TYPE zc102fit0010-waers,
         color    TYPE lvc_t_scol,
         sort_key.
         INCLUDE STRUCTURE zc102cms0001.
DATA : END OF gs_clear.
DATA : gt_clear LIKE TABLE OF gs_clear.

*-- 전기키 마스터 데이터
DATA : BEGIN OF gs_bschl,
         bschl TYPE zc102fit0004-bschl,
         shkzg TYPE zc102fit0004-shkzg,
         koart TYPE zc102fit0004-koart,
         ltext TYPE zc102fit0004-ltext,
       END OF gs_bschl,
       gt_bschl LIKE TABLE OF gs_bschl.

*-- 전표유형 마스터 데이터
DATA : BEGIN OF gs_blart,
         blart TYPE zc102fit0005-blart,
         ltext TYPE zc102fit0005-ltext,
         spras TYPE zc102fit0005-spras,
       END OF gs_blart,
       gt_blart LIKE TABLE OF gs_blart.

*-- BP 마스터 데이터
DATA : BEGIN OF gs_partner,
         partner TYPE zc102bpt0001-partner,
         accno   TYPE zc102bpt0001-accno,
         bp_role TYPE zc102bpt0001-bp_role,
         bp_type TYPE zc102bpt0001-bp_type,
       END OF gs_partner,
       gt_partner LIKE TABLE OF gs_partner.

*-- 계정과목 마스터 데이터
DATA : BEGIN OF gs_saknr,
         saknr TYPE zc102fit0002-saknr,
         txt20 TYPE zc102fit0002-txt20,
       END OF gs_saknr,
       gt_saknr LIKE TABLE OF gs_saknr.

*-- 계좌 마스터 데이터
DATA : BEGIN OF gs_account,
         accno  TYPE zc102fit0013-accno,
         accnum TYPE zc102fit0013-accnum,
         bank   TYPE zc102fit0013-bank,
       END OF gs_account,
       gt_account LIKE TABLE OF gs_account.

*-- 미결 전표 아이템 Fcat
DATA : gt_fcat_open   TYPE lvc_t_fcat,
       gs_fcat_open   TYPE lvc_s_fcat,
       gs_layout_open TYPE lvc_s_layo,
       gs_variant     TYPE disvariant,
       gt_sort_open   TYPE lvc_t_sort,
       gs_sort_open   TYPE lvc_s_sort.

*-- 반제 전표 아이템 Fcat
DATA : gt_fcat_clear   TYPE lvc_t_fcat,
       gs_fcat_clear   TYPE lvc_s_fcat,
       gs_layout_clear TYPE lvc_s_layo,
       gt_sort_clear   TYPE lvc_t_sort,
       gs_sort_clear   TYPE lvc_s_sort.

*--------------------------------------------------------------------*
* 반제 전표 생성 ALV
*--------------------------------------------------------------------*
*-- For Screen 0110
DATA : BEGIN OF gs_clear_header,
         bukrs TYPE zc102fit0009-bukrs,
         belnr TYPE zc102fit0009-belnr,
         gjahr TYPE zc102fit0009-gjahr,
         rebzg TYPE zc102fit0016-rebzg,
         bldat TYPE zc102fit0009-bldat,
         blart TYPE zc102fit0009-blart,
         ltext TYPE zc102fit0005-ltext,
         budat TYPE zc102fit0009-budat,
         waers TYPE zc102fit0009-waers,
         bktxt TYPE zc102fit0009-bktxt.
         INCLUDE STRUCTURE zc102cms0001.
DATA :
       END OF gs_clear_header,
       gs_clear_header_s TYPE zc102fit0009. " 헤더 저장 용 WA

DATA : BEGIN OF gs_clear_item,
         bukrs   TYPE zc102fit0009-bukrs,
         belnr   TYPE zc102fit0009-belnr,
         gjahr   TYPE zc102fit0009-gjahr,
         augbl   TYPE zc102fit0016-augbl,
         buzei   TYPE zc102fit0010-buzei,
         saknr   TYPE zc102fit0010-saknr,
         augdt   TYPE zc102fit0010-augdt,
         rebzg   TYPE zc102fit0016-rebzg,
         bschl   TYPE zc102fit0010-bschl,
         ltext   TYPE zc102fit0004-ltext,
         shkzg   TYPE zc102fit0010-shkzg,
         koart   TYPE zc102fit0010-koart,
         partner TYPE zc102fit0010-partner,
         sgtxt   TYPE zc102fit0010-sgtxt,
         wrbtr   TYPE zc102fit0010-wrbtr,
         waers   TYPE zc102fit0010-waers.
         INCLUDE STRUCTURE zc102cms0001.
DATA:
       END OF gs_clear_item,
       gt_clear_item LIKE TABLE OF gs_clear_item.

*-- 반제 아이템 생성 Fcat
DATA : gt_fcat_pop_clear   TYPE lvc_t_fcat,
       gs_fcat_pop_clear   TYPE lvc_s_fcat,
       gs_layout_pop_clear TYPE lvc_s_layo,
       gt_ui_functions     TYPE TABLE OF ui_func,
       gs_button           TYPE stb_button.

*--------------------------------------------------------------------*
* 계좌 거래 내역 조회
*--------------------------------------------------------------------*
DATA : BEGIN OF gs_history,
         serno   TYPE zc102fit0014-serno,
         accno   TYPE zc102fit0014-accno,
         dwdate  TYPE zc102fit0014-dwdate,
         wrbtr   TYPE zc102fit0014-wrbtr,
         waers   TYPE zc102fit0014-waers,
         history TYPE zc102fit0014-history,
         ttype   TYPE zc102fit0014-ttype,
         partner TYPE zc102fit0014-partner,
       END OF gs_history,
       gt_history LIKE TABLE OF gs_history.

*-- 거래 내역 Fcat
DATA : gt_fcat_account   TYPE lvc_t_fcat,
       gs_fcat_account   TYPE lvc_s_fcat,
       gs_layout_account TYPE lvc_s_layo.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode        TYPE sy-ucomm,
       gv_pop_flag      TYPE abap_bool VALUE abap_true,  " 시작 팝업, 선택 팝업 체크
       gv_sum_open      TYPE zc102fit0010-wrbtr,         " 미결액
       gv_sum_clear     TYPE zc102fit0010-wrbtr,         " 반제액
       gv_remain        TYPE zc102fit0010-wrbtr VALUE 1, " 잔액
       gv_waers_open    TYPE zc102fit0010-waers,         " 미결액 통화키
       gv_waers_clear   TYPE zc102fit0010-waers,         " 반제액 통화키
       gv_waers_remain  TYPE zc102fit0010-waers,         " 잔액 통화키
       gv_status        TYPE icon_d,                     " 반제 상태 체킹
       gv_missing_field TYPE string,                     " 누락 필드 저장
       gv_clear_status  TYPE icon_d,                     " 반제 전표 생성 가능 여부
       gv_clear_sum     TYPE zc102fit0016-wrbtr,         " 반제 전표 생성 차/대변 합
       gv_clear_debit   TYPE zc102fit0016-wrbtr.         " 반제 전표 차번 합

*-- 계좌 팝업 검색 조건 데이터
DATA : gv_bank      TYPE zc102fit0013-bank,
       gv_accno     TYPE zc102fit0013-accno,
       gv_accnum    TYPE zc102fit0013-accnum,
       gv_dwdate_fr TYPE zc102fit0014-dwdate,
       gv_dwdate_to TYPE zc102fit0014-dwdate,
       gv_flag.

FIELD-SYMBOLS : <gs_fs> TYPE any. " 필드 값 지정 용 Field Symbol

RANGES : gr_bank    FOR zc102fit0013-bank,
         gr_accno   FOR zc102fit0013-accno,
         gr_partner FOR zc102fit0014-partner,
         gr_dwdate  FOR zc102fit0014-dwdate.
*         gr_belnr   FOR ZC102FIT0014.
