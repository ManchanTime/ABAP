``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0100.
  MODULE init_process_control.
  MODULE init_condition.
*
PROCESS AFTER INPUT.
  MODULE exit AT EXIT-COMMAND.
  MODULE user_command_0100.
