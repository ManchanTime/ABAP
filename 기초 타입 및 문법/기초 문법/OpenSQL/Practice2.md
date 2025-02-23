``` abap
*&---------------------------------------------------------------------*
*& Report ZCL1R16010
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zcl1r16010.

**********************************************************************
* Table : ZCL1BKPF16 Practice
**********************************************************************
*-- 1. 001000000
* Work Area를 이용한 INSERT 구문으로 Single Record 생성
*--------------------------------------------------------------------*
DATA : gs_zcl1bkpf16 TYPE zcl1bkpf16,
       gt_zcl1bkpf16 TYPE TABLE OF zcl1bkpf16.

CLEAR : gs_zcl1bkpf16, gt_zcl1bkpf16.

gs_zcl1bkpf16-bukrs      = '1000'.
gs_zcl1bkpf16-belnr      = '0010000000'.
gs_zcl1bkpf16-gjahr      = '2025'.
gs_zcl1bkpf16-blart      = 'SA'.
gs_zcl1bkpf16-budat      = '20250115'.
gs_zcl1bkpf16-bktxt      = 'First document header'.
gs_zcl1bkpf16-waers      = 'KRW'.
gs_zcl1bkpf16-issue_no   = 'TR202501001'.
gs_zcl1bkpf16-issue_date = '20250105'.

*DELETE FROM zcl1bkpf16 WHERE belnr = '0010000000' AND bukrs = '1000'.
INSERT zcl1bkpf16 FROM gs_zcl1bkpf16.

*--------------------------------------------------------------------*
*-- 2. 001000001 001000003 001000004 001000005
* 데이터는 Internal Table을 이용한 INSERT 구문으로 Multiple Record 생성
*--------------------------------------------------------------------*
CLEAR gs_zcl1bkpf16.
gs_zcl1bkpf16-bukrs      = '1000'.
gs_zcl1bkpf16-belnr      = '0010000001'.
gs_zcl1bkpf16-gjahr      = '2025'.
gs_zcl1bkpf16-blart      = 'SA'.
gs_zcl1bkpf16-budat      = '20250114'.
gs_zcl1bkpf16-bktxt      = 'Second document header'.
gs_zcl1bkpf16-waers      = 'USD'.
gs_zcl1bkpf16-issue_no   = 'TR202501002'.
gs_zcl1bkpf16-issue_date = '20250105'.

APPEND gs_zcl1bkpf16 TO gt_zcl1bkpf16.

CLEAR gs_zcl1bkpf16.
gs_zcl1bkpf16-bukrs      = '1000'.
gs_zcl1bkpf16-belnr      = '0010000003'.
gs_zcl1bkpf16-gjahr      = '2025'.
gs_zcl1bkpf16-blart      = 'AB'.
gs_zcl1bkpf16-budat      = '20250110'.
gs_zcl1bkpf16-bktxt      = 'Third documeny header'.
gs_zcl1bkpf16-waers      = 'EUR'.
gs_zcl1bkpf16-issue_no   = 'TR202501003'.
gs_zcl1bkpf16-issue_date = '20250105'.

APPEND gs_zcl1bkpf16 TO gt_zcl1bkpf16.

CLEAR gs_zcl1bkpf16.
gs_zcl1bkpf16-bukrs      = '1000'.
gs_zcl1bkpf16-belnr      = '0010000004'.
gs_zcl1bkpf16-gjahr      = '2025'.
gs_zcl1bkpf16-blart      = 'AB'.
gs_zcl1bkpf16-budat      = '20250102'.
gs_zcl1bkpf16-bktxt      = 'Customer invoice'.
gs_zcl1bkpf16-waers      = 'KRW'.
gs_zcl1bkpf16-issue_no   = 'TR202501004'.
gs_zcl1bkpf16-issue_date = '20250105'.

APPEND gs_zcl1bkpf16 TO gt_zcl1bkpf16.

CLEAR gs_zcl1bkpf16.
gs_zcl1bkpf16-bukrs      = '1000'.
gs_zcl1bkpf16-belnr      = '0010000005'.
gs_zcl1bkpf16-gjahr      = '2025'.
gs_zcl1bkpf16-blart      = 'KR'.
gs_zcl1bkpf16-budat      = '20250103'.
gs_zcl1bkpf16-bktxt      = 'Vendor invoice'.
gs_zcl1bkpf16-waers      = 'KRW'.
gs_zcl1bkpf16-issue_no   = 'TR202501005'.
gs_zcl1bkpf16-issue_date = '20250110'.

APPEND gs_zcl1bkpf16 TO gt_zcl1bkpf16.

*DELETE zcl1bkpf16 FROM TABLE gt_zcl1bkpf16.
INSERT zcl1bkpf16 FROM TABLE gt_zcl1bkpf16
  ACCEPTING DUPLICATE KEYS.

*--------------------------------------------------------------------*
*-- 3. 001000003
* BKTXT의 내용을 'Clearing document'로 Update한다.
* Work Area를 이용한 UPDATE 구문 활용
*--------------------------------------------------------------------*
CLEAR : gs_zcl1bkpf16, gt_zcl1bkpf16.
SELECT SINGLE bukrs belnr gjahr blart budat
              bktxt waers issue_no issue_date
  INTO CORRESPONDING FIELDS OF gs_zcl1bkpf16
  FROM zcl1bkpf16
  WHERE bukrs = '1000' AND belnr = '0010000003' AND gjahr = '2025'.

gs_zcl1bkpf16-bktxt = 'Clearing document'.

UPDATE zcl1bkpf16 FROM gs_zcl1bkpf16.

*--------------------------------------------------------------------*
*-- 4. 001000004 001000005
* 2개의 전표의 WAERS를 변경. 4번 전표는 EUR, 5번 전표는 USD
* Itab를 이용한 UPDATE 구문
*--------------------------------------------------------------------*
CLEAR : gs_zcl1bkpf16, gt_zcl1bkpf16.
SELECT SINGLE bukrs belnr gjahr blart budat
              bktxt waers issue_no issue_date
  INTO CORRESPONDING FIELDS OF gs_zcl1bkpf16
  FROM zcl1bkpf16
  WHERE bukrs = '1000' AND belnr = '0010000004' AND gjahr = '2025'.

gs_zcl1bkpf16-waers = 'EUR'.

APPEND gs_zcl1bkpf16 TO gt_zcl1bkpf16.

CLEAR gs_zcl1bkpf16.
SELECT SINGLE bukrs belnr gjahr blart budat
              bktxt waers issue_no issue_date
  INTO CORRESPONDING FIELDS OF gs_zcl1bkpf16
  FROM zcl1bkpf16
  WHERE bukrs = '1000' AND belnr = '0010000005' AND gjahr = '2025'.

gs_zcl1bkpf16-waers = 'USD'.

APPEND gs_zcl1bkpf16 TO gt_zcl1bkpf16.

UPDATE zcl1bkpf16 FROM TABLE gt_zcl1bkpf16.

*--------------------------------------------------------------------*
*-- 5. 001000001
* 전표를 삭제한다.
*--------------------------------------------------------------------*
CLEAR gs_zcl1bkpf16.
gs_zcl1bkpf16-bukrs = '1000'.
gs_zcl1bkpf16-belnr = '0010000001'.
gs_zcl1bkpf16-gjahr = '2025'.

DELETE zcl1bkpf16 FROM gs_zcl1bkpf16.

*--------------------------------------------------------------------*
*-- 6. 0010000000 001000006
* 전표의 BUDAT을 '20250115'로 변경 ITAB을 이용한 MODIFY
*--------------------------------------------------------------------*
CLEAR : gs_zcl1bkpf16, gt_zcl1bkpf16.
SELECT bukrs belnr gjahr blart budat
       bktxt waers issue_no issue_date
  INTO CORRESPONDING FIELDS OF TABLE gt_zcl1bkpf16
  FROM zcl1bkpf16
  WHERE bukrs = '1000' AND belnr IN ('0010000000', '0010000005')
    AND gjahr = '2025'.

*-- LOOP AT
LOOP AT gt_zcl1bkpf16 INTO gs_zcl1bkpf16.
  IF ( gs_zcl1bkpf16-bukrs = '1000' AND gs_zcl1bkpf16-gjahr = '2025' ).
    IF ( gs_zcl1bkpf16-belnr = '0010000000' ).
      gs_zcl1bkpf16-budat = '20250116'.
    ELSEIF ( gs_zcl1bkpf16-belnr = '0010000005' ).
      gs_zcl1bkpf16-belnr = '0010000006'.
    ENDIF.
  ENDIF.
  MODIFY gt_zcl1bkpf16 FROM gs_zcl1bkpf16 INDEX sy-tabix
    TRANSPORTING belnr budat.
ENDLOOP.

*-- DO ~ ENDDO
DO.
  READ TABLE gt_zcl1bkpf16 INTO gs_zcl1bkpf16 INDEX sy-index.
  IF ( gs_zcl1bkpf16-bukrs = '1000' AND gs_zcl1bkpf16-gjahr = '2025' ).
    IF ( gs_zcl1bkpf16-belnr = '0010000000' ).
      gs_zcl1bkpf16-budat = '20250116'.
    ELSEIF ( gs_zcl1bkpf16-belnr = '0010000005' ).
      gs_zcl1bkpf16-belnr = '0010000006'.
    ENDIF.
  ENDIF.
  MODIFY gt_zcl1bkpf16 FROM gs_zcl1bkpf16 INDEX sy-index
    TRANSPORTING belnr budat.
  IF ( sy-subrc <> 0 ).
    EXIT.
  ENDIF.
ENDDO.

*-- READ TABLE
*READ TABLE gt_zcl1bkpf16 INTO gs_zcl1bkpf16
*  WITH KEY belnr = '0010000000' bukrs = '1000' gjahr = '2025'.
*gs_zcl1bkpf16-budat = '20250116'.
*MODIFY gt_zcl1bkpf16 FROM gs_zcl1bkpf16 INDEX sy-tabix.
*
*READ TABLE gt_zcl1bkpf16 INTO gs_zcl1bkpf16
*  WITH KEY belnr = '0010000005' bukrs = '1000' gjahr = '2025'.
*gs_zcl1bkpf16-belnr = '0010000006'.
*MODIFY gt_zcl1bkpf16 FROM gs_zcl1bkpf16 INDEX sy-tabix.

MODIFY zcl1bkpf16 FROM TABLE gt_zcl1bkpf16.

*--------------------------------------------------------------------*

IF sy-subrc = 0.
  COMMIT WORK.
ELSE.
  ROLLBACK WORK.
ENDIF.

CASE sy-subrc.
  WHEN 0.
    COMMIT WORK.
  WHEN OTHERS.
    ROLLBACK WORK.
ENDCASE.
