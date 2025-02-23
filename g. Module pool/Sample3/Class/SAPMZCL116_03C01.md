``` abap
*&---------------------------------------------------------------------*
*& Include          SAPMZCL116_03C01
*&---------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*& Class LCL_EVENT_HANDLER
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
CLASS lcl_event_handler DEFINITION FINAL.

  PUBLIC SECTION.
    CLASS-METHODS : double_click FOR EVENT double_click
                                 OF cl_gui_alv_grid
                                 IMPORTING e_column e_row.

ENDCLASS.
*&---------------------------------------------------------------------*
*& Class (Implementation) LCL_EVENT_HANDLER
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
CLASS lcl_event_handler IMPLEMENTATION.

  METHOD double_click.
    PERFORM handle_double_click USING e_column e_row.
  ENDMETHOD.

ENDCLASS.
