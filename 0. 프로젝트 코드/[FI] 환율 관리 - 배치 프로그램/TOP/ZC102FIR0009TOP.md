``` abap
*&---------------------------------------------------------------------*
*& Include ZC102FIR0009TOP                          - Report ZC102FIR0009
*&---------------------------------------------------------------------*
REPORT zc102fir0009 MESSAGE-ID zc102msg.

**********************************************************************
* Class instance
**********************************************************************
DATA : lo_http_client TYPE REF TO if_http_client. " 커넥션 생성

**********************************************************************
* Internal table and work area
**********************************************************************
DATA : gt_save_data TYPE TABLE OF zc102fit0015,    " 저장 ITAB
       gs_save_data TYPE zc102fit0015.             " 저장 WA

*-- Json data -> 변경 대상 통화키, 금액
DATA : BEGIN OF gs_exchange_rate,
         cur_unit   TYPE string,
         deal_bas_r TYPE string,
       END OF gs_exchange_rate,
       gt_exchange_rate LIKE STANDARD TABLE OF gs_exchange_rate
                        WITH DEFAULT KEY.


**********************************************************************
* Common Variable
**********************************************************************
DATA : gv_url         TYPE string,  " API URI

       gv_status_code TYPE i,       " Http Status code
       gv_status_text TYPE string,  " Http Status text
       gv_response    TYPE string.  " Json Data - API로 받아옴
