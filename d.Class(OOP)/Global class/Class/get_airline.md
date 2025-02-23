``` abap
  METHOD get_airline.

    SELECT SINGLE carrid carrname currcode url
      INTO CORRESPONDING FIELDS OF es_scarr
      FROM scarr
      WHERE carrid = iv_carrid.

    IF es_scarr IS INITIAL.
      ev_message = TEXT-e01.
    ENDIF.
  ENDMETHOD.
