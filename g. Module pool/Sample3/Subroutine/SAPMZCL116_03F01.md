``` abap
*&---------------------------------------------------------------------*
*& Include          SAPMZCL116_03F01
*&---------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*& Form display_screen
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM display_screen .

  IF go_main_cont IS NOT BOUND.

    CLEAR : gt_tfcat, gs_tfact.
    PERFORM set_fcat TABLES gt_tfcat
                     USING : 'X' 'CARRID' 'SCARR' 'C' ' '
                             CHANGING gs_tfact,
                             ' ' 'CARRNAME' 'SCARR' ' ' 'X'
                             CHANGING gs_tfact,
                             ' ' 'CURRCODE' 'SCARR' 'C' ' '
                             CHANGING gs_tfact,
                             ' ' 'URL' 'SCARR' ' ' ' '
                             CHANGING gs_tfact.

    PERFORM set_fcat TABLES gt_blfcat
                     USING : 'X' 'CARRID'   'SPFLI' 'C' ' '
                             CHANGING gs_blfcat,
                             'X' 'CONNID'   'SPFLI' ' ' ' '
                             CHANGING gs_blfcat,
                             ' ' 'AIRPFROM' 'SPFLI' 'C' ' '
                             CHANGING gs_blfcat,
                             ' ' 'AIRPTO'   'SPFLI' 'C' ' '
                             CHANGING gs_blfcat,
                             ' ' 'DEPTIME'  'SPFLI' 'C' ' '
                             CHANGING gs_blfcat,
                             ' ' 'ARRTIME'  'SPFLI' 'C' ' '
                             CHANGING gs_blfcat.

    PERFORM set_fcat TABLES gt_brfcat
                 USING : 'X' 'CARRID'      'ZVSFLIGHT16_CDS' 'C' ' '
                         CHANGING gs_brfcat,
                         'X' 'CONNID'      'ZVSFLIGHT16_CDS' 'C' ' '
                         CHANGING gs_brfcat,
                         ' ' 'FLDATE'      'ZVSFLIGHT16_CDS' 'C' ' '
                         CHANGING gs_brfcat,
                         ' ' 'PRICE'       'ZVSFLIGHT16_CDS' ' ' ' '
                         CHANGING gs_brfcat,
                         ' ' 'CURRENCY'    'ZVSFLIGHT16_CDS' 'C' ' '
                         CHANGING gs_brfcat,
                         ' ' 'SEATSMAX'    'ZVSFLIGHT16_CDS' ' ' ' '
                         CHANGING gs_brfcat,
                         ' ' 'SEATSOCC'    'ZVSFLIGHT16_CDS' ' ' ' '
                         CHANGING gs_brfcat,
                         ' ' 'SEATSEMPTY'  'ZVSFLIGHT16_CDS' ' ' ' '
                         CHANGING gs_brfcat.

    PERFORM set_layout.

    PERFORM create_object.

    CALL METHOD go_top_grid->set_table_for_first_display
      EXPORTING
        is_variant      = gs_variant
        i_save          = 'A'
        i_default       = 'X'
        is_layout       = gs_layout
      CHANGING
        it_outtab       = gt_scarr
        it_fieldcatalog = gt_tfcat.


    SET HANDLER lcl_event_handler=>double_click FOR go_bot_lef_grid.
    CALL METHOD go_bot_lef_grid->set_table_for_first_display
      EXPORTING
        is_variant      = gs_variant
        i_save          = 'A'
        i_default       = 'X'
        is_layout       = gs_layout
      CHANGING
        it_outtab       = gt_spfli
        it_fieldcatalog = gt_blfcat.

    CALL METHOD go_bot_rig_grid->set_table_for_first_display
      EXPORTING
        is_variant      = gs_variant
        i_save          = 'A'
        i_default       = 'X'
        is_layout       = gs_layout
      CHANGING
        it_outtab       = gt_sflight
        it_fieldcatalog = gt_brfcat.

  ENDIF.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form get_scarr_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM get_scarr_data .

  CLEAR gt_scarr_store.
  SELECT carrid carrname currcode url
    INTO CORRESPONDING FIELDS OF TABLE gt_scarr_store
    FROM scarr.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form get_spfli_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM get_spfli_data .

  CLEAR gt_spfli_store.
  SELECT carrid connid airpfrom airpto deptime arrtime
    INTO CORRESPONDING FIELDS OF TABLE gt_spfli_store
    FROM spfli.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form get_base_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM get_base_data .
  PERFORM get_scarr_data.
  PERFORM get_spfli_data.
  PERFORM get_sflight_data.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form SET_FCAT
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      --> GT_SCARR
*&      --> P_
*&      --> P_
*&      --> P_
*&      --> P_
*&      --> P_
*&      <-- GS_TFACT
*&---------------------------------------------------------------------*
FORM set_fcat TABLES pt_fcat TYPE lvc_t_fcat
              USING pv_key pv_field pv_table pv_just pv_emph
              CHANGING ps_fcat TYPE lvc_s_fcat.

  ps_fcat-key       = pv_key.
  ps_fcat-fieldname = pv_field.
  ps_fcat-ref_table = pv_table.
  ps_fcat-just      = pv_just.
  ps_fcat-emphasize = pv_emph.

  CASE pv_field.
    WHEN 'SEATSEMPTY'.
      ps_fcat-coltext = 'Remain'.
    WHEN 'PRICE'.
      ps_fcat-cfieldname = 'CURRENCY'.
  ENDCASE.

  APPEND ps_fcat TO pt_fcat.
  CLEAR ps_fcat.

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

*-- Main cont
  CREATE OBJECT go_main_cont
    EXPORTING
      container_name = 'MAIN_CONT'.

*-- Splitter cont
  CREATE OBJECT go_split_cont
    EXPORTING
      parent  = go_main_cont
      rows    = 2
      columns = 1.

*-- Top cont
  CALL METHOD go_split_cont->get_container
    EXPORTING
      row       = 1
      column    = 1
    RECEIVING
      container = go_top_cont.

*-- Bot cont
  CALL METHOD go_split_cont->get_container
    EXPORTING
      row       = 2
      column    = 1
    RECEIVING
      container = go_bot_cont.

*-- Bot Splitter cont
  CREATE OBJECT go_split_bot_cont
    EXPORTING
      parent  = go_bot_cont
      rows    = 1
      columns = 2.

*-- Bot Left Cont
  CALL METHOD go_split_bot_cont->get_container
    EXPORTING
      row       = 1
      column    = 1
    RECEIVING
      container = go_bot_lef_cont.

*-- Bot Right Cont
  CALL METHOD go_split_bot_cont->get_container
    EXPORTING
      row       = 1
      column    = 2
    RECEIVING
      container = go_bot_rig_cont.

*-- Set Top grid
  CREATE OBJECT go_top_grid
    EXPORTING
      i_parent = go_top_cont.

*-- Set Bot Left grid
  CREATE OBJECT go_bot_lef_grid
    EXPORTING
      i_parent = go_bot_lef_cont.

*-- Set Bot Right grid
  CREATE OBJECT go_bot_rig_grid
    EXPORTING
      i_parent = go_bot_rig_cont.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form search_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM search_data .

*-- Get scarr data
  CLEAR gt_scarr.
  LOOP AT gt_scarr_store INTO gs_scarr_store WHERE carrid = gv_carrid.

    MOVE-CORRESPONDING gs_scarr_store TO gs_scarr.
    APPEND gs_scarr TO gt_scarr.

  ENDLOOP.

*-- Get spfli data
  CLEAR gt_spfli.
  LOOP AT gt_spfli_store INTO gs_spfli_store WHERE carrid = gv_carrid.

    MOVE-CORRESPONDING gs_spfli_store TO gs_spfli.
    APPEND gs_spfli TO gt_spfli.

  ENDLOOP.

*-- Get sflight data

ENDFORM.
*&---------------------------------------------------------------------*
*& Form refresh_table_top
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM refresh_table_top .

  DATA : ls_stbl TYPE lvc_s_stbl.

  ls_stbl-row = abap_true.
  ls_stbl-col = abap_true.

  go_top_grid->refresh_table_display( EXPORTING is_stable = ls_stbl ).

ENDFORM.
*&---------------------------------------------------------------------*
*& Form refresh_table_bot_left
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM refresh_table_bot_left .

  DATA : ls_stbl TYPE lvc_s_stbl.

  ls_stbl-row = abap_true.
  ls_stbl-col = abap_true.

  go_bot_lef_grid->refresh_table_display( EXPORTING is_stable = ls_stbl ).

ENDFORM.
*&---------------------------------------------------------------------*
*& Form refresh_table_bot_right
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM refresh_table_bot_right .

  DATA : ls_stbl TYPE lvc_s_stbl.

  ls_stbl-row = abap_true.
  ls_stbl-col = abap_true.

  go_bot_rig_grid->refresh_table_display( EXPORTING is_stable = ls_stbl ).

ENDFORM.
*&---------------------------------------------------------------------*
*& Form handle_double_click
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      --> E_COLUMN
*&      --> E_ROW
*&---------------------------------------------------------------------*
FORM handle_double_click USING pv_column pv_row.

  CLEAR gs_spfli.
  READ TABLE gt_spfli INTO gs_spfli INDEX pv_row.

  PERFORM search_sflight_data.
  PERFORM refresh_table_bot_right.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form search_sflight_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM search_sflight_data .

  CLEAR gt_sflight.
  LOOP AT gt_sflight_store INTO gs_sflight_store
                           WHERE carrid = gs_spfli-carrid
                             AND connid = gs_spfli-connid.

    MOVE-CORRESPONDING gs_sflight_store TO gs_sflight.
    APPEND gs_sflight TO gt_sflight.

  ENDLOOP.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form set_empty
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM set_empty .

  CLEAR : gt_scarr, gt_spfli, gt_sflight, gv_carrid.

ENDFORM.
*&---------------------------------------------------------------------*
*& Form get_sflight_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM get_sflight_data .

  CLEAR gt_sflight_store.

  SELECT carrid, connid, fldate, price, currency,
         seatsmax, seatsocc, seatsempty
    FROM zvsflight16_cds
    INTO CORRESPONDING FIELDS OF TABLE @gt_sflight_store.
ENDFORM.
*&---------------------------------------------------------------------*
*& Form refresh_all
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM refresh_all .

  DATA : ls_stbl TYPE lvc_s_stbl.

  ls_stbl-row = abap_true.
  ls_stbl-col = abap_true.

  go_bot_lef_grid->refresh_table_display( EXPORTING is_stable = ls_stbl ).
  go_bot_rig_grid->refresh_table_display( EXPORTING is_stable = ls_stbl ).
  go_top_grid->refresh_table_display( EXPORTING is_stable = ls_stbl ).
ENDFORM.
