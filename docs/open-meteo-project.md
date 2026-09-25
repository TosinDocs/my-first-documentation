# Open Meteo API

## Overview
This endpoint returns hourly weather forecasts for 7 days, with forecasts available for up to 16 days.

## Endpoint
**Method;** `GET`

**URL;** `https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41`

**Endpoint;**  `/v1/forecast`

**Authentication;** Not Required

## Parameters

| Parameters | Format | Required | Description |
| --- | --- | --- | --- |
| `latitude`, `longitude` | floating point | Yes | Specifies the geographical location for the weather data. You can provide multiple locations by separating coordinates with commas. |
| `hourly` | String Array | No | Specifies the weather variables to return for each hour |
| `daily` | String Array | No | Specifies the weather information to return for each day. A `timezone` must also be provided when using this parameter |
| `current` | String Array | No | Specifies the weather variables to return for the current conditions |
| `temperature_unit` | String | No | Specifies the temperature unit. Defaults to `celcius`. Use `fahrenheit` to return temperatures in Fahrenheit. |
| `wind_speed_unit` | String | No | Specifies the wind speed unit. Options include ms, mph, and kn. |
| `timezone` | String | No | Specifies the timezone used for the returned timestamps. Use `auto` to automatically determine the timezone from the location. |
| `precipitation_unit` | String | No | Specifies the unit used for precipitation. Use inch for inches. |
| `past_days` | Integer (0–92) | No | Specifies how many days of past weather data to include. |
|  `forecast_days` | Integer (0–16) | No | Specifies how many days of forecast data to return. The default is 7 days, and up to 16 days can be requested. |

## Request Example

### Single Location Forecast 

 `GET https://api.open-meteo.com/v1/forecast?latitude=6.4654&longitude=3.4064&current=temperature_2m`

**Successful Response;** 
```json
{"latitude":6.4323373,
"longitude":3.3948028,
"generationtime_ms":0.031232833862304688,
"utc_offset_seconds":0,
"timezone":"GMT",
"timezone_abbreviation":"GMT",
"elevation":0.0,
"current_units":{
    "time":"iso8601",
    "interval":"seconds",
    "temperature_2m":"°C"
    },
"current":{
    "time":"2026-09-23T15:15",
    "interval":900,
    "temperature_2m":26.3
    }
}
```

### Multiple Location Request

`GET https://api.open-meteo.com/v1/forecast?latitude=6.4654,9.072264&longitude=3.4064,7.491302&current=temperature_2m`

**Successful Response;** 
```json
{
    {
        "latitude": 6.4323373,
        "longitude": 3.3948028,
        "generationtime_ms": 0.0011920928955078125,
        "utc_offset_seconds": 0,
        "timezone": "GMT",
        "timezone_abbreviation": "GMT",
        "elevation": 0.0
    },
    {
        "latitude": 9.103691,
        "longitude": 7.4805193,
        "generationtime_ms": 0.000476837158203125,
        "utc_offset_seconds": 0,
        "timezone": "GMT",
        "timezone_abbreviation": "GMT",
        "elevation": 492.0,
        "location_id": 1
    }
}
```

## Unsuccessful Response

**Invalid Latitude Request;** 
`GET https://api.open-meteo.com/v1/forecast?latitude=999&longitude=3.4064&current=temperature_2m`

**Response;**
>503 Service Unavailable; 
The server is down (maybe for maintenance or maybe overloaded).
``` json
{
    "error": true,
    "reason": "The service is overloaded"
}
```

**URL Parameter not correctly specified;**
`GET https://api.open-meteo.com/v1/forecast?latitude=9.99&longitude=3.4064&current=tem`

**Response;**
> 400 Bad Request; 
The server could not understand the request. Maybe a bad syntax?
``` json
{
    "reason": "Invalid value: Cannot initialize SurfacePressureAndHeightVariable<VariableAndPreviousDay, VariableOrSpread<ForecastPressureVariable>, ForecastHeightVariable> from invalid String value tem",
    "error": true
}
```

## Important Notes

- Always enter valid `latitude` and `longitude` values to receive accurate weather data.

- Make sure parameter names and values are correctly specified.

- `forecast_days` returns 7 days by default and can be increased to a maximum of 16 days.

- When using `daily` weather variables, you must also specify a `timezone`.

- Use `temperature_unit`, `wind_speed_unit`, and `precipitation_unit` to change the units returned by the API.