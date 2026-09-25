---
sidebar_position: 3
---

# How to Retrieve Historical Weather Data Using the Open-Meteo API

## Overview

This tutorial walks you through retrieving historical weather data using the Open-Meteo API.

We'll retrieve hourly temperature data for Lagos, Nigeria, for a specific date range and examine the response returned by the API.

## Prerequisites

Before you begin, you will need:

- An internet connection
- Postman or another tool for sending API requests
- The latitude and longitude of your chosen location
- A start date and end date for the historical data you want to retrieve

No API key or authentication is required.

## 1. Choose a Location

The Open-Meteo API uses latitude and longitude to identify locations.

For this tutorial, we'll use Lagos, Nigeria:

| Parameter | Value |
|---|---|
| Latitude | `6.4654` |
| Longitude | `3.4064` |

These coordinates will be included in the API request.

## 2. Choose a Date Range

Next, choose the period for which you want to retrieve historical weather data.

For this example, we'll retrieve data from January 1 to January 7, 2025.

| Parameter | Value |
|---|---|
| Start date | `2025-01-01` |
| End date | `2025-01-07` |

The dates must use the `YYYY-MM-DD` format.

## 3. Select the Weather Data

The `hourly` parameter determines which weather variables the API returns for each hour.

For this example, we'll retrieve the hourly temperature at 2 metres above ground:

```text
hourly=temperature_2m
```

## 4. Build the API Request

#### Endpoint

The Open-Meteo historical weather endpoint is:

```text
https://archive-api.open-meteo.com/v1/archive
```
#### Query Parameters

Add the location, date range, and weather variable as query parameters:

```text
https://archive-api.open-meteo.com/v1/archive?latitude=6.4654&longitude=3.4064&start_date=2025-01-01&end_date=2025-01-07&hourly=temperature_2m
```
The request contains the following parameters;

| Parameter    | Value            | Purpose                                    |
| ------------ | ---------------- | ------------------------------------------ |
| `latitude`   | `6.4654`         | Specifies the latitude of Lagos.           |
| `longitude`  | `3.4064`         | Specifies the longitude of Lagos.          |
| `start_date` | `2025-01-01`     | Specifies the beginning of the date range. |
| `end_date`   | `2025-01-07`     | Specifies the end of the date range.       |
| `hourly`     | `temperature_2m` | Requests hourly temperature data.          |

## 5. Send the Request

Open Postman and create a new GET request.

Enter the following URL:

```text
https://archive-api.open-meteo.com/v1/archive?latitude=6.4654&longitude=3.4064&start_date=2025-01-01&end_date=2025-01-07&hourly=temperature_2m
```
Send the request.

If the request is successful, the API returns a JSON response containing information about the location and the requested hourly weather data.

## 6. Understand the Response

A successful response contains several sections.

For example:
```json
{
    "latitude": 6.4323373,
    "longitude": 3.3948028,
    "generationtime_ms": 0.2397298812866211,
    "utc_offset_seconds": 0,
    "timezone": "GMT",
    "timezone_abbreviation": "GMT",
    "elevation": 0.0,
    "hourly_units": {
        "time": "iso8601",
        "temperature_2m": "°C"
    },
    "hourly": {
        "time": [
            "2025-01-01T00:00",
            "2025-01-01T01:00",
            "2025-01-01T02:00",
     ],
        "temperature_2m": [
            27.1,
            27.1,
            26.9,
    ]
  }
}
// Additional response fields omitted for brevity.
```

The `hourly` object contains the requested weather data.

The `time` array shows when each measurement was recorded, while the `temperature_2m` array contains the corresponding temperature values.

For example:

```text
"2025-01-01T00:00" → 25.4°C
"2025-01-01T01:00" → 25.1°C
```

Each value in the `temperature_2m` array corresponds to the time at the same position in the time array.

## 7. Request Additional Weather Variables

You can request more than one hourly weather variable by separating the variables with commas.

For example:
```text
hourly=temperature_2m,relative_humidity_2m,precipitation
```

The complete request would be:

```text
https://archive-api.open-meteo.com/v1/archive?latitude=6.4654&longitude=3.4064&start_date=2025-01-01&end_date=2025-01-07&hourly=temperature_2m,relative_humidity_2m,precipitation
```
The response will then include the requested variables inside the hourly object.

### Troubleshooting

#### The API returns an error

Check that:

- `latitude` and `longitude` contain valid values.

- `start_date` and `end_date` use the `YYYY-MM-DD` format.

- The requested date range is supported by the historical weather API.

- The weather variable names are spelled correctly.

#### The response contains unexpected data

Check the parameters in your request and make sure you requested the weather variables you actually need.

### Conclusion

You have now retrieved historical hourly weather data using the Open-Meteo API.

The same process can be used to retrieve other weather variables by changing the `hourly` parameter and adjusting the location and date range.