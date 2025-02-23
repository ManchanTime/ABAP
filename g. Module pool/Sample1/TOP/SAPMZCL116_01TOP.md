``` abap
*&---------------------------------------------------------------------*
*& Include SAPMZCL116_01TOP                         - Module Pool      SAPMZCL116_01
*&---------------------------------------------------------------------*
PROGRAM sapmzcl116_01 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, spfli, sflight.

**********************************************************************
* Macro
**********************************************************************
DEFINE _init.

  REFRESH &1.
  CLEAR &1.

END-OF-DEFINITION.

**********************************************************************
* Screen field
**********************************************************************
DATA : gv_carrid   TYPE scarr-carrid,
       gv_carrname TYPE scarr-carrname,
       gv_price    TYPE sflight-price,
       gv_currency TYPE sflight-currency,
       gv_fldate   TYPE sflight-fldate.

DATA : gs_scarr TYPE scarr.

*-- Date search
DATA : gv_date_fr TYPE sy-datum,
       gv_date_to TYPE sy-datum.

**********************************************************************
* Common variable
**********************************************************************
RANGES gr_fldate FOR sflight-fldate.    " SELECT-OPTIONS와 동일 타입 변수(테이블 아님 -> TABLES 없어도 됨!)

DATA : gv_okcode TYPE sy-ucomm.
