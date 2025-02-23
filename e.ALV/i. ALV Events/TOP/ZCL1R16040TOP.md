``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16040TOP                            - Report ZCL1R16040
*&---------------------------------------------------------------------*
REPORT zcl1r16040 MESSAGE-ID zcl1msg16.
CLASS lcl_event_handler DEFINITION DEFERRED.

**********************************************************************
* TABLES
**********************************************************************
TABLES : bkpf, bseg.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

DATA : go_event TYPE REF TO lcl_event_handler.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_header,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         gjahr TYPE bkpf-gjahr,
         budat TYPE bkpf-budat,
         bktxt TYPE bkpf-bktxt,
         waers TYPE bkpf-waers,
       END OF gs_header.

DATA : BEGIN OF gs_item,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         gjahr TYPE bkpf-gjahr,
         buzei TYPE bseg-buzei,
         shkzg TYPE bseg-shkzg,
         sgtxt TYPE bseg-sgtxt,
         wrbtr TYPE bseg-wrbtr,
         pswsl TYPE bseg-pswsl,
       END OF gs_item.

DATA : gt_header LIKE TABLE OF gs_header,
       gt_item   LIKE TABLE OF gs_item.


DATA : gs_layout  TYPE lvc_s_layo,
       gs_fcat    TYPE lvc_s_fcat,
       gt_fcat    TYPE lvc_t_fcat,
       gs_variant TYPE disvariant.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
