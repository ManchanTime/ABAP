``` ABAP
@AbapCatalog.sqlViewName: 'ZVSFLIGHT16_01'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: '[C1] Sflight Data'
@Metadata.ignorePropagatedAnnotations: true
define view ZCDS_SFLIGHT16_01
  with parameters
    pa_carrid : s_carr_id,
    pa_connid : s_conn_id,
    pa_fldate : s_date
  as select from sflight
{
  key carrid     as Carrid,
  key connid     as Connid,
  key fldate     as Fldate,
      price      as Price,
      currency   as Currency,
      planetype  as Planetype,
      seatsmax   as Seatsmax,
      seatsocc   as Seatsocc,
      paymentsum as Paymentsum,
      seatsmax_b as SeatsmaxB,
      seatsocc_b as SeatsoccB,
      seatsmax_f as SeatsmaxF,
      seatsocc_f as SeatsoccF
}
where
      carrid = :pa_carrid
  and connid = :pa_connid
  and fldate = :pa_fldate
