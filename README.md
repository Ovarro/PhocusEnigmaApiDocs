# Ovarro LeakVision Phocus/Enigma Data API documentation

Questions? Find us at [support.ld@ovarro.com](mailto:support.ld@primayer.co.uk)

# Phocus/Enigma Data Access

This API provides access to the data recorded by a Phocus/Enigma logger. To use the API third parties must provide an authorization token with each request. The authorization token can be obtained by calling the auth endpoint with your Atrium credentials.

# Methods

- [*auth*](#auth): Returns authentication bearer token.
- [*alllogger*](#loggerall): Returns all loggers.
- [*logger*](#loggerserial-begin-end): Returns data for logger specified by date time range.
- [*loggerinfo*](#loggerinfoserial-begin-end): Returns info for logger specified by date time range.
- [*signal*](#signalserial-begin-end): Returns signal for logger specified by date time range.
- [*summary*](#summarydate): Returns all Enigma groups with leak count.
- [*groups*](#groups): Returns all groups.
- [*group*](#groupid-date): Returns leak summary for group.
- [*grouplogger*](#grouploggerid-begin-end): Returns all logger data for group by date range.
- [*groupinfo*](#groupinfoid-begin-end): Returns all logger info for group by date range.
- [*poi*](#poiid): Returns PoI items for group.
- [*groupaudio*](#groupaudioid-date): Returns audio for group.
- [*loggerreport*](#loggerreportdate): CSV logger report.
- [*grouploggerreport*](#grouploggerreportid-date): CSV logger report for a group.
- [*dmareport*](#dmareportbegin-end): CSV Phocus Dma report.
- [*poistatus*](#poistatus): Update the status of a PoI.
- [*poiunassign*](#poiunassign): Remove technician assignment.
- [*poinoleak*](#poinoleak): Update or set the no leak found meta data of a PoI.
- [*poileak*](#poileak): Update or set the leak found meta data of a PoI.

#### Enumerations
- [*PoI Status*](#poi-status): PoI status enums.
- [*Leak Types*](#leak-types): Leak type enums.
- [*Asset Leak Types*](#asset-leak-types): Asset Leak type enums.
- [*Method Types*](#method-types): Method type enums.
- [*Pipe Materials*](#pipe-materials): Pipe material enums.
- [*No Leak Reasons*](#no-leak-reasons): No Leak reasons enums.

# API

## auth

##### Purpose
Returns authentication bearer token. This token must be passed as an Authorization header for every request. The token has a default expiry of 1 hour. An optional parameter can be passed to extend the life of the token (max 24).
<pre>
  Authorization: bearer {token}
</pre>

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/auth
  2. Method
    - POST

##### Body

<pre>
{
  "username": string,
  "password": string,
  "tokenLifeHours": int (optional 1 -> 24)
}
</pre>

##### Return Value

<pre>
[
  string
]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/auth
<pre>
{
  "username": "user123",
  "password": "myP4ssword!"
  "tokenLifeHours": 24
}
</pre>

<pre>
token
</pre>

<br />

## loggerall

##### Purpose
Returns all loggers

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/logger/all
      
##### Return Value

<pre>
[
  string
]
</pre>
##### Example

https://leakvisiondata.atriumiot.com/v2/logger/all/

Example Output:

<pre>
[
  "111111",
  "111112",
  "111113",
  "111114",
  "111115",
  "111116",
  "111117",
  "111118",
  "111119"
]
</pre>

<br />

## logger(serial, begin, end)

##### Purpose
Returns all data for the logger within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/logger/serial/begin/end
  2. Params
   - serial: (string)
     - logger serial number
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
      
##### Return Value

<pre>
[{
  serial: string,
  name: string,
  epochs: [
    {
      battery: double,
      cnv: int,
      lcf: int,
      latitude: double,
      longitude: double,
      temperature: double,
      timestamp: string,
      gain: int,
      scale: int,
      histograms: [
        {
          timestamp: string,
          bins: [int]
        }
      ]
    }
  ]
}]
</pre>
##### Example

https://leakvisiondata.atriumiot.com/v2/logger/12345/2021-01-01/2021-01-02

Example Output:

<pre>
[{
  "serial": "123456",
  "name": "123456"
  "epochs": [
    {
      "battery": 99.9,
      "cnv": 10,
      "lcf": 1,
      "latitude": 50.1,
      "longitude": -1.1,
      "temperature": 24.65,
      "timestamp": "2022-03-25 12:00",
      "gain": 4,
      "scale": 123456,
      "histograms": [
        {
          "timestamp": "2022-03-25 12:00",
          "bins": [0,1,2,3,4,5,6,7,8,9]
        }
      ]
    }
  ]
}]
</pre>

<br />

## loggerinfo(serial, begin, end)

##### Purpose
Returns all info for the logger within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/logger/info/serial/begin/end
  2. Params
   - serial: (string)
     - logger serial number
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
      
##### Return Value

<pre>
[{
  serial: string,
  name: string,
  epochs: [
    {
      battery: double,
      cnv: int,
      lcf: int,
      timestamp: string,
      latitude: double,
      longitude: double,
      gain: int,
      scale: int,
    }
  ]
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/logger/info/12345/2021-01-01/2021-01-02

<pre>
[{
  "serial": "123456",
  "name": "123456",
  "epochs": [
    {
      "battery": 99.9,
      "cnv": 10,
      "lcf": 1,
      "timestamp": "2022-03-25 12:00",
      "latitude": 50.1,
      "longitude": -1.1,
      "gain": 4,
      "scale": 123456,
    }
  ]
}]
</pre>

<br />

## signal(serial, begin, end)

##### Purpose
Returns signal data for the logger within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/logger/signal/serial/begin/end
  2. Params
   - serial: (string)
     - logger serial number
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
      
##### Return Value

<pre>
[{
  serial: string
  signals: [
    {
      name: string,
      signal: string,
      type: string,
      timestamp: string
    }
  ]
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/logger/signal/12345/2021-01-01/2021-01-02

<pre>
[{
  "serial": "123456",
  "signals": [
    {
      "name": "network name",
      "signal": "35%",
      "type": "network type",
      "timestamp": "2022-03-25 12:00"
    }
  ]
}]
</pre>

<br />

## summary(date)

##### Purpose
Returns Enigma groups with leak count

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/group/summary/date
   2. Params
   - date: (string - yyyy-MM-dd)
     - Date at which to get summary
     
##### Return Value

<pre>
[{
  id: long,
  groupName: string,
  leaks: int,
  timestamp: string
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/group/summary/2021-01-01

<pre>
[{
  "id": 123456,
  "groupName": "Group 1%",
  "leaks": 5,
  "timestamp": "2022-03-25 12:00"
}]
</pre>

<br />

## groups

##### Purpose
Returns all group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/group
  
     
##### Return Value

<pre>
[{
  id: int,
  name: string
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/group

<pre>
[{
  "id": 123456,
  "name": "group123"
}]
</pre>

<br />

## group(id, date)

##### Purpose
Returns leak summary for group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/group/id/date
  2. Params
   - id: (int)
     - Group ID (returned in the summary)
   - date: (string - yyyy-MM-dd)
     - Date at which to get summary.
     
##### Return Value

<pre>
[{
  left: string,
  right: string,
  confidence: int,
  distanceFromLeft: double,
  distanceFromRight: double
  date: string,
  peakMs: double,
  correction: double,
  filterMin: string,
  filterMax: string,
  latitude: double,
  longitude: double
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/group/1234/2021-01-01

<pre>
[{
  "left": 123456,
  "right": 654321,
  "confidence": 41,
  "distanceFromLeft": 150.5,
  "distanceFromRight": 12.5,
  "date": "2022-03-25",
  "peakMs": 123,
  "correction": -1234,
  "filterMin": "275",
  "filterMax": "575",
  "latitude": 50,
  "longitude" -1
}]
</pre>

<br />

## grouplogger(id, begin, end)

##### Purpose
Returns all data for the loggers in a group within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/group/loggers/id/begin/end
  2. Params
   - id: (int)
     - Group ID
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
      
##### Return Value

<pre>
[{
  serial: string,
  name: string,
  epochs: [
    {
      battery: double,
      cnv: int,
      lcf: int,
      latitude: double,
      longitude: double,
      temperature: double,
      timestamp: string,
      gain: int,
      scale: int,
      histograms: [
        {
          timestamp: string,
          bins: [int]
        }
      ]
    }
  ]
}]
</pre>
##### Example

https://leakvisiondata.atriumiot.com/v2/logger/12345/2021-01-01/2021-01-02

Example Output:

<pre>
[{
  "serial": "123456",
  "name": "123456"
  "epochs": [
    {
      "battery": 99.9,
      "cnv": 10,
      "lcf": 1,
      "latitude": 50.1,
      "longitude": -1.1,
      "temperature": 24.65,
      "timestamp": "2022-03-25 12:00",
      "gain": 4,
      "scale": 123456,
      "histograms": [
        {
          "timestamp": "2022-03-25 12:00",
          "bins": [0,1,2,3,4,5,6,7,8,9]
        }
      ]
    }
  ]
}]
</pre>

<br />

## groupinfo(id, begin, end)

##### Purpose
Returns all info for the loggers in a group within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/group/loggerinfo/id/begin/end
  2. Params
   - id: (int)
     - Group ID
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
      
##### Return Value

<pre>
[{
  serial: string,
  name: string,
  epochs: [
    {
      battery: double,
      cnv: int,
      lcf: int,
      timestamp: string,
      latitude: double,
      longitude: double,
      gain: int,
      scale: int,
    }
  ]
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/logger/info/12345/2021-01-01/2021-01-02

<pre>
[{
  "serial": "123456",
  "name": "123456",
  "epochs": [
    {
      "battery": 99.9,
      "cnv": 10,
      "lcf": 1,
      "timestamp": "2022-03-25 12:00",
      "latitude": 50.1,
      "longitude": -1.1,
      "gain": 4,
      "scale": 123456,
    }
  ]
}]
</pre>

<br />

## poi(id)

##### Purpose
Returns PoI items for group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/group/poi/id
  2. Params
   - id: (int)
     - Group ID (returned in the summary)
     
##### Return Value

<pre>
[{
  name: string,
  groupId: int,
  groupName: string,
  status: int,
  technicianData: {
    dateIssued: string,
    dateCompleted: string,
    completed: bool,
    paused: bool,
    escalated: bool,
    technicianName: string,
    firstOnSite: string
    leaks: [
      {
        leakType: int,
        methods: [int],
        AssetType: int,
        latitude: double,
        longitude: double,
        dateFound: string,
        priority: int,
        workOrder: string,
        plannedDate: string,
        actualRepairDate: string,
        actualLeakType: int
      }
    ],
    noLeak: {
      reason: int,
      methods: [int],
      dateFound: string
    }
  },
  correlations: [
    {
      date: string,
      confidence: double,
      leftLogger: string,
      rightLogger: string,
      distanceFromLeft: double,
      distanceFromRight: double,
      correction: double,
      filterMin: double,
      filterMax: double,
      latitude: double,
      longitude: double,
      pipes: [
        {
          material: int,
          diameter: double,
          length: double,
          velocity: double
        }
      ],
      correlationPipe: {
        material: int,
        diameter: double,
        length: double,
        velocity: double
      },
      leftPipe: {
        material: int,
        diameter: double,
        length: double,
        velocity: double
      },
      rightPipe: {
        material: int,
        diameter: double,
        length: double,
        velocity: double
      }
    }
  ]
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/group/poi/1234

<pre>
[{
  "name": "ABC123",
  "groupId": 12345,
  "groupName": "Group 1",
  "status": 3,
  "technicianData": {
    "dateIssued": "2022-01-01",
    "dateCompleted": "2022-01-20",
    "completed": true,
    "paused": false,
    "escalated": false,
    "technicianName": "Test Name",
    "firstOnSite": "2022-01-20"
    "leaks": [
      {
        "leakType": 1,
        "methods": [0,1],
        "AssetType": 1,
        "latitude": 50.1,
        "longitude": -1.6,
        "dateFound": "2022-01-20",
        "priority": 3,
        "workOrder": "123456",
        "plannedDate": "2022-02-20",
        "actualRepairDate": null,
        "actualLeakType": null
      }
    ],
    noLeak: {
      "reason": 1,
      "methods": [0,1],
      "dateFound": "2022-01-20"
    }
  },
  "correlations": [
    {
      "date": "2022-01-01",
      "confidence": 65,
      "leftLogger": "123456",
      "rightLogger": "654321",
      "distanceFromLeft": 150.1,
      "distanceFromRight": 49.9,
      "correction": -123,
      "filterMin": 375,
      "filterMax": 675,
      "latitude": 50.1,
      "longitude": -1.6,
      "pipes": [
        {
          "material": 0,
          "diameter": 72.6,
          "length": 100,
          "velocity": 1234
        },
        {
          "material": 8,
          "diameter": 100,
          "length": 100,
          "velocity": 1234
        }
      ],
      "correlationPipe": {
        "material": 8,
        "diameter": 100,
        "length": 125.2,
        "velocity": 1234
      },
      "leftPipe": {
        "material": 0,
        "diameter": 72.6,
        "length": 100,
        "velocity": 1234
      },
      "rightPipe": {
        "material": 8,
        "diameter": 100,
        "length": 100,
        "velocity": 1234
      }
    }
  ]
}]
</pre>

<br />

## groupaudio(id, date)

##### Purpose
Returns audio data for group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/group/audio/id/date
  2. Params
   - id: (int)
     - group ID (returned in the summary)
   - date: (string - yyyy-MM-dd)
     - Date at which to get audio.
     
##### Return Value

<pre>
[{
  logger: string,
  timestamp: string,
  audio: [byte]
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/group/audio/1234/2021-01-01

<pre>
[{
  "logger": 123456,
  "timestamp": "2022-03-25",
  "audio": [0,0,0,0,0,0,0,0,0,0,0]
}]
</pre>

<br />

## loggerreport(date)

##### Purpose
Returns logger report csv

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/report/logger/date
  2. Params
   - date: (string - yyyy-MM-dd)
     - Date at which to run the report.
     
##### Return Value
  csv report

##### Example

https://leakvisiondata.atriumiot.com/v2/report/logger/2021-01-01

<pre>
  DMA,ID,Logger,Commissioned,Latest,Latitude,Longitude,Battery,Signal
  Dma,1234,123456,22/09/2021,05/12/2022,50.84410095,-1.064781666,80.56%,59%
</pre>

<br />

## grouploggerreport(id, date)

##### Purpose
Returns logger report csv for a group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/report/group/id/date
  2. Params
   - id: (int)
     - Group ID
   - date: (string - yyyy-MM-dd)
     - Date at which to run the report.
     
##### Return Value
  csv report

##### Example

https://leakvisiondata.atriumiot.com/v2/report/group/12345/2021-01-01

<pre>
  DMA,ID,Logger,Commissioned,Latest,Latitude,Longitude,Battery,Signal
  Dma,1234,123456,22/09/2021,05/12/2022,50.84410095,-1.064781666,80.56%,59%
</pre>

<br />

## dmareport(begin, end)

##### Purpose
Returns Phocus DMA report csv

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/report/dma/begin/end
  2. Params
   - begin: (string - yyyy-MM-dd HH:mm )
     - Date at which to start querying.
   - end: (string - yyyy-MM-dd HH:mm )
     - Date at which to finish querying.
     
##### Return Value
  csv report

##### Example

https://leakvisiondata.atriumiot.com/v2/report/dma/2021-01-01%2000:00/2021-01-02%2000:00

<br />

## poistatus

##### Purpose
Update the status enum of a POI

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/poi/status
   2. Method
    - PUT

##### Body

<pre>
{
  "groupId": int,
  "poiName": string,
  "status": int (see POI status for values)
}
</pre>
  
##### Return Value

<pre>
Boolean
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/poi/status
<pre>
{
  "groupId": 123456,
  "poiName": "poi-12345",
  "status": 2
}
</pre>

<pre>
true
</pre>

<br />

## poiunassign

##### Purpose
Unassign technician from a POI

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/poi/unassign
   2. Method
    - PUT

##### Body

<pre>
{
  "groupId": int,
  "poiName": string,
}
</pre>
  
##### Return Value

<pre>
Boolean
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/poi/unassign
<pre>
{
  "groupId": 123456,
  "poiName": "poi-12345",
}
</pre>

<pre>
true
</pre>

<br />

## poinoleak

##### Purpose
Update or set no leak found meta data in a POI

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/poi/noleak
   2. Method
    - PUT

##### Body

<pre>
{
  "groupId": int,
  "poiName": string,
  "dateFound": string (yyyy-MM-dd or yyyy-MM-dd HH:mm),
  "equipmentEnums": array of int (see methods for values),
  "reasonEnum": int (see reasons for values)
}
</pre>
  
##### Return Value

<pre>
Boolean
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/poi/noleak
<pre>
{
  "groupId": 123456,
  "poiName": "poi-12345",
  "dateFound": "2026-01-21",
  "equipmentEnums": [1,3],
  "reasonEnum": 1
}
</pre>

<pre>
true
</pre>

<br />

## poileak

##### Purpose
Update or set leak found meta data in a POI

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/v2/poi/leak
   2. Method
    - PUT

##### Body

<pre>
{
  "groupId": int,
  "poiName": string,
  "repairedDate": string (yyyy-MM-dd or yyyy-MM-dd HH:mm),
  "workOrder": string (if work order is empty, all found leaks in the POI will be updated)
}
</pre>
  
##### Return Value

<pre>
Boolean
</pre>

##### Example

https://leakvisiondata.atriumiot.com/v2/poi/leak
<pre>
{
  "groupId": 123456,
  "poiName": "poi-12345",
  "repairedDate": "2026-01-21",
  "workOrder": "WO-12345",
}
</pre>

<pre>
true
</pre>

<br />

# PoI Status
<pre>
{
  Undefined = 0,
  AwaitingCategorization = 1,
  AwaitingFollowUp = 2,
  AwaitingRepair = 3,
  LeakRepaired = 4,
  AdditionalDataNeeded = 5,
  NoLeakDetected = 6,
  NoLeak = 7
}
</pre>

# Leak Types
<pre>
{
  Main150 = 0,
  Main250 = 1,
  Main250+ = 2,
  SluiceValve = 3,
  AirValve = 4,
  FireHydrant = 5,
  Washout = 6,
  Meter = 7,
  Stoptap = 8,
  CommPipe = 9,
  ServicePipe = 10,
  BTBB = 11,
  Ferrule = 12,
  Waste = 13,
  Other = 14,
  Enabling = 15,
  FireFixed = 16,
  WashoutFixed = 17,
  UnrecordedConsumptionSm = 18,
  UnrecordedConsumptionMd = 19,
  UnrecordedConsumptionLg = 20,
  WasteOverflow = 21,
  SluiceFixed = 22,
  StopTapFixed = 23,
  AirFixed = 24,
  MeterFixed = 25,
  BTBBLow = 26,
  DryHole = 27
}
</pre>

# Asset Leak Types
<pre>
{
  VisibleHighVolume = 0,
  VisibleLowVolume = 1,
  NonVisibleLowVolume = 3,
  NonVisibleHighVolume = 4
}
</pre>

# Method Types
<pre>
{
  ListeningStick = 0,
  GroundMicrophone = 1,
  LiveCorrelatorAccelerometer = 2,
  LiveCorrelatorHydrophone = 3,
  LiftAndShiftCorrelatingLogger = 4,
  LiftAndShiftNoiseLogger = 5,
  Hydrogen Gas = 6,
  Other = 7
}
</pre>

# Pipe Materials
<pre>
{
  CastIron = 0,
  Concrete = 1,
  Copper = 2,
  DuctileIron = 3,
  DuctileIronConcreteLined = 4,
  GalvanisedIron = 5,
  HDPE = 6,
  Lead = 7,
  MDPE = 8,
  PVC = 9,
  Steel = 10,
  SteelConcreteLine = 11,
  AsbestosCement = 12,
  SpunIron = 13
}
</pre>

# No Leak Reasons
<pre>
{
  AirConditioningUnit = 0,
  ElectricalNoise = 1,
  IndustrialUser = 2,
  PRV = 3,
  Other = 4,
  Usage = 5,
  MechanicalNoise = 6,
  RattlingValveOrHydrant = 7,
  WasteWaterPipe = 8,
  StormDrain = 9
}
</pre>
