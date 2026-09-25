# Get Film Information from Studio Ghibli

## Overview
This endpoint returns information about all films in the Studio Ghibli collection.
## Endpoint
**Method:** `GET`

**URL:** `https://ghibliapi.vercel.app/films`

**Authentication:** Not required 
## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `limit` | integer | No | Number of results to return. Defaults to 50; maximum is 250 |
| `fields`| string | no| List of fields to include in the response, separated by a comma |

## Request Example
`GET https://ghibliapi.vercel.app/films/   `

`GET https://ghibliapi.vercel.app/films?limit=5`

## Response to Expect
**200 OK;** An Array of Films
  ```json
   {
        "id": "2baf70d1-42bb-4437-b551-e5fed5a87abe",
        "title": "Castle in the Sky",
        "original_title": "天空の城ラピュタ",
        "original_title_romanised": "Tenkū no shiro Rapyuta",
        "image": "https://image.tmdb.org/t/p/w600_and_h900_bestv2/npOnzAbLh6VOIu3naU5QaEcTepo.jpg",
    }
        // Additional response fields omitted for breviy.
```
    #### Response Fields;

    | Field | Type | Description |
    |---|---|---|
    | `id` | Integer | The unique identifier of each film |
    | `title` | String | Title of the film |
     `original_title` | String | The film's original japanese title in japanese characters |
    | `original_title_romanised` | String | The original japanese title written in roman letters|
    | `image` | String | URL of the film's image |

**400;** Bad Request

**404;** Not Found
