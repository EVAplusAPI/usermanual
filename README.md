# EVA+ API usermanual
## Abstract:
Describes the EVA+ API for technically advanced users who wants to use EVA+ data in 3rd party applications.
<img src="API image.png" alt="title image" class="inline"/>
# Introduction
Keep control of all vehicle data while making them available to other stakeholders/parties.

The REST based data API allows to make data available for humans and 3rd  party applications. It allows the operator, the owner of the data to give out what he wants and protect what is private.

Based on the available modules, the operator can define datasets which are accessible from outside the application. 



#	REST API 
 In the following examples `_EVA+ URL_` has to be replaced with the customer specific URL e.g.: railwaycompany.evaplus.com 
##	Security
###	User and Roles
In the “user settings” a role with the name “API” has to be created.
Every users credential with this role can then be used for the process below.
A user can only perform one request at the time.
### Login

Request Method: POST
https://`_EVA+ URL_`/EvaCloudAPI/login

Body (Content-Type = application/json )
```
{ "username": "guest", "password": "guest}
```
Response:
```
{
  "name": "guest",
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJJc3N"
}
```
### Logout
Request Method: POST
https://` _EVA+ URL_` /EvaCloudAPI/logout

Response:
```
{
  "message": "User successfully logout."
}
```
###	HTTPS

https is used for the rest call. 
##	General Data
###	Vehicle List
Request Method: GET
https://`_EVA+ URL_` /EvaCloudAPI/getVehicleFleet
- Header key: Authorization
- Header Value: Token

Response:
```
[
  {
    "fleet": "Class3000",
    "vehicle": [
      "3012",
      "3013",
      "3014",
      "3016",
      "3017",
      "3018",
      "3020",
      "3021",
      "3024",
      "3025",
      "3026",
      "3029",
      "3113"
    ]
  },
  {
    "fleet": "Class3000_Comeng",
    "vehicle": [
      "3002",
      "3004",
      "3005",
      "3006",
      "3008",
      "3107",
      "3110"
    ]
  },
  {
    "fleet": "EastAnglia",
    "vehicle": [
      "93 85 0 501 005-8",
      "745.101-A"
    ]
  }
]
```
 
### Recorder List
Request Method: GET
https://`_EVA+ URL_`/EvaCloudAPI/getRecorderVehicle
- Header key: Authorization
- Header Value: Token

Response:
```
[
  {
    "id": "19102892",
    "vehicle": [
      "195002"
    ]
  },
  {
    "id": "17085829",
    "vehicle": [
      "195003"
    ]
  },
  {
    "id": "18113756",
    "vehicle": [
      "195004"
    ]
  }
]
```
 
### Signal Name List

Request Method: GET
https://`_EVA+ URL_`/EvaCloudAPI/getSignalNameList
- Header key: Authorization
- Header Value: Token

| Parameter	| Optional	| Argument	| Comment
| --- | --- | --- | --- |
| vehicleId	| mandatory	| Vehicle ID as stored in the TELOC.	


Response:
```
[
  {
    "signalname": "System_SPEED",
    "signaltype": "SIGNAL_TYPE_ANALOG",
    "signalmemory": "T_3000_GUM_INT_TDATA"
  },
  {
    "signalname": "autotrans_trigger",
    "signaltype": "SIGNAL_TYPE_DIGITAL",
    "signalmemory": "T_3000_GUM_INT_TDATA"
  },
  {
    "signalname": "GPS_LOCATION",
    "signaltype": "SIGNAL_TYPE_OTHER",
    "signalmemory": "T_3000_GUM_INT_TDATA"
  }
]
```
### Event Name List

Request Method: **GET**
https://`_EVA+ URL_`/EvaCloudAPI/getEventNameList
- Header key: Authorization
- Header Value: Token

Response:
```
{
  "events": [
    "AWS_Reaction",
    "Cab_setup",
    "Deadman Buzzer",
    "Fire in car",
    "Fire_in_Car",
    "GEOFENCING_EVENT",
    "Horn",
    "ISO_Pneumatic_Brake",
    "Journey",
    "Journey_stop",
    "Segment",
    "Speed>105mph",
    "Speed>110mph",
    "Standstill",
    "TPWS_AWS_brake",
    "TPWS_emergency_brake",
    "Trip",
    "WSP_event",
    "WheelslideDMS",
    "WheelslideDMSL",
    "WheelslidePTS",
    "WheelslideTS",
    "WheelslipDMS",
    "WheelslipDMSL",
    "WheelslipPTS",
    "WheelslipTS",
    "Wheelslip_DMS",
    "Wheelslip_DMSL",
    "temp01"
  ]
}
```

 
##	Signal Querying
Data can be pulled from EVA+ with a REST Query from:


| Parameter	| Optional	| Argument	| Comment
| --- | --- | --- | --- |
| vehicleId | mandatory | Vehicle ID as stored in the TELOC.	
| recorderId | mandatory | Serial Number of the TELOC® if Multiple Recorders are mounted on the Vehicle	
| startTime | mandatory | (yyyyMMddHHmm) | Data from this timestamp will be retrieved, including this timestamp,If no date is specified only limit is taken into consideration.
| endTime | mandatory |  (yyyyMMddHHmm) | Data up to this timestamp will be retrieved, including this timestamp,If no date is specified new data is selected
| signal | Optional | Signal Name of the signal to be transferred, List of the signals separated by “;” or “ALL” for all signals | If no  signal is selected all signals are selected
| page | Optional | page number 0-n | If there is no page specified, service will return the first page. | In result there will be information how many pages are there for specific period of time and signals.	Page range is form 0 till "page_count" in result set.


### Signal POST Method
Request Method: POST (all options)
https://`_EVA+ URL_`/EvaCloudAPI/getSignals

Request Header Data
```
{
	"Authorization": token,
	"vehicleId": "3002",
	"recorderId": "5004354",
	"startTime": "201912301234",
	"endTime": "201912301239",
	"signals": ["Speed", "Distance", "B1_0105-BBR_ATPIsol"],
	"page": "0"
}
```

 
### Signal Response

Last element in array of result set is "page_count" that give information how many page are there for specific date time range and signal.
```
[
  {
    "conf_issue": "Class3000_Adelaide:01",
    "teloc_file": "3001_20191002-212549-212554-16929.tel/INT_TDATA",
    "teloc_memory": "T_3000_GUM_INT_TDATA",
    "time": "2019-10-03 06:47:49:162",
    "distance": "55099.90349",
    "identifier": "DISTANCE",
    "value": "55099.90349",
    "name": "DISTANCE",
    "sourceId": "DISTANCE"
  },
  {
    "conf_issue": "Class3000_Adelaide:01",
    "teloc_file": "3001_20191002-212549-212554-16929.tel/INT_TDATA",
    "teloc_memory": "T_3000_GUM_INT_TDATA",
    "time": "2019-10-03 06:47:49:082",
    "distance": "55099.90349",
    "identifier": "DISTANCE",
    "value": "55099.90349",
    "name": "DISTANCE",
    "sourceId": "DISTANCE"
  },
  {
    "page_count": 5
  }
]
```
EVA+ does not store information regarding the state of transfer. Therefore, EVA+ cannot be queried for the newest data which was not previously transferred.


 
##	Event Querying

| Parameter	| Optional	| Argument	| Comment
| --- | --- | --- | --- |
| vehicleId | optional | Vehicle ID as stored in the TELOC.	
| startTime | mandatory | (yyyyMMddHHmm) | Data from this timestamp will be retrieved, including this timestamp.  	If no date is specified only limit is taken into consideration.
| endTime | mandatory | (yyyyMMddHHmm) | Data up to this timestamp will be retrieved, including this timestamp. 	If no date is specified new data is selected
|  event | optional | Event Type to be transferred, List of the events separated by “;” or “ALL” for all events.	If no event is selected all events are selected
| page | optional | page >= 0 | If there is no page specify, service will give back first page. In result there will be information how many pages are there for specific period of time and events.	Page range is form 0 till "page_count" in result set.


### Events POST Method
Request Method: **POST** (all options)
https://`_EVA+ URL_`/EvaCloudAPI/getEvents

Request Header Data
```
{
	  vehicleId : "3001",
	  startTime: "201910020000",
	  endTime: "201910020600",
	  events: ["Door_Fault"],
	  page: "0"
}
```
Both cases get and post when signal list is empty, api will give all signal for that vehicle, recorder and time.

* parameter "events": ["ALL"] will give all event types in result set.


 
Event Response:
```
[
  {
    "teloc_file": "3001_20191001-185124-185129-16818.tel/INT_TDATA",
    "vehicle_id": "3001",
    "time": "1569946478382",
    "time_str": "02-10-2019 01:44:38",
    "event_type": "Door_Fault",
    "event_desc": "Door Fault D1",
    "driver_id": "",
    "route_code": "",
    "gps_str": "lon=138.593075867-lat=-34.921256517"
  },
  {
    "teloc_file": "3001_20191001-185124-185129-16818.tel/INT_TDATA",
    "vehicle_id": "3001",
    "time": "1569955360336",
    "time_str": "02-10-2019 04:12:40",
    "event_type": "Door_Fault",
    "event_desc": "Door Fault D5",
    "driver_id": "",
    "route_code": "",
    "gps_str": "lon=138.593075867-lat=-34.921256517"
  },
  {
    "page_count": 0
  }
]
```

# Tutorials
## postman
tbd
## google docs
[google docs repository](https://github.com/EVAplusAPI/googledocs)
## R
[R repository](https://github.com/EVAplusAPI/R)
## Python
[Python repository](https://github.com/EVAplusAPI/python)



