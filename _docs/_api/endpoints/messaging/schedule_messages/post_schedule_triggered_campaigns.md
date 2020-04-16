---
nav_title: "POST: Schedule API Triggered Campaign Messages"
page_order: 4

layout: api_page

page_type: reference
platform: API
tool:
  - Campaigns

description: "This article outlines details about the Update Scheduled Campaigns Braze endpoint."
---
{% api %}
# Schedule API Triggered Campaigns
{% apimethod post %}
/campaigns/trigger/schedule/create
{% endapimethod %}

Use this endpoint to trigger API Triggered Campaigns, which are created on the Dashboard and initiated via the API. You can pass in `trigger_properties` that will be templated into the message itself.

This endpoint allows you to send Campaign messages via API Triggered delivery, allowing you to decide what action should trigger the message to be sent. Please note that to send messages with this endpoint, you must have a Campaign ID, created when you build an [API Triggered Campaign]({{site.baseurl}}/api/api_campaigns/).

{% apiref swagger %}https://www.braze.com/docs/api/interactive/#/Messaging/CreateScheduledApiTriggeredCampaignExample {% endapiref %}
{% apiref postman %}https://documenter.getpostman.com/view/4689407/SVYrsdsG?version=latest#2608e4a6-24eb-4d24-88b2-86382e62d6dc {% endapiref %}


## Request Body

```
Content-Type: application/json
```

```json
{
  "campaign_id": (required, string) see Campaign Identifier,
  "send_id": (optional, string) see Send Identifier,
  // Including 'recipients' will send only to the provided user ids if they are in the campaign's segment
  "recipients": (optional, Array of Recipient Object),
  // for any keys that conflict between these trigger properties and those in a Recipient Object, the value from the Recipient Object will be used
  "audience": (optional, Connected Audience Object) see Connected Audience,
  // Including 'audience' will only send to users in the audience
  // If 'recipients' and 'audience' are not provided and broadcast is not set to 'false',
  // the message will send to entire segment targeted by the campaign
  "broadcast": (optional, boolean) see Broadcast -- defaults to false on 8/31/17, must be set to true if "recipients" object is omitted,
  "trigger_properties": (optional, object) personalization key-value pairs for all users in this send; see Trigger Properties,
  "schedule": {
    "time": (required, datetime as ISO 8601 string) time to send the message,
    "in_local_time": (optional, bool),
    "at_optimal_time": (optional, bool),
  }
}
```
## Request Components

- [Campaign Identifier]({{site.baseurl}}/api/identifier_types/)
- [Recipients]({{site.baseurl}}/api/objects_filters/recipient_object/)
- [Connected Audience]({{site.baseurl}}/api/objects_filters/connected_audience/)
- [Broadcast]({{site.baseurl}}/api/parameters/#broadcast)
- [Trigger Properties]({{site.baseurl}}/api/objects_filters/trigger_properties_object/)
- [Schedule Object]({{site.baseurl}}/api/objects_filters/schedule_object/)

### Exmaple Request
```
curl --location --request POST 'https://rest.iad-01.braze.com/campaigns/trigger/schedule/create?campaign_id=&send_id&recipients=&audience=&broadcast=&trigger_properties=&schedule=' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-API-KEY-HERE' \
--data-raw '{
  "campaign_id": "",
  "send_id": "",
  "recipients": {
    "user_alias": "",
    "external_user_id": "",
    "trigger_properties": "",
    "canvas_entry_properties": ""
  },
  "audience": {
    "AND": [
      {
        "custom_attribute": {
          "custom_attribute_name": "eye_color",
          "comparison": "equals",
          "value": "blue"
        }
      },
      {
        "custom_attribute": {
          "custom_attribute_name": "favorite_foods",
          "comparison": "includes_value",
          "value": "pizza"
        }
      },
      {
        "OR": [
          {
            "custom_attribute": {
              "custom_attribute_name": "last_purchase_time",
              "comparison": "less_than_x_days_ago",
              "value": 2
            }
          },
          {
            "push_subscription_status": {
              "comparison": "is",
              "value": "opted_in"
            }
          }
        ]
      },
      {
        "email_subscription_status": {
          "comparison": "is_not",
          "value": "subscribed"
        }
      },
      {
        "last_used_app": {
          "comparison": "after",
          "value": "2019-07-22T13:17:55+0000"
        }
      }
    ]
  },
  "broadcast": false,
  "trigger_properties": "",
  "schedule": {
    "time": "",
    "in_local_time": false,
    "at_optimal_time": false
  }
}
'
```


{% endapi %}
