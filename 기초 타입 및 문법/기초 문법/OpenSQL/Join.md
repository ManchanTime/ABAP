``` abap
*&---------------------------------------------------------------------*
*& Report ZCL1R16008
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zcl1r16008.

**********************************************************************
* Internal Table and Work Area
**********************************************************************
DATA : BEGIN OF gs_schedule,
         carrid   TYPE scarr-carrid,
         connid   TYPE spfli-connid,
         cityfrom TYPE spfli-cityfrom,
         cityto   TYPE spfli-cityto,
         carrname TYPE scarr-carrname,
       END OF gs_schedule,
       gt_schedule LIKE TABLE OF gs_schedule.

DATA : BEGIN OF gs_flight,
         carrid   TYPE spfli-carrid,
         connid   TYPE spfli-connid,
         fldate   TYPE sflight-fldate,
         airpfrom TYPE spfli-airpfrom,
         airpto   TYPE spfli-airpto,
         seatsmax TYPE sflight-seatsmax,
         seatsocc TYPE sflight-seatsocc,
       END OF gs_flight,
       gt_flight LIKE TABLE OF gs_flight.

DATA : BEGIN OF gs_airline,
         carrid   TYPE scarr-carrid,
         connid   TYPE spfli-connid,
         cityfrom TYPE spfli-cityfrom,
         cityto   TYPE spfli-cityto,
         seatsmax TYPE sflight-seatsmax,
         seatsocc TYPE sflight-seatsocc,
         carrname TYPE scarr-carrname,
       END OF gs_airline,
       gt_airline LIKE TABLE OF gs_airline.

DATA : BEGIN OF gs_booking,
         carrid   TYPE spfli-carrid,
         connid   TYPE spfli-connid,
         fldate   TYPE sflight-fldate,
         bookid   TYPE sbook-bookid,
         cityfrom TYPE spfli-cityfrom,
         cityto   TYPE spfli-cityto,
         seatsmax TYPE sflight-seatsmax,
         seatsocc TYPE sflight-seatsocc,
         custtype TYPE sbook-custtype,
         smoker   TYPE sbook-smoker,
       END OF gs_booking,
       gt_booking LIKE TABLE OF gs_booking.

DATA : BEGIN OF gs_mara_marc,
         index TYPE sy-tabix,
         matnr TYPE mara-matnr,
         werks TYPE marc-werks,
         ersda TYPE mara-ersda,
         mtart TYPE mara-mtart,
         pstat TYPE marc-pstat,
       END OF gs_mara_marc,
       gt_mara_marc LIKE TABLE OF gs_mara_marc.

**********************************************************************
*Get data - JOIN SQL.
**********************************************************************
CLEAR : gs_schedule, gt_schedule.
SELECT a~carrid b~connid b~cityfrom b~cityto a~carrname
  INTO CORRESPONDING FIELDS OF TABLE gt_schedule
  FROM scarr AS a
  INNER JOIN spfli AS b
    ON a~carrid EQ b~carrid
  WHERE b~connid BETWEEN '0400' AND '0800'.

*cl_demo_output=>display( gt_schedule ).

*-- SPFLI와 SFLIGHT를 INNER JOIN하여 위의 Internal table에 DATA를 적재한다.
CLEAR : gs_flight, gt_flight.

SELECT a~carrid a~connid fldate airpfrom airpto seatsmax seatsocc
  INTO CORRESPONDING FIELDS OF TABLE gt_flight
  FROM spfli AS a
  INNER JOIN sflight AS b
  ON a~carrid EQ b~carrid AND a~connid EQ b~connid
  WHERE a~connid BETWEEN '0400' AND '0800'
  ORDER BY seatsmax DESCENDING seatsocc ASCENDING.

*cl_demo_output=>display( gt_flight ).

CLEAR : gs_schedule, gt_schedule.
*-- SCARR와 SPFLI를 LEFT OUTER JOIN하여 Internal Table에 적재시킨다.
SELECT a~carrid connid cityfrom cityto carrname
  INTO CORRESPONDING FIELDS OF TABLE gt_schedule
  FROM scarr AS a
    LEFT OUTER JOIN spfli AS b
    ON a~carrid EQ b~carrid
  WHERE a~carrid BETWEEN 'AA' AND 'ZZ'.

*cl_demo_output=>display( gt_schedule ).

*-- SCARR AS a INNER JOIN SPFLI AS b INNER JOIN SFLIGHT AS c
CLEAR : gs_airline, gt_airline.
SELECT a~carrid b~connid cityfrom cityto seatsmax seatsocc carrname
  INTO CORRESPONDING FIELDS OF TABLE gt_airline
  FROM scarr AS a
    INNER JOIN spfli   AS b ON a~carrid EQ b~carrid
    INNER JOIN sflight AS c ON b~carrid EQ c~carrid
                           AND b~connid EQ c~connid   " A -> B -> C"
*    INNER JOIN sflight AS c on a~carrid EQ c~carrid  " A -> B AND A -> C
  WHERE b~connid BETWEEN '0400' AND '0800'.

*cl_demo_output=>display( gt_airline ).

CLEAR : gs_booking, gt_booking.
SELECT a~carrid a~connid b~fldate bookid cityfrom
       cityto seatsmax seatsocc custtype smoker
  INTO CORRESPONDING FIELDS OF TABLE gt_booking "UP TO 100 ROWS
  FROM spfli AS a
    INNER JOIN sflight AS b
      ON a~carrid EQ b~carrid AND a~connid EQ b~connid
    INNER JOIN sbook   AS c
      ON b~carrid EQ c~carrid AND b~connid EQ c~connid
         AND b~fldate EQ c~fldate
  WHERE a~connid BETWEEN '0400' AND '0800'
        AND custtype NE 'P'
  ORDER BY seatsmax DESCENDING seatsocc DESCENDING.

*cl_demo_output=>display( gt_booking ).

*-- Practice
CLEAR : gs_mara_marc, gt_mara_marc.

SELECT a~matnr werks ersda mtart b~pstat
  INTO CORRESPONDING FIELDS OF TABLE gt_mara_marc
  FROM mara AS a
    INNER JOIN marc AS b ON a~matnr = b~matnr.

LOOP AT gt_mara_marc INTO gs_mara_marc.
  gs_mara_marc-index = sy-tabix.
  MODIFY gt_mara_marc FROM gs_mara_marc
    INDEX sy-tabix TRANSPORTING index.
ENDLOOP.

*cl_demo_output=>display( gt_mara_marc ).

*-- BKPF와 BSIS 테이블을 INNER JOIN하여 아래의 데이터를 SELECT 한다.
*-- BKPF AS a, BSIS AS b
*-- SELECT 조건 :
*--   BUKRS EQ '0001' AND GJAHR EQ '2025' AND BUDAT IN '20250101' ~ '20250131'
*-- Internal Table : GT_DOCU
*DATA : BEGIN OF gs_docu,
*         bukrs TYPE bkpf-bukrs,     " 공통 PK
*         belnr TYPE bkpf-belnr,     " 공통 PK
*         gjahr TYPE bkpf-gjahr,     " 공통 PK
*         buzei TYPE bsis-buzei,
*         bktxt TYPE bkpf-bktxt,
*         mwskz TYPE bsis-mwskz,
*         kostl TYPE bsis-kostl,
*         sgtxt TYPE bsis-sgtxt,
*         shkzg TYPE bsis-shkzg,
*         wrbtr TYPE bsis-wrbtr,
*         waers TYPE bkpf-waers,
*       END OF gs_docu,
*       gt_docu LIKE TABLE OF gs_docu.
*
*CLEAR : gs_docu, gt_docu.
*
*SELECT a~bukrs a~belnr a~gjahr buzei bktxt
*       mwskz kostl sgtxt shkzg wrbtr a~waers
*  INTO CORRESPONDING FIELDS OF TABLE gt_docu
*  FROM bkpf AS a
*    INNER JOIN bsis AS b
*      ON a~bukrs EQ b~bukrs AND a~belnr EQ b~belnr
*                            AND a~gjahr EQ b~gjahr
*  WHERE a~bukrs EQ '0001' AND a~gjahr EQ '2025'
*    AND a~budat BETWEEN '20250101' AND '20250131'.
*
*cl_demo_output=>display( gt_docu ).

DATA : BEGIN OF gs_docu,
         bukrs TYPE bkpf-bukrs,
         belnr TYPE bkpf-belnr,
         gjahr TYPE bkpf-gjahr,
         buzei TYPE bsis-buzei,
         bktxt TYPE bkpf-bktxt,
         mwskz TYPE bsis-mwskz,
         kostl TYPE bsis-kostl,
         sgtxt TYPE bsis-sgtxt,
         shkzg TYPE bsis-shkzg,
         wrbtr TYPE bsis-wrbtr,
         waers TYPE bkpf-waers,
       END OF gs_docu.
DATA gt_docu LIKE TABLE OF gs_docu.
DATA anumbering TYPE i.

SELECT a~bukrs a~belnr a~gjahr b~buzei a~bktxt b~mwskz b~kostl
       b~sgtxt b~shkzg b~wrbtr a~waers
  INTO CORRESPONDING FIELDS OF TABLE gt_docu
  FROM bkpf AS a INNER JOIN bsis AS b
  ON a~bukrs = b~bukrs AND a~belnr = b~belnr
  AND a~gjahr = b~gjahr
  WHERE a~bukrs = '0001' AND a~gjahr = '2025'
  AND a~budat BETWEEN '20250101' AND '20250131'.
DESCRIBE TABLE gt_docu LINES anumbering.
*  cl_demo_output=>write( |출력된 행 수: { anumbering }| ).
*  cl_demo_output=>write( sy-uline ).
*  cl_demo_output=>display( gt_DOCU ).

*-- MARA 테이블과 MTART의 Text Table을 INNER JOIN하여 아래의 DATA를 조회한다.
DATA : BEGIN OF gs_material,
         matnr TYPE mara-matnr,
         ersda TYPE mara-ersda,
         vpsta TYPE mara-vpsta,
         pstat TYPE mara-pstat,
         mtart TYPE mara-mtart,
         mtbez TYPE t134t-mtbez,
         ntgew TYPE mara-ntgew,
         gewei TYPE mara-gewei,
       END OF gs_material,
       gt_material LIKE TABLE OF gs_material.

CLEAR : gs_material, gt_material.

SELECT matnr ersda vpsta pstat a~mtart mtbez ntgew gewei
  INTO CORRESPONDING FIELDS OF TABLE gt_material
  FROM mara AS a
    INNER JOIN t134t AS b ON a~mtart = b~mtart
  WHERE spras EQ sy-langu.

*DATA : lang(10).
*CASE sy-langu.
*  WHEN 'E'.
*    lang = 'English'.
*  WHEN OTHERS.
*    lang = 'Others'.
*ENDCASE.
*
*LOOP AT gt_material INTO gs_material.
*  gs_material-langu = lang.
*  MODIFY gt_material FROM gs_material INDEX sy-tabix TRANSPORTING langu.
*ENDLOOP.

*cl_demo_output=>write( lang ).
cl_demo_output=>display( gt_material ).
