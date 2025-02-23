``` abap
*&---------------------------------------------------------------------*
*& Report ZCL1R16007
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zcl1r16007.

**********************************************************************
* Practice
**********************************************************************
*-- 2.1-1.
DATA : BEGIN OF gs_info,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         gjahr TYPE bkpf-gjahr,
         buzei TYPE bseg-buzei,
         budat TYPE bkpf-budat,
         bktxt TYPE bkpf-bktxt,
         wrbtr TYPE bseg-wrbtr,
         waers TYPE bkpf-waers,
         sgtxt TYPE bseg-sgtxt,
       END OF gs_info,
       gt_info LIKE TABLE OF gs_info.

*-- 2.1-2.
CLEAR : gt_info, gs_info.
gs_info-bukrs = '1000'.
gs_info-belnr = '0010000007'.
gs_info-gjahr = '2025'.
gs_info-buzei = '0002'.
gs_info-budat = sy-datum.
gs_info-bktxt = 'Document header'.
gs_info-wrbtr = 15000.
gs_info-waers = 'KRW'.
gs_info-sgtxt = 'Line item'.

APPEND gs_info TO gt_info.

WRITE : / 'APPEND'.
LOOP AT gt_info INTO gs_info.
  WRITE :/ sy-tabix, 'index', gs_info-bukrs, gs_info-belnr, gs_info-gjahr, gs_info-buzei, gs_info-budat, gs_info-bktxt,
           gs_info-wrbtr, gs_info-waers, gs_info-sgtxt.
ENDLOOP.

WRITE : sy-uline.

*-- 2.1-3.
CLEAR : gs_info.
gs_info-bukrs = '1000'.
gs_info-belnr = '0010000006'.
gs_info-gjahr = '2024'.
gs_info-buzei = '0001'.
gs_info-budat = sy-datum.
gs_info-bktxt = 'Document insert header'.
gs_info-wrbtr = 7000.
gs_info-waers = 'KRW'.
gs_info-sgtxt = 'Line item insert'.

INSERT gs_info INTO gt_info INDEX 1.

WRITE : / 'INSERT'.
LOOP AT gt_info INTO gs_info.
  WRITE :/ sy-tabix, 'index', gs_info-bukrs, gs_info-belnr, gs_info-gjahr, gs_info-buzei, gs_info-budat, gs_info-bktxt,
           gs_info-wrbtr, gs_info-waers, gs_info-sgtxt.
ENDLOOP.

WRITE : sy-uline.


*-- 2.1-4.
READ TABLE gt_info INTO gs_info INDEX 2.

gs_info-bktxt = 'Document header updated'.
gs_info-wrbtr = 12000.
gs_info-waers = 'USD'.
gs_info-sgtxt = 'Line item updated'.

MODIFY gt_info FROM gs_info INDEX 2 TRANSPORTING bktxt wrbtr waers sgtxt.

WRITE : / 'MODIFY'.
LOOP AT gt_info INTO gs_info.
  WRITE :/ sy-tabix, 'index', gs_info-bukrs, gs_info-belnr, gs_info-gjahr, gs_info-buzei, gs_info-budat, gs_info-bktxt,
           gs_info-wrbtr, gs_info-waers, gs_info-sgtxt.
ENDLOOP.

WRITE : sy-uline.

*-- 2.2
DATA : BEGIN OF gs_scarr.
         INCLUDE STRUCTURE scarr.
DATA :   connid    TYPE sflight-connid,
         price     TYPE sflight-price,
         currency  TYPE sflight-currency,
         planetype TYPE sflight-planetype.
*         INCLUDE STRUCTURE sapplco_customer.
DATA : END OF gs_scarr.

DATA : gt_scarr LIKE TABLE OF gs_scarr.

*-- 3.
TYPES : BEGIN OF ts_scustom,
          id     TYPE scustom-id,
          name   TYPE scustom-name,
          form   TYPE scustom-form,
          street TYPE scustom-street,
          city   TYPE scustom-city,
          email  TYPE scustom-email,
        END OF ts_scustom.
DATA : gs_scustom TYPE ts_scustom,
       gt_scustom TYPE TABLE OF ts_scustom.

CLEAR : gs_scustom, gt_scustom.
SELECT id name form street city email
  INTO CORRESPONDING FIELDS OF TABLE gt_scustom
  FROM scustom
  WHERE id GE 24 AND form NE 'Firma' AND region EQ 'PA'.

cl_demo_output=>display( gt_scustom ).
