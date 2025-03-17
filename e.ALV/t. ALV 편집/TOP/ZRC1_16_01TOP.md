``` abap
*&---------------------------------------------------------------------*
*& Include ZRC1_16_01TOP                            - Report ZRC1_16_01
*&---------------------------------------------------------------------*
REPORT zrc1_16_01 MESSAGE-ID zcl1msg16.

**********************************************************************
* Class instance
**********************************************************************
DATA : go_container TYPE REF TO cl_gui_docking_container,
       go_alv_grid  TYPE REF TO cl_gui_alv_grid.

**********************************************************************
* Internal table and work area
**********************************************************************
DATA : BEGIN OF gs_body.                      " Main data WA
         INCLUDE STRUCTURE ztc1_docu_16.      " 기존 ztc1_docu_16 info get
DATA :   txt50    TYPE skat-txt50,            " TXT50 필드
         cell_tab TYPE lvc_t_styl,            " Style 적용 테이블 필드
         modi_yn,                             " Timestamp를 위한 수정 여부 파악
       END OF gs_body,
       gt_body   LIKE TABLE OF gs_body,       " Main data ITAB
       gt_body_d TYPE TABLE OF ztc1_docu_16,  " 삭제 데이터 저장 용 ITAB
       gs_body_d TYPE ztc1_docu_16,           " 삭제 데이터 저장 용 WA
       gt_body_s TYPE TABLE of ztc1_docu_16.  " 수정 데이터 Transparent Table 반영 용 ITAB

*-- ALV
DATA : gs_layout  TYPE lvc_s_layo,
       gs_variant TYPE disvariant,
       gs_fcat    TYPE lvc_s_fcat,
       gt_fcat    TYPE lvc_t_fcat.

DATA : gs_button       TYPE stb_button,
       gt_ui_functions TYPE ui_functions.

**********************************************************************
* Common variable
**********************************************************************
DATA : gv_okcode TYPE sy-ucomm,
       gv_mode   TYPE i VALUE 0.
