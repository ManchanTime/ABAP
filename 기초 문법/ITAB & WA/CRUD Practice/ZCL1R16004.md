``` abap
*&---------------------------------------------------------------------*
*& Report ZCL1R16004
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zcl1r16004.

*-- Practice1
*-- 1.
" Line Type
TYPES : BEGIN OF ts_prac1,
          bukrs TYPE bkpf-bukrs,
          belnr TYPE bkpf-belnr,
          gjahr TYPE bkpf-gjahr,
          buzei TYPE bseg-buzei,
          budat TYPE bkpf-budat,
          bktxt TYPE bkpf-bktxt,
          wrbtr TYPE bseg-wrbtr,
          waers TYPE bkpf-waers,
          sgtxt TYPE bseg-sgtxt,
        END OF ts_prac1.

" Table Type
TYPES : tt_prac1 TYPE TABLE OF ts_prac1.

DATA : gs_prac1 TYPE ts_prac1,
       gt_prac1 TYPE TABLE OF ts_prac1,
       gt_prac2 LIKE TABLE OF gs_prac1,
       gt_prac3 LIKE gt_prac1,
       gt_prac4 TYPE tt_prac1,
       gs_prac2 TYPE LINE OF tt_prac1.

CLEAR gt_prac1.
*-- 2.
CLEAR gs_prac1.
gs_prac1-bukrs = '1000'.
gs_prac1-belnr = '0010000007'.
gs_prac1-gjahr = '2025'.
gs_prac1-buzei = '0002'.
gs_prac1-budat = sy-datum.
gs_prac1-bktxt = 'Document header'.
gs_prac1-wrbtr = 15000.
gs_prac1-waers = 'KRW'.
gs_prac1-sgtxt = 'Line item'.

APPEND gs_prac1 TO gt_prac1.

*-- 3.
CLEAR gs_prac1.
gs_prac1-bukrs = '1000'.
gs_prac1-belnr = '0010000006'.
gs_prac1-gjahr = '2024'.
gs_prac1-buzei = '0001'.
gs_prac1-budat = sy-datum.
gs_prac1-bktxt = 'Document insert header'.
gs_prac1-wrbtr = 7000.
gs_prac1-waers = 'KRW'.
gs_prac1-sgtxt = 'Line item insert'.

INSERT gs_prac1 INTO gt_prac1 INDEX 1.

*-- 4.
CLEAR gs_prac1.
READ TABLE gt_prac1 INTO gs_prac1 INDEX 2.
gs_prac1-bktxt = 'Document header updated'.
gs_prac1-wrbtr = 12000.
gs_prac1-waers = 'USD'.
gs_prac1-sgtxt = 'Line item updated'.

MODIFY gt_prac1 FROM gs_prac1 INDEX 2 TRANSPORTING bktxt wrbtr waers sgtxt.

CLEAR gs_prac1.

cl_demo_output=>display( gt_prac1 ).
