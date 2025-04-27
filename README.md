# Konya ATUS API

This project provides an API developed to fetch real-time public transportation data from the Konya ATUS (Akıllı Toplu Ulaşım Sistemi) website. It offers easy access to up-to-date information about bus stops and arrival times for public transport users in Konya. Built with Python and FastAPI, it features asynchronous processing for optimal performance.

## Features

- **Real-time Data Access** : Provides up-to-date information directly from Konya ATUS system
- **Comprehensive Bus Stop Directory** : Complete listing of all bus and tram stops in Konya
- **Live Arrival Times** : Real-time bus and tram arrival predictions at any stop
- **Rate Limiting** : Built-in protection against excessive requests (60 requests per minute)
- **Caching Mechanism** : Optimized performance with 30-second cache for bus stop data
- **Simple JSON Responses** : Consistent and easy-to-parse response format
- **Error Handling** : Clear error responses with appropriate HTTP status codes
- **Cross-Origin Support** : Full CORS support for browser-based applications

### Endpoints

---

### Endpoint - Root

Provides API health status information.

```http

GET https://ahmetalper-konya-atus-api.hf.space

```

```json

{

    "status" : "success",
    "status_code" : 200

}

```

---

### Endpoint - Bus Stops

Retrieves a comprehensive list of all bus and tram stops in Konya. Results are sorted by `stop_no` with regular bus stops displayed first, followed by tram stops (prefixed with 'T').

```http

GET https://ahmetalper-konya-atus-api.hf.space/api/v1/bus-stops

```

```json

{

    "status" : "success",
    "status_code" : 200,

    "data" : {

        "bus_stops" : [

            {
                "stop_no" : "1",
                "stop_name" : "stop_name_1"
            },

            {
                "stop_no" : "2",
                "stop_name" : "stop_name_2"
            },

            // ... other stops

            {
                "stop_no" : "T1",
                "stop_name" : "stop_name_3"
            },

            {
                "stop_no" : "T2",
                "stop_name" : "stop_name_4"
            }

            // ... other tram stops

        ]

    }

}

```

---

### Endpoint - Bus Stops With Bus Stop No

Retrieves real-time information about buses scheduled to arrive at a specific stop. Returns details including route numbers, bus names, directions, and estimated arrival times. Results are sorted by `bus_arrival_time` in ascending order.

`bus_stop_no` : The bus stop number to get information for. Can be a regular bus stop number (e.g. '1', '2') or a tram stop number (e.g. 'T1', 'T2').

```http

GET https://ahmetalper-konya-atus-api.hf.space/api/v1/bus-stops/{bus_stop_no}

```

```json

{

    "status" : "success",
    "status_code" : 200,

    "data" : {

        "bus_stop_no" : "{bus_stop_no}",

        "bus_stop_data" : [

            {
                "bus_route_no" : "bus_route_no_1",
                "bus_name" : "bus_name_1",
                "bus_direction" : "bus_direction_1",
                "bus_arrival_time" : 0

            },

            {
                "bus_route_no" : "bus_route_no_2",
                "bus_name" : "bus_name_2",
                "bus_direction" : "bus_direction_2", 
                "bus_arrival_time" : 3
            },

            {
                "bus_route_no" : "bus_route_no_3",
                "bus_name" : "bus_name_3",
                "bus_direction" : "bus_direction_3", 
                "bus_arrival_time" : 5
            },

            {
                "bus_route_no" : "bus_route_no_4",
                "bus_name" : "bus_name_4",
                "bus_direction" : "bus_direction_4", 
                "bus_arrival_time" : 8
            }

        ]

    }

}

```

### Errors

---

### Not Found Error

Returned when the requested endpoint or resource cannot be found.

```json

{

    "status" : "error",
    "status_code" : 404,
    "message" : "not found"

}

```

---

### Too Many Requests Error

Returned when the client exceeds the rate limit of 60 requests per minute.

```json

{

    "status" : "error",
    "status_code" : 429,
    "message" : "too many requests"

}

```

---

### Internal Server Error

Returned when an unexpected internal server error occurs during request processing.

```json

{

    "status" : "error",
    "status_code" : 500,
    "message" : "an unexpected error has occurred"

}

```
