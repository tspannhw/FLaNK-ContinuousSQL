# FLaNK-ContinuousSQL



#### SQL

````
select `schemareg1`.`default_database`.`adsb`.hex as ICAO, 
    `schemareg1`.`default_database`.`adsb`.flight as IDENT,
    adsb.baro_rate, adsb.category, adsb.mach, adsb.nav_heading,
    adsb.squawk, 
  adsb.alt_baro as altitudefeet,
  adsb.alt_geom as gaaltitudefeet,
  adsb.gs as groundspeed,
  weather1.longitude, weather1.latitude, weather1.observation_time, 
  weather1.temperature_string, weather1.wind_string, weather1.dewpoint_string, weather1.weather
from `schemareg1`.`default_database`.`adsb`  LEFT JOIN
weather1 on `schemareg1`.`default_database`.`adsb`.lat = weather1.latitude
and adsb.lon = weather1.longitude
and adsb.flight is not null



select max(alt_baro) as MaxAltitudeFeet, min(alt_baro) as MinAltitudeFeet, avg(alt_baro) as AvgAltitudeFeet,
       max(alt_geom) as MaxGAltitudeFeet, min(alt_geom) as MinGAltitudeFeet, avg(alt_geom) as AvgGAltitudeFeet,
       max(gs) as MaxGroundSpeed, min(gs) as MinGroundSpeed, avg(gs) as AvgGroundSpeed, 
       count(alt_baro) as RowCount, 
       hex as ICAO, flight as IDENT
from `schemareg1`.`default_database`.`adsb` 
where flight is not null 
group by flight, hex;


select COALESCE(location,weather1.station_id,'?') || ' ' || cast(adsb.lat as string) || ',' || cast(adsb.lon as string) as Location, 
       COALESCE(adsb.flight,'-','-') || ' ' || COALESCE(adsb.hex, '-','-') as FlightNum, 
       cast(adsb.alt_baro  as string) || ' ' ||  cast(adsb.alt_geom as string) as Altitude, 
       adsb.gs as Speed,
       weather1.temperature_string || weather1.weather as Weather, 
       adsb.mach,  adsb.baro_rate, adsb.nav_heading,
       adsb.squawk, adsb.category,  weather1.observation_time, 
  weather1.temperature_string, weather1.wind_string, weather1.dewpoint_string
FROM `schemareg1`.`default_database`.`adsb`  
     LEFT JOIN weather1 
    ON adsb.lat = weather1.latitude
      and adsb.lon = weather1.longitude
WHERE adsb.flight is not null


````



### Reference

* https://github.com/tspannhw/pulsar-weather-function
* https://github.com/tspannhw/FLaNK-ADSB
