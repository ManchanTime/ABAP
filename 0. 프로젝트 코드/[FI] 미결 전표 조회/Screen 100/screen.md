``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0100.
  MODULE init_process_control.
*
PROCESS AFTER INPUT.
  CHAIN.
    FIELD: gv_belnr, gv_budat_fr, gv_budat_to.
    MODULE user_command_0100.
  ENDCHAIN.
  MODULE exit AT EXIT-COMMAND.
