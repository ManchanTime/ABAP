``` abap
*&---------------------------------------------------------------------*
*& Include          SAPMZCL116_02F01
*&---------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*& Form display_screen_0100
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM display_screen_0100 .

  IF go_container IS NOT BOUND.

    CLEAR : gt_fcat, gs_fcat.
    PERFORM set_fcat USING : 'X' 'BUKRS' 'BKPF' 'C' ' ',
                             'X' 'BELNR' 'BKPF' 'C' ' ',
                             'X' 'GJAHR' 'BKPF' 'C' ' ',
                             ' ' 'BUZEI' 'BSEG' 'C' ' ',
                             ' ' 'HKONT' 'BSEG' ' ' ' ',
                             ' ' 'TXT50' 'SKAT' ' ' 'X',
                             ' ' 'BSCHL' 'BSEG' 'C' ' ',
                             ' ' 'KOART' 'BSEG' 'C' ' ',
                             ' ' 'SGTXT' 'BSEG' ' ' ' '.
    PERFORM set_layout.
    PERFORM create_object.

    CALL METHOD go_alv_grid->set_table_for_first_display
      EXPORTING
        is_variant      = gs_variant
        i_save          = 'A'
        i_default       = 'X'
        is_layout       = gs_layout
      CHANGING
        it_outtab       = gt_body
        it_fieldcatalog = gt_fcat.
  ENDIF.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form get_text_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM get_text_data .

  CLEAR gt_text.
  SELECT saknr txt50
    INTO CORRESPONDING FIELDS OF TABLE gt_text
    FROM skat
    WHERE spras = sy-langu
      AND ktopl = 'INT'.

  SORT gt_text BY saknr ASCENDING.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_text_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM set_text_data .

  DATA : lv_tabix TYPE sy-tabix.

  LOOP AT gt_body INTO gs_body.

    lv_tabix = sy-tabix.

    CLEAR gs_text.
    READ TABLE gt_text INTO gs_text WITH KEY saknr = gs_body-hkont.

    IF sy-subrc = 0.
      gs_body-txt50 = gs_text-txt50.
      MODIFY gt_body FROM gs_body INDEX lv_tabix TRANSPORTING txt50.
    ENDIF.

  ENDLOOP.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_fcat
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      --> P_
*&      --> P_
*&      --> P_
*&      --> P_
*&      --> P_
*&---------------------------------------------------------------------*
FORM set_fcat USING pv_key pv_field pv_table pv_just pv_emph.

  gs_fcat-key       = pv_key.
  gs_fcat-fieldname = pv_field.
  gs_fcat-ref_table = pv_table.
  gs_fcat-just      = pv_just.
  gs_fcat-emphasize = pv_emph.
  APPEND gs_fcat TO gt_fcat.
  CLEAR gs_fcat.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_layout
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM set_layout .

  gs_layout-zebra      = abap_true.
  gs_layout-cwidth_opt = 'A'.
  gs_layout-sel_mode   = 'D'.

  gs_variant-report = sy-repid.
  gs_variant-handle = 'ALV1'.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form create_object
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM create_object .
  CREATE OBJECT go_container
    EXPORTING
      container_name = 'MAIN_CONT'.

  CREATE OBJECT go_alv_grid
    EXPORTING
      i_parent = go_container.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_init_value
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM set_init_value .

  gv_bukrs = '0001'.
  gv_gjahr = '2025'.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_condition
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
*FORM set_condition .
*  LOOP AT SCREEN.
*    IF screen-group1 = 'LOC'.
*      screen-input = 0.
*    ENDIF.
*    MODIFY SCREEN.
*  ENDLOOP.
*ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_date_value
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM set_date_condition .

  _init gr_budat.

  IF ( gv_budat_fr IS NOT INITIAL ) AND
     ( gv_budat_to IS NOT INITIAL ).
    PERFORM set_date_value USING 'I' 'BT' gv_budat_fr gv_budat_to.
  ELSEIF ( gv_budat_fr IS NOT INITIAL ).
    PERFORM set_date_value USING 'I' 'EQ' gv_budat_fr gv_budat_to.
  ENDIF.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_date_value
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      --> P_
*&      --> P_
*&      --> GV_BUDAT_FR
*&      --> GV_BUDAT_TO
*&---------------------------------------------------------------------*
FORM set_date_value USING pv_sign pv_option pv_from pv_to.

  gr_budat-sign   = pv_sign.
  gr_budat-option = pv_option.
  gr_budat-low    = pv_from.
  gr_budat-high   = pv_to.
  APPEND gr_budat.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form refresh_table
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM refresh_table .

  DATA : ls_stabl TYPE lvc_s_stbl.

  ls_stabl-col = abap_true.
  ls_stabl-row = abap_true.

  CALL METHOD go_alv_grid->refresh_table_display
    EXPORTING
      is_stable = ls_stabl.

ENDFORM.
