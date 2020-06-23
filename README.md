
## Lead Ingestion
### __Create Lead__
#### Request
**Type**: Ingestion

**Method**: POST /zapier HTTP/1.1

**Host**: https://zapier.agentology.com/v1

##### Headers
```
Content-Type: application/json
Accept: application/json
X-API-KEY: <Your Zapier Api Key>
```
##### Request
The requests for the creation of a lead in Verse.io has the following possible parameters

###### Sample 
``` js 
{
     lead: {
        firstName        : 'Buzz',
        lastName         : 'Aldrin',
        email            : 'buzzaldrin@verse.io',
        phoneNumber      : '619-123-4567',
        type             : 'buyer',
        street           : '101 West Broadway',
        city             : 'San Diego',
        state            : 'CA',
        postalCode       : '92101',
        leadComment      : '!! Example Lead Comment !!',
        channelWebsite   : 'Verse',
        externalLeadId   : 'f30d896a-1317-4814-88ca-ff71c40bead1'
    },
    dynamic_agent: {
        firstName: 'James',
        lastName: 'Bond',
        email: '007@verse.io',
        phone: '007-070-0707',
        calendly: 'https://calendly.com/verse007'
    }
}
```

**Definition**:
``` js
inputFields: [
	{
		key :  'firstName',
		type :  'string',
		helpText:  '(First Name) Can be any value - field is capped at 85 characters.',
		label :  'First Name',
	},
	{
		key :  'lastName',
		type :  'string',
		helpText:  '(Last Name) Can be any value - field is capped at 85 characters.',
		label :  'Last Name',
	},
	{
		key :  'email',
		type :  'string',
		helpText:  '(Email) Can be any value - field is capped at 125 characters.',
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
			seller:  'seller',
			mortgage:  'mortgage',
		},
	},
	{
		key :  'street',
		type :  'string',
		helpText:  '(Street Address) Can be any value - field is capped at 85 characters.',
		label :  'Street Address',
	},
	{
		key :  'city',
		type :  'string',
		helpText:  '(City) Can be any value - field is capped at 85 characters.',
		label :  'City',
	},
	{
		key :  'state',
		type :  'string',
		helpText:  '(State) Can be any value - field is capped at 85 characters.',
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
		helpText:  '(Lead Source) Can be any value - field is capped at 85 characters.',
		label :  'Lead Source',
	},
	{
		key :  'zapierLeadId',
		type :  'string',
		helpText:  '(External Lead Id) Should be used to reference your lead.',
		label :  'External Lead Id',
	},
	{
		key:  'agent.firstName',
		type:  'string',
		helpText:  '(Agent First Name) Can be any value - field is capped at 85 characters.',
		label:  'Agent First Name'
	},
	{
		key:  'agent.lastName',
		type:  'string',
		helpText:  '(Agent Last Name) Can be any value - field is capped at 85 characters.',
		label:  'Agent Last Name'
	},
	{
		key:  'agent.email',
		type:  'string',
		helpText:  '(Agent Email) Can be any value - field is capped at 125 characters.',
		label:  'Agent Email'
	},
	{
		key:  'agent.phone',
		type:  'string',
		helpText:  '(Agent Phone Number) Enter the agent\'s phone number',
		label:  'Agent Phone Number'
	},
	{
		key:  'agent.calendly',
		type:  'string',
		helpText:  '(Agent Calendly Link) Enter the agent\'s calendly link',
		label:  'Agent Calendly Link'
	}
],
```

##### Response 
The response will contain an UUID which is the ID for the lead if it is created and not blocked based on duplication rules and other user settings

``` js
{
    "id": "cfdc10f0-f7ae-49f6-8343-b651f4620cbc"
}
```

### End Conversation
#### Request
**Type**: Ingestion

**Method**: POST /zapier/end-conversation HTTP/1.1

**Host**: https://zapier.agentology.com/v1

##### Headers
```
Content-Type: application/json
Accept: application/json
X-API-KEY: <Your Zapier Api Key>
```
##### Request
The requests ends a converation Verse.io is having with your lead

###### Sample 
``` js 
{
    "searchValue" : "buzz@aol.com"
}
```

**Definition**:
``` js
inputFields: [
    {
        key: 'searchValue',
        type: 'string',
        label: 'Email or Phone',
        helpText: 'End Conversation with a lead using this value.',
    }
],
```

##### Response 
The response will a status code of success of failure (if there is no lead for the search parameter, etc) and a body payload that will contain basic lead information to use in subsequent Zaps

``` js
{
    "statusCode": 200,
    "body"      : {
                    firstName: 'Buzz',
                    lastName: 'Aldrin',
                    email: 'buzzaldrin@verse.io',
                    phone: '619-123-4567',
                    leadType: 'buyer',
                    street: '101 West Broadway',
                    city: 'San Diego',
                    state: 'CA',
                    postalCode: '92101',
                    leadComment: '!! Example Lead Comment !!',
                    leadSource: 'Zillow'
                },
}
```

### Lead Activity Notification
#### Request
**Type**: Outbound Webhook

**Method**: POST /hooks/standard/{zapier_id}/{zapier_hook_id}/

**Host**: https://hooks.zapier.com

> Note: The `zapier_id` and `zapier_hook_id` are both set on zapier's side of the communication. We do not have any control over that.

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
]
```    

### Lead Unqualified Notification
#### Request
**Type**: Outbound Webhook

**Method**: POST /hooks/standard/{zapier_id}/{zapier_hook_id}/

**Host**: https://hooks.zapier.com

> Note: The `zapier_id` and `zapier_hook_id` are both set on zapier's side of the communication. We do not have any control over that.

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

**Method**: POST /hooks/standard/{zapier_id}/{zapier_hook_id}/

**Host**: https://hooks.zapier.com

> Note: The `zapier_id` and `zapier_hook_id` are both set on zapier's side of the communication. We do not have any control over that.

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
        key: 'customQuestions',
        label: 'Custom Questions'
    },
]
```    
