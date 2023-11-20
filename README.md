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


select COALESCE(location,weather1.station_id,'?') || ' ' || cast(adsb.lat as string) || ',' || cast(adsb.lon as string) as Location, 
       COALESCE(adsb.flight,'-','-') || ' ' || COALESCE(adsb.hex, '-','-') as FlightNum, 
       cast(adsb.alt_baro  as string) || ' ' ||  cast(adsb.alt_geom as string) as Altitude, 
       adsb.gs as Speed,
       weather1.temperature_string || weather1.weather as Weather, 
       adsb.mach,  adsb.baro_rate, adsb.nav_heading,
       adsb.squawk, adsb.category,  weather1.observation_time, 
  weather1.temperature_string, weather1.wind_string, weather1.dewpoint_string
FROM `schemareg1`.`default_database`.`adsb` ,  weather1 
WHERE adsb.flight is not null
AND (adsb.lat > weather1.latitude - 0.1) 
and (adsb.lat < weather1.latitude + 0.1)
and (adsb.lon < weather1.longitude + 0.1) 
and (adsb.lon > weather1.longitude - 0.1)

select COALESCE(location,aircraftweather.station_id,'?') || ' ' || cast(adsb.lat as string) || ',' || cast(adsb.lon as string) as Location, 
       COALESCE(adsb.flight,'-','-') || ' ' || COALESCE(adsb.hex, '-','-') as FlightNum, 
       cast(adsb.alt_baro  as string) || ' ' ||  cast(adsb.alt_geom as string) as Altitude, 
       adsb.gs as Speed,
       aircraftweather.temperature_string || aircraftweather.weather as Weather, 
       adsb.mach,  adsb.baro_rate, adsb.nav_heading,
       adsb.squawk, adsb.category,  aircraftweather.observation_time, 
  aircraftweather.temperature_string, aircraftweather.wind_string, aircraftweather.dewpoint_string
FROM `schemareg1`.`default_database`.`adsb` ,  aircraftweather 
WHERE adsb.flight is not null
AND (adsb.lat > aircraftweather.latitude - 0.3) 
and (adsb.lat < aircraftweather.latitude + 0.3)
and (adsb.lon < aircraftweather.longitude + 0.3) 
and (adsb.lon > aircraftweather.longitude - 0.3)




CREATE TABLE `ssb`.`Meetups`.`aircraftweather` (
  `uuid` VARCHAR(2147483647),
  `ts` BIGINT,
  `credit` VARCHAR(2147483647),
  `credit_URL` VARCHAR(2147483647),
  `image` ROW<`url` VARCHAR(2147483647), `title` VARCHAR(2147483647), `link` VARCHAR(2147483647)>,
  `suggested_pickup` VARCHAR(2147483647),
  `suggested_pickup_period` BIGINT,
  `location` VARCHAR(2147483647),
  `station_id` VARCHAR(2147483647),
  `latitude` DOUBLE,
  `longitude` DOUBLE,
  `observation_time` VARCHAR(2147483647),
  `observation_time_rfc822` VARCHAR(2147483647),
  `weather` VARCHAR(2147483647),
  `temperature_string` VARCHAR(2147483647),
  `temp_f` DOUBLE,
  `temp_c` DOUBLE,
  `relative_humidity` BIGINT,
  `wind_string` VARCHAR(2147483647),
  `wind_dir` VARCHAR(2147483647),
  `wind_degrees` BIGINT,
  `wind_mph` DOUBLE,
  `wind_kt` BIGINT,
  `pressure_in` DOUBLE,
  `dewpoint_string` VARCHAR(2147483647),
  `dewpoint_f` DOUBLE,
  `dewpoint_c` DOUBLE,
  `visibility_mi` DOUBLE,
  `icon_url_base` VARCHAR(2147483647),
  `two_day_history_url` VARCHAR(2147483647),
  `icon_url_name` VARCHAR(2147483647),
  `ob_url` VARCHAR(2147483647),
  `disclaimer_url` VARCHAR(2147483647),
  `copyright_url` VARCHAR(2147483647),
  `privacy_policy_url` VARCHAR(2147483647),
  `eventTimeStamp` TIMESTAMP(3) WITH LOCAL TIME ZONE METADATA FROM 'timestamp',
  WATERMARK FOR `eventTimeStamp` AS `eventTimeStamp` - INTERVAL '3' SECOND
) WITH (
  'deserialization.failure.policy' = 'ignore_and_log',
  'properties.request.timeout.ms' = '120000',
  'format' = 'json',
  'properties.bootstrap.servers' = 'kafka:9092',
  'connector' = 'kafka',
  'properties.transaction.timeout.ms' = '900000',
  'topic' = 'weather',
  'scan.startup.mode' = 'group-offsets',
  'properties.auto.offset.reset' = 'earliest',
  'properties.group.id' = 'aircraftweatherflink'
)



select COALESCE(location,aircraftweather.station_id,'?') || ' ' || cast(adsb.lat as string) || ',' || cast(adsb.lon as string) as Location, 
       COALESCE(adsb.flight,'-','-') || ' ' || COALESCE(adsb.hex, '-','-') as FlightNum, 
       cast(adsb.alt_baro  as string) || '|' ||  cast(adsb.alt_geom as string) as Altitude, 
       adsb.gs as Speed,
       aircraftweather.temperature_string || aircraftweather.weather as Weather, 
       adsb.mach,  adsb.baro_rate, adsb.nav_heading,
       adsb.squawk, adsb.category,  aircraftweather.observation_time, 
  aircraftweather.temperature_string, aircraftweather.wind_string, aircraftweather.dewpoint_string
FROM `schemareg1`.`default_database`.`adsb` ,  aircraftweather 
WHERE adsb.flight is not null
AND (adsb.lat > aircraftweather.latitude - 0.3) 
and (adsb.lat < aircraftweather.latitude + 0.3)
and (adsb.lon < aircraftweather.longitude + 0.3) 
and (adsb.lon > aircraftweather.longitude - 0.3)





select COALESCE(location,aircraftweather.station_id,'?') || ' ' || cast(adsb.lat as string) || ',' || cast(adsb.lon as string) as Location, 
       COALESCE(adsb.flight,'-','-') || ' ' || COALESCE(adsb.hex, '-','-') as FlightNum, 
       cast(adsb.alt_baro  as string) || '|' ||  cast(adsb.alt_geom as string) as Altitude, 
       adsb.gs as Speed,
       aircraftweather.temperature_string || aircraftweather.weather as Weather, 
       adsb.mach,  adsb.baro_rate, adsb.nav_heading,
       adsb.squawk, adsb.category,  aircraftweather.observation_time, 
  aircraftweather.temperature_string, aircraftweather.wind_string, aircraftweather.dewpoint_string,
  (adsb.uuid || '-' || aircraftweather.uuid || '-' || adsb.flight || '-' || cast(adsb.lat as string) || '-' || cast(adsb.lon as string) ) as jointkey
FROM `schemareg1`.`default_database`.`adsb` ,  aircraftweather    
WHERE adsb.flight is not null
AND (adsb.lat > aircraftweather.latitude - 0.3) 
and (adsb.lat < aircraftweather.latitude + 0.3)
and (adsb.lon < aircraftweather.longitude + 0.3) 
and (adsb.lon > aircraftweather.longitude - 0.3)

````



### Reference

* https://github.com/tspannhw/pulsar-weather-function
* https://github.com/tspannhw/FLaNK-ADSB
