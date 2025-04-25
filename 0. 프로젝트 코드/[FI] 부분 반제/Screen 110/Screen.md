``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0110.
  MODULE init_process_control_0110.
  MODULE set_screen_control_0110.
*
PROCESS AFTER INPUT.
  CHAIN.
    FIELD : gs_clear_item-saknr, gs_clear_item-bschl,
            gs_clear_item-augdt, gs_clear_item-wrbtr,
            gs_clear_item-waers, gs_clear_item-partner,
            gs_clear_item-sgtxt, gs_clear_header-blart,
            gs_clear_header-budat, gs_clear_header-bktxt.
    MODULE user_command_0110.
    MODULE exit_0110 AT EXIT-COMMAND.
  ENDCHAIN.

PROCESS ON VALUE-REQUEST.
*-- Blart Search Help 생성
  FIELD gs_clear_header-blart MODULE cust_f4_blart.
*-- Partner Search Help 생성
  FIELD gs_clear_item-partner MODULE cust_f4_partner.
*-- Saknr Search Help 생성
  FIELD gs_clear_item-saknr MODULE cust_f4_saknr.
