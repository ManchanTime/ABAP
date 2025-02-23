``` abap
*&---------------------------------------------------------------------*
*& Report ZCL1R16003
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zcl1r16003.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_data,
         carrid   TYPE scarr-carrid,    " 항공사 코드
         connid   TYPE spfli-connid,    " 연결항공편 번호
         fldate   TYPE sflight-fldate,  " 운항일자
         carrname TYPE scarr-carrname,  " 항공사명
         cityfrom TYPE spfli-cityfrom,  " 출발도시
         cityto   TYPE spfli-cityto,    " 도착도시
         size     TYPE i,               " Length
       END OF gs_data,
       gt_data LIKE TABLE OF gs_data.

DATA : gt_data2 LIKE TABLE OF gs_data,
       gt_data3 LIKE gt_data2,                  " Internal Table To Internal Table
       gs_data3 LIKE LINE OF gt_data,           " Internal Table To Work Area
       gs_data4 TYPE LINE OF zlvc_t_cl1_info16, " Table Type To Work Area
       gt_data4 TYPE zlvc_t_cl1_info16.         " Internal Table With Table Type
**********************************************************************
* Data access
**********************************************************************
CLEAR : gt_data, gs_data.
*-- 1. Work area에 Data를 할당한다. (CARRID <- 'LH', CONNID <- '0440')
gs_data-carrid    = 'LH'.
gs_data-connid    = '0440'.
gs_data-fldate    = sy-datum.
gs_data-carrname  = 'Luft Hanza'.
gs_data-cityfrom  = 'Seoul'.
gs_data-cityto    = 'Busan'.

APPEND gs_data TO gt_data.

CLEAR gs_data.
gs_data-carrid    = 'AA'.
gs_data-connid    = '0100'.
gs_data-fldate    = '20250105'.
gs_data-carrname  = 'American airline'.


INSERT gs_data INTO TABLE gt_data.

CLEAR gs_data.
gs_data-carrid    = 'CA'.
gs_data-connid    = '0700'.
gs_data-fldate    = '20250105'.
gs_data-carrname  = 'Canada airline'.
INSERT gs_data INTO gt_data INDEX 1.

CLEAR gs_data.
gs_data-carrid    = 'AA'.
gs_data-connid    = '0200'.
gs_data-fldate    = '20250105'.
gs_data-carrname  = 'American airline'.

INSERT gs_data INTO gt_data INDEX 4.

*-- CASE 1 : WITH KEY
CLEAR gs_data.
READ TABLE gt_data INTO gs_data WITH KEY carrid = 'AA'
                                         connid = '0200'.

*cl_demo_output=>display( gs_data ).

**-- CASE 2 : INDEX
CLEAR gs_data.
READ TABLE gt_data INTO gs_data INDEX 2.

*cl_demo_output=>display( gs_data ).

CLEAR gs_data.
gs_data-carrid    = 'CA'.
gs_data-connid    = '0700'.
gs_data-fldate    = '20250101'.
gs_data-carrname  = 'Canada airline'.
gs_data-size      = 17.                 " Updated data

MODIFY gt_data FROM gs_data INDEX 1 TRANSPORTING size. " TRANSPORTING 붙이면 PATCH랑 비슷! 특정 칼럼 값만 반영 가능! (굳이 필요없는 값은 건들지 말 것!)

*DELETE gt_data. " CLEAR gt_data 와 동일
DELETE gt_data WHERE carrid EQ 'AA' AND                 " 그냥 쿼리문인듯?
                     connid EQ '0100'.


*cl_demo_output=>display( gt_data ).
CLEAR gs_data.

WRITE :/ 'ALL DATA'.
SKIP 1.
LOOP AT gt_data INTO gs_data FROM 1 TO 2.
  WRITE :/ gs_data-carrid, gs_data-connid, gs_data-carrname, gs_data-size.
  gs_data-size += 1.
  MODIFY gt_data FROM gs_data INDEX sy-tabix TRANSPORTING size.
  WRITE :/ gs_data-carrid, gs_data-connid, gs_data-carrname, gs_data-size.
ENDLOOP.

WRITE sy-uline. " 구분선

WRITE :/ 'carrid EQ "AA"'.
SKIP 1.
LOOP AT gt_data INTO gs_data WHERE carrid EQ 'AA'.
  WRITE :/ gs_data-carrid, gs_data-connid, gs_data-carrname.
ENDLOOP.

*-- Process set data gt_data -> gt_dat2
CLEAR gt_data2.
INSERT LINES OF gt_data FROM 2 TO 3 INTO TABLE gt_data2.

WRITE sy-uline.
WRITE :/ 'Copy Table With INSERT TABLE (gt_data -> gt_data2)'.
SKIP 1.
LOOP AT gt_data2 INTO gs_data.
  WRITE :/ gs_data-carrid, gs_data-connid, gs_data-carrname.
ENDLOOP.

CLEAR gt_data2.
INSERT LINES OF gt_data FROM 2 TO 3 INTO gt_data2 INDEX 1.

WRITE sy-uline.
WRITE :/ 'Copy Table With INSERT (gt_data -> gt_data2)'.
SKIP 1.
LOOP AT gt_data2 INTO gs_data.
  WRITE :/ gs_data-carrid, gs_data-connid, gs_data-carrname.
ENDLOOP.

CLEAR gt_data2.
APPEND LINES OF gt_data FROM 2 TO 3 TO gt_data2.

WRITE sy-uline.
WRITE :/ 'Copy Table With APPEND (gt_data -> gt_data2)'.
SKIP 1.
LOOP AT gt_data2 INTO gs_data.
  WRITE :/ gs_data-carrid, gs_data-connid, gs_data-carrname.
ENDLOOP.

*-- 이것저것 연습
*CLEAR gs_data.
*CLEAR gt_data.
INSERT gs_data INTO TABLE gt_data.
INSERT gs_data INTO gt_data2 INDEX 1.
INSERT LINES OF gt_data FROM 2 TO 10 INTO gt_data2 INDEX 1.
INSERT LINES OF gt_data FROM 2 TO 10 INTO TABLE gt_data2.
APPEND gs_data TO gt_data.
APPEND LINES OF gt_data TO gt_data2.
MODIFY gt_data FROM gs_data INDEX 1 TRANSPORTING size.
READ TABLE gt_data INTO gs_data INDEX 1.
READ TABLE gt_data INTO gs_data WITH KEY carrid = 'AA'.
DELETE gt_data FROM 1 TO 10.
DELETE gt_data WHERE carrid = 'AA'.
APPEND LINES OF gt_data FROM 1 TO 10 TO gt_data2.
