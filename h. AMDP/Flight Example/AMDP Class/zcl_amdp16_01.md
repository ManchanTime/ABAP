``` abap
CLASS zcl_amdp16_01 DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    INTERFACES if_amdp_marker_hdb.
    TYPES : BEGIN OF ts_schedule,
              carrid    TYPE spfli-carrid,
              connid    TYPE spfli-connid,
              countryfr TYPE spfli-countryfr,
              airpfrom  TYPE spfli-airpfrom,
              cityfrom  TYPE spfli-cityfrom,
              countryto TYPE spfli-countryto,
              airpto    TYPE spfli-airpto,
              cityto    TYPE spfli-cityto,
            END OF ts_schedule,
            tt_schedule TYPE TABLE OF ts_schedule.
            
    TYPES : BEGIN OF ts_flight,
              carrid     TYPE sflight-carrid,
              connid     TYPE sflight-connid,
              fldate     TYPE sflight-fldate,
              seatsmax   TYPE sflight-seatsmax,
              seatsocc   TYPE sflight-seatsocc,
              currency   TYPE sflight-currency,
              paymentsum TYPE sflight-paymentsum,
            END OF ts_flight,
            tt_flight TYPE TABLE OF ts_flight.

    TYPES : BEGIN OF ts_custom,
              id   TYPE scustom-id,
              name TYPE scustom-name,
            END OF ts_custom,
            tt_custom TYPE TABLE OF ts_custom WITH DEFAULT KEY, " For DATABASE FUNCTION
            tt_sbook  TYPE TABLE OF sbook.                      " FOR Result data

    CLASS-METHODS : get_schedule IMPORTING VALUE(iv_carrid)   TYPE spfli-carrid
                                 EXPORTING VALUE(et_schedule) TYPE tt_schedule,

                    get_flight   IMPORTING VALUE(iv_carrid)   TYPE s_carr_id
                                           VALUE(iv_connid)   TYPE s_conn_id
                                 EXPORTING VALUE(et_flight)   TYPE tt_flight,

                    get_cust     IMPORTING VALUE(iv_name)     TYPE s_custname
                                 RETURNING VALUE(re_cust)     TYPE tt_custom,

                    get_book     IMPORTING VALUE(iv_name)     TYPE scustom-name
                                 EXPORTING VALUE(et_sbook)    TYPE tt_sbook.

ENDCLASS.

CLASS zcl_amdp16_01 IMPLEMENTATION.

  METHOD get_schedule BY DATABASE PROCEDURE FOR HDB LANGUAGE SQLSCRIPT
                      USING spfli.

    et_schedule = SELECT carrid, connid, countryfr, airpfrom, cityfrom, countryto, airpto, cityto
                    FROM spfli
                    WHERE mandt  = session_context( 'CLIENT' )
                      AND carrid = :iv_carrid;
  ENDMETHOD.

  METHOD get_flight BY DATABASE PROCEDURE FOR HDB LANGUAGE SQLSCRIPT
                    USING sflight.

    et_flight = SELECT carrid, connid, fldate, seatsmax, seatsocc, currency, paymentsum
                  FROM sflight
                  WHERE mandt = session_context( 'CLIENT' )
                   AND carrid = :iv_carrid
                   AND connid = :iv_connid;
  ENDMETHOD.


  METHOD get_cust BY DATABASE FUNCTION FOR HDB LANGUAGE SQLSCRIPT    " Function 호출
                  OPTIONS READ-ONLY
                  USING scustom.

    RETURN SELECT id, name
            FROM scustom
            WHERE mandt = session_context( 'CLIENT' )
* FUZZY -> 모든 경우의 수에 대해 검색하겠다(앞이든 뒤든 가운데든 암데든)
              AND CONTAINS( name, :iv_name, FUZZY);
  ENDMETHOD.

  METHOD get_book BY DATABASE PROCEDURE FOR HDB LANGUAGE SQLSCRIPT
                  USING sbook zcl_amdp16_01=>get_cust.

* 다른 메소드를 SQL에 사용할 시 반드시 대문자로 해줘야 함!!!!!
     et_sbook = SELECT *
                  FROM sbook
                 WHERE mandt = session_context( 'CLIENT' )
                   AND customid IN ( SELECT id FROM "ZCL_AMDP16_01=>GET_CUST"( iv_name => :iv_name ) );

  ENDMETHOD.
ENDCLASS.
