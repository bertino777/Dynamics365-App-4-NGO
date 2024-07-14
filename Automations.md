# Automations

As part of automations, Power automate flows and plug-ins have been built.

## Power automate flows

![](images/flow_zoom.png) 

### 1. Create Zoom Meeting

This flow help users to create a Zoom meeting directly from dynamics 365.
From Event in Dynamics 365, we click on the new button to add a new event; we should specify the event type is zoom and then the system will display fields and tabs related to zoom event creation.

![](images/new_meeting.png) 

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

Once the meeting is created, main information reagrding the meeting are reflecting in the event record in Dynamics 365.

![](images/meeting_created_.png) 


### 2. Get zoom registration link

After the meeting is created, we can update details to specifiy the registration is mandatory; then a registration link will be generated and we can run an automation to retrieve that link in Dynamics 365.

![](images/get_registration_link.png) 

#### Global flow
![](images/get_zoom_registration_link.png) 

#### Detailed flow

| - | - |
|---|---|
| ![](/images/registration_link_manual.png) | ![](/images/event_type_get_reg_link.png)  |
| ![](/images/meeting_id_reg_link.png) | ![](/images/event_id_reg_link.png) |
| ![](/images/http_body_reg_link.png) | ![](/images/array_var_reg_link.png) |
| ![](/images/token_reg_link.png) | ![](/images/http_body_val_reg_link.png) |
| ![](/images/token_extracted_reg_link.png) | ![](/images/xxx.png) |
| ![](/images/condition_reg_link.png) | ![](/images/get_meeting_reg_link.png) |
| ![](/images/compose_reg_link.png) | ![](/images/condition2_reg_link.png) |
| ![](/images/update_row_reg_link.png) | ![](/imagesxxx.png) |

### 3. ZoomUpdated

This flow help users to get in Dynamics 365 information about registrants, participants to a zoom meeting.
By clicking on a flown button the user can run the flow and the below will happen:

- Power automate will connect on Zoom API to retrieve registrants to  the Zoom meeting and they will be displayed under Zoom registrants tab on the event record
- Power automate will connect on Zoom API to retrieve participants to  the Zoom meeting and they will be displayed under Zoom registrants tab on the event record
- Registrants who are both registrants and participants will be displayed under Zoom registrants & participants tab under event record
- All registrants/participants who are not leads nor contacts will be created as lead in the CRM
- All registrants/participants who are contacts will be updated in the CRM and records about details updated will be created under Contact Updates


#### Flow

| - | - |
|---|---|
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_2.png)  |
| ![](/images/zoom_updated_3.png) | ![](/images/zoom_updated_4.png)  |
| ![](/images/zoom_updated_5.png) | ![](/images/zoom_updated_6.png)  |
| ![](/images/zoom_updated_7.png) | ![](/images/zoom_updated_8.png)  |
| ![](/images/zoom_updated_9.png) | ![](/images/zoom_updated_10.png)  |
| ![](/images/zoom_updated_11.png) | ![](/images/zoom_updated_12.png)  |
| ![](/images/zoom_updated_13.png) | ![](/images/zoom_updated_14.png)  |
| ![](/images/zoom_updated_15.png) | ![](/images/zoom_updated_16.png)  |
| ![](/images/zoom_updated_17.png) | ![](/images/zoom_updated_18.png)  |
| ![](/images/zoom_updated_19.png) | ![](/images/zoom_updated_20.png)  |
| ![](/images/zoom_updated_21.png) | ![](/images/zoom_updated_22.png)  |
| ![](/images/zoom_updated_23.png) | ![](/images/zoom_updated_24.png)  |
| ![](/images/zoom_updated_25.png) | ![](/images/zoom_updated_26.png)  |
| ![](/images/zoom_updated_27.png) | ![](/images/zoom_updated_28.png)  |
| ![](/images/zoom_updated_29.png) | ![](/images/zoom_updated_30.png)  |
| ![](/images/zoom_updated_31.png) | ![](/images/zoom_updated_32.png)  |
| ![](/images/zoom_updated_33.png) | ![](/images/zoom_updated_34.png)  |
| ![](/images/zoom_updated_35.png) | ![](/images/zoom_updated_36.png)  |
| ![](/images/zoom_updated_37.png) | ![](/images/zoom_updated_38.png)  |
| ![](/images/zoom_updated_39.png) | ![](/images/zoom_updated_40.png)  |
| ![](/images/zoom_updated_41.png) | ![](/images/zoom_updated_42.png)  |
| ![](/images/zoom_updated_43.png) | ![](/images/zoom_updated_44.png)  |
| ![](/images/zoom_updated_45.png) | ![](/images/zoom_updated_46.png)  |
| ![](/images/zoom_updated_47.png) | ![](/images/zoom_updated_48.png)  |
| ![](/images/zoom_updated_49.png) | ![](/images/zoom_updated_50.png)  |
| ![](/images/zoom_updated_51.png) | ![](/images/zoom_updated_52.png)  |
| ![](/images/zoom_updated_53.png) | ![](/images/zoom_updated_54.png)  |
| ![](/images/zoom_updated_55.png) | ![](/images/zoom_updated_56.png)  |
| ![](/images/zoom_updated_57.png) | ![](/images/zoom_updated_58.png)  |
| ![](/images/zoom_updated_59.png) | ![](/images/zoom_updated_60.png)  |
| ![](/images/zoom_updated_61.png) | ![](/images/zoom_updated_62.png)  |
| ![](/images/zoom_updated_63.png) | ![](/images/zoom_updated_64.png)  |
| ![](/images/zoom_updated_65.png) | ![](/images/zoom_updated_66.png)  |
| ![](/images/zoom_updated_67.png) | ![](/images/zoom_updated_68.png)  |
| ![](/images/zoom_updated_69.png) | ![](/images/zoom_updated_70.png)  |
| ![](/images/zoom_updated_71.png) | ![](/images/zoom_updated_72.png)  |
| ![](/images/zoom_updated_73.png) | ![](/images/zoom_updated_74.png)  |
| ![](/images/zoom_updated_75.png) | ![](/images/zoom_updated_76.png)  |
| ![](/images/zoom_updated_77.png) | ![](/images/zoom_updated_78.png)  |
| ![](/images/zoom_updated_79.png) | ![](/images/zoom_updated_80.png)  |
| ![](/images/zoom_updated_81.png) | ![](/images/zoom_updated_82.png)  |
| ![](/images/zoom_updated_83.png) | ![](/images/zoom_updated_84.png)  |
| ![](/images/zoom_updated_85.png) | ![](/images/zoom_updated_86.png)  |
| ![](/images/zoom_updated_87.png) | ![](/images/zoom_updated_88.png)  |
| ![](/images/zoom_updated_89.png) | ![](/images/zoom_updated_90.png)  |
| ![](/images/zoom_updated_91.png) | ![](/images/zoom_updated_92.png)  |
| ![](/images/zoom_updated_93.png) | ![](/images/zoom_updated_94.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |
| ![](/images/zoom_updated_1.png) | ![](/images/zoom_updated_1.png)  |




 
## C# Plug-Ins
