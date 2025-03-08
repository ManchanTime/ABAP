``` abap
@AbapCatalog.sqlViewName: 'ZVSCHEDULE16_01'
@AbapCatalog.compiler.compareFilter: true
@AbapCatalog.preserveKey: true
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Air schedule OData'
@Metadata.ignorePropagatedAnnotations: true
// Begin of OData
@OData.entitySet.name: 'ScheduleSet'
@OData.publish: true
// End of OData
define view ZCDS_SCHEDULE16_01
  as select from scarr as a
    inner join   spfli as b on a.carrid = b.carrid
{
  key a.carrid    as Carrid,
  key b.connid    as Connid,
      a.carrname  as Carrname,
      a.currcode  as Currcode,
      a.url       as Url,
      b.countryfr as Countryfr,
      b.cityfrom  as Cityfrom,
      b.airpfrom  as Airpfrom,
      b.countryto as Countryto,
      b.cityto    as Cityto,
      b.airpto    as Airpto,
      b.deptime   as Deptime,
      b.arrtime   as Arrtime,
      b.distance  as Distance,
      b.distid    as Distid,
      b.fltype    as Fltype,
      b.period    as Period
}
