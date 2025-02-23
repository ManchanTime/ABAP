``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16022TOP                            - Report ZCL1R16022
*&---------------------------------------------------------------------*
REPORT zcl1r16022 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, spfli.

**********************************************************************
* Internal table & Work area
**********************************************************************
DATA : BEGIN OF gs_body,
         carrid   TYPE scarr-carrid,
         connid   TYPE spfli-connid,
         cityfrom TYPE spfli-cityfrom,
         cityto   TYPE spfli-cityto,
         carrname TYPE scarr-carrname,
         url      TYPE scarr-url,
       END OF gs_body,
       gt_body LIKE TABLE OF gs_body.

DATA : gt_scarr TYPE TABLE OF scarr,
       gs_scarr TYPE scarr.
