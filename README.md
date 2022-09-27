# Ovarro LeakVision Phocus/Enigma Data API documentation

Questions? Find us at [development@ovarro.com](mailto:development@primayer.co.uk)

# Phocus/Enigma Data Access

This API provides access to the data recorded by a Phocus/Enigma logger. To use the API third parties must provide an authorization token with each request. The authorization token can be found within the account management section of [Atrium](https://atriumiot.com/accountmanagement).

# Methods

- [*logger*](#loggerserial-begin-end-token): Returns data for logger specified by date time range.
- [*loggerinfo*](#loggerinfoserial-begin-end-token): Returns info for logger specified by date time range.
- [*signal*](#signalserial-begin-end-token): Returns signal for logger specified by date time range.
- [*summary*](#summarydate-token): Returns all Enigma groups with leak count.
- [*group*](#groupid-date-token): Returns leak summary for group.
- [*poi*](#poiid-token): Returns PoI items for group.
- [*groupaudio*](#groupaudioid-date-token): Returns audio for group.
- [*loggerreport*](#loggerreportdate-token): CSV logger report.
- [*dmareport*](#dmareportbegin-end-token): CSV Phocus Dma report.

#### Enumerations
- [*PoI Status*](#poi-status): PoI status enums.
- [*Leak Types*](#leak-types): Leak type enums.
- [*Asset Leak Types*](#asset-leak-types): Asset Leak type enums.
- [*Method Types*](#method-types): Method type enums.
- [*Pipe Materials*](#pipe-materials): Pipe material enums.
- [*No Leak Reasons*](#no-leak-reasons): No Leak reasons enums.

# API


## logger(serial, begin, end, token)

##### Purpose
Returns all data for the logger within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/logger/serial/begin/end/token
  2. Params
   - serial: (string)
     - logger serial number
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
   - token: (string)
     - api authorization token.
      
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

https://leakvisiondata.atriumiot.com/logger/12345/2021-01-01/2021-01-02/00000000-0000-0000-0000-000000000000

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

## loggerinfo(serial, begin, end, token)

##### Purpose
Returns all info for the logger within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/logger/info/serial/begin/end/token
  2. Params
   - serial: (string)
     - logger serial number
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
   - token: (string )
     - api authorization token.
      
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

https://leakvisiondata.atriumiot.com/logger/info/12345/2021-01-01/2021-01-02/00000000-0000-0000-0000-000000000000

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

## signal(serial, begin, end, token)

##### Purpose
Returns signal data for the logger within the date range

##### Signature
  1. Endpoint
    - https://leakvisiondata.atriumiot.com/logger/signal/serial/begin/end/token
  2. Params
   - serial: (string)
     - logger serial number
   - begin: (string - yyyy-MM-dd)
     - Date at which to start querying logger data.
   - end: (string - yyyy-MM-dd)
     - Date at which to finish querying logger data.
   - token: (string)
     - api authorization token.
      
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

https://leakvisiondata.atriumiot.com/logger/signal/12345/2021-01-01/2021-01-02/00000000-0000-0000-0000-000000000000

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

## summary(date, token)

##### Purpose
Returns Enigma groups with leak count

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/group/summary/date/token
   2. Params
   - date: (string - yyyy-MM-dd)
     - Date at which to get summary
   - token: (string)
     - api authorization token
     
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

https://leakvisiondata.atriumiot.com/group/summary/2021-01-01/00000000-0000-0000-0000-000000000000

<pre>
[{
  "id": 123456,
  "groupName": "Group 1%",
  "leaks": 5,
  "timestamp": "2022-03-25 12:00"
}]
</pre>

<br />

## group(id, date, token)

##### Purpose
Returns leak summary for group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/group/id/date/token
  2. Params
   - id: (Int)
     - group Id (returned in the summary)
   - date: (string - yyyy-MM-dd)
     - Date at which to get summary.
   - token: (string)
     - api authorization token.
     
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
  filterMax: string
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/group/1234/2021-01-01/00000000-0000-0000-0000-000000000000

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
  "filterMax": "575"
}]
</pre>

<br />

## poi(id, token)

##### Purpose
Returns PoI items for group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/group/poi/id/token
  2. Params
   - id: (Int)
     - group Id (returned in the summary)
   - token: (string)
     - api authorization token.
     
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

https://leakvisiondata.atriumiot.com/group/poi/1234/00000000-0000-0000-0000-000000000000

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

## groupaudio(id, date, token)

##### Purpose
Returns audio data for group

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/group/audio/id/date/token
  2. Params
   - id: (Int)
     - group Id (returned in the summary)
   - date: (string - yyyy-MM-dd)
     - Date at which to get audio.
   - token: (string)
     - api authorization token.
     
##### Return Value

<pre>
[{
  logger: string,
  timestamp: string,
  audio: [byte]
}]
</pre>

##### Example

https://leakvisiondata.atriumiot.com/group/audio/1234/2021-01-01/00000000-0000-0000-0000-000000000000

<pre>
[{
  "logger": 123456,
  "timestamp": "2022-03-25",
  "audio": [0,0,0,0,0,0,0,0,0,0,0]
}]
</pre>

<br />

## loggerreport(date, token)

##### Purpose
Returns logger report csv

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/report/logger/date/token
  2. Params
   - date: (string - yyyy-MM-dd)
     - Date at which to run the report.
   - token: (string)
     - api authorization token.
     
##### Return Value
  csv report

##### Example

https://leakvisiondata.atriumiot.com/report/logger/2021-01-01/00000000-0000-0000-0000-000000000000

<br />

## dmareport(begin, end, token)

##### Purpose
Returns Phocus DMA report csv

##### Signature
   1. Endpoint
    - https://leakvisiondata.atriumiot.com/report/dma/begin/end/token
  2. Params
   - begin: (string - yyyy-MM-dd HH:mm )
     - Date at which to start querying.
   - end: (string - yyyy-MM-dd HH:mm )
     - Date at which to finish querying.
   - token: (string)
     - api authorization token.
     
##### Return Value
  csv report

##### Example

https://leakvisiondata.atriumiot.com/report/dma/2021-01-01%2000:00/2021-01-02%2000:00/00000000-0000-0000-0000-000000000000

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
  Usage = 5
}
</pre>
