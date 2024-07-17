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


#### Detailed flow

![](images/mk_list_to_mailchimp.png) 

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

## C# Plug-Ins
