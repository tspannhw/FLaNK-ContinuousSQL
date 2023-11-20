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



````
