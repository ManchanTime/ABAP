``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0100.
  MODULE initial_process_control.
*
PROCESS AFTER INPUT.
  MODULE exit AT EXIT-COMMAND.
  MODULE user_command_0100.
