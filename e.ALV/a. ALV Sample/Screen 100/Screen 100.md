``` abap
PROCESS BEFORE OUTPUT.
  MODULE status_0100.  " Screen의 Menu와 Title 설정
  MODULE init_process_control.

PROCESS AFTER INPUT.
  MODULE exit AT EXIT-COMMAND.
* MODULE USER_COMMAND_0100.
