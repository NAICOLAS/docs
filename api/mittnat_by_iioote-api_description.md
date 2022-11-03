# API description - "MittNät" (by iioote)

MittNät by iioote (Thingsboard)

Complete API documentation (Thingsboards Swagger) is available here: [https://demo.thingsboard.io/swagger-ui/#/](https://demo.thingsboard.io/swagger-ui/#/)

Below follows instructions for fetching data from all available sensors.

Python3 is used.

#

## Python libraries used in the examples below
```Python
import json
import requests
```

#

## **Authorization (POST)**

The **_payload_** variable must be JSON encoded.

Login and get authorization tokens by sending a request to:
```Python
user_email = "example@exampleemail.xxx"
user_password = "ThisPasswordIsSoS3CuR3!"

url = "https://iot.mittnat.nu/api/auth/login"

headers = {
    "Accept": "application/json",
    "Content-Type": "application/json"
}

payload = {
    "username": str(user_email),
    "password": str(user_password)
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

tokens = json.loads(response.text)
```
### Response data (parsed):
```Python
tokens: {
    token: "ACCESS_TOKEN",
    refreshToken: "REFRESH_TOKEN"
}

ACCESS_TOKEN = tokens.get("token") # Access token
REFRESH_TOKEN = tokens.get("refreshToken") # Refresh token
```

#

## **Get devices (GET)**

Get all devices and their id's by sending a request to:

```Python
url = "https://iot.mittnat.nu/api/user/devices" + "?page=0&pageSize=100"

headers = {
    "Accept": "application/json",
    "X-Authorization": "Bearer: " + str(ACCESS_TOKEN)
}

response = requests.post(url, headers=headers)

devices = json.loads(response.text)
```

### Response data (parsed):
```Python
devices: {
    "data": [
        {
        "id": {
            "id": "784f394c-42b6-435a-983c-b7beff2784f9",
            "entityType": "DEVICE"
        },
        "createdTime": 1609459200000,
        "tenantId": {
            "id": "784f394c-42b6-435a-983c-b7beff2784f9",
            "entityType": "TENANT"
        },
        "customerId": {
            "id": "784f394c-42b6-435a-983c-b7beff2784f9",
            "entityType": "CUSTOMER"
        },
        "name": "A4B72CCDFF33",
        "type": "Temperature Sensor",
        "label": "Room 234 Sensor",
        "deviceProfileId": {
            "id": "784f394c-42b6-435a-983c-b7beff2784f9",
            "entityType": "DEVICEPROFILE"
        },
        "deviceData": {
        "configuration": {},
        "transportConfiguration": {}
        },
        "additionalInfo": {}
        }
    ],
    "totalPages": 0,
    "totalElements": 0,
    "hasNext": false
}
```

#

## **Get timeseries data (GET)**

Get the latest timeseries data values for Temperature and Humidity by sending request to:

```Python
entity_id = "784f394c-42b6-435a-983c-b7beff2784f9" # EXAMPLE ID
entity_type = "DEVICE" # Entity type

url = "https://iot.mittnat.nu/api/plugins/telemetry/" + str(entity_type) + "/" + str(entity_id) + "/values/timeseries" + "?keys=Temperature,Humidity"

headers = {
    "Accept": "application/json",
    "X-Authorization": "Bearer: " + str(ACCESS_TOKEN)
}

response = requests.post(url, headers=headers)

devices = json.loads(response.text)

```
### Response data (parsed):
```Python
data: {
    "Temperature": [{ "value": "value", "ts": 1609459200000}],
    "Humidity": [{ "value": "false", "ts": 1609459200000}]
}
```

#
