``` abap
  METHOD get_schedule.

    SELECT carrid connid countryfr cityfrom countryto cityto
      INTO CORRESPONDING FIELDS OF TABLE et_spfli
      FROM spfli
      WHERE carrid = iv_carrid.

    IF et_spfli IS INITIAL.
      ev_message = TEXT-e01.
    ENDIF.

  ENDMETHOD.
