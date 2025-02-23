```abap
*&---------------------------------------------------------------------*
*& Report ZCL1R16001
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zcl1r16001.

**********************************************************************
* Local type - ABAP Dictionary
*--------------------------------------------------------------------*
* Declaration command : TYPES
**********************************************************************
TYPES : tv_char3 TYPE c LENGTH 3,
        tv_dec3  TYPE p LENGTH 5 DECIMALS 3,
        tv_int4  TYPE i,
        tv_date  TYPE d.

**********************************************************************
* Declaration of variable
*--------------------------------------------------------------------*
* Using ABAP Incompete type
**********************************************************************
DATA : gv_char3    TYPE tv_char3,  " Reference to local data type
       gv_char2    TYPE c LENGTH 2,
       gv_val1(2)  TYPE c,
       gv_var2     TYPE c,         " Length를 지정하지 않으면 기본 1자리를 차지함
       gv_var3,                    " TYPE을 지정하지 않으면 기본 Char 유형 -> c 타입의 길이 1
       gv_var4(10),                " Char 유형, 길이 10
       gv_carrid   TYPE s_carr_id, " Refence to global type
       gv_bukrs    TYPE bukrs.

DATA : gv_pvar1    TYPE p LENGTH 5 DECIMALS 2,
       gv_pvar2(5) TYPE p DECIMALS 2,
       gv_num1     TYPE n LENGTH 5,
       gv_num2(5)  TYPE n,
       gv_num3     LIKE gv_num1.   " DATA 구문으로 선언된 변수를 참조하여 선언

**********************************************************************
* Using complete type
**********************************************************************
DATA : gv_date1 TYPE d,
       gv_date2 TYPE tv_date,  " Reference to local type
       gv_budat TYPE budat,    " Reference to global type
       gv_date3 LIKE gv_date1, " Data 구문으로 선언된 변수를 참조하여 선언
       gv_time  TYPE t,
       gv_int4  TYPE i,
       gv_float TYPE f,
       gv_str   TYPE string.   " Dynamic character string

**********************************************************************
* Reference to ABAP Dictionary - Global type ( Table or Structure field)
**********************************************************************
DATA : gv_carrid2   TYPE spfli-carrid,   " ${소유자}-${필드이름}
       gv_countryfr TYPE spfli-countryfr,
       gv_cityfrom  TYPE spfli-cityfrom.

DATA : gv_bukrs2 TYPE bkpf-bukrs,  " Company code
       gv_belnr  TYPE bkpf-belnr,  " Document number
       gv_gjahr  TYPE bkpf-gjahr.  " Fiscal year

DATA : gv_augbl TYPE bseg-augbl,
       gv_koart TYPE bseg-koart,
       gv_zumsk TYPE bseg-zumsk.

DATA : gv_mandt TYPE sy-mandt,
       gv_uname TYPE sy-uname,
       gv_langu TYPE sy-langu,
       gv_datum TYPE sy-datum,
       gv_uzeit TYPE sy-uzeit,
       gv_tcode TYPE sy-tcode,
       gv_repid TYPE sy-repid,
       gv_index TYPE sy-index.

**********************************************************************
* Internal table and work area (Structure)
**********************************************************************
*-- Internal table : Reference to Transparent table
DATA : gt_scarr   TYPE TABLE OF scarr,
       gt_spfli   TYPE TABLE OF spfli,
       gt_sflight TYPE TABLE OF sflight,
       gt_sbook   TYPE TABLE OF sbook.

*-- Work area : Reference to Transparent table
DATA : gs_scarr   TYPE scarr,
       gs_spfli   TYPE spfli,
       gs_sflight TYPE sflight,
       gs_sbook   TYPE sbook.

*-- Internal table : Reference to Structure
DATA : gt_comp TYPE TABLE OF zscl1_comp_16.

*-- Work area : Reference to Structure
DATA : gs_comp TYPE zscl1_comp_16.

*-- Internal table : Reference to Table type, Internal table 정의를 위한 타입으로 TABLE OF 생략 가능
DATA : gt_info TYPE zlvc_t_cl1_info16.

*-- Work area : Reference to line type in Table type
DATA : gs_info TYPE zscl1_comp_16.

*-- Practice
DATA : gt_ztcl1board16 TYPE TABLE OF ztcl1board16,
       gs_ztcl1board16 TYPE ztcl1board16.

DATA : gt_ztc1_tax_16 TYPE TABLE OF ztc1_tax_16,
       gs_ztc1_tax_16 TYPE ztc1_tax_16.

**********************************************************************
* Calculation
**********************************************************************
DATA : gv_string  TYPE string VALUE 'SYNC6',
       gv_len     TYPE i,
       gv_result  TYPE i,
       gv_number1 TYPE i,
       gv_number2 TYPE i.

*-- Assign data
gv_number1 = 7.
gv_number2 = 5.

CLEAR gv_result.

gv_result = gv_number1 + gv_number2.          " Adding
gv_result = gv_number1 - gv_number2.          " Subtraction
gv_result = gv_number1 * gv_number2.          " Multiply
gv_result = gv_number1 / gv_number2.          " Divide
gv_result = gv_result + 1.                    " Summary
gv_result = 2 ** 3.                           " Exponetitation
gv_result = ( gv_number1 * gv_number2 ) ** 2.
gv_result = gv_number1 DIV gv_number2.        " DIV
gv_result = gv_number1 MOD gv_number2.        " MOD

gv_len = strlen( gv_string ).

WRITE : 'gv_len : ', gv_len.
