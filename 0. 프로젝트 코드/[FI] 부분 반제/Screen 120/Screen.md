``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0120.
  MODULE init_process_control_0120.
  MODULE set_screen_control_0120.
*
PROCESS AFTER INPUT.
  CHAIN.
    FIELD : gv_bank, gv_accno, gs_clear_item-partner,
            gv_dwdate_fr, gv_dwdate_to.
    MODULE user_command_0120.
  ENDCHAIN.
  MODULE exit_0120 AT  EXIT-COMMAND.

PROCESS ON VALUE-REQUEST.

*-- 계좌 코드 Search Help 생성
  FIELD gv_accno MODULE cust_f4_accno.

*-- Partner Search Help 생성
  FIELD gs_clear_item-partner MODULE cust_f4_partner.
