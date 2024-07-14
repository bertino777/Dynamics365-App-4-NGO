# Automations

As part of automations, Power automate flows and plug-ins have been built.

## Power automate flows

![](images/flow_zoom.png) 

### 1. Create Zoom Meeting

This flow help users to create a Zoom meeting directly from dynamics 365.
From Event in Dynamics 365, we click on the new button to add a new event; we should specify the event type is zoom and then the system will display fields and tabs related to zoom event creation.


#### Global flow
![](images/create_zoom_meeting_global.png) 

#### Detailed flow
![](images/zoom_trigger.png) 
```
{
  "type": "InitializeVariable",
  "inputs": {
    "variables": [
      {
        "name": "http body",
        "type": "object"
      }
    ]
  },
  "runAfter": {},
  "metadata": {
    "operationMetadataId": "0d45e7f9-10f1-436b-9080-129d0fab2c47"
  }
}
```
![](images/create_zoom_meeting_var1.png) 

```
{
  "type": "InitializeVariable",
  "inputs": {
    "variables": [
      {
        "name": "http body",
        "type": "object"
      }
    ]
  },
  "runAfter": {},
  "metadata": {
    "operationMetadataId": "0d45e7f9-10f1-436b-9080-129d0fab2c47"
  }
}
```

![](images/create_zoom_meeting_var2.png) 

```
{
  "type": "InitializeVariable",
  "inputs": {
    "variables": [
      {
        "name": "zoomAccountValue",
        "type": "string"
      }
    ]
  },
  "runAfter": {
    "http_body": [
      "Succeeded"
    ]
  },
  "metadata": {
    "operationMetadataId": "d0c5f62a-2e86-40ff-929f-ff7144c012eb"
  }
}
```

![](images/zoom_token.png) 

```
{
  "type": "Http",
  "inputs": {
    "uri": "https://zoom.us/oauth/token",
    "method": "POST",
    "headers": {
      "Authorization": "Basic SndXbHNXY2RTZmlGUkIIOK5525JhhhjKK3ojKIDIJiIIS55J2NXQjlQQnYxTlo1Q0xYNzE5Vg=="
    },
    "queries": {
      "grant_type": "account_credentials",
      "account_id": "jX3wqUSPS_-kijSH54jj-A"
    }
  },
  "runAfter": {
    "zoomAccountValue": [
      "Succeeded"
    ]
  },
  "metadata": {
    "operationMetadataId": "43d51270-0389-452e-b725-035622a38e5b"
  }
}
```

![](images/store_token_body.png) 

```
{
  "type": "SetVariable",
  "inputs": {
    "name": "http body",
    "value": "@body('Getting_token')"
  },
  "runAfter": {
    "Getting_token": [
      "Succeeded"
    ]
  },
  "metadata": {
    "operationMetadataId": "5d3e88f0-c2c0-40fd-bd64-7819881b1a36"
  }
}
```
![](images/token_value.png) 

```
{
  "type": "Compose",
  "inputs": "@variables('http body')['access_token']",
  "runAfter": {
    "Set_http_body": [
      "Succeeded"
    ]
  },
  "metadata": {
    "operationMetadataId": "e5ff2171-a0c3-439c-bf6d-af93a3315cc0"
  }
}
```

![](images/zoom_account.png) 

```
{
  "type": "Compose",
  "inputs": "@triggerOutputs()?['body/ren_zoomaccount']",
  "runAfter": {
    "token_value": [
      "Succeeded"
    ]
  },
  "metadata": {
    "operationMetadataId": "c9588f80-1fd1-4d61-a71f-ba65cb3af0ac"
  }
}
```

![](images/create_zoom_switch.png) 
![](images/create_zoom_switch_detail.png) 

```
{
  "type": "Switch",
  "expression": "@outputs('zoom_account')",
  "default": {
    "actions": {
      "Set_variable_3": {
        "type": "SetVariable",
        "inputs": {
          "name": "zoomAccountValue",
          "value": "me"
        },
        "metadata": {
          "operationMetadataId": "6d2789ef-a260-40c4-b6c6-6c18303eb7ed"
        }
      }
    }
  },
  "cases": {
    "Case": {
      "actions": {
        "Set_zoomAccountValue": {
          "type": "SetVariable",
          "inputs": {
            "name": "zoomAccountValue",
            "value": "secretariat@ren21.net"
          },
          "metadata": {
            "operationMetadataId": "8afd31f5-7f19-438c-913f-684dcd1e37c8"
          }
        }
      },
      "case": 910190000
    },
    "Case_2": {
      "actions": {
        "Set_variable": {
          "type": "SetVariable",
          "inputs": {
            "name": "zoomAccountValue",
            "value": "community@ren21.net"
          },
          "metadata": {
            "operationMetadataId": "a0aab98b-136f-4446-afeb-6cd28f91270d"
          }
        }
      },
      "case": 910190001
    },
    "Case_3": {
      "actions": {
        "Set_variable_2": {
          "type": "SetVariable",
          "inputs": {
            "name": "zoomAccountValue",
            "value": "gsr@ren21.net"
          },
          "metadata": {
            "operationMetadataId": "29c71f18-328d-4f80-9eba-540a0a88214f"
          }
        }
      },
      "case": 910190002
    }
  },
  "runAfter": {
    "zoom_account": [
      "Succeeded"
    ]
  },
  "metadata": {
    "operationMetadataId": "77690a38-9cc2-43dc-a4fb-7dd8ee890181"
  }
}
```
![](images/create_zoom_switch_case.png) 
![](images/create_zoom_switch_case_set.png) 

```
{
  "type": "SetVariable",
  "inputs": {
    "name": "zoomAccountValue",
    "value": "secretariat@ren21.net"
  },
  "metadata": {
    "operationMetadataId": "8afd31f5-7f19-438c-913f-684dcd1e37c8"
  }
}
```

![](images/create_zoom_switch_case2.png) 
![](images/create_zoom_switch_case2_set.png) 

```
{
  "type": "SetVariable",
  "inputs": {
    "name": "zoomAccountValue",
    "value": "community@ren21.net"
  },
  "metadata": {
    "operationMetadataId": "a0aab98b-136f-4446-afeb-6cd28f91270d"
  }
}
```

![](images/create_zoom_switch_case3.png) 
![](images/create_zoom_switch_case3_set.png) 

```
{
  "type": "SetVariable",
  "inputs": {
    "name": "zoomAccountValue",
    "value": "gsr@ren21.net"
  },
  "metadata": {
    "operationMetadataId": "29c71f18-328d-4f80-9eba-540a0a88214f"
  }
}
```

![](images/create_zoom_switch_case_default_set.png) 

```
{
  "type": "SetVariable",
  "inputs": {
    "name": "zoomAccountValue",
    "value": "me"
  },
  "metadata": {
    "operationMetadataId": "6d2789ef-a260-40c4-b6c6-6c18303eb7ed"
  }
}
```


### 2. ZoomUpdated

This flow help users to get in Dynamics 365 information about registrants, participants to a zoom meeting.
By clicking on a button the user can run the flow and the below will happen:

- 


#### Global flow
![](images/xxx.png) 

#### Detailed flow
 
## C# Plug-Ins
