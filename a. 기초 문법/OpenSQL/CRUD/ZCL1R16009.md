``` abap
*&---------------------------------------------------------------------*
*& Report ZCL1R16009
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zcl1r16009.

**********************************************************************
*Internal Table and Work Area
**********************************************************************
DATA : gt_board TYPE TABLE OF ztcl1board16,
       gs_board TYPE ztcl1board16.

**********************************************************************
* Main Logic
**********************************************************************
CLEAR : gs_board, gt_board.

*-- Creation of Single record
*gs_board-seqno    = 1.
*gs_board-author   = sy-uname.
*gs_board-comp     = '1000'.
*gs_board-zyear    = sy-datum.
*gs_board-zip_code = '113870'.
*gs_board-zzone    = 'A'.
*
*INSERT ztcl1board16 FROM gs_board.

*-- Creation of multiple record
gs_board-seqno    = 2.
gs_board-author   = sy-uname.
gs_board-comp     = '2000'.
gs_board-zyear    = sy-datum.
gs_board-zip_code = '113870'.
gs_board-zzone    = 'A'.
APPEND gs_board TO gt_board.

CLEAR gs_board.

gs_board-seqno    = 3.
gs_board-author   = sy-uname.
gs_board-comp     = '3000'.
gs_board-zyear    = sy-datum.
gs_board-zip_code = '113871'.
gs_board-zzone    = 'C'.
APPEND gs_board TO gt_board.

INSERT ztcl1board16 FROM TABLE gt_board
  ACCEPTING DUPLICATE KEYS.

*-- Update of single record
CLEAR gs_board.

**-- Set PK
gs_board-seqno    = 1.
gs_board-author   = sy-uname.

*-- Set Normal field for update
gs_board-comp       = '3000'.
gs_board-zyear      = '2024'.
gs_board-zip_code   = '113871'.
gs_board-zzone      = 'A'.

UPDATE ztcl1board16 FROM gs_board.

*-- Update after get single record
CLEAR gs_board.
SELECT SINGLE *
  INTO CORRESPONDING FIELDS OF gs_board
  FROM ztcl1board16
  WHERE seqno = 2 AND author = sy-uname.

gs_board-comp       = '2000'.
gs_board-zyear      = '2022'.
gs_board-zip_code   = '113872'.
gs_board-zzone      = 'B'.

UPDATE ztcl1board16 FROM gs_board.

*-- Using SET Mode
UPDATE ztcl1board16
  SET comp     = gs_board-comp
      zyear    = sy-datum
      zip_code = gs_board-zip_code
      zzone    = gs_board-zzone
      title    = 'I am not a good person'
  WHERE seqno = 2 AND author = 'LEE'.

*-- Update multiple record
CLEAR : gs_board, gt_board.
*-- Set PK
gs_board-seqno  = 1.
gs_board-author = 'LSW'.

*-- Set normal field for update
gs_board-kind   = 'c1'.
gs_board-title  = 'MULTIPLE TITLE'.
APPEND gs_board TO gt_board.

CLEAR gs_board.

gs_board-seqno  = 2.
gs_board-author = 'LEE'.
gs_board-kind   = 'D1'.
gs_board-title  = 'MULTIPLE TITLE2'.
APPEND gs_board TO gt_board.

UPDATE ztcl1board16 FROM TABLE gt_board.

*-- Delete of single record
CLEAR : gs_board, gt_board.
*-- Set PK
gs_board-seqno  = 3.
gs_board-author = 'KKK'.

DELETE ztcl1board16 FROM gs_board.

*-- Delete of multiple record
CLEAR : gs_board, gt_board.
*-- Set PK
gs_board-seqno  = 1.
gs_board-author = 'LSW'.
APPEND gs_board TO gt_board.

CLEAR gs_board.
gs_board-seqno  = 2.
gs_board-author = 'LEE'.
APPEND gs_board TO gt_board.

DELETE ztcl1board16 FROM TABLE gt_board.

*-- Using FROM Mode
DELETE FROM ztcl1board16
  WHERE seqno BETWEEN 2 AND 3.

*-- Using MODIFY : Single record
*-- Insert Data
CLEAR gs_board.
gs_board-seqno    = 1.
gs_board-author   = sy-uname.
gs_board-comp     = '1000'.
gs_board-zyear    = sy-datum.
gs_board-zip_code = '113870'.
gs_board-zzone    = 'A'.

INSERT ztcl1board16 FROM gs_board.

*-- Insert set
CLEAR gs_board.
gs_board-seqno  = 5.
gs_board-author = sy-uname.
gs_board-title  = 'NEW INSERTED'.

MODIFY ztcl1board16 FROM gs_board.

*-- Update set
CLEAR gs_board.
gs_board-seqno  = 1.
gs_board-author = sy-uname.
gs_board-title  = 'NEW UPDATED'.

MODIFY ztcl1board16 FROM gs_board.

*-- Using MODIFY : Multiple record
CLEAR : gs_board, gt_board.
gs_board-seqno  = 1.
gs_board-author = sy-uname.
gs_board-title  = 'MUILTIPLE UPDATED BY MODIFY'.
APPEND gs_board TO gt_board.

CLEAR : gs_board.
gs_board-seqno  = 5.
gs_board-author = sy-uname.
gs_board-title  = 'MUILTIPLE UPDATED BY MODIFY'.
APPEND gs_board TO gt_board.

CLEAR : gs_board.
gs_board-seqno  = 3.
gs_board-author = sy-uname.
gs_board-title  = 'MULTIPLE INSERTED BY MODIFY'.
APPEND gs_board TO gt_board.

MODIFY ztcl1board16 FROM TABLE gt_board.

IF sy-subrc EQ 0.
  COMMIT WORK.
ELSE.
  ROLLBACK WORK.
ENDIF.
