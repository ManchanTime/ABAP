``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16025TOP                            - Report ZCL1R16025
*&---------------------------------------------------------------------*
REPORT zcl1r16025 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : t134.

**********************************************************************
* Internal table & Work area
**********************************************************************
DATA : BEGIN OF gs_data,
         mtart TYPE t134-mtart,
         mtref TYPE t134-mtref,
         mbref TYPE t134-mbref,
         pstat TYPE t134-pstat,
         vmtpo TYPE t134-vmtpo,
         kzgrp TYPE t134-kzgrp,
         kkref TYPE t134-kkref,
         krftx TYPE t025l-krftx,
       END OF gs_data,
       gt_data LIKE TABLE OF gs_data.

DATA : BEGIN OF gs_text,
         kkref TYPE t134-kkref,
         krftx TYPE t025l-krftx,
       END OF gs_text,
       gt_text LIKE TABLE OF gs_text.
