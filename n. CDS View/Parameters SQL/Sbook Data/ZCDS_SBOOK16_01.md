``` abap
@AbapCatalog.sqlViewName: 'ZVSBOOK16_01'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: '[C1] Sbook Data'
@Metadata.ignorePropagatedAnnotations: true
define view ZCDS_SBOOK16_01
  with parameters
    pa_carrid : s_carr_id,
    pa_connid : s_conn_id,
    pa_fldate : s_date
  as select from sbook
{
  key carrid,
  key connid,
  key fldate,
  key bookid,
      customid,
      custtype,
      case custtype
          when 'P' then 'Private Customer'
          when 'B' then 'Business Customer'
          else 'Nothing'
      end as custtype_desc,
      luggweight,
      wunit,
      class,
      forcuram,
      forcurkey
}
where carrid = :pa_carrid
  and connid = :pa_connid
  and fldate = :pa_fldate
