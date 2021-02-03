

# Verse Core API

## Table of Contents
- [Lead Ingestion](#lead-ingestion)
    - [Create Lead](#create-lead)
    - [Definition](#definition)
- [End Conversation](#end-conversation)
- [Lead Activity Notification](#lead-activity-notification)
- [Header Codes](#header-codes)



# Lead Ingestion
## __Create Lead__
### Request
**Type**: Ingestion

**Method**: POST /zapier HTTP/1.1

**Host**: https://api.agentology.com/v1

### Headers
```
Content-Type: application/json
Accept: application/json
X-API-KEY: <Your Api Key>
```
### Request
The requests for the creation of a lead in Verse.io has the following possible parameters

#### Sample

``` js
{
    "firstName"       : 'Buzz',
    "lastName"        : 'Aldrin',
    "email"           : 'buzzaldrin@verse.io',
    "phoneNumber"     : '619-123-4567',
    "type"            : 'buyer',
    "street"          : '101 West Broadway',
    "city"            : 'San Diego',
    "state"           : 'CA',
    "postalCode"      : '92101',
    "leadComment"     : 'Example Lead Comment',
    "channelWebsite"  : 'Verse',
    "zapierLeadId"    : 'f30d896a-1317-4814-88ca-ff71c40bead1'
    "agent.firstName" : 'James',
    "agent.lastName"  : 'Bond',
    "agent.email"     : '007@verse.io',
    "agent.phone"     : '700-070-0707',
    "agent.calendly"  : 'https://calendly.com/verse007',
    "agent.teamName"  : '007'
}
```

### Definition:

``` js
inputFields: [
	{
		key :  'firstName',
		type :  'string',
		helpText:  '(First Name)',
		label :  'First Name',
	},
	{
		key :  'lastName',
		type :  'string',
		helpText:  '(Last Name)',
		label :  'Last Name',
	},
	{
		key :  'email',
		type :  'string',
		helpText:  '(Email)',
		label :  'Email',
	},
	{
		key :  'phoneNumber',
		type :  'string',
		helpText:  '(Phone Number) Enter the lead\'s phone number',
		label :  'Phone Number',
		required:  true,
	},
	{
		key :  'type',
		type :  'string',
		label :  'Lead Type',
		choices : {
			buyer :  'buyer',
			seller :  'seller',
			mortgage :  'mortgage',
			refi :  'refi',
		},
	},
	{
		key :  'street',
		type :  'string',
		helpText:  '(Street Address)',
		label :  'Street Address',
	},
	{
		key :  'city',
		type :  'string',
		helpText:  '(City)',
		label :  'City',
	},
	{
		key :  'state',
		type :  'string',
		helpText:  '(State)',
		label :  'State',
	},
	{
		key :  'postalCode',
		type :  'string',
		helpText:  '(Postal Code) Valid postal codes for US and Canada only',
		label :  'Postal Code',
	},
	{
		key :  'leadComment',
		type :  'text',
		helpText:  '(Lead Comment) Can be any value - field is capped at 1024 characters.',
		label :  'Lead Comment',
	},
	{
		key :  'channelWebsite',
		type :  'string',
		helpText:  '(Lead Source)',
		label :  'Lead Source',
	},
	{
		key :  'zapierLeadId',
		type :  'string',
		helpText:  '(External Lead Id) Should be used to reference your lead in your system.',
		label :  'External Lead Id',
	},
	{
		key:  'agent.firstName',
		type:  'string',
		helpText:  '(Owner First Name)',
		label:  'Owner First Name'
	},
	{
		key:  'agent.lastName',
		type:  'string',
		helpText:  '(Owner Last Name)',
		label:  'Owner Last Name'
	},
	{
		key:  'agent.email',
		type:  'string',
		helpText:  '(Owner Email)',
		label:  'Owner  Email'
	},
	{
		key:  'agent.phone',
		type:  'string',
		helpText:  '(Phone Number) - E164 preferred',
		label:  'Owner Phone Number'
	},
    {
        key: 'agent.calendly',
        type: 'string',
        helpText: '(Calendar Integration Link)',
        label: 'Owner  Calendar Integration Link',
    },
    {
        key: 'agent.teamName',
        type: 'string',
        helpText: '(Owner Team Name)',
        label: 'Owner Team Name'
    },
    {
        key: 'customFields',
        type: 'object',
        helpText: '(Custom Field) Additional information',
        label: 'Custom Fields'
    }
],
```

### Response

The response will contain an UUID V4 which is the ID for the lead if it is created and not blocked based on duplication rules and other user settings

``` js
{
    "id": "cfdc10f0-f7ae-49f6-8343-b651f4620cbc"
}
```

### Lead Activity Notification
#### Request
**Type**: Outbound Webhook

**Method**: POST /{your url}

**Header**: Authentication Method

**Host**: https://{your url}
#### Payload

##### Sample
```js
{
    id                      : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    channelWebsite          : "Referral Exchange",
    city                    : "Yadkinville",
    email                   : "buzz@yahoo.com",
    externalLeadId          : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e1",
    firstName               : "Katrina",
    lastName                : "Jones",
    leadComment             : "Katrina is working with an agent and is not interested in our services.",
    leadType                : "seller",
    link                    : "https://app.verse.io/leads/5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    message                 : "Outbound Forwarded",
    phone                   : "(619) 426-8081",
    postalCode              : null,
    state                   : "NC",
    street                  : "Yadkinville",
    title                   : "Verse Activity Log",
    event                   : "lead_activity",
    userId                  : "3fc665d4-4016-4f95-a9f4-374530d41640"
}
```

##### Definition

``` js
outputFields: [
    {
      key: 'channelWebsite',
      label: 'Lead Channel Website'
    },
    {
      key: 'city',
      label: 'Lead City'
    },
    {
      key: 'email',
      label: 'Lead Email'
    },
    {
      key: 'externalLeadId',
      label: 'External Lead Id'
    },
    {
      key: 'firstName',
      label: 'Lead First Name'
    },
    {
      key: 'id',
      label: 'Lead Id'
    },
    {
      key: 'lastName',
      label: 'Lead Last Name'
    },
    {
      key: 'leadComment',
      label: 'Lead Comment'
    },
    {
      key: 'leadType',
      label: 'Lead Type'
    },
    {
      key: 'link',
      label: 'Lead Link'
    },
    {
      key: 'message',
      label: 'Verse Note'
    },
    {
      key: 'postalCode',
      label: 'Lead Postal Code'
    },
    {
      key: 'state',
      label: 'Lead State'
    },
    {
      key: 'street',
      label: 'Lead Street Address'
    },
    {
      key: 'title',
      label: 'Verse Type'
    },
    {
      key: 'event',
      label: 'Verse Event'
    },
    {
      key: 'userId',
      label: 'Verse User ID'
    }
]
```

### Lead Unqualified Notification
#### Request
**Type**: Outbound Webhook

**Method**: POST /{your url}

**Header**: Authentication Method

**Host**: https://{your url}

#### Payload

##### Sample
> Note: `reason_unqualified` is dynamic based on the reason we deemed your lead unqualified
```js
{
    channelWebsite          : "Referral Exchange",
    city                    : "Yadkinville",
    email                   : "buzz@yahoo.com",
    externalLeadId          : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e1",
    firstName               : "Katrina",
    id                      : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    lastName                : "Jones",
    leadComment             : "Katrina is working with an agent and is not interested in our services.",
    leadType                : "seller",
    link                    : "https://app.verse.io/leads/5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    message                 : "We've Unqualified this lead for the following reason: {reason_unqualified}",
    phone                   : "(336) 426-8081",
    postalCode              : null,
    state                   : "NC",
    street                  : "Yadkinville",
    title                   : "Unqualified Lead",
    event                   : "lead_unqualify",
    userId                  : "3fc665d4-4016-4f95-a9f4-374530d41640",
    customQuestions         : {},
}
```

##### Definition
> Note: The custom fields that are sent back are custom per user based on the original questions you designed with our CSAs
``` js
outputFields: [
    {
        key: 'channelWebsite',
        label: 'Lead Channel Website'
    },
    {
        key: 'city',
        label: 'Lead City'
    },
    {
        key: 'email',
        label: 'Lead Email'
    },
    {
        key: 'externalLeadId',
        label: 'External Lead Id'
    },
    {
        key: 'firstName',
        label: 'Lead First Name'
    },
    {
        key: 'id',
        label: 'Lead Id'
    },
    {
        key: 'lastName',
        label: 'Lead Last Name'
    },
    {
        key: 'leadComment',
        label: 'Lead Comment'
    },
    {
        key: 'leadType',
        label: 'Lead Type'
    },
    {
        key: 'link',
        label: 'Lead Link'
    },
    {
        key: 'message',
        label: 'Verse Note'
    },
    {
        key: 'phone',
        label: 'Lead Phone'
    },
    {
        key: 'postalCode',
        label: 'Lead Postal Code'
    },
    {
        key: 'state',
        label: 'Lead State'
    },
    {
        key: 'street',
        label: 'Lead Street Address'
    },
    {
        key: 'title',
        label: 'Verse Type'
    },
    {
        key: 'event',
        label: 'Verse Event'
    },
    {
        key: 'userId',
        label: 'Verse User ID'
    },
    {
        key: 'reasonUnqualified',
        label : 'Lead Reason Unqualified'
    },
    {
        key: 'customQuestions',
        label: 'Custom Questions'
    },
]
```


### Lead Qualified Notification
#### Request
**Type**: Outbound Webhook

**Method**: POST /{your url}

**Header**: Authentication Method

**Host**: https://{your url}

#### Payload

##### Sample
```js
{
    channelWebsite          : "Referral Exchange",
    city                    : "Yadkinville",
    email                   : "buzz@yahoo.com",
    externalLeadId          : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e1",
    firstName               : "Katrina",
    id                      : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    lastName                : "Jones",
    leadComment             : "Katrina is working with an agent and is not interested in our services.",
    leadType                : "seller",
    link                    : "https://app.verse.io/leads/5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    message                 : "We've qualified a new lead for you!",
    phone                   : "(336) 426-8081",
    postalCode              : null,
    state                   : "NC",
    street                  : "Yadkinville",
    title                   : "Qualified Lead",
    event                   : "lead_qualify",
    userId                  : "3fc665d4-4016-4f95-a9f4-374530d41640",
    customQuestions         : {},
}
```

##### Definition
> Note: The custom fields that are sent back are custom per user based on the original questions you designed with our CSMs
``` js
outputFields: [
    {
        key: 'channelWebsite',
        label: 'Lead Channel Website'
    },
    {
        key: 'city',
        label: 'Lead City'
    },
    {
        key: 'email',
        label: 'Lead Email'
    },
    {
        key: 'externalLeadId',
        label: 'External Lead Id'
    },
    {
        key: 'firstName',
        label: 'Lead First Name'
    },
    {
        key: 'id',
        label: 'Lead Id'
    },
    {
        key: 'lastName',
        label: 'Lead Last Name'
    },
    {
        key: 'leadComment',
        label: 'Lead Comment'
    },
    {
        key: 'leadType',
        label: 'Lead Type'
    },
    {
        key: 'link',
        label: 'Lead Link'
    },
    {
        key: 'message',
        label: 'Verse Note'
    },
    {
        key: 'phone',
        label: 'Lead Phone'
    },
    {
        key: 'postalCode',
        label: 'Lead Postal Code'
    },
    {
        key: 'state',
        label: 'Lead State'
    },
    {
        key: 'street',
        label: 'Lead Street Address'
    },
    {
        key: 'title',
        label: 'Verse Type'
    },
    {
        key: 'event',
        label: 'Verse Event'
    },
    {
        key: 'userId',
        label: 'Verse User ID'
    },
    {
        key: 'customQuestions',
        label: 'Custom Questions'
    },
]
```


### Lead Created Notification
#### Request
**Type**: Outbound Webhook

**Method**: POST /{your url}

**Header**: Authentication Method

**Host**: https://{your url}

#### Payload

##### Sample
```js
{
    channelWebsite          : "Referral Exchange",
    city                    : "Yadkinville",
    email                   : "buzz@yahoo.com",
    externalLeadId          : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e1",
    firstName               : "Katrina",
    id                      : "5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    lastName                : "Jones",
    leadComment             : "Katrina is working with an agent and is not interested in our services.",
    leadType                : "seller",
    link                    : "https://app.verse.io/leads/5261a2b5-ddf7-11e8-ba7d-0a04f6df74e2",
    message                 : "We've qualified a new lead for you!",
    phone                   : "(336) 426-8081",
    postalCode              : null,
    state                   : "NC",
    street                  : "Yadkinville",
    title                   : "Lead Created",
    event                   : "lead_created",
    userId                  : "3fc665d4-4016-4f95-a9f4-374530d41640"
}
```

##### Definition
> Note: The custom fields that are sent back are custom per user based on the original questions you designed with our CSAs
``` js
outputFields: [
    {
        key: 'channelWebsite',
        label: 'Lead Channel Website'
    },
    {
        key: 'city',
        label: 'Lead City'
    },
    {
        key: 'email',
        label: 'Lead Email'
    },
    {
        key: 'externalLeadId',
        label: 'External Lead Id'
    },
    {
        key: 'firstName',
        label: 'Lead First Name'
    },
    {
        key: 'id',
        label: 'Lead Id'
    },
    {
        key: 'lastName',
        label: 'Lead Last Name'
    },
    {
        key: 'leadComment',
        label: 'Lead Comment'
    },
    {
        key: 'leadType',
        label: 'Lead Type'
    },
    {
        key: 'link',
        label: 'Lead Link'
    },
    {
        key: 'message',
        label: 'Verse Note'
    },
    {
        key: 'phone',
        label: 'Lead Phone'
    },
    {
        key: 'postalCode',
        label: 'Lead Postal Code'
    },
    {
        key: 'state',
        label: 'Lead State'
    },
    {
        key: 'street',
        label: 'Lead Street Address'
    },
    {
        key: 'title',
        label: 'Verse Type'
    },
    {
        key: 'event',
        label: 'Verse Event'
    },
    {
        key: 'userId',
        label: 'Verse User ID'
    }
]
```


#### Header Codes

HTTP response codes are used to determine the status of an API request.

<table>
  <tr>
    <th>Code</th>
    <th>Explanation</th>
  </tr>
  <tr>
    <td><strong>200</strong></td>
    <td>Successful request</td>
  </tr>
  <tr>
    <td><strong>400</strong></td>
    <td>Mal-formed request could not be processed</td>
  </tr>
  <tr>
    <td><strong>401</strong></td>
    <td>Missing or invalid authentication key/token</td>
  </tr>
  <tr>
    <td><strong>403</strong></td>
    <td>Authentication key/token does not have required permissions for this request</td>
  </tr>
  <tr>
    <td><strong>404</strong></td>
    <td>Requested resource could not be found</td>
  </tr>
  <tr>
    <td><strong>409</strong></td>
    <td>Requested conflicts with another request</td>
  </tr>
  <tr>
    <td><strong>422</strong></td>
    <td>Well-formed request could not be processed</td>
  </tr>
  <tr>
    <td><strong>429</strong></td>
    <td>Request has exceeded API rate limit</td>
  </tr>
  <tr>
    <td><strong>5xx</strong></td>
    <td>Verse server error</td>
  </tr>
</table>