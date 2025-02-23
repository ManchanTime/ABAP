``` abap
*&---------------------------------------------------------------------*
*& Include          SAPMZCL116_03I01
*&---------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*&      Module  EXIT  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE exit INPUT.
  CALL METHOD : go_bot_rig_grid->free, go_bot_lef_grid->free, go_top_grid->free,
                go_bot_lef_cont->free, go_bot_rig_cont->free, go_split_bot_cont->free,
                go_bot_cont->free, go_top_cont->free, go_split_cont->free,
                go_main_cont->free.

  FREE : go_bot_rig_grid, go_bot_lef_grid, go_top_grid,
                go_bot_lef_cont, go_bot_rig_cont, go_split_bot_cont,
                go_bot_cont, go_top_cont, go_split_cont,
                go_main_cont.

  LEAVE TO SCREEN 0.

ENDMODULE.
*&---------------------------------------------------------------------*
*&      Module  USER_COMMAND_0100  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE user_command_0100 INPUT.

  CASE gv_okcode.
    WHEN 'SRCH'.
      PERFORM search_data.
    WHEN 'RFSH'.
      PERFORM set_empty.
  ENDCASE.

  PERFORM refresh_all.
*  PERFORM refresh_table_top.
*  PERFORM refresh_table_bot_left.
*  PERFORM refresh_table_bot_right.
ENDMODULE.
