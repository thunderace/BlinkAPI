---
title: Blink Camera API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

search: true
---
This is unofficial documentation for the Client API of the Blink Wire-Free HD Home Monitoring & Alert System based on [BlinkMonitorProtocol](https://github.com/MattTW/BlinkMonitorProtocol) and [Blink](https://github.com/keredson/blink).

I am not affiliated with the company in any way - this documentation is strictly "AS-IS". My goal was to uncover enough to arm and disarm the system programatically so that I can issue those commands in sync with my home alarm system arm/disarm. Just some raw notes at this point but should be enough for creating programmatic APIs. Lots more to be discovered and documented - feel free to contribute!

# Highlighted APIs

## Login

Client login to the Blink Servers.

> Request:

```shell
curl -X POST \
  https://rest.prod.immedia-semi.com/login \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'email=youremail&password=yourpassword&client_specifier=%22iPhone%209.2%20%7C%202.2%20%7C%20222%22'
```

```python
import requests

url = "https://rest.prod.immedia-semi.com/login"

payload = "email=youremail&password=yourpassword&client_specifier=%22iPhone%209.2%20%7C%202.2%20%7C%20222%22"
headers = {
    'Host': "rest.prod.immedia-semi.com",
    'Content-Type': "application/x-www-form-urlencoded",
    }

response = requests.request("POST", url, data=payload, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.prod.immedia-semi.com/login",
  "method": "POST",
  "headers": {
    "Host": "rest.prod.immedia-semi.com",
    "Content-Type": "application/x-www-form-urlencoded",
  },
  "data": {
    "email": "youremail",
    "password": "yourpassword",
    "client_specifier": "\"iPhone 9.2 | 2.2 | 222\""
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

```json
{
    "authtoken": {
        "authtoken": "yourauthtoken",
        "message": xxx
    },
    "networks": {
        "networkid": {
            "name": xxx
            "onboarded": xxx
        }
    },
    "region": {
        "yourregion": xxx
    }
}
```

**HTTP Request**

`POST https://rest.prod.immedia-semi.com/login`

**HEADERS**

Parameter | Default
--------- | -----------
Content-Type | application/x-www-form-urlencoded

**Body Parameters**

Parameter | Description
--------- | -----------
email | Your Email |                            
password  | Your Password |              
client_specifier | Client Name

**Response**
JSON response containing information including AuthToken, Network ID and Region.

<aside class="notice">
<code>yourauthtoken</code> and <code>yourregion</code> are passed in a header in future calls.
</aside>

## Get Network

Obtain information about the Blink networks defined for the logged in user.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/networks \
  -H 'TOKEN_AUTH: {{yourtoken}}'
  ```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/networks"

headers = {
    'TOKEN_AUTH': "{{yourtoken}}",
    }

response = requests.request("GET", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/networks",
  "method": "GET",
  "headers": {
    "TOKEN_AUTH": "{{yourtoken}",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

```json
{
    "summary": {
        "yournetworkid": {
            "name": xxx
            "onboarded": xxx
        }
    },
    "networks": [
        {
            "id": yournetworkid,
            "created_at": xxx
            "updated_at": xxx
            "name": xxx
            "network_key": xxx
            "description": xxx
            "network_origin": xxx
            "locale": xxx
            "time_zone": xxx
            "dst": xxx
            "ping_interval": xxx
            "encryption_key": xxx
            "armed": xxx
            "autoarm_geo_enable": xxx
            "autoarm_time_enable": xxx
            "lv_mode": xxx
            "lfr_channel": xxx
            "video_destination": xxx
            "storage_used": xxx
            "storage_total": xxx
            "video_count": xxx
            "video_history_count": xxx
            "arm_string": xxx
            "busy": xxx
            "camera_error": xxx
            "sync_module_error": xxx
            "feature_plan_id": xxx
            "account_id": xxx
        }
    ]
}
```

**HTTP Request**

`GET https://rest.{{yourregion}}.immedia-semi.com/networks`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

<aside class="notice">
<code>yournetworkid</code> value might be used in some future calls.
</aside>

**Response** 
JSON response containing network information.


## Get Cameras

Obtain cameras information for users.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/cameras \
  -H 'TOKEN_AUTH: {{yourtoken}}'
  ```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/cameras"

headers = {
    'TOKEN_AUTH': "{{yourtoken}}",
    }

response = requests.request("GET", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/cameras",
  "method": "GET",
  "headers": {
    "TOKEN_AUTH": "{{yourtoken}",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

```json
{
    "devicestatus": [
        {
            "camera_id": yourcameraid,
            "created_at": xxx
            "updated_at": xxx
            "wifi_strength": xxx
            "lfr_strength": xxx
            "battery_voltage": xxx
            "temperature": xxx
            "fw_version": xxx
            "mac": xxx
            "ipv": xxx
            "ip_address": xxx
            "error_codes": xxx
            "battery_alert_status": xxx
            "temp_alert_status": xxx
            "ac_power": xxx
            "light_sensor_ch0": xxx
            "light_sensor_ch1": xxx
            "light_sensor_data_valid": xxx
            "light_sensor_data_new": xxx
            "time_first_video": xxx
            "time_108_boot": xxx
            "time_wlan_connect": xxx
            "time_dhcp_lease": xxx
            "time_dns_resolve": xxx
            "lfr_108_wakeups": xxx
            "total_108_wakeups": xxx
            "lfr_tb_wakeups": xxx
            "total_tb_wakeups": xxx
            "wifi_connect_failure_count": xxx
            "dhcp_failure_count": xxx
            "socket_failure_count": xxx
            "dev_1": xxx
            "dev_2": xxx
            "dev_3": xxx
            "unit_number": xxx
            "serial": xxx
            "sync_module_id": xxx
            "network_id": xxx
            "account_id": xxx
            "id": xxx
            "deleted_at": xxx
            "camera_key": xxx
            "mac_address": xxx
            "thumbnail": xxx
            "name": xxx
            "liveview_enabled": xxx
            "siren_enable": xxx
            "siren_volume": xxx
            "onboarded": xxx
            "motion_sensitivity": xxx
            "enabled": xxx
            "alert_tone_enable": xxx
            "alert_tone_volume": xxx
            "alert_repeat": xxx
            "alert_interval": xxx
            "video_length": xxx
            "temp_alarm_enable": xxx
            "temp_interval": xxx
            "temp_adjust": xxx
            "temp_min": xxx
            "temp_max": xxx
            "temp_hysteresis": xxx
            "illuminator_enable": xxx
            "illuminator_duration": xxx
            "illuminator_intensity": xxx
            "battery_alarm_enable": xxx
            "battery_voltage_interval": xxx
            "battery_voltage_threshold": xxx
            "battery_voltage_hysteresis": xxx
            "last_battery_alert": xxx
            "battery_alert_count": xxx
            "lfr_sync_interval": xxx
            "video_50_60hz": xxx
            "invert_image": xxx
            "flip_image": xxx
            "record_audio_enable": xxx
            "clip_rate": xxx
            "liveview_rate": xxx
            "max_resolution": xxx
            "auto_test": xxx
            "wifi_timeout": xxx
            "retry_count": xxx
            "status": xxx
            "a1": xxx
            "last_temp_alert": xxx
            "temp_alert_count": xxx
            "last_wifi_alert": xxx
            "wifi_alert_count": xxx
            "last_lfr_alert": xxx
            "lfr_alert_count": xxx
            "last_offline_alert": xxx
            "offline_alert_count": xxx
            "temp_alert_state": xxx
            "battery_state": xxx
            "battery_check_time": xxx
            "motion_regions": xxx
        }
    ]
}
```

**HTTP Request**

`GET https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/cameras`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response** 
JSON response containing camera information.

<aside class="notice">
<code>yourcameraid</code> value might be used in some future calls.
</aside>

## Get Home Screen

Return information displayed on the home screen of the mobile client.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/homescreen \
  -H 'TOKEN_AUTH: {{yourtoken}}'
  ```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/homescreen"

headers = {
    'TOKEN_AUTH': "{{yourtoken}}",
    }

response = requests.request("GET", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/homescreen",
  "method": "GET",
  "headers": {
    "TOKEN_AUTH": "{{yourtoken}",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

```json
{
    "account": {
        "notifications": xxx
    },
    "network": {
        "name": xxx
        "wifi_strength": xxx
        "status": xxx
        "armed": xxx
        "notifications": xxx
        "warning": xxx
        "enable_temp_alerts": xxx
        "error_msg": xxx
    },
    "devices": [
        {
            "device_type": xxx
            "device_id": xxx
            "updated_at": xxx
            "name": xxx
            "thumbnail": xxx
            "active": xxx
            "notifications": xxx
            "warning": xxx
            "error_msg": xxx
            "status": xxx
            "enabled": xxx
            "armed": xxx
            "errors": xxx
            "wifi_strength": xxx
            "lfr_strength": xxx
            "temp": xxx
            "battery": xxx
            "battery_state": xxx
            "usage_rate": xxx
        },
        {
            "device_type": xxx
            "device_id": xxx
            "updated_at": xxx
            "notifications": xxx
            "warning": xxx
            "error_msg": xxx
            "status": xxx
            "errors": xxx
            "last_hb": xxx
        }
    ]
}
```

**HTTP Request**

`GET https://rest.{{yourregion}}.immedia-semi.com/homescreen`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response**
JSON response containing information that the mobile client displays on the home page, including: status, armed state, links to thumbnails for each camera, etc.


## Download Thumbnail

Download the thumbnail for a camera.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/{{yourthumbnail}}.jpg \
  -H 'TOKEN_AUTH: {{yourtoken}}'
  ```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/{{yourthumbnail}}.jpg"

headers = {
    'TOKEN_AUTH': "{{yourtoken}}",
    }

response = requests.request("GET", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/{{yourthumbnail}}.jpg",
  "method": "GET",
  "headers": {
    "TOKEN_AUTH": "{{yourtoken}",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

**HTTP Request**

`GET https://rest.{{yourregion}}.immedia-semi.com/{{yourthumbnail}}.jpg`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response**
Thumbnail binary data. 


## Get Videos

Gets a paginated set of video information.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/page/{{pagenumber}} \
  -H 'TOKEN_AUTH: {{yourtoken}}'
  ```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/page/{{pagenumber}}"

headers = {
    'TOKEN_AUTH': "{{yourtoken}}",
    }

response = requests.request("GET", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/page/{{pagenumber}}",
  "method": "GET",
  "headers": {
    "TOKEN_AUTH": "{{yourtoken}",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

A list of video clips in json format, with 10 videos in one page.

```json
[
    {
        "id": yourvideoid,
        "created_at": xxx
        "updated_at": xxx
        "camera_name": xxx
        "description": xxx,
        "length": xxx
        "size": xxx
        "upload_time": xxx
        "viewed": xxx
        "locked": xxx
        "ready": xxx
        "encryption": xxx
        "encryption_key": xxx
        "storage_location": xxx
        "thumbnail": "yourthumbnail",
        "address": "yourvideo",
        "account_id": xxx
        "network_id": xxx
        "camera_id": xxx
        "event_id": xxx
        "partial": xxx
        "network_name": xxx
        "time_zone": xxx
    },
    ...
]
```

**HTTP Request**

`GET https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/page/{{pagenumber}}`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response** 
JSON response containing a set of video information, including: camera name, creation time, thumbnail URI, size, length.

<aside class="notice">
If <code>pagenumber</code> is 0, you will retrieve some latest video clips.
If <code>pagenumber</code> is larger than 0, you will retrieve some earlier video clips.
</aside>

## Download Video

Download one video clip.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/{{yourvideo}} \
  -H 'TOKEN_AUTH: {{yourtoken}}'
  ```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/{{yourvideo}}"

headers = {
    'TOKEN_AUTH': "{{yourtoken}}",
    }

response = requests.request("GET", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/{{yourvideo}}",
  "method": "GET",
  "headers": {
    "TOKEN_AUTH": "{{yourtoken}",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

**HTTP Request**

`GET https://rest.{{yourregion}}.immedia-semi.com/{{yourvideo}}`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response**
Video binary data.

## Get Video Count

Get total number of videos.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/count \
  -H 'TOKEN_AUTH: {{yourtoken}}'
  ```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/count"

headers = {
    'TOKEN_AUTH': "{{yourtoken}}",
    }

response = requests.request("GET", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/count",
  "method": "GET",
  "headers": {
    "TOKEN_AUTH": "{{yourtoken}",
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

```json
{
    "count": xxx
}
```

**HTTP Request**

`GET https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/count`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response** 
JSON response containing the total video count.

## Capture Thumbnail

Capture camera thumbnails.

> Request:

```shell
curl -X POST \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/thumbnail \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/thumbnail"

headers = {
    'TOKEN_AUTH': "{{yourauthtoken}}"    
}


response = requests.request("POST", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/thumbnail",
  "method": "POST",
  "headers": {
    "TOKEN_AUTH": "{{yourauthtoken}}"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

```json
{
    "id": xxx,
    "created_at": xxx
    "updated_at": xxx
    "execute_time": xxx
    "command": xxx
    "state_stage": xxx
    "stage_rest": xxx
    "stage_cs_db": xxx
    "stage_cs_sent": xxx
    "stage_sm": xxx
    "stage_dev": xxx
    "stage_is": xxx
    "stage_lv": xxx
    "stage_vs": xxx
    "state_condition": xxx
    "sm_ack": xxx
    "lfr_ack": xxx
    "sequence": xxx
    "attempts": xxx
    "transaction": xxx
    "player_transaction": xxx
    "server": xxx
    "duration": xxx
    "by_whom": xxx
    "diagnostic": xxx
    "debug": xxx
    "parent_command_id": xxx
    "camera_id": xxx
    "siren_id": xxx
    "firmware_id": xxx
    "network_id": xxx
    "account_id": xxx
    "sync_module_id": xxx
}
```

**HTTP Request**

`POST https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/thumbnail`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response** 
Command information.

## Capture Videos

Capture new camera videos.

> Request:

```shell
curl -X POST \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/clip \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

```python
import requests

url = "https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/clip"

headers = {
    'TOKEN_AUTH': "{{yourauthtoken}}"
    }

response = requests.request("POST", url, headers=headers)

```


```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/clip",
  "method": "POST",
  "headers": {
    "TOKEN_AUTH": "{{yourauthtoken}}"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Sample response:

```json
{
    "id": xxx,
    "created_at": xxx
    "updated_at": xxx
    "execute_time": xxx
    "command": xxx
    "state_stage": xxx
    "stage_rest": xxx
    "stage_cs_db": xxx
    "stage_cs_sent": xxx
    "stage_sm": xxx
    "stage_dev": xxx
    "stage_is": xxx
    "stage_lv": xxx
    "stage_vs": xxx
    "state_condition": xxx
    "sm_ack": xxx
    "lfr_ack": xxx
    "sequence": xxx
    "attempts": xxx
    "transaction": xxx
    "player_transaction": xxx
    "server": xxx
    "duration": xxx
    "by_whom": xxx
    "diagnostic": xxx
    "debug": xxx
    "parent_command_id": xxx
    "camera_id": xxx
    "siren_id": xxx
    "firmware_id": xxx
    "network_id": xxx
    "account_id": xxx
    "sync_module_id": xxx
}
```

**HTTP Request**

`POST https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/clip`

**HEADERS**

Parameter | Default
--------- | -----------
TOKEN_AUTH | yourauthtoken

**Response** 
Command information.

# Other APIs

## Get Sync Modules

Obtain information about the Blink Sync Modules on the given network.	

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/syncmodules \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing information about the known state of the Sync module, most notably if it is online

> Sample response:

```json
{
    "syncmodule": {
        "id": yourcommandid,
        "created_at": xxx
        "updated_at": xxx
        "name": xxx
        "fw_version": xxx
        "mac_address": xxx
        "ip_address": xxx
        "lfr_frequency": xxx
        "serial": xxx
        "status": xxx
        "onboarded": xxx
        "server": xxx
        "last_hb": xxx
        "os_version": xxx
        "last_wifi_alert": xxx
        "wifi_alert_count": xxx
        "last_offline_alert": xxx
        "offline_alert_count": xxx
        "table_update_sequence": xxx
        "feature_plan_id": xxx
        "account_id": xxx
        "network_id": xxx
        "wifi_strength": xxx
    }
}
```
    
## Enable Motion Event

Start recording/reporting motion events.

> Request:

```shell
curl -X POST \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/arm \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing information about the arm command request, including the command/request ID.

> Sample response:

```json
{
    "id": yourcommandid,
    "created_at": xxx
    "updated_at": xxx
    "execute_time": xxx
    "command": xxx
    "state_stage": xxx
    "stage_rest": xxx
    "stage_cs_db": xxx
    "stage_cs_sent": xxx
    "stage_sm": xxx
    "stage_dev": xxx
    "stage_is": xxx
    "stage_lv": xxx
    "stage_vs": xxx
    "state_condition": xxx
    "sm_ack": xxx
    "lfr_ack": xxx
    "sequence": xxx
    "attempts": xxx
    "transaction": xxx
    "player_transaction": xxx
    "server": xxx
    "duration": xxx
    "by_whom": xxx
    "diagnostic": xxx
    "debug": xxx
    "parent_command_id": xxx
    "camera_id": xxx
    "siren_id": xxx
    "firmware_id": xxx
    "network_id": xxx
    "account_id": xxx
    "sync_module_id": xxx
}
```


## Disable Motion Event	
Stop recording/reporting motion events.

> Request:

```shell
curl -X POST \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/disarm \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing information about the disarm command request, including the command/request ID

> Sample response:

```json
{
    "id": yourcommandid,
    "created_at": xxx
    "updated_at": xxx
    "execute_time": xxx
    "command": xxx
    "state_stage": xxx
    "stage_rest": xxx
    "stage_cs_db": xxx
    "stage_cs_sent": xxx
    "stage_sm": xxx
    "stage_dev": xxx
    "stage_is": xxx
    "stage_lv": xxx
    "stage_vs": xxx
    "state_condition": xxx
    "sm_ack": xxx
    "lfr_ack": xxx
    "sequence": xxx
    "attempts": xxx
    "transaction": xxx
    "player_transaction": xxx
    "server": xxx
    "duration": xxx
    "by_whom": xxx
    "diagnostic": xxx
    "debug": xxx
    "parent_command_id": xxx
    "camera_id": xxx
    "siren_id": xxx
    "firmware_id": xxx
    "network_id": xxx
    "account_id": xxx
    "sync_module_id": xxx
}
```

## Get Command Status	

Get status info on the given command.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/command/{{yourcommandid}} \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing state information of the given command, most notably whether it has completed and was successful.

> Sample response:

```json
{
    "complete": xxx
    "status": xxx
    "status_msg": xxx
    "status_code": xxx
    "commands": [
        {
            "id": xxx
            "created_at": xxx
            "updated_at": xxx
            "execute_time": xxx
            "command": xxx
            "state_stage": xxx
            "stage_rest": xxx
            "stage_cs_db": xxx
            "stage_cs_sent": xxx
            "stage_sm": xxx
            "stage_dev": xxx
            "stage_is": xxx
            "stage_lv": xxx
            "stage_vs": xxx
            "state_condition": xxx
            "sm_ack": xxx
            "lfr_ack": xxx
            "sequence": xxx
            "attempts": xxx
            "transaction": xxx
            "player_transaction": xxx
            "server": xxx
            "duration": xxx
            "by_whom": xxx
            "diagnostic": xxx
            "debug": xxx
            "parent_command_id": xxx
            "camera_id": xxx
            "siren_id": xxx
            "firmware_id": xxx
            "network_id": xxx
            "account_id": xxx
            "sync_module_id": xxx
        }
    ]
}
```

## Get Events From Sync Module	
Get events for a given network (sync module).

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/events/network/{{yournetworkid}}/ \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** A json list of evets incluing URL's. Replace the "mp4" with "jpg" extension to get the thumbnail of each clip

> Sample response:

```json
{
    "event": [
        {
            "id": xxx,
            "created_at": xxx
            "updated_at": xxx
            "type": xxx
            "notified": xxx
            "duration": xxx
            "status": xxx
            "command_id": xxx
            "camera_id": xxx
            "siren_id": xxx
            "sync_module_id": xxx
            "network_id": xxx
            "account_id": xxx
            "syncmodule": xxx
            "camera": xxx
            "siren": xxx
            "account": xxx
        },
        ...
    ]
}
```

## Get Video Info

Gets information for a specific video by ID.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/api/v2/video/{{yourvideoid}} \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```
**Response** JSON response containing video information.

> Sample response:

```json
[
    {
        "id": xxx,
        "created_at": xxx
        "updated_at": xxx
        "camera_name": xxx
        "description": xxx
        "length": xxx
        "size": xxx
        "upload_time": xxx
        "viewed": xxx
        "locked": xxx
        "ready": xxx
        "encryption": xxx
        "encryption_key": xxx
        "storage_location": xxx
        "thumbnail": xxx
        "address": xxx
        "account_id": xxx
        "network_id": xxx
        "camera_id": xxx
        "event_id": xxx
        "partial": xxx
        "network_name": xxx
        "time_zone": xxx
    }
]
```

## Get Unwatched Videos	

Gets a list of unwatched videos.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/api/v2/videos/unwatched/page/0 \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing unwatched video information.

> Sample response:


```json
[
    {
        "id": xxx,
        "created_at": xxx
        "updated_at": xxx
        "camera_name": xxx
        "description": xxx
        "length": xxx
        "size": xxx
        "upload_time": xxx
        "viewed": xxx
        "locked": xxx
        "ready": xxx
        "encryption": xxx
        "encryption_key": xxx
        "storage_location": xxx
        "thumbnail": xxx
        "address": xxx
        "account_id": xxx
        "network_id": xxx
        "camera_id": xxx
        "event_id": xxx
        "partial": xxx
        "network_name": xxx
        "time_zone": xxx
    },
    ...
]
```	

## Delete Video

Delete a video by ID

> Request:

```shell
curl -X POST \
  https://rest.prd2.immedia-semi.com/api/v2/video/{{yourvideoid}}/delete \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** Json response with message and code.

> Sample response:


```json
{
    "message": xxx,
    "code": xxx
}
```

## Delete All Videos

NOT TESTED.

```shell
curl -H "Host: prod.immedia-semi.com" -H "TOKEN_AUTH: authtoken from login" --data-binary --compressed https://prod.immedia-semi.com/api/v2/videos/deleteall
```

## Get One Camera
Gets information for one camera.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}} \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing camera information.

> Sample response:

```json
{
    "camera_status": {
        "camera_id": xxx,
        "created_at": xxx
        "updated_at": xxx
        "wifi_strength": xxx
        "lfr_strength": xxx
        "battery_voltage": xxx
        "temperature": xxx
        "fw_version": xxx
        "mac": xxx
        "ipv": xxx
        "ip_address": xxx
        "error_codes": xxx
        "battery_alert_status": xxx
        "temp_alert_status": xxx
        "ac_power": xxx
        "light_sensor_ch0": xxx
        "light_sensor_ch1": xxx
        "light_sensor_data_valid": xxx
        "light_sensor_data_new": xxx
        "time_first_video": xxx
        "time_108_boot": xxx
        "time_wlan_connect": xxx
        "time_dhcp_lease": xxx
        "time_dns_resolve": xxx
        "lfr_108_wakeups": xxx
        "total_108_wakeups": xxx
        "lfr_tb_wakeups": xxx
        "total_tb_wakeups": xxx
        "wifi_connect_failure_count": xxx
        "dhcp_failure_count": xxx
        "socket_failure_count": xxx
        "dev_1": xxx
        "dev_2": xxx
        "dev_3": xxx
        "unit_number": xxx
        "serial": xxx
        "sync_module_id": xxx
        "network_id": xxx
        "account_id": xxx
        "id": xxx
        "thumbnail": xxx
    }
}
```	

## Get Sensors For One Camera

Gets camera sensor information.

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/network/{{yournetworkid}}/camera/{{yourcameraid}}/signals \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing camera sensor information, such as wifi strength, temperature, and battery level.

> Sample response:


```json
{
    "lfr": xxx,
    "wifi": xxx,
    "updated_at": xxx,
    "temp": xxx,
    "battery": xxx
}
```	

## Get Clients
Gets information about devices that have connected to the blink service.	

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/account/clients \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing client information, including: type, name, connection time, user ID.

> Sample response:


```json
{
    "clients": [
        {
            "id": xxx
            "created_at": xxx
            "updated_at": xxx
            "name": xxx
            "notify_sound": xxx
            "client_type": xxx
            "client_specifier": xxx
            "notification_key": xxx
            "aws_key": xxx
            "os_version": xxx
            "device_identifier": xxx
            "app_version": xxx
            "unique_id": xxx
            "home": xxx
            "user_id": xxx
            "account_id": xxx
        },
        ...
    ]
}
```	


## Get Regions	
Gets information about supported regions.

> Request:

```shell
curl -X GET \
  https://rest.prod.immedia-semi.com/regions \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```

**Response** JSON response containing region information.

> Sample response:


```json
{
    "preferred": xxx
    "regions": {
        "us": {
            "dns": xxx
            "friendly_name": xxx
            "registration": xxx
        },
        "eu": {
            "dns": xxx
            "friendly_name": xxx
            "registration": xxx
        },
        "au": {
            "dns": xxx
            "friendly_name": xxx
            "registration": xxx
        },
        "sg": {
            "dns": xxx
            "friendly_name": xxx
            "registration": xxx
        }
    }
}
```	

## Get System Health	

Gets information about system health.	

<aside class="notice">NOT WORKING!</aside>


> Request:

```shell
curl -X GET \
  https://rest.prod.immedia-semi.com/health \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```


## Get Program Info

Gets information about programs

<aside class="notice">NO RESPONSE!</aside>

> Request:

```shell
curl -X GET \
  https://rest.{{yourregion}}.immedia-semi.com/api/v1/networks/{{yournetworkid}}/programs \
  -H 'TOKEN_AUTH: {{yourauthtoken}}'
```
