``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0130.
*
PROCESS AFTER INPUT.
  MODULE exit_0130 AT EXIT-COMMAND.
  MODULE user_command_0130.
