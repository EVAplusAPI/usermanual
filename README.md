# EVA+ API usermanual
Abstract
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
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJJc3N1ZWRBdCI6MTU4MTU4NjMyNSwicm9sZXMiOiJBZG1pbiIsIm5hbWUiOiJndWVzdCIsInZlaGljbGVzIjoiIiwibGFuZyI6IkVOIiwiYWN0aW9ucyI6IiIsImVtYWlsIjoiZ3Vlc3RAZ21haWwuY29tIn0.EOmOanRY7Bg-hzY914yUM1rNY05ZhLAXZ06v_3rlae4"
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
Header key: Authorization
Header Value: Token

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
Header key: Authorization
Header Value: Token

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
Header key: Authorization
Header Value: Token


