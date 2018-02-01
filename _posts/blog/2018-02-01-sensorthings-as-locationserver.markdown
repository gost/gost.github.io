---
layout: post
title: "Using SensorThings server as Location server"
date: 2018-02-01 11:49:00
author: Bert
categories: 
- blog
- SensorThings
---
In most cases a SensorThings 'Thing' will have a static Location. For example a wheaterstation will not move around usually. But in some usecases the Thing will move 
around and leave a trail of locations while measuring Observations. Think about usecases like getting sensor data from moving drones, phones, players in a soccer field or autonomous cars.

In the SensorThings API there is support for storing and retrieving the actual position and Trail of a Thing.
If we look at the SensorThings entity model, on the left side we see 2 entities for Location and HistoricalLocation.

<img src="../../../assets/img/blog/SensorThings_API_data_model_locations.png">

Now let's explore how to make use of the actual position and trail functionality in SensorThings API. As a demo we create a Thing with two Locations and retrieve actual position/trail using cURL statements.

Step 1: Create a Thing

```
$ curl -X POST http://localhost:8080/v1.0/Things -H 'Content-Type: application/json' -d '{
"name": "Berts Android Phone",
 "description": "Berts Android Phone"
}'
```

Result: Thing ID = 1 is created

Step 2: Add a first Location (0,0) to the just created Thing

```
$ curl -X POST 'http://localhost:8080/v1.0/Things(1)/Locations' -H 'Content-Type: application/json' -d '{
"name": "First place",
    "description": "First place",
    "encodingType": "application/vnd.geo+json",
    "location": {
        "type": "Point",
        "coordinates": [0, 0]
}
}'
```

Result: Location id = 1 is created

Step 3: Add a second Location (1,1) to the just created Thing

```
$ curl -X POST 'http://localhost:8080/v1.0/Things(1)/Locations' -H 'Content-Type: application/json' -d '{
"name": "Second place",
    "description": "Second place",
    "encodingType": "application/vnd.geo+json",
    "location": {
        "type": "Point",
        "coordinates": [1,1]
}
}'
```

Step 4: Request actual position of Thing(1)

```
$ curl 'http://localhost:8080/v1.0/Things(1)/Locations'

{
   "value": [
      {
         "@iot.id": 2,
         "@iot.selfLink": "http://localhost:8080/v1.0/Locations(2)",
         "name": "Second place",
         "description": "Second place",
         "encodingType": "application/vnd.geo+json",
         "location": {
            "coordinates": [
               1,
               1
            ],
            "type": "Point"
         },
         "Things@iot.navigationLink": "http://localhost:8080/v1.0/Locations(2)/Things",
         "HistoricalLocations@iot.navigationLink": "http://localhost:8080/v1.0/Locations(2)/HistoricalLocations"
      }
   ]
```

Result: The last known Location (id=2, at 1,1) is retrieved. So remember: http://localhost:8080/v1.0/Things(1)/Locations will return 1 location (the last created) and will not return a collection of Locations.

Step 5: Request the trail of Thing(1)

```
$ curl 'http://localhost:8080/v1.0/Things(1)?$expand=HistoricalLocations/Locations'

{
   "@iot.id": 1,
   "@iot.selfLink": "http://localhost:8080/v1.0/Things(1)",
   "name": "Berts Android Phone",
   "description": "Berts Android Phone",
   "Locations@iot.navigationLink": "http://localhost:8080/v1.0/Things(1)/Locations",
   "Datastreams@iot.navigationLink": "http://localhost:8080/v1.0/Things(1)/Datastreams",
   "HistoricalLocations": [
      {
         "@iot.id": 2,
         "@iot.selfLink": "http://localhost:8080/v1.0/HistoricalLocations(2)",
         "time": "2018-02-01T10:31:34.568Z",
         "Thing@iot.navigationLink": "http://localhost:8080/v1.0/HistoricalLocations(2)/Thing",
         "Locations": [
            {
               "@iot.id": 2,
               "@iot.selfLink": "http://localhost:8080/v1.0/Locations(2)",
               "name": "Second place",
               "description": "Second place",
               "encodingType": "application/vnd.geo+json",
               "location": {
                  "coordinates": [
                     1,
                     1
                  ],
                  "type": "Point"
               },
               "Things@iot.navigationLink": "http://localhost:8080/v1.0/Locations(2)/Things",
               "HistoricalLocations@iot.navigationLink": "http://localhost:8080/v1.0/Locations(2)/HistoricalLocations"
            }
         ]
      },
      {
         "@iot.id": 1,
         "@iot.selfLink": "http://localhost:8080/v1.0/HistoricalLocations(1)",
         "time": "2018-02-01T10:30:04.397Z",
         "Thing@iot.navigationLink": "http://localhost:8080/v1.0/HistoricalLocations(1)/Thing",
         "Locations": [
            {
               "@iot.id": 1,
               "@iot.selfLink": "http://localhost:8080/v1.0/Locations(1)",
               "name": "First place",
               "description": "First place",
               "encodingType": "application/vnd.geo+json",
               "location": {
                  "coordinates": [
                     0,
                     0
                  ],
                  "type": "Point"
               },
               "Things@iot.navigationLink": "http://localhost:8080/v1.0/Locations(1)/Things",
               "HistoricalLocations@iot.navigationLink": "http://localhost:8080/v1.0/Locations(1)/HistoricalLocations"
            }
         ]
      }
   ]
}
````

Result: Using the $expand operator on =HistoricalLocations/Locations ($expand=HistoricalLocations/Locations) The Trail of the Thing is returned (Location 1 and 2). 

In a client (mapping) application, we can use the actual position and trail to visualize the location/trail of the Things. Another option to get the actual position of the Thing is to use MQTT using a subscribe on topic 'Things1/Locations'. 











