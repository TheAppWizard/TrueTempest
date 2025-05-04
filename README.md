# TrueTempest Weather API

A simple Flask-based API that provides weather forecasts and geocoding services.

## Table of Contents
- [Setup](#setup)
- [API Endpoints](#api-endpoints)
  - [Weather Forecast](#weather-forecast)
  - [Geocoding](#geocoding)
- [Examples](#examples)
  - [Geocoding Example](#geocoding-example)
  - [Weather Example](#weather-example)
- [Weather Codes](#weather-codes)

## Setup

```bash
# Install dependencies
pip install flask requests

# Run the application
python app.py
```

The server will start at `http://127.0.0.1:5000/`.

## API Endpoints

### Weather Forecast

Get weather forecast for a specific location using latitude and longitude coordinates.

```
GET /weather
```

#### Parameters

| Parameter | Type   | Required | Description                                 |
|-----------|--------|----------|---------------------------------------------|
| city      | string | No       | City name (for reference only)              |
| lat       | float  | Yes      | Latitude coordinate                         |
| lon       | float  | Yes      | Longitude coordinate                        |
| days      | int    | No       | Number of forecast days (default: 7)        |

#### Example curl request

```bash
curl "http://127.0.0.1:5000/weather?city=London&lat=51.5074&lon=-0.1278&days=3"
```

#### Response

```json
{
  "city": "London",
  "forecast": [
    {
      "conditions": "Partly cloudy",
      "date": "2023-10-23",
      "day": "Monday",
      "humidity": 85,
      "max_temperature": 16.8,
      "min_temperature": 13.1,
      "precipitation_probability": 70,
      "quotes": "There are f*cking clouds.",
      "sunrise": "2023-10-23T07:41",
      "sunset": "2023-10-23T17:51",
      "weatherCode": 2,
      "max_wind_speed": 18.7
    },
    // Additional days...
  ]
}
```

### Geocoding

Convert a place name into geographic coordinates.

```
GET /geocoding
```

#### Parameters

| Parameter | Type   | Required | Description                                 |
|-----------|--------|----------|---------------------------------------------|
| place     | string | Yes      | Place name to search for                    |

#### Example curl request

```bash
curl "http://127.0.0.1:5000/geocoding?place=New+York"
```

#### Response

```json
[
  {
    "addresstype": "administrative",
    "display_name": "New York, United States of America",
    "lat": "40.7127281",
    "lon": "-74.0060152",
    "name": "New York",
    "place_id": "123456789"
  },
  // Additional results...
]
```

## Examples

### Complete Usage Example

To get the weather for a location, you typically need to:

1. First use the geocoding endpoint to get coordinates for a place name
2. Then use those coordinates with the weather endpoint

### Geocoding Example

```bash
curl "http://127.0.0.1:5000/geocoding?place=Paris"
```

### Weather Example

```bash
curl "http://127.0.0.1:5000/weather?city=Paris&lat=48.8566&lon=2.3522&days=5"
```

### One-liner (using jq for JSON processing)

```bash
# Get coordinates for Paris, then get weather forecast
COORDS=$(curl -s "http://127.0.0.1:5000/geocoding?place=Paris" | jq -r '.[0] | "lat=\(.lat)&lon=\(.lon)"') && \
curl -s "http://127.0.0.1:5000/weather?city=Paris&$COORDS&days=3"
```

## Weather Codes

The API uses the following weather codes:

| Code | Description                     | Quote                                |
|------|---------------------------------|--------------------------------------|
| 0    | Clear sky                       | "It's f*cking beautiful out."        |
| 1    | Mainly clear                    | "It's f*cking nice today."           |
| 2    | Partly cloudy                   | "There are f*cking clouds."          |
| 3    | Overcast                        | "It's f*cking gloomy."               |
| 45   | Foggy                           | "It's f*cking foggy."                |
| 48   | Depositing rime fog             | "Watch out for this f*cking fog."    |
| 51   | Light drizzle                   | "It's f*cking drizzling."            |
| ... and more

Each weather code corresponds to specific weather conditions and includes a colorful quote in the response.
