``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16061TOP                            - Report ZCL1R16061
*&---------------------------------------------------------------------*
REPORT ZCL1R16061 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : sbook.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
