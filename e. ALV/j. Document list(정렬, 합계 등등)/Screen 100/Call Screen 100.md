``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0100.
  MODULE initial_screen.
*
PROCESS AFTER INPUT.
  MODULE exit AT EXIT-COMMAND.
*  MODULE user_command_0100.
