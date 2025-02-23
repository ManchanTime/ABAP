``` abap
*&---------------------------------------------------------------------*
*& Include ZCL1R16043TOP                            - Report ZCL1R16043
*&---------------------------------------------------------------------*
REPORT zcl1r16043 MESSAGE-ID zcl1msg16.

**********************************************************************
* TABLES
**********************************************************************
TABLES : scarr, sflight.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

DATA : go_pop_cont TYPE REF TO cl_gui_custom_container,
       go_pop_grid TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and Work area
**********************************************************************
DATA : BEGIN OF gs_body,
         carrid    TYPE scarr-carrid,
         carrname  TYPE scarr-carrname,
         connid    TYPE sflight-connid,
         fldate    TYPE sflight-fldate,
         planetype TYPE sflight-planetype,
         price     TYPE sflight-price,
         currency  TYPE sflight-currency,
         url       TYPE scarr-url,
         color     TYPE lvc_t_scol,
       END OF gs_body,
       gt_body LIKE TABLE OF gs_body.

DATA : BEGIN OF gs_planetype,
         planetype TYPE saplane-planetype,
         seatsmax  TYPE saplane-seatsmax,
         tankcap   TYPE saplane-tankcap,
         cap_unit  TYPE saplane-cap_unit,
         weight    TYPE saplane-weight,
         wei_unit  TYPE saplane-wei_unit,
       END OF gs_planetype,
       gt_planetype LIKE TABLE OF gs_planetype.

DATA : BEGIN OF gs_sbook,
         carrid     TYPE sbook-carrid,
         connid     TYPE sbook-connid,
         fldate     TYPE sbook-fldate,
         bookid     TYPE sbook-bookid,
         customid   TYPE sbook-customid,
         custtype   TYPE sbook-custtype,
         luggweight TYPE sbook-luggweight,
         wunit      TYPE sbook-wunit,
         loccuram   TYPE sbook-loccuram,
         loccurkey  TYPE sbook-loccurkey,
       END OF gs_sbook,
       gt_sbook LIKE TABLE OF gs_sbook.

DATA : gs_layout  TYPE lvc_s_layo,
       gs_fcat    TYPE lvc_s_fcat,
       gt_fcat    TYPE lvc_t_fcat,
       gs_variant TYPE disvariant.

DATA : gs_playout TYPE lvc_s_layo,
       gs_pfcat   TYPE lvc_s_fcat,
       gt_pfcat   TYPE lvc_t_fcat.

DATA : gt_ppfcat TYPE lvc_t_fcat,
       gs_ppfcat TYPE lvc_s_fcat.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm.
