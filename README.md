# TrueTempest Weather API

A simple API that provides weather forecasts and geocoding services.

## API Endpoints

The base URL for the API is: `http://3zx.ce0.mytemp.website/tempest`

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

#### Curl Example

```bash
curl "http://3zx.ce0.mytemp.website/tempest/weather?city=London&lat=51.5074&lon=-0.1278&days=3"
```

#### Postman Example

For Postman, simply create a GET request with the following URL:
```
http://3zx.ce0.mytemp.website/tempest/weather?city=London&lat=51.5074&lon=-0.1278&days=3
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

#### Curl Example

```bash
curl "http://3zx.ce0.mytemp.website/tempest/geocoding?place=New+York"
```

#### Postman Example

For Postman, create a GET request with the following URL:
```
http://3zx.ce0.mytemp.website/tempest/geocoding?place=New+York
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

## Practical Usage Examples

### Common API Combinations

Here are the most useful combinations for this API:

#### 1. Get coordinates and then weather for a location

**Step 1:** Get coordinates for a location using geocoding
```bash
curl "http://3zx.ce0.mytemp.website/tempest/geocoding?place=Paris"
```

**Step 2:** Use the returned coordinates to get weather forecast
```bash
curl "http://3zx.ce0.mytemp.website/tempest/weather?city=Paris&lat=48.8566&lon=2.3522&days=5"
```

#### 2. Get weather for major cities

**New York:**
```bash
curl "http://3zx.ce0.mytemp.website/tempest/weather?city=New+York&lat=40.7128&lon=-74.0060&days=3"
```

**Tokyo:**
```bash
curl "http://3zx.ce0.mytemp.website/tempest/weather?city=Tokyo&lat=35.6762&lon=139.6503&days=3"
```

**Sydney:**
```bash
curl "http://3zx.ce0.mytemp.website/tempest/weather?city=Sydney&lat=-33.8688&lon=151.2093&days=3"
```

#### 3. Get longer forecast periods

7-day forecast:
```bash
curl "http://3zx.ce0.mytemp.website/tempest/weather?city=London&lat=51.5074&lon=-0.1278&days=7"
```

#### 4. Get coordinates for various location types

**For a city:**
```bash
curl "http://3zx.ce0.mytemp.website/tempest/geocoding?place=Berlin"
```

**For a landmark:**
```bash
curl "http://3zx.ce0.mytemp.website/tempest/geocoding?place=Eiffel+Tower"
```

**For a country:**
```bash
curl "http://3zx.ce0.mytemp.website/tempest/geocoding?place=New+Zealand"
```

### Weather Codes

The API returns the following weather codes in the response:

| Code | Description                     | Quote                                |
|------|---------------------------------|--------------------------------------|
| 0    | Clear sky                       | "It's f*cking beautiful out."        |
| 1    | Mainly clear                    | "It's f*cking nice today."           |
| 2    | Partly cloudy                   | "There are f*cking clouds."          |
| 3    | Overcast                        | "It's f*cking gloomy."               |
| 45   | Foggy                           | "It's f*cking foggy."                |
| 48   | Depositing rime fog             | "Watch out for this f*cking fog."    |
| 51   | Light drizzle                   | "It's f*cking drizzling."            |
| 53   | Moderate drizzle                | "The drizzle is f*cking steady."     |
| 55   | Dense drizzle                   | "This drizzle is f*cking heavy."     |
| 61   | Slight rain                     | "It's f*cking sprinkling."           |
| 63   | Moderate rain                   | "It's f*cking raining now."          |
| 65   | Heavy rain                      | "It's f*cking pouring."              |
| 71   | Slight snow                     | "It's f*cking snowing lightly."      |
| 73   | Moderate snow                   | "The snow is f*cking steady."        |
| 75   | Heavy snow                      | "It's f*cking dumping snow."         |
| 95   | Thunderstorm                    | "It's f*cking thundering."           |
