``` abap
CLASS zcl_amdp16_02 DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES  if_amdp_marker_hdb.

    TYPES : BEGIN OF ts_docu,
              bukrs TYPE bkpf-bukrs,
              belnr TYPE bkpf-belnr,
              gjahr TYPE bkpf-gjahr,
              budat TYPE bkpf-budat,
              blart TYPE bkpf-blart,
              bktxt TYPE bkpf-bktxt,
              waers TYPE bkpf-waers,
            END OF ts_docu,
            tt_docu TYPE TABLE OF ts_docu.

    CLASS-METHODS : get_bkpf IMPORTING VALUE(iv_bukrs) TYPE bukrs
                                       VALUE(iv_gjahr) TYPE gjahr
                             EXPORTING VALUE(et_docu)  TYPE tt_docu.

ENDCLASS.



CLASS zcl_amdp16_02 IMPLEMENTATION.

    METHOD get_bkpf BY DATABASE PROCEDURE FOR HDB LANGUAGE SQLSCRIPT
                    USING bkpf.

        et_docu = SELECT bukrs, belnr, gjahr, budat, blart, bktxt, waers
                    FROM bkpf
                    WHERE mandt = session_context( 'CLIENT' )
                      AND bukrs = iv_bukrs
                      AND gjahr = iv_gjahr;

    ENDMETHOD.

ENDCLASS.
