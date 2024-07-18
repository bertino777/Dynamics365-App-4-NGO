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
| ![](/images/zoom_updated_95.png) | ![](/images/zoom_updated_96.png)  |
| ![](/images/zoom_updated_97.png) | ![](/images/zoom_updated_98.png)  |
| ![](/images/zoom_updated_99.png) | ![](/images/zoom_updated_100.png)  |
| ![](/images/zoom_updated_101.png) | ![](/images/zoom_updated_102.png)  |
| ![](/images/zoom_updated_103.png) | ![](/images/zoom_updated_104.png)  |
| ![](/images/zoom_updated_105.png) | ![](/images/zoom_updated_106.png)  |
| ![](/images/zoom_updated_107.png) | ![](/images/zoom_updated_108.png)  |
| ![](/images/zoom_updated_109.png) | ![](/images/zoom_updated_110.png)  |
| ![](/images/zoom_updated_111.png) | ![](/images/zoom_updated_112.png)  |
| ![](/images/zoom_updated_113.png) | ![](/images/zoom_updated_114.png)  |
| ![](/images/zoom_updated_115.png) | ![](/images/zoom_updated_116.png)  |
| ![](/images/zoom_updated_117.png) | ![](/images/zoom_updated_118.png)  |
| ![](/images/zoom_updated_119.png) | ![](/images/zoom_updated_120.png)  |
| ![](/images/zoom_updated_121.png) | ![](/images/zoom_updated_122.png)  |
| ![](/images/zoom_updated_123.png) | ![](/images/zoom_updated_124.png)  |
| ![](/images/zoom_updated_125.png) | ![](/images/zoom_updated_126.png)  |
| ![](/images/zoom_updated_127.png) | ![](/images/zoom_updated_128.png)  |
| ![](/images/zoom_updated_129.png) | ![](/images/zoom_updated_130.png)  |
| ![](/images/zoom_updated_131.png) | ![](/images/zoom_updated_132.png)  |
| ![](/images/zoom_updated_133.png) | ![](/images/zoom_updated_134.png)  |
| ![](/images/zoom_updated_135.png) | ![](/images/zoom_updated_136.png)  |
| ![](/images/zoom_updated_137.png) | ![](/images/zoom_updated_138.png)  |
| ![](/images/zoom_updated_139.png) | ![](/images/zoom_updated_140.png)  |
| ![](/images/zoom_updated_1_1.png) | ![](/images/zoom_updated_1_2.png)  |
| ![](/images/zoom_updated_1_3.png) | ![](/images/zoom_updated_1_4.png)  |
| ![](/images/zoom_updated_1_5.png) | ![](/images/zoom_updated_1_6.png)  |
| ![](/images/zoom_updated_1_7.png) | ![](/images/zoom_updated_1_8.png)  |
| ![](/images/zoom_updated_1_9.png) | ![](/images/zoom_updated_1_10.png)  |
| ![](/images/zoom_updated_1_11.png) | ![](/images/zoom_updated_1_12.png)  |
| ![](/images/zoom_updated_1_13.png) | ![](/images/zoom_updated_1_14.png)  |
| ![](/images/zoom_updated_1_15.png) | ![](/images/zoom_updated_1_16.png)  |
| ![](/images/zoom_updated_1_17.png) | ![](/images/zoom_updated_1_18.png)  |
| ![](/images/zoom_updated_1_19.png) | ![](/images/zoom_updated_1_20.png)  |
| ![](/images/zoom_updated_1_21.png) | ![](/images/zoom_updated_1_22.png)  |
| ![](/images/zoom_updated_1_23.png) | ![](/images/zoom_updated_1_24.png)  |
| ![](/images/zoom_updated_1_25.png) | ![](/images/zoom_updated_1_26.png)  |
| ![](/images/zoom_updated_1_27.png) | ![](/images/zoom_updated_1_28.png)  |
| ![](/images/zoom_updated_1_29.png) | ![](/images/zoom_updated_1_30.png)  |
| ![](/images/zoom_updated_1_31.png) | ![](/images/zoom_updated_1_32.png)  |
| ![](/images/zoom_updated_1_33.png) | ![](/images/zoom_updated_1_34.png)  |
| ![](/images/zoom_updated_1_35.png) | ![](/images/zoom_updated_1_36.png)  |
| ![](/images/zoom_updated_1_37.png) | ![](/images/zoom_updated_1_38.png)  |
| ![](/images/zoom_updated_1_39.png) | ![](/images/zoom_updated_1_40.png)  |
| ![](/images/zoom_updated_1_41.png) | ![](/images/zoom_updated_1_42.png)  |
| ![](/images/zoom_updated_1_43.png) | ![](/images/zoom_updated_1_44.png)  |
| ![](/images/zoom_updated_1_45.png) | ![](/images/zoom_updated_1_46.png)  |
| ![](/images/zoom_updated_1_47.png) | ![](/images/zoom_updated_1_48.png)  |
| ![](/images/zoom_updated_1_49.png) | ![](/images/zoom_updated_1_50.png)  |
| ![](/images/zoom_updated_1_51.png) | ![](/images/zoom_updated_1_52.png)  |
| ![](/images/zoom_updated_1_53.png) | ![](/images/zoom_updated_1_54.png)  |
| ![](/images/zoom_updated_1_55.png) | ![](/images/zoom_updated_1_56.png)  |
| ![](/images/zoom_updated_1_57.png) | ![](/images/zoom_updated_1_58.png)  |
| ![](/images/zoom_updated_1_59.png) | ![](/images/zoom_updated_1_60.png)  |
| ![](/images/zoom_updated_1_61.png) | ![](/images/zoom_updated_1_62.png)  |
| ![](/images/zoom_updated_1_63.png) | ![](/images/zoom_updated_1_64.png)  |
| ![](/images/zoom_updated_1_65.png) | ![](/images/zoom_updated_1_66.png)  |
| ![](/images/zoom_updated_1_67.png) | ![](/images/zoom_updated_1_68.png)  |
| ![](/images/zoom_updated_1_69.png) | ![](/images/zoom_updated_1_70.png)  |
| ![](/images/zoom_updated_1_71.png) | ![](/images/zoom_updated_1_72.png)  |
| ![](/images/zoom_updated_1_73.png) | ![](/images/zoom_updated_1_74.png)  |


### 4. Marketing list to mailchimp

When we create a dynamic marketing list, we have the possibility to precise that the list will be synchronized with a Mailchimp list. The automation will therefore connect on Mailchimp API to create a list that will carry the same name as the dynamcic marketing list.

![](/images/mk_list_to_mailchimp_1.png)

#### Global flow
![](images/mk_list_to_mailchimp.png) 

#### Detailed flow

| - | - |
|---|---|
| ![](/images/mk_list_to_mailchimp.png) | ![](/images/mk_list_to_mailchimp1.png)  |
| ![](/images/mk_list_to_mailchimp2.png) | ![](/images/mk_list_to_mailchimp3.png)  |
| ![](/images/mk_list_to_mailchimp4.png) | ![](/images/mk_list_to_mailchimp5.png)  |
| ![](/images/mk_list_to_mailchimp6.png) | ![](/images/mk_list_to_mailchimp7.png)  |
| ![](/images/mk_list_to_mailchimp8.png) | ![](/images/mk_list_to_mailchimp9.png)  |
| ![](/images/mk_list_to_mailchimp10.png) | ![](/images/mk_list_to_mailchimp11.png)  |
| ![](/images/mk_list_to_mailchimp12.png) | ![](/images/mk_list_to_mailchimp13.png)  |
| ![](/images/mk_list_to_mailchimp14.png) | ![](/images/mk_list_to_mailchimp15.png)  |
| ![](/images/mk_list_to_mailchimp16.png) | ![](/images/mk_list_to_mailchimp17.png)  |


*Parse JSON - mailchimp lists shema*

```
{
    "type": "array",
    "items": {
        "type": "object",
        "properties": {
            "id": {
                "type": "string"
            },
            "web_id": {
                "type": "number"
            },
            "name": {
                "type": "string"
            },
            "contact": {
                "type": "object",
                "properties": {
                    "company": {
                        "type": "string"
                    },
                    "address1": {
                        "type": "string"
                    },
                    "address2": {
                        "type": "string"
                    },
                    "city": {
                        "type": "string"
                    },
                    "state": {
                        "type": "string"
                    },
                    "zip": {
                        "type": "string"
                    },
                    "country": {
                        "type": "string"
                    },
                    "phone": {
                        "type": "string"
                    }
                }
            },
            "permission_reminder": {
                "type": "string"
            },
            "use_archive_bar": {
                "type": "boolean"
            },
            "campaign_defaults": {
                "type": "object",
                "properties": {
                    "from_name": {
                        "type": "string"
                    },
                    "from_email": {
                        "type": "string"
                    },
                    "subject": {
                        "type": "string"
                    },
                    "language": {
                        "type": "string"
                    }
                }
            },
            "notify_on_subscribe": {
                "type": "string"
            },
            "notify_on_unsubscribe": {
                "type": "string"
            },
            "date_created": {
                "type": "string"
            },
            "list_rating": {
                "type": "number"
            },
            "email_type_option": {
                "type": "boolean"
            },
            "subscribe_url_short": {
                "type": "string"
            },
            "subscribe_url_long": {
                "type": "string"
            },
            "beamer_address": {
                "type": "string"
            },
            "visibility": {
                "type": "string"
            },
            "double_optin": {
                "type": "boolean"
            },
            "has_welcome": {
                "type": "boolean"
            },
            "marketing_permissions": {
                "type": "boolean"
            },
            "modules": {
                "type": "array"
            },
            "stats": {
                "type": "object",
                "properties": {
                    "member_count": {
                        "type": "number"
                    },
                    "unsubscribe_count": {
                        "type": "number"
                    },
                    "cleaned_count": {
                        "type": "number"
                    },
                    "member_count_since_send": {
                        "type": "number"
                    },
                    "unsubscribe_count_since_send": {
                        "type": "number"
                    },
                    "cleaned_count_since_send": {
                        "type": "number"
                    },
                    "campaign_count": {
                        "type": "number"
                    },
                    "campaign_last_sent": {
                        "type": "string"
                    },
                    "merge_field_count": {
                        "type": "number"
                    },
                    "avg_sub_rate": {
                        "type": "number"
                    },
                    "avg_unsub_rate": {
                        "type": "number"
                    },
                    "target_sub_rate": {
                        "type": "number"
                    },
                    "open_rate": {
                        "type": "number"
                    },
                    "click_rate": {
                        "type": "number"
                    },
                    "last_sub_date": {
                        "type": "string"
                    },
                    "last_unsub_date": {
                        "type": "string"
                    }
                }
            },
            "_links": {
                "type": "array",
                "items": {
                    "type": "object",
                    "properties": {
                        "rel": {
                            "type": "string"
                        },
                        "href": {
                            "type": "string"
                        },
                        "method": {
                            "type": "string"
                        },
                        "targetSchema": {
                            "type": "string"
                        },
                        "schema": {
                            "type": "string"
                        }
                    },
                    "required": [
                        "rel",
                        "href",
                        "method"
                    ]
                }
            }
        }
    }
}
```

### 5. Synch members MK list & Mailchimp

We can manually run a flow to synchronize the dynamics marketing list from the CRM with its corresponding marketing list in Mailchimp.
Dynamics 365 will then compare all members from the two list and add or delete any member in Mailchimp list to make members be exactly the same in the two lists.

![](/images/mk_list_to_mailchimp_2.png)

#### Global flow
![](images/mk_list_to_mailchimp.png) 

#### Detailed flow

| - | - |
|---|---|
| ![](/images/mk_list_to_mailchimp_2.png) | ![](/images/mk_list_to_mailchimp_3.png) |
| ![](/images/mk_list_to_mailchimp_4.png) | ![](/images/mk_list_to_mailchimp_5.png) |
| ![](/images/mk_list_to_mailchimp_6.png) | ![](/images/mk_list_to_mailchimp_7.png) |
| ![](/images/mk_list_to_mailchimp_8.png) | ![](/images/mk_list_to_mailchimp_9.png) |
| ![](/images/mk_list_to_mailchimp_10.png) | ![](/images/mk_list_to_mailchimp_11.png) |
| ![](/images/mk_list_to_mailchimp_12.png) | ![](/images/mk_list_to_mailchimp_13.png) |
| ![](/images/mk_list_to_mailchimp_14.png) | ![](/images/mk_list_to_mailchimp_15.png) |
| ![](/images/mk_list_to_mailchimp_16.png) | ![](/images/mk_list_to_mailchimp_17.png) |
| ![](/images/mk_list_to_mailchimp_18.png) | ![](/images/mk_list_to_mailchimp_19.png) |
| ![](/images/mk_list_to_mailchimp_20.png) | ![](/images/mk_list_to_mailchimp_21.png) |
| ![](/images/mk_list_to_mailchimp_22.png) | ![](/images/mk_list_to_mailchimp_23.png) |
| ![](/images/mk_list_to_mailchimp_24.png) | ![](/images/mk_list_to_mailchimp_25.png) |
| ![](/images/mk_list_to_mailchimp_26.png) | ![](/images/mk_list_to_mailchimp_27.png) |
| ![](/images/mk_list_to_mailchimp_28.png) | ![](/images/mk_list_to_mailchimp_29.png) |
| ![](/images/mk_list_to_mailchimp_30.png) | ![](/images/mk_list_to_mailchimp_31.png) |
| ![](/images/mk_list_to_mailchimp_32.png) | ![](/images/mk_list_to_mailchimp_33.png) |
| ![](/images/mk_list_to_mailchimp_34.png) | ![](/images/mk_list_to_mailchimp_35.png) |
| ![](/images/mk_list_to_mailchimp_36.png) | ![](/images/mk_list_to_mailchimp_37.png) |
| ![](/images/mk_list_to_mailchimp_38.png) | ![](/images/mk_list_to_mailchimp_39.png) |
| ![](/images/mk_list_to_mailchimp_40.png) | ![](/images/mk_list_to_mailchimp_41.png) |
| ![](/images/mk_list_to_mailchimp_42.png) | ![](/images/mk_list_to_mailchimp_43.png) |
| ![](/images/mk_list_to_mailchimp_44.png) | ![](/images/mk_list_to_mailchimp_45.png) |
| ![](/images/mk_list_to_mailchimp_46.png) | ![](/images/mk_list_to_mailchimp_47.png) |
| ![](/images/mk_list_to_mailchimp_48.png) | ![](/images/mk_list_to_mailchimp_49.png) |
| ![](/images/mk_list_to_mailchimp_50.png) | ![](/images/mk_list_to_mailchimp_51.png) |
| ![](/images/mk_list_to_mailchimp_52.png) | ![](/images/mk_list_to_mailchimp_53.png) |
| ![](/images/mk_list_to_mailchimp_54.png) | ![](/images/mk_list_to_mailchimp_55.png) |
| ![](/images/mk_list_to_mailchimp_56.png) | ![](/images/mk_list_to_mailchimp_57.png) |


```
<fetch version="1.0" output-format="xml-platform" mapping="logical" no-lock="false" distinct="true">
<entity name="lead">
<attribute name="entityimage_url"/>
<attribute name="fullname"/>
<attribute name="firstname"/>
<attribute name="lastname"/>
<attribute name="emailaddress1"/>
<order attribute="createdon" descending="true"/>
<attribute name="statuscode"/>
<attribute name="createdon"/>
<attribute name="subject"/>
<attribute name="leadid"/>
<attribute name="companyname"/>
<attribute name="description"/>
<link-entity name="listmember" intersect="true" visible="false" from="entityid" to="leadid">
<link-entity name="list" alias="aa" from="listid" to="listid">
<filter type="and">
<condition attribute="listid" operator="eq" value="{@{variables('marketingListId')}}" uiname="@{variables('marketingListName')}" uitype="list"/>
</filter>
</link-entity>
</link-entity>
</entity>
</fetch>
```


*List contact rows*

```
<fetch version="1.0" output-format="xml-platform" mapping="logical" distinct="true" no-lock="false">
	<entity name="contact">
		<attribute name="entityimage_url"/>
		<attribute name="firstname"/>
		<attribute name="lastname"/>
		<attribute name="emailaddress1"/>
		<attribute name="parentcustomerid"/>
		<attribute name="jobtitle"/>
		<attribute name="fullname"/>
		<attribute name="contactid"/>
		<attribute name="ren_accountmanager"/>
		<attribute name="mobilephone"/>
		<attribute name="msnfp_vip"/>
		<link-entity name="listmember" intersect="true" visible="false" from="entityid" to="contactid">
			<link-entity name="list" alias="aa" from="listid" to="listid">
				<filter type="and">
					<condition attribute="listid" operator="eq" value="{@{variables('marketingListId')}}" uiname="@{variables('marketingListName')}" uitype="list"/>
				</filter>
			</link-entity>
		</link-entity>
	</entity>
</fetch>
```

*Parse JSON (Compose Contact rows)*

```
{
    "type": "array",
    "items": {
        "type": "object",
        "properties": {
            "@@odata.type": {
                "type": "string"
            },
            "@@odata.id": {
                "type": "string"
            },
            "@@odata.etag": {
                "type": "string"
            },
            "@@odata.editLink": {
                "type": "string"
            },
            "firstname": {
                "type": "string"
            },
            "emailaddress1": {
                "type": "string"
            },
            "contactid@odata.type": {
                "type": "string"
            },
            "contactid": {
                "type": "string"
            },
            "_ren_accountmanager_value@OData.Community.Display.V1.FormattedValue": {
                "type": "string"
            },
            "_ren_accountmanager_value@Microsoft.Dynamics.CRM.lookuplogicalname": {
                "type": "string"
            },
            "_ren_accountmanager_value@odata.type": {
                "type": "string"
            },
            "_ren_accountmanager_value": {
                "type": "string"
            },
            "msnfp_vip@OData.Community.Display.V1.FormattedValue": {
                "type": "string"
            },
            "msnfp_vip": {
                "type": "boolean"
            },
            "fullname": {
                "type": "string"
            },
            "lastname": {
                "type": "string"
            }
        }
    }
}
```

*List vip contacts*

```
<fetch version="1.0" output-format="xml-platform" mapping="logical" distinct="true" no-lock="false">
	<entity name="contact">
		<attribute name="entityimage_url"/>
		<attribute name="firstname"/>
		<attribute name="lastname"/>
		<attribute name="emailaddress1"/>
		<attribute name="parentcustomerid"/>
		<attribute name="jobtitle"/>
		<attribute name="fullname"/>
		<attribute name="contactid"/>
		<attribute name="ren_accountmanager"/>
		<attribute name="mobilephone"/>
		<attribute name="msnfp_vip"/>
		<link-entity name="listmember" intersect="true" visible="false" from="entityid" to="contactid">
			<link-entity name="list" alias="aa" from="listid" to="listid">
				<filter type="and">
					<condition attribute="listid" operator="eq" value="{@{variables('marketingListId')}}" uiname="@{variables('marketingListName')}" uitype="list"/>
				</filter>
			</link-entity>
		</link-entity>
	</entity>
</fetch>
```

### 6. Synch members Tag & Mailchimp

We can manually run a flow to synchronize a tag record from the CRM with its corresponding marketing list in Mailchimp.
Dynamics 365 will then compare all members from the tag and the list then add or delete any member in Mailchimp list to make members be exactly the same in both the tag and the lists. Note that tag is a custom table created in the CRM having many to many relationship with Contact entity.

![](/images/tag.png)

#### Detailed flow

| - | - |
|---|---|
| ![](/images/tag_mklist1.png) | ![](/images/tag_mklist2.png) |
| ![](/images/tag_mklist3.png) | ![](/images/tag_mklist4.png) |
| ![](/images/tag_mklist5.png) | ![](/images/tag_mklist6.png) |
| ![](/images/tag_mklist7.png) | ![](/images/tag_mklist8.png) |
| ![](/images/tag_mklist9.png) | ![](/images/tag_mklist10.png) |
| ![](/images/tag_mklist11.png) | ![](/images/tag_mklist12.png) |
| ![](/images/tag_mklist13.png) | ![](/images/tag_mklist14.png) |
| ![](/images/tag_mklist15.png) | ![](/images/tag_mklist16.png) |
| ![](/images/tag_mklist17.png) | ![](/images/tag_mklist18.png) |
| ![](/images/tag_mklist19.png) | ![](/images/tag_mklist20.png) |
| ![](/images/tag_mklist21.png) | ![](/images/tag_mklist22.png) |
| ![](/images/tag_mklist23.png) | ![](/images/tag_mklist24.png) |
| ![](/images/tag_mklist25.png) | ![](/images/tag_mklist26.png) |



```
<fetch version="1.0" output-format="xml-platform" mapping="logical" distinct="true" no-lock="false">
	<entity name="contact">
		<attribute name="entityimage_url"/>
		<attribute name="firstname"/>
		<attribute name="lastname"/>
		<attribute name="emailaddress1"/>
		<attribute name="parentcustomerid"/>
		<attribute name="jobtitle"/>
		<attribute name="fullname"/>
		<attribute name="contactid"/>
        <attribute name="ren_accountmanager"/>
		<attribute name="mobilephone"/>
<attribute name="msnfp_vip"/>
		<link-entity name="ren_contact_ren_tags" intersect="true" visible="false" from="contactid" to="contactid">
			<link-entity name="ren_tags" alias="aa" from="ren_tagsid" to="ren_tagsid">
				<filter type="and">
					<condition attribute="ren_tagsid" operator="eq" value="{@{variables('tagId')}}" uiname="@{variables('tagName')}" uitype="ren_tags"/>
				</filter>
			</link-entity>
		</link-entity>
	</entity>
</fetch>
```

### 7. Create Newsletter Member

The organization has a WordPress website where several forms can be filled by visitors. We have implemented an Azure service bus to collect information from forms submited by users and we use these information to create website visitors as leads in Dynamics 365 that can be qualified as contacts.


#### Detailed flow

| - | - |
|---|---|
| ![](/images/sb1.png) | ![](/images/sb2.png) |
| ![](/images/sb3.png) | ![](/images/sb4.png) |
| ![](/images/sb5.png) | ![](/images/sb6.png) |
| ![](/images/sb.png) | ![](/images/sb.png) |
| ![](/images/sb7.png) | ![](/images/sb8.png) |
| ![](/images/sb9.png) | ![](/images/sb10.png) |
| ![](/images/sb11.png) | ![](/images/sb12.png) |

### 8. No Zoom flow

This automation will help the organization getting the list of those have registred and participated to any non zoom meeting.


#### Detailed flow

| - | - |
|---|---|
| ![](/images/no_zoom1.png) | ![](/images/no_zoom2.png)|
| ![](/images/no_zoom3.png) | ![](/images/no_zoom4.png)|
| ![](/images/no_zoom5.png) | ![](/images/no_zoom6.png)|
| ![](/images/no_zoom7.png) | ![](/images/no_zoom8.png)|
| ![](/images/no_zoom9.png) | ![](/images/no_zoom10.png)|
| ![](/images/no_zoom11.png) | ![](/images/no_zoom12.png)|
| ![](/images/no_zoom13.png) | ![](/images/no_zoom14.png)|
| ![](/images/no_zoom15.png) | ![](/images/no_zoom16.png)|
| ![](/images/no_zoom17.png) | ![](/images/no_zoom18.png)|
| ![](/images/no_zoom19.png) | ![](/images/no_zoom20.png)|
| ![](/images/no_zoom21.png) | ![](/images/no_zoom22.png)|
| ![](/images/no_zoom23.png) | ![](/images/no_zoom24.png)|
| ![](/images/no_zoom25.png) | ![](/images/no_zoom26.png)|
| ![](/images/no_zoom27.png) | ![](/images/no_zoom28.png)|
| ![](/images/no_zoom29.png) | ![](/images/no_zoom30.png)|
| ![](/images/no_zoom31.png) | ![](/images/no_zoom32.png)|
| ![](/images/no_zoom33.png) | ![](/images/no_zoom34.png)|
| ![](/images/no_zoom35.png) | ![](/images/no_zoom36.png)|
| ![](/images/no_zoom37.png) | ![](/images/no_zoom38.png)|
| ![](/images/no_zoom39.png) | ![](/images/no_zoom40.png)|
| ![](/images/no_zoom41.png) | ![](/images/no_zoom42.png)|
| ![](/images/no_zoom43.png) | ![](/images/no_zoom44.png)|
| ![](/images/no_zoom45.png) | ![](/images/no_zoom46.png)|
| ![](/images/no_zoom47.png) | ![](/images/no_zoom48.png)|
| ![](/images/no_zoom49.png) | ![](/images/no_zoom50.png)|
| ![](/images/no_zoom51.png) | ![](/images/no_zoom52.png)|
| ![](/images/no_zoom53.png) | ![](/images/no_zoom54.png)|
| ![](/images/no_zoom55.png) | ![](/images/no_zoom56.png)|
| ![](/images/no_zoom57.png) | ![](/images/no_zoom58.png)|
| ![](/images/no_zoom59.png) | ![](/images/no_zoom60.png)|
| ![](/images/no_zoom61.png) | ![](/images/no_zoom62.png)|
| ![](/images/no_zoom63.png) | ![](/images/no_zoom64.png)|
| ![](/images/no_zoom65.png) | ![](/images/no_zoom66.png)|
| ![](/images/no_zoom67.png) | ![](/images/no_zoom68.png)|
| ![](/images/no_zoom69.png) | ![](/images/no_zoom70.png)|
| ![](/images/no_zoom71.png) | ![](/images/no_zoom72.png)|
| ![](/images/no_zoom73.png) | ![](/images/no_zoom74.png)|
| ![](/images/no_zoom75.png) | ![](/images/no_zoom76.png)|


## C# Codes

### Plug-Ins

#### 1. Link Contacts with organizations using accounid

```
using Microsoft.Xrm.Sdk;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.Xrm.Sdk.Query;
using Microsoft.Xrm.Sdk.Messages;
using System.ServiceModel;

namespace Linking_Organization
    {
    public class LinkOrgContact : IPlugin
        {
        public void Execute(IServiceProvider serviceProvider)
            {
            IPluginExecutionContext context = (IPluginExecutionContext)serviceProvider.GetService(typeof(IPluginExecutionContext));
            IOrganizationServiceFactory factory = (IOrganizationServiceFactory)serviceProvider.GetService(typeof(IOrganizationServiceFactory));
            IOrganizationService service = factory.CreateOrganizationService(context.UserId);
            ITracingService tracingService = (ITracingService)serviceProvider.GetService(typeof(ITracingService));

            if (context.InputParameters.Contains("Target") && context.InputParameters["Target"] is Entity)
                {
                Entity contactEntity = (Entity)context.InputParameters["Target"];

                if (contactEntity.LogicalName != "contact")
                    {
                    return;
                    }

                try
                    {
                    Entity contactRecord = service.Retrieve("contact", contactEntity.Id, new ColumnSet("ren_organization_id_import"));
                    string importOrgId = contactRecord.GetAttributeValue<string>("ren_organization_id_import");

                    if (!string.IsNullOrEmpty(importOrgId))
                        {
                        Guid accountId = default(Guid);
                        QueryByAttribute query = new QueryByAttribute
                            {
                            EntityName = "account"
                            };                                                                          // no columnset, we just want the id
                        query.AddAttributeValue("ren_a_id_import", importOrgId);



                        EntityCollection results = service.RetrieveMultiple(query);
                        if (results.Entities.Count > 0)
                            {
                            accountId = results[0].Id;
                            }

                        contactRecord["parentcustomerid"] = new EntityReference("account", accountId);
                        service.Update(contactRecord);
                        }

                    }

                catch (FaultException<OrganizationServiceFault> ex) // This uses System.ServiceModel;
                    {
                    throw new InvalidPluginExecutionException("An error occured", ex);
                    }
                }
            }
        }


    }
```

#### 2. Link Contacts with organizations using alternate key (email address)

```
using Microsoft.Xrm.Sdk;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.Xrm.Sdk.Query;
using Microsoft.Xrm.Sdk.Messages;
using System.ServiceModel;

namespace Linking_Organization
    {
    public class LinkOrgContact : IPlugin
        {
        public void Execute(IServiceProvider serviceProvider)
            {
            IPluginExecutionContext context = (IPluginExecutionContext)serviceProvider.GetService(typeof(IPluginExecutionContext));
            IOrganizationServiceFactory factory = (IOrganizationServiceFactory)serviceProvider.GetService(typeof(IOrganizationServiceFactory));
            IOrganizationService service = factory.CreateOrganizationService(context.UserId);
            ITracingService tracingService = (ITracingService)serviceProvider.GetService(typeof(ITracingService));

            if (context.InputParameters.Contains("Target") && context.InputParameters["Target"] is Entity)
                {
                Entity contactEntity = (Entity)context.InputParameters["Target"];

                if (contactEntity.LogicalName != "contact")
                    {
                    return;
                    }

                try
                    {
                    Entity contactRecord = service.Retrieve("contact", contactEntity.Id, new ColumnSet("ren_organization_id_import"));
                    string importOrgId = contactRecord.GetAttributeValue<string>("ren_organization_id_import");
                    contactRecord["parentcustomerid"] = new EntityReference("account", "ren_a_id_import", importOrgId);
                    service.Update(contactRecord);

                    /*if (!string.IsNullOrEmpty(importOrgId))
                        {
                        Guid accountId = default(Guid);
                        QueryByAttribute query = new QueryByAttribute
                            {
                            EntityName = "account"
                            };                                                                          // no columnset, we just want the id
                        query.AddAttributeValue("ren_a_id_import", importOrgId);



                        EntityCollection results = service.RetrieveMultiple(query);
                        if (results.Entities.Count > 0)
                            {
                            accountId = results[0].Id;
                            }

                        contactRecord["parentcustomerid"] = new EntityReference("account", accountId);
                        service.Update(contactRecord);
                        }*/

                    }

                catch (FaultException<OrganizationServiceFault> ex) // This uses System.ServiceModel;
                    {
                    throw new InvalidPluginExecutionException("An error occured", ex);
                    }
                }
            }
        }


    }
```

### Console Application codes

#### C# Code for data migration

```
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Tooling.Connector;
using Microsoft.Xrm.Sdk.Messages;
using Microsoft.Office.Interop.Excel;
using System.Threading.Tasks;
using Microsoft.Xrm.Sdk.Metadata;
using System.Data.OleDb;
using System.Text.RegularExpressions;
using System.ServiceModel;
using Serilog;
using System.Data;



namespace Dynamics365ConsoleApp
    {

    internal class ImportFromExcelFile
        {

        public static CrmServiceClient GetServiceClient()
            {
            // Creating CRM Service Client
            // Initialize D365 connection
            string clientId = "???";
            string clientSecret = "??????";
            string authority = "https://login.microsoftonline.com/d144fa3f-7f04-4054-bdd9-e8885f3f0821";
            string crmURL = "https://ren21uat.crm4.dynamics.com";

            string connString = $"AuthType=ClientSecret;Url={crmURL};ClientId={clientId};ClientSecret={clientSecret};Authority={authority};RequireNewInstance=True;";
            CrmServiceClient serviceClient = new CrmServiceClient(connString);

            return serviceClient;
            }

        // This method help to retrieve option set numeri value by passing the string value
        public static int GetOptionSetValue(string entityName, string attributeName, string optionsetText)
            {
            int optionSetValue = 0;
            RetrieveAttributeRequest retrieveAttributeRequest = new RetrieveAttributeRequest();
            retrieveAttributeRequest.EntityLogicalName = entityName;
            retrieveAttributeRequest.LogicalName = attributeName;
            retrieveAttributeRequest.RetrieveAsIfPublished = true;

            RetrieveAttributeResponse retrieveAttributeResponse =
              (RetrieveAttributeResponse)GetServiceClient().Execute(retrieveAttributeRequest);
            PicklistAttributeMetadata picklistAttributeMetadata =
              (PicklistAttributeMetadata)retrieveAttributeResponse.AttributeMetadata;

            OptionSetMetadata optionsetMetadata = picklistAttributeMetadata.OptionSet;

            foreach (OptionMetadata optionMetadata in optionsetMetadata.Options)
                {
                if (optionMetadata.Label.UserLocalizedLabel.Label.ToLower() == optionsetText.ToLower())
                    {
                    optionSetValue = optionMetadata.Value.Value;
                    return optionSetValue;
                    }

                }
            return optionSetValue;
            }

        // This method helps to get the Option set iself as a list
        public static List<GlobalOptionSetModel> GetGlobalOptionSetList(string globalOptionSetName)
            {
            List<GlobalOptionSetModel> result = new List<GlobalOptionSetModel>();
            RetrieveOptionSetRequest retrieveOptionSetRequest = new RetrieveOptionSetRequest { Name = globalOptionSetName };
            RetrieveOptionSetResponse retrieveOptionSetResponse = (RetrieveOptionSetResponse)GetServiceClient().Execute(retrieveOptionSetRequest);
            OptionSetMetadata optionSetMetadata = (OptionSetMetadata)retrieveOptionSetResponse.OptionSetMetadata;
            List<OptionMetadata> optionList = optionSetMetadata.Options.ToList();
            result = optionList.ConvertAll(z => new GlobalOptionSetModel { Label = z.Label.UserLocalizedLabel.Label, Value = z.Value.Value, LanguageCode = z.Label.UserLocalizedLabel.LanguageCode.ToString() });
            return result;
            }

        public class GlobalOptionSetModel
            {
            public String LanguageCode { get; set; }
            public int Value { get; set; }
            public String Label { get; set; }
            }

        public static Dictionary<string, Int64> ExcelDictionary(string filePath)
            {

            string excelFilePath = filePath;
            string providerString = "Provider=Microsoft.ACE.OLEDB.12.0;Data Source={0};Extended Properties=\"Excel 12.0 Xml; HDR=YES;IMEX=1\";";

            Dictionary<string, Int64> columnWithIndex = new Dictionary<string, Int64>();
            List<string> columnNames = new List<string>();
            List<Int64> columnPosition = new List<Int64>();

            using (OleDbConnection oleConn = new OleDbConnection(String.Format(providerString, excelFilePath)))
                {
                oleConn.Open();
                System.Data.DataTable schemaDT = oleConn.GetOleDbSchemaTable(OleDbSchemaGuid.Columns, null);
                string firstSheet = schemaDT.Rows[0]["TABLE_NAME"].ToString();
                DataRow[] shtRows = schemaDT.Select("[TABLE_NAME] =" + "'" + firstSheet + "'");
                columnNames = shtRows.Select(o => o.Field<string>("COLUMN_NAME")).ToList();
                columnPosition = shtRows.Select(o => o.Field<Int64>("ORDINAL_POSITION")).ToList();
                for (int i = 0; i < columnNames.Count; i++)
                    {
                    columnWithIndex.Add(columnNames[i], columnPosition[i]);
                    }
                oleConn.Close();
                }

            return columnWithIndex;
            }



        static List<GlobalOptionSetModel> GlobalOptionModeratorAssessment = GetGlobalOptionSetList("ren_moderator_assessment");
        static List<GlobalOptionSetModel> GlobalOptionKeynoteAssessment = GetGlobalOptionSetList("ren_keynote_assessment");
        static List<GlobalOptionSetModel> GlobalOptionPanellistAssessment = GetGlobalOptionSetList("ren_panellist_assessment");
        static List<GlobalOptionSetModel> GlobalOptionSpeakerRole = GetGlobalOptionSetList("ren_speakingrole");

        static List<GlobalOptionSetModel> GlobalOptionCountry = GetGlobalOptionSetList("ren_countrylist");
        static List<GlobalOptionSetModel> GlobalOptionExpertiseRegion = GetGlobalOptionSetList("ren_contactexpertiseregion");
        static List<GlobalOptionSetModel> GlobalOptionExpertiseTopic = GetGlobalOptionSetList("ren_expertise_topic");
        static List<GlobalOptionSetModel> GlobalOptionStakeholder = GetGlobalOptionSetList("ren_stakeholdergroup");
        static string filePath = @"J:\1_Ren21\Final..Final\step.by.step.contact.import\humm1.xlsx";


        public void ImportFile()
            {

            if (GetServiceClient().IsReady)
                {
                Console.WriteLine($"CRM Connection Successfull");
                }


            //Loading the workbook from Excel file table
            // Open the Excel file
            var excelApp = new Application();
            var workbook = excelApp.Workbooks.Open(filePath);
            var worksheet = (Worksheet)workbook.Sheets[1];

            #region Part 3: Initialize Template and Async Task List
            // Read data from Excel
            List<ExcelDataTemplate> excelData = ReadExcelData(worksheet);

            // Batch size for bulk creation
            int batchSize = 10;

            // Create a list to store tasks for parallel execution
            var tasks = new List<Task>();
            #endregion

            #region Part 4: Loop through all the records loaded from excel file
            for (int startIndex = 0; startIndex < excelData.Count; startIndex += batchSize)
                {
                // Get a batch of data
                List<ExcelDataTemplate> batch = excelData.Skip(startIndex).Take(batchSize).ToList();

                // Create a new task for each batch

                var task = new Task(() =>
                {
                    try
                        {
                        Log.Logger = new LoggerConfiguration()
                        .WriteTo.Console()
                        .WriteTo.File(@"J:\1_Ren21\Final..Final\step.by.step.contact.import\SinkLog_.txt", rollingInterval: RollingInterval.Day)
                        .CreateLogger();
                        // Create entities for the batch
                        var entities = batch.Select(MapToD365Entity).ToList();

                        // Create and execute multiple requests
                        ExecuteMultipleCreateRequests(GetServiceClient(), entities);
                        }
                    catch (Exception ex)
                        {
                        // Handle errors and log them
                        Log.Error(ex, "This is the error log");
                        }
                }
                    );
                task.Start();
                task.Wait();
                }
            #endregion

            #region Part 5: Close the application and releae object
            // Wait for all tasks to complete
            Task.WhenAll(tasks).Wait();

            // Close Excel and release resources
            workbook.Close(false);
            excelApp.Quit();
            ReleaseObject(worksheet);
            ReleaseObject(workbook);
            ReleaseObject(excelApp);
            Console.WriteLine("Data import completed.");
            #endregion
            }

        #region ReadExcelData
        static List<ExcelDataTemplate> ReadExcelData(Worksheet worksheet)
            {

            Dictionary<string, Int64> my_dict = ExcelDictionary(filePath);

            List<ExcelDataTemplate> data = new List<ExcelDataTemplate>();

            int rowCount = worksheet.UsedRange.Rows.Count;

            for (int row = 2; row <= rowCount; row++)
                {
                var Id_Contact = ((Range)worksheet.Cells[row, my_dict["Id_Contact"]]).Value2?.ToString();
                var EmailAddress = ((Range)worksheet.Cells[row, my_dict["EmailAddress"]]).Value2?.ToString();
                var FirstName = ((Range)worksheet.Cells[row, my_dict["FirstName"]]).Value2?.ToString();
                var LastName = ((Range)worksheet.Cells[row, my_dict["LastName"]]).Value2?.ToString();
                var Id_Organization = ((Range)worksheet.Cells[row, my_dict["Id_Organization"]]).Value2?.ToString();
                var Role = ((Range)worksheet.Cells[row, my_dict["Role"]]).Value2?.ToString();
                var MailAddressStreet = ((Range)worksheet.Cells[row, my_dict["MailAddressStreet"]]).Value2?.ToString();
                var MailAddressCity = ((Range)worksheet.Cells[row, my_dict["MailAddressCity"]]).Value2?.ToString();
                var MailAddressState = ((Range)worksheet.Cells[row, my_dict["MailAddressState"]]).Value2?.ToString();
                var MailAddressCountry = ((Range)worksheet.Cells[row, my_dict["MailAddressCountry"]]).Value2?.ToString();
                var MailAddressPostalCode = ((Range)worksheet.Cells[row, my_dict["MailAddressPostalCode"]]).Value2?.ToString();
                var BusinessPhone = ((Range)worksheet.Cells[row, my_dict["BusinessPhone"]]).Value2?.ToString();
                var MobilePhone = ((Range)worksheet.Cells[row, my_dict["MobilePhone"]]).Value2?.ToString();
                var GSR_Notes = ((Range)worksheet.Cells[row, my_dict["GSR_Notes"]]).Value2?.ToString();
                var Contact_Expertise_Region = ((Range)worksheet.Cells[row, my_dict["Contact_Expertise_Region"]]).Value2?.ToString();
                var Contact_Expertise_Country = ((Range)worksheet.Cells[row, my_dict["Contact_Expertise_Country"]]).Value2?.ToString();
                var Contact_Expertise_Topic = ((Range)worksheet.Cells[row, my_dict["Contact_Expertise_Topic"]]).Value2?.ToString();
                var Email_2 = ((Range)worksheet.Cells[row, my_dict["Email_2"]]).Value2?.ToString();
                var Speaker_Role = ((Range)worksheet.Cells[row, my_dict["Speaker_Role"]]).Value2?.ToString();
                var Panellist_assessment = ((Range)worksheet.Cells[row, my_dict["Panellist_assessment"]]).Value2?.ToString();
                var Keynote_assessment = ((Range)worksheet.Cells[row, my_dict["Keynote_assessment"]]).Value2?.ToString();
                var Moderator_assessment = ((Range)worksheet.Cells[row, my_dict["Moderator_assessment"]]).Value2?.ToString();
                var Stakeholder_Group = ((Range)worksheet.Cells[row, my_dict["Stakeholder_Group"]]).Value2?.ToString();

                if (!string.IsNullOrEmpty(EmailAddress))
                    {
                    var excelData = new ExcelDataTemplate
                        {
                        Id_Contact = Id_Contact,
                        EmailAddress = EmailAddress,
                        FirstName = FirstName,
                        LastName = LastName,
                        Id_Organization = Id_Organization,
                        Role = Role,
                        MailAddressStreet = MailAddressStreet,
                        MailAddressCity = MailAddressCity,
                        MailAddressState = MailAddressState,
                        MailAddressCountry = MailAddressCountry,
                        MailAddressPostalCode = MailAddressPostalCode,
                        BusinessPhone = BusinessPhone,
                        MobilePhone = MobilePhone,
                        GSR_Notes = GSR_Notes,
                        Contact_Expertise_Region = Contact_Expertise_Region,
                        Contact_Expertise_Country = Contact_Expertise_Country,
                        Contact_Expertise_Topic = Contact_Expertise_Topic,
                        Email_2 = Email_2,
                        Speaker_Role = Speaker_Role,
                        Panellist_assessment = Panellist_assessment,
                        Keynote_assessment = Keynote_assessment,
                        Moderator_assessment = Moderator_assessment,
                        Stakeholder_Group = Stakeholder_Group
                        };
                    data.Add(excelData);
                    }
                }

            return data;
            }
        #endregion


        #region MapToD365Entity
        static Entity MapToD365Entity(ExcelDataTemplate excelData)
            {
            Entity entity = new Entity("contact");

            // Simple fields

            entity["ren_contactid_import"] = excelData.Id_Contact;
            entity["emailaddress1"] = excelData.EmailAddress;
            entity["firstname"] = excelData.FirstName;
            entity["lastname"] = excelData.LastName;
            entity["ren_organization_id_import"] = excelData.Id_Organization;
            entity["jobtitle"] = excelData.Role;
            entity["address1_line1"] = excelData.MailAddressStreet;
            entity["address1_city"] = excelData.MailAddressCity;
            entity["address1_stateorprovince"] = excelData.MailAddressState;
            entity["address1_postofficebox"] = excelData.MailAddressPostalCode;
            entity["ren_businessphone"] = excelData.BusinessPhone;
            entity["ren_mobilephone"] = excelData.MobilePhone;
            entity["ren_gsr_note"] = excelData.GSR_Notes;
            entity["emailaddress2"] = excelData.Email_2;



            /* Lookup field*/

            if (!string.IsNullOrEmpty(excelData.Id_Organization))
                {
                entity["parentcustomerid"] = new EntityReference("account", "ren_a_id_import", excelData.Id_Organization);
                }
            else
                {
                entity["parentcustomerid"] = null;
                }

            // Option set fields - Single selection
            // "ren_speakerrole" option set
            if (!string.IsNullOrEmpty(excelData.Speaker_Role))
                {
                var findSpeakerRole = GlobalOptionSpeakerRole.Find(x => x.Label == excelData.Speaker_Role);
                if (findSpeakerRole != null)
                    {
                    OptionSetValue speakerRole = new OptionSetValue(findSpeakerRole.Value);
                    entity["ren_speakerrole"] = speakerRole;
                    }
                else
                    {
                    entity["ren_speakerrole"] = null;
                    }
                }
            else
                {
                entity["ren_speakerrole"] = null;
                }

            // "ren_panellist_assessment" option set

            if (!string.IsNullOrEmpty(excelData.Panellist_assessment))
                {
                var findPanellistAssessment = GlobalOptionPanellistAssessment.Find(x => x.Label == excelData.Panellist_assessment);
                if (findPanellistAssessment != null)
                    {
                    OptionSetValue panellistAssessement = new OptionSetValue(findPanellistAssessment.Value);
                    entity["ren_panellist_assessment"] = panellistAssessement;
                    }
                else
                    {
                    entity["ren_panellist_assessment"] = null;
                    }
                }
            else
                {
                entity["ren_panellist_assessment"] = null;
                }

            // "ren_keynote_assessment" option set

            if (!string.IsNullOrEmpty(excelData.Keynote_assessment))
                {
                var findKeynoteAssessment = GlobalOptionKeynoteAssessment.Find(x => x.Label == excelData.Keynote_assessment);
                if (findKeynoteAssessment != null)
                    {
                    OptionSetValue keynoteAssessement = new OptionSetValue(findKeynoteAssessment.Value);
                    entity["ren_keynote_assessment"] = keynoteAssessement;
                    }
                else
                    {
                    entity["ren_keynote_assessment"] = null;
                    }
                }
            else
                {
                entity["ren_keynote_assessment"] = null;
                }

            // "ren_moderator_assessment" option set

            if (!string.IsNullOrEmpty(excelData.Moderator_assessment))
                {
                var findModeratorAssessment = GlobalOptionModeratorAssessment.Find(x => x.Label == excelData.Moderator_assessment);
                if (findModeratorAssessment != null)
                    {
                    OptionSetValue moderatorAssessement = new OptionSetValue(findModeratorAssessment.Value);
                    entity["ren_moderator_assessment"] = moderatorAssessement;
                    }
                else
                    {
                    entity["ren_moderator_assessment"] = null;
                    }
                }
            else
                {
                entity["ren_moderator_assessment"] = null;
                }

            // Option set fields - Multiple selection


            // "ren_contactexpertisecountry"


            if (!string.IsNullOrEmpty(excelData.Contact_Expertise_Country))
                {
                OptionSetValueCollection country = new OptionSetValueCollection();
                OptionSetValue countryOption = new OptionSetValue();

                string[] countries = excelData.Contact_Expertise_Country.Split(';');
                for (int i = 0; i < countries.Length; i++)
                    countries[i] = Regex.Replace(countries[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < countries.Length; i++)
                    {
                    var findCountry = GlobalOptionCountry.Find(x => x.Label == countries[i]);
                    if (findCountry != null)
                        {
                        countryOption = new OptionSetValue(findCountry.Value);
                        country.Add(countryOption);
                        }
                    }
                entity["ren_contactexpertisecountry"] = country;
                }
            else
                {
                entity["ren_contactexpertisecountry"] = null;
                }

            // ren_address1country

            if (!string.IsNullOrEmpty(excelData.MailAddressCountry))
                {
                OptionSetValueCollection country2 = new OptionSetValueCollection();
                OptionSetValue countryOption2 = new OptionSetValue();
                string[] countries = excelData.MailAddressCountry.Split(';');
                for (int i = 0; i < countries.Length; i++)
                    countries[i] = Regex.Replace(countries[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < countries.Length; i++)
                    {
                    var findCountry = GlobalOptionCountry.Find(x => x.Label == countries[i]);
                    if (findCountry != null)
                        {
                        countryOption2 = new OptionSetValue(findCountry.Value);
                        country2.Add(countryOption2);
                        }
                    }
                entity["ren_address1country"] = country2;
                }
            else
                {
                entity["ren_address1country"] = null;
                }

            // "ren_contactexpertiseregion"


            OptionSetValueCollection region = new OptionSetValueCollection();
            OptionSetValue expertRegionOption = new OptionSetValue();

            if (!string.IsNullOrEmpty(excelData.Contact_Expertise_Region))
                {
                string[] regions = excelData.Contact_Expertise_Region.Split(';');
                for (int i = 0; i < regions.Length; i++)
                    regions[i] = Regex.Replace(regions[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < regions.Length; i++)
                    {
                    var findRegion = GlobalOptionExpertiseRegion.Find(x => x.Label == regions[i]);
                    if (findRegion != null)
                        {
                        expertRegionOption = new OptionSetValue(findRegion.Value);
                        region.Add(expertRegionOption);
                        }
                    }
                entity["ren_contactexpertiseregion"] = region;
                }
            else
                {
                entity["ren_contactexpertiseregion"] = null;
                }


            // "ren_contactexpertisetopics"


            OptionSetValueCollection topic = new OptionSetValueCollection();
            OptionSetValue expertTopicOption = new OptionSetValue();

            if (!string.IsNullOrEmpty(excelData.Contact_Expertise_Topic))
                {
                string[] topics = excelData.Contact_Expertise_Topic.Split(';');
                for (int i = 0; i < topics.Length; i++)
                    topics[i] = Regex.Replace(topics[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < topics.Length; i++)
                    {
                    var findTopic = GlobalOptionExpertiseTopic.Find(x => x.Label == topics[i]);
                    if (findTopic != null)
                        {
                        expertTopicOption = new OptionSetValue(findTopic.Value);
                        topic.Add(expertTopicOption);
                        }

                    }
                entity["ren_contactexpertisetopics"] = topic;
                }
            else
                {
                entity["ren_contactexpertisetopics"] = null;
                }

            // "ren_stakeholdergroup"


            OptionSetValueCollection stakeholder = new OptionSetValueCollection();
            OptionSetValue stakeholderOption = new OptionSetValue();

            if (!string.IsNullOrEmpty(excelData.Stakeholder_Group))
                {
                string[] stakeholders = excelData.Stakeholder_Group.Split(';');
                for (int i = 0; i < stakeholders.Length; i++)
                    stakeholders[i] = Regex.Replace(stakeholders[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < stakeholders.Length; i++)
                    {
                    var findStakeholder = GlobalOptionStakeholder.Find(x => x.Label == stakeholders[i]);
                    if (findStakeholder != null)
                        {
                        stakeholderOption = new OptionSetValue(findStakeholder.Value);
                        stakeholder.Add(stakeholderOption);
                        }

                    }
                entity["ren_stakeholdergroup"] = stakeholder;
                }
            else
                {
                entity["ren_stakeholdergroup"] = null;
                }

            return entity;
            }
        #endregion

        #region ReleaseObject
        static void ReleaseObject(object obj)
            {
            try
                {
                System.Runtime.InteropServices.Marshal.ReleaseComObject(obj);
                }
            catch
                {
                throw new Exception("$ReleaseObject Exception");
                }
            finally
                {
                GC.Collect();
                }
            }
        #endregion

        #region ExecuteMultipleCreateRequests
        static void ExecuteMultipleCreateRequests(IOrganizationService service, List<Entity> entities)
            {

            ///Define ExecuteMultipleRequest
            ExecuteMultipleRequest multipleRequest = new ExecuteMultipleRequest()
                {
                Settings = new ExecuteMultipleSettings()
                    {
                    ContinueOnError = true,
                    ReturnResponses = true
                    },
                Requests = new OrganizationRequestCollection()
                };

            foreach (var entity in entities)
                {
                ////use alternate key for contact
                Entity contactToCreate = new Entity("contact", "emailaddress1", entity["emailaddress1"]);

                ///Map fields data
                contactToCreate.Attributes["ren_contactid_import"] = entity["ren_contactid_import"];
                contactToCreate.Attributes["emailaddress1"] = entity["emailaddress1"];
                contactToCreate.Attributes["firstname"] = entity["firstname"];
                contactToCreate.Attributes["lastname"] = entity["lastname"];
                contactToCreate.Attributes["ren_organization_id_import"] = entity["ren_organization_id_import"];
                contactToCreate.Attributes["jobtitle"] = entity["jobtitle"];
                contactToCreate.Attributes["address1_line1"] = entity["address1_line1"];
                contactToCreate.Attributes["address1_city"] = entity["address1_city"];
                contactToCreate.Attributes["address1_stateorprovince"] = entity["address1_stateorprovince"];
                contactToCreate.Attributes["address1_postofficebox"] = entity["address1_postofficebox"];
                contactToCreate.Attributes["ren_businessphone"] = entity["ren_businessphone"];
                contactToCreate.Attributes["ren_mobilephone"] = entity["ren_mobilephone"];
                contactToCreate.Attributes["ren_gsr_note"] = entity["ren_gsr_note"];
                contactToCreate.Attributes["emailaddress2"] = entity["emailaddress2"];
                contactToCreate.Attributes["parentcustomerid"] = entity["parentcustomerid"];
                contactToCreate.Attributes["ren_speakerrole"] = entity["ren_speakerrole"];
                contactToCreate.Attributes["ren_panellist_assessment"] = entity["ren_panellist_assessment"];
                contactToCreate.Attributes["ren_keynote_assessment"] = entity["ren_keynote_assessment"];
                contactToCreate.Attributes["ren_moderator_assessment"] = entity["ren_moderator_assessment"];
                contactToCreate.Attributes["ren_contactexpertisecountry"] = entity["ren_contactexpertisecountry"];
                contactToCreate.Attributes["ren_address1country"] = entity["ren_address1country"];
                contactToCreate.Attributes["ren_contactexpertiseregion"] = entity["ren_contactexpertiseregion"];
                contactToCreate.Attributes["ren_contactexpertisetopics"] = entity["ren_contactexpertisetopics"];
                contactToCreate.Attributes["ren_stakeholdergroup"] = entity["ren_stakeholdergroup"];
                ///Define UpsertRequest
                var request = new UpsertRequest()
                    {
                    Target = contactToCreate
                    };
                // string my_guid = request.Target.Id.ToString();
                //Log.Information(contactToCreate.Attributes["emailaddress1"].ToString());
                try
                    {

                    // Execute UpsertRequest and obtain UpsertResponse.
                    var response = (UpsertResponse)service.Execute(request);
                    if (response.RecordCreated)
                        {
                        Console.WriteLine("New record created - email: {0} ", contactToCreate["emailaddress1"]);
                        }

                    else
                        {
                        Console.WriteLine("Existing record updated - email: {0} ", contactToCreate["emailaddress1"]);
                        }

                    }

                // Catch any service fault exceptions that Dataverse throws.
                catch (FaultException<Microsoft.Xrm.Sdk.OrganizationServiceFault>)
                    {
                    throw;
                    }
                }
            }
        #endregion
        }

    class ExcelDataTemplate
        {
        public string EmailAddress { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Id_Organization { get; set; }
        public string Role { get; set; }
        public string MailAddressStreet { get; set; }
        public string MailAddressCity { get; set; }
        public string MailAddressState { get; set; }
        public string MailAddressCountry { get; set; }
        public string MailAddressPostalCode { get; set; }
        public string BusinessPhone { get; set; }
        public string MobilePhone { get; set; }
        public string GSR_Notes { get; set; }
        public string Contact_Expertise_Region { get; set; }
        public string Contact_Expertise_Country { get; set; }
        public string Contact_Expertise_Topic { get; set; }
        public string Email_2 { get; set; }
        public string Speaker_Role { get; set; }
        public string Panellist_assessment { get; set; }
        public string Keynote_assessment { get; set; }
        public string Moderator_assessment { get; set; }
        public string Stakeholder_Group { get; set; }
        public string Id_Contact { get; set; }

        }
    }

```


## JavaScript codes

### Entity records count

```
function entityRecordCount() {
    // getGlobalContext() method provides access to the global context
    var globalContext = Xrm.Utility.getGlobalContext();

    // getClientUrl() method returns the base URL that was used to access the application
    var serverURL = globalContext.getClientUrl();

    // Count total records under account entity
    var query = "RetrieveTotalRecordCount(EntityNames=['account'])";

    // XMLHttpRequest is a JavaScript class containing methods to asynchronously transmit HTTP requests from a web browser to a web server.
    // Constructor initializes an XMLHttpRequest
    var req = new XMLHttpRequest ();

    // open() method will prepare the HTTP request to be sent, it takes three arguments: 
       //- The HTTP method you want to use
       //- The URL where you will send that request
       //- A boolean value, true for asynchronous request
    req.open("GET", serverURL + "/api/data/v9.2/" + query, true);

    // setRequestHeader() method sets the value of an HTTP request header
    req.setRequestHeader("Accept", "application/json");
    req.setRequestHeader("Content-Type", "application/json; charset=utf-8");
    req.setRequestHeader("OData-MaxVersion", "4.0");
    req.setRequestHeader("OData-Version", "4.0");

    // onreadystatechange defines a function to be called when the readyState property changes
    req.onreadystatechange = function () {
        // readyState hold the status of the XMLHttpRequest
           // 0: request is not initialized
           // 1: Server connection established
           // 2: Request received
           // 3: Processing request
           // 4: Request finished and response is ready

       if (this.readyState == 4 && this.status == 200)
       {
            // The JSON.parse() method parses a JSON string
            result = JSON.parse(this.response);
            alert(result['EntityRecordCountCollection']['Values'])

       }
    };

    // send() method is used to send the request
    req.send();

}
```
