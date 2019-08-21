# Authentication 
- Comm100 Ticket API provides 2 authentication methods: 
    - API_key Authentication 
    - OAuth Authentication 
- [document](https://www.comm100.com/doc/api/introduction.htm#/) 

# Parameter introduction 
- Incoming parameters:
    - Get API passes parameters through the query string 
    - Put/Post API passes parameters through json data. 
    - DateTime Parameters: 
        - All time parameters need to be entered in the standard format of <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO-8601</a>
    - The maximum size for a ticket’s attachments is 20MB.
- All time values in response are UTC time. The caller can convert to different time zones where needed. 

# Includes
- Following APIs can use `Includes` as parameters to get related objects.

    | Endpoints | Support including parameters |
    | - | - |
    | `get api/v2/ticket/tickets` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy |
    | `get api/v2/ticket/tickets/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v2/ticket/tickets/{id}/messages` | sender |
    | `get api/v2/ticket/deletedTickets` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy |
    | `get api/v2/ticket/deletedTickets/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v2/ticket/deletedTickets/{id}/messages` | sender |
    | `get api/v2/ticket/portalTickets/{id}` | contact, messages |
    | `get api/v2/ticket/portalTickets` | contact |
    | `get api/v2/ticket/portalTickets/{id}/messages` | sender | 

- Sample:
    - request: `get api/v2/ticket/tickets/{id}?include=agentAssignee,contact,createdBy,messages`
    - response:

        ``` javascript
        {
            "ticket": {
                "id": 1,
                "agentAssigneeId": 1,
                "agentAssignee": {  //included the agent object
                    "id": 1,
                    //...
                },
                "contactId":2
                "contact": {  //included the contact object
                    "id": 2,
                    //...
                },
                "createdById": 3,
                "createdByType": "agent",
                "createdBy": {  //included the agent or contact object according to the createdByType.
                    "id": 3,
                    //...
                },
                "messages":[    //included the messages.
                    {
                        "id": 56, 
                        //...
                    },
                    {
                          "id": 57, 
                        //...
                    }
                ]
            //...
            }
        }, 
        ``` 

# Resource List 
|Name|EndPoint|Note| 
|---|---|---| 
|[Ticket](#tickets)|/api/v2/ticket/tickets| Points for agent console| 
|[PortalTicket](#portalTickets)|/api/v2/ticket/portalTickets| Points for portal and contacts |
|[Filter](#filters)|/api/v2/ticket/filters| Agent console filters| 
|[Field](#fields)|/api/v2/ticket/fields| System fields and custom fields | 
|[Attachment](#attachments)|/api/v2/ticket/attachments| Upload attachment for tickets | 
|[BlockedSender](#blockedsenders)|/api/v2/ticket/blockedSenders|Blocked email or domain| 
|[CannedResponse](#cannedresponses)|/api/v2/ticket/cannedResponses| Canned response for quickly reply ticket in agent console |
|[Config](#configs)|/api/v2/ticket/configs| Get site settings| 
|[Department](#departments)|/api/v2/ticket/departments|Ticket departments | 
|[EmailAccount](#emailaccounts)|/api/v2/ticket/emailAccounts| Email accounts| 
|[JunkEmail](#junkemails)|/api/v2/ticket/junkEmails| Emails from blocked senders| 
|[Tag](#tags)|/api/v2/ticket/tags|| 

# Tickets 
## objects 
### ticket 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of ticket | 
| `subject` | string | ticket subject | 
| `agentAssigneeId` | integer | agent assignee id | 
| `departmentAssigneeId` | integer | department assignee id | 
| `contactId` | integer | contact id | 
| `receivedFrom` | string | received email address for email channel | 
| `channel` | string | `portal`, `email`| 
| `priority` | string | `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, <br/>`pendingExternal`, `onHold`, `closed` | 
| `isRead` | boolean | if the ticket is read | 
| `isReadByContact` | boolean | if the portal ticket is read by contact |
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array | 
| `createdById` | integer | contact id or agent id | 
| `createdByType` |  string | agent or contact or system | 
| `createdTime` | datetime | create time of ticket | 
| `lastActivityTime` | datetime | last activity time of ticket | 
| `lastStatusChangeTime` | datetime | last status change time of ticket | 
| `lastRepliedTime` | datetime | last replied time | 
| `lastRepliedById` | integer | contact id or agent id | 
| `lastRepliedByType` | string | `agent` or `contact` or `system`| 
| `hasDraft` | boolean | if has draft | 
| `tagIds` | integer[] | tag id array | 
| `isDeleted` | boolean | if deleted | 
| `slaPolicyId` | integer | SLA id of this ticket matched | 
| `firstRespondBreachAt` | datetime | Timestamp that denotes when the first <br/> response is due | 
| `nextRespondBreachAt` | datetime | Timestamp that denotes when the next <br/> response is due | 
| `resolveBreachAt` | datetime | Timestamp that denotes when the ticket is <br/> due to be resolved | 
| `mentionedAgents`|[mentioned agent](#mentioned-agent)[]| mentioned agents list | 
| `isEditable`| boolean | if the current agent can update\reply the ticket | 
| `lastMessage`| string | plain text of last message | 
| `totalReplies`| int | total replies number | 

### custom field value
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | the id of custom field |
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### custom field id and value
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | the id of custom field | 
| `value` | string | the value of custom field |

### mentioned agent 
| Name | Type | Description | 
| - | - | - | 
| `agentId` | integer | the agent id of mentioned | 
| `isRead`| boolean | if the mentioned ticket is read | 
| `messageId`| integer| message id| 

### message 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of message | 
| `ticketId` | integer | id of ticket | 
| `type` | string | `note`, `email`, `reply` | 
| `htmlBody` | string | html body of message | 
| `plainBody` | string | plain text body of message | 
| `quote` | string | quoted content of the message, only for email message | 
| `senderId`| integer | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | the sent time of the message | 
| `subject` | string | subject | 
| `from` | string | from email address| 
| `to` | string | the to email addresses | 
| `cc` | string | cc email addresses |  
| `attachments` | [attachment](#attachment)[] | attachment array| 
| `mentionedAgentIds` | integer[] | only for Note, @mentioned agents id array |

### ticket draft 
| Name | Type | Description | 
| - | - | - | 
| `draftId` | integer | id of ticket draft | 
| `ticketId` | integer | id of ticket | 
| `subject` | string | draft subject | 
| `htmlBody` | string | html body of ticket draft | 
| `plainBody` | string | plain text body of ticket draft | 
| `quote` | string | quoted body | 
| `from` | string | from email address | 
| `to` | string | to email address | 
| `cc` | string | cc email addresses | 
| `savedTime` | datetime | | 
| `savedById` | integer | the id of the agent who saved the ticket draft | 
| `attachments` | [attachment](#attachment)[] | draft attachments | 

## endpoints 
### List tickets 
`get api/v2/ticket/tickets` 
+ Each request returns a maximum of 50 tickets. 
+ Parameters 
    - filterId: integer, filter id  
    - tagId: integer, tag id
    - keywords: string
    - timeFrom: DateTime, last reply time, default search the last 30 days, ISO-8601 time format,
    - timeTo: DateTime, last reply time, default value is the current time, ISO-8601 time format,
    - timeZoneOffset, float, time zone based on your date parameters in ticket conditions. Such date parameters might be @today, @last 7 days for example.
    - pageIndex: integer
    - sortBy: string, `nextSLABreach`, `lastReplyTime`, `lastActivityTime`, `priority`, `status` , default value: `lastReplyTime`
    - sortOrder: string, `ascending` or `descending`, default value: `descending`
    - conditions: parameter format: `conditions[0][field]=subject&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=status&conditions[1][matchType]=is&conditions[1][value]=new`, fields can be ticket system fields and custom fields.
        - field: string, field name
        - matchType: string 
        - value: string
+ Response 
    - tickets: [ticket](#tickets) list, 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/tickets?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/tickets?include=departmentAssignee` |
    | contact | `get api/v2/ticket/tickets?include=contact` |
    | createdBy | `get api/v2/ticket/tickets?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/tickets?include=lastRepliedBy` | 

### Get a ticket 
`get api/v2/ticket/tickets/{id} ` 
+ Parameters 
    - id: integer, ticket  
+ Response 
    - ticket: [ticket](#ticket) 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/tickets/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/tickets/{id}?include=departmentAssignee` |
    | contact | `get api/v2/ticket/tickets/{id}?include=contact` |
    | createdBy | `get api/v2/ticket/tickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/tickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v2/ticket/tickets/{id}?include=messages` |
 
### Submit a new ticket 
`post api/v2/ticket/tickets` 
- Parameters 
    - subject: string, ticket subject, required
    - channel: string, `portal`, `email`, required 
    - contactId: integer, contact id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, `urgent`, `high`, `normal`, `low`, default value: `normal` 
    - status: string, `new`, `pendingInternal`, `pendingExternal`, `onHold`, `closed`, default value: `new`  
    - customFields: [custom field id and value](#custom-field-id-and-value)[], custom field value array
    - tagIds: integer[], tag id array
    - message: the first message of the ticket, required
        - type: string, `note`, `email`, `reply`, required
        - subject: string, for email message, email subject
        - htmlBody: string, html body of message
        - plainBody: string, plain text body of message
        - quote: string
        - from: string, for email type message, one of email account address 
        - cc: string, message cc emails 
        - attachments: [attachment](#attachment)[], attachment array
+ Response 
    - ticket: [ticket](#tickets)

### List messages of a ticket 
`get api/v2/ticket/tickets/{id}/messages` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - messages: [message](#message) list 
+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v2/ticket/tickets/{id}/messages?include=sender` |

### Update a ticket 
`put api/v2/ticket/tickets/{id}` 
- Parameters 
    - id: integer, ticket id
    - subject: string, ticket subject
    - contactId: integer, the contact id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, priority: `urgent`, `high`, `normal`, `low`
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed`
    - isRead: boolean
    - customFields: [custom field id and value](#custom-field-id-and-value)[], custom field value array
    - tagIds: integer[], tag id array
- Response 
    - ticket: [ticket](#ticket) 

### Batch update tickets
`put api/v2/ticket/tickets/` 
+ Parameters 
    - ids: integer[], ticket id array
    - status, string
    - priority, string
    - agentAssigneeId, integer
    - departmentAssigneeId, integer
    - isRead, boolean
+ Response 
    - tickets: [ticket](#ticket) list 

### Reply a ticket 
`post api/v2/ticket/tickets/{id}/messages` 
- Parameters  
    - type: string, `note`, `email`, `reply`, required
    - subject: string, for email message, email subject
    - htmlBody: string, html body of message, if you want to @mention an agent in a note, you can use the format: `<span data-id=agentId class="athighlight">note body</span>`
    - plainBody: string, plain text body of message
    - quote: string, quote content, only for email message
    - from: string, for email type message, one of email account address 
    - cc: string, message cc emails 
    - attachments: [attachment](#attachment)[], attachment array
- Response 
    - message: [message](#message) 

### Mark a ticket as read 
`put api/v2/ticket/tickets/{id}/read` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - http status code

### Mark a ticket as unread 
`put api/v2/ticket/tickets/{id}/unread` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - http status code

### Delete a ticket 
`delete api/v2/ticket/tickets/{id}` 
- Parameters 
    - id: integer, ticket id
- Response 
    - http status code 

### Batch delete tickets 
`delete api/v2/ticket/tickets` 
+ Parameters 
    - ids: integer[], id array
+ Response 
    - http status code 

### List deleted tickets 
`get api/v2/ticket/deletedTickets/` 
- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime, last reply time, default search the last 30 days
    - timeTo: DateTime, last reply time, defautl value is the current time
- Response 
    - deletedTickets: [ticket](#ticket) list 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/deletedTickets?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/deletedTickets?include=departmentAssignee` |
    | contact | `get api/v2/ticket/deletedTickets?include=contact` |
    | createdBy | `get api/v2/ticket/deletedTickets?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/deletedTickets?include=lastRepliedBy` | 

### Get a deleted ticket 
`get api/v2/ticket/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - deletedTicket: [ticket](#ticket) 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/deletedTickets/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/deletedTickets/{id}?include=departmentAssignee` |
    | contact | `get api/v2/ticket/deletedTickets/{id}?include=contact` |
    | createdBy | `get api/v2/ticket/deletedTickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/deletedTickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v2/ticket/deletedTickets/{id}?include=messages` |

### List messages of a deleted ticket
`get api/v2/ticket/deletedTickets/{id}/messages` 
- Parameters 
    - id: integer, ticket id
- Response 
    - messages: [message](#message) 
- Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v2/ticket/deletedTickets/{id}/messages?include=sender` |


### Restore a deleted ticket 
`post api/v2/ticket/deletedTickets/{id}/restore ` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - deletedTicket: [ticket](#ticket)  

### Delete a ticket permanently 
`delete api/v2/ticket/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Get a ticket draft 
`get api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - ticketDraft: [ticket draft](#ticket-draft) 

### Create a ticket draft 
`post api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - [ticket draft](#ticket-draft) 
- Response 
    - ticketDraft: [ticket draft](#ticket-draft) 

### Update a ticket draft 
`put api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - [ticket draft](#ticket-draft) 
- Response 
    - ticketDraft: [ticket draft](#ticket-draft) 

### Delete a ticket draft 
`delete api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Merge a ticket 
`post api/v2/ticket/tickets/{id}/merge`
- Parameters 
    - id: integer, target ticket id, 
    - sourceId: integer, source ticket id 
- Response 
    - ticket: [ticket](#ticket) 

### List unread tickets number for filters 
`get api/v2/ticket/tickets/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}`
- Parameters 
    - filterIds: filter id array 
- Response 
    - allCount: integer, all unread ticket number. 
    - array including: 
        - filterId: integer, filter id 
        - unreadCount: integer, count unread tickets of a filter 
        - unreadMentionedCount: integer, the number of tickets which is unread and mentioned to me 

# PortalTickets
## objects
### portal ticket
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of ticket |
| `subject` | string | subject |
| `contactId` | integer | id of the contact who submitted the portal ticket |
| `isRead` | boolean | if the ticket is read | 
| `isReadByContact` | boolean | if the portal ticket is read by contact |
| `agentAssigneeId` | integer | agent assignee id | 
| `departmentAssigneeId` | integer | department assignee id | 
| `receivedFrom` | string | received email address for email channel | 
| `channel` | string | `portal`, `email`| 
| `priority` | string | `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, <br/>`pendingExternal`, `onHold`, `closed` | 
| `closedTime` | datetime | close time |
| `createdById` | integer | contact id or agent id | 
| `createdByType` |  string | agent or contact or system | 
| `createdTime` | datetime | create time of ticket | 
| `lastActivityTime` | datetime | last activity time of ticket | 
| `lastStatusChangeTime` | datetime | last status change time of ticket | 
| `lastRepliedTime` | datetime | last replied time | 
| `lastRepliedById` | integer | contact id or agent id | 
| `lastRepliedByType` | string | `agent` or `contact` or `system`| 
| `lastMessage`| string | plain text of last message | 
| `totalReplies`| int | total replies number | 
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array |

### portal ticket message 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of message | 
| `ticketId` | integer | id of ticket | 
| `htmlBody` | string | html body | 
| `plainBody` | string | plain text body | 
| `senderId`| integer | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | |   
| `attachments` | [attachment](#attachment)[] | attachment array| 

## endpoints
### Get a portal ticket by id
`get api/v2/ticket/portalTickets/{id}`
- Parameters
    - id, integer, portal ticket id
    - contactId, integer
- Response
    - portalTicket: [portal ticket](#portal-ticket) 
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v2/ticket/portalTickets/{id}?include=contact` | 
    | messages | `get api/v2/ticket/portalTickets/{id}?include=messages` |
 
### List portal tickets
`get api/v2/ticket/portalTickets`
- Parameters:
    - contactIds, integer array
    - keywords: string
    - timeFrom: DateTime, last reply time, default search the last 90 days, ISO-8601 time format,
    - timeTo: DateTime, last reply time, default value is the current time, ISO-8601 time format,
    - timeZoneOffset, float, time zone based on your date parameters in ticket conditions. Such date parameters might be @today, @last 7 days for example.
    - conditions: can be ticket system field and custom fields.
        - field: string, field name
        - matchType: string 
        - value: string

    Here is the list of match types and values supported by ticket system field.    
    | Field | Match Type | Values |
    | - | - | - |
    | Ticket Id | Is, IsNot  | number |
    | Subject | Contains, NotContains  | string |
    | Department Assignee | Is, IsNot  | Department Id |
    | Agent Assignee | Is, IsNot  | Agent Id |
    | Status | Is, IsNot  | `new`, `pendingExternal`, `pendingInternal`, `onHold`, `Closed` |
    | Priority | Is, IsNot  | `urgent`, `high`, `normal`, `low` |
    | Channel | Is, IsNot  | `portal`, `email`, `chat`, `offlineMessage` | 
    | Date Created | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Last Activity Time | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Last Status Change Time | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Close Time | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Total Replies | Is, IsNot, IsMoreThan, IsLessThan | number |
    | ContactId | Is, IsNot | number, contact Id |
    | @Mentioned Agent | Is, IsNot | number, agent Id |
    
    Here is the list of match types and values supported by ticket custom field.    
    | Field DataType | Match Type | Values |
    | - | - | - |
    | Date | Is, IsNot，After, Before | time format: `2019-01-03` |
    | Drop-down list | Is, IsNot | option text |
    | Check-box list | Is, IsNot | option text |
    | Radio button | Is, IsNot | option text |
    | Check-box | Is, IsNot | `true`, `false` |
    | Single-line text box | Contains, NotContains | string |
    | Multi-line text box | Contains, NotContains | string |
    | Agent | Is, IsNot | agent id |
    | Department | Is, IsNot | department id |
    | Link | Contains, NotContains | string |
    | Url | Contains, NotContains | string |


- Response: 
    - portalTickets: [portal ticket](#portal-ticket) list, returns a maximum of 100 records. 
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v2/ticket/portalTickets?include=contact` |  
- Sample
    - `get api/v2/ticket/portalTickets?contactIds=1&contactIds=2&contactIds=3&conditions[0][field]=subject&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=status&conditions[1][matchType]=is&conditions[1][value]=1`, note: pass the option id of dropdownlist field as the value.
### Submit a portal ticket
`post api/v2/ticket/portalTickets`
- Parameters: 
    - subject: string, subject, required
    - contactId: integer, id of the contact who submitted the portal ticket
    - customFields: [custom field id and value](#custom-field-id-and-value)[], custom field value array
    - message:  the first portal message
        - htmlBody: string, html body
        - plainBody: string, plain text
        - attachments: [attachment](#attachment)[], attachment array of message
- Response: 
  - portalTicket: [portal ticket](#portal-ticket) 

### Close a portalTicket
`put api/v2/ticket/portalTickets/{id}/close` 
- Parameters: 
    - id, integer, portal ticket id,
    - contactId, integer, required
- Response: 
    - portalTicket: [portal ticket](#portal-ticket) 

### Reopen a portalTicket
`put api/v2/ticket/portalTickets/{id}/reopen` 
- Parameters: 
    - id, integer, portal ticket id,
    - contactId, integer, required
- Response: 
    - portalTicket: [portal ticket](#portal-ticket) 

### List messages of a portal ticket 
`get api/v2/ticket/portalTickets/{id}/messages`
- Parameters: 
    - id, integer
    - contactId, integer
- Response: 
    - messages: [portal ticket message](#portal-ticket-message) list
- Includes

    |Includes| Description |
    | - | - |
    | sender| `get api/v2/ticket/portalTickets/{id}/messages?include=sender` | 

### Reply a portal ticket
 `post api/v2/ticket/portalTickets/{id}/messages`
- Parameters:
    - id: integer
    - contactId: integer required
    - htmlBody: string, html body
    - plainBody: string, plain text
    - attachments: [attachment](#attachment)[], attachment array
- Response: 
    - message: [portal ticket message](#portal-ticket-message)

### Contact mark a portal ticket as read
`put api/v2/ticket/portalTickets/{id}/read`
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code

### Contact mark a portal ticket as unread
`put api/v2/ticket/portalTickets/{id}/unread`
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code

# Filters 
## objects 
### filter 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | filter id | 
| `name` | string | filter name | 
| `isPrivate` | boolean | if private filter| 
| `createdById` | integer | agent id | 
| `conditions` | [condition](#condition)[] | array of filter condition | 

### condition 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | condition id | 
| `fieldId` | integer | field id | 
| `matchType` | string | `contains`, `notContains`, `is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 

## endpoints 
### List all public and private filters 
`get /api/v2/ticket/filters`
- Parameters 
    - no parameters 
- Response 
    - filters: [filter](#filter) list, without conditions

### Create a new filter 
`post api/v2/ticket/filters`
- Parameters 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter, default value: `false` 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filters: [filter](#filter) list 

### Get a filter and its conditions 
`get api/v2/ticket/filters/{id}` 
- Parameters 
    - id: integer, filter id 
- Response 
    - filter: [filter](#filter) 

### Update a filter 
`put api/v2/ticket/filters/{id}` 
- Parameters 
    - id: integer, filter id 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filter: [filter](#filter) 

### Delete a filter 
`delete api/v2/ticket/filters/{id}` 
- Parameters 
    - id: integer, filter id 
- Response 
    - http status code 


# Fields 
## objects 
### field 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | field id | 
| `type` | string | `text`, `textarea`, `email`, `url`, `date`, `integer`, `float`, `operator`, <br/>`radio`, `checkbox`, `dropdownList`, `checkboxList`, `link`, `department` | 
| `name` | integer | field name | 
| `isSystemField` | boolean | if is system field | 
| `isRequired` | boolean | value if is required | 
| `defaultValue` | string | field default value | 
| `helpText` | string | field help text | 
| `length` | integer | field value max length | 
| `options` | [field option](#fieldoption)[] | value option | 

### fieldOption 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | option id | 
| `name` | string | option name | 
| `value` | string | field value | 
| `order` | integer | option order | 

## endpoints 
### List all fields and their options 
`get api/v2/ticket/fields` 
- Parameters
    - no parameters
- Response 
    - fields: [field](#field) list 

# Attachments 
## objects 
### attachment 
| Name | Type | Description | 
| - | - | - | 
| `guid` | string | attachment unique id | 
| `fileName` | string | attachment file name| 
| `url` | string | attachment download link | 
| `isAvailable` | boolean | if the attachment is available | 
## endpoints 
### Upload attachment
`post /api/v2/ticket/attachments` 
- Parameters 
    - file: file, support to upload multiple files at a time
- Response 
    - attachments: [attachment](#attachment) array
    
### Update status of attachment
`Put /api/v2/ticket/attachments/{guid}`
#### Parameters:
- isAvailable - boolean `true` or  `false`
#### Response
 - attachment: [attachment](#attachment) 

### Delete attachment 
`delete /api/v2/ticket/attachments/{guid}` 
- Parameters 
    - guid: string, the guid of the attachment
- Response 
    - httpStatusCode


# BlockedSenders 
## objects 
### blocked sender 
| Name | Type | Description | 
| - | - | - | 
| `email` | string | email or domain | 
| `blockType` | string | `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### List blocked senders 
`get /api/v2/ticket/blockedSenders` 
- Parameters 
    - email: string, domain or email address 
- Response 
    - blockedSenders: [block sender](#blocked-sender) list 

### Add/update a block sender 
`put api/v2/ticket/blockedSenders` 
- Parameters 
    - `email`, string, domain or email address 
    - `blockType`, string, `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain`
- Response 
    - blockedSender: [block sender](#blocked-sender) 

### Remove a block sender 
`delete api/v2/ticket/blockedSenders` 
- Parameters 
   - email: string, domain or email address 
- Response 
    - http status code

# CannedResponses 
## objects 
### canned response 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | canned response name | 
| `htmlContent` | string | html format content | 
| `textContent` | string | text format content | 

## endpoints 
### List all canned responses 
`get api/v2/ticket/cannedResponses` 
- Parameters 
    - no parameters
- Response 
    - cannedResponses: [Canned responses](#canned-response) list 

### Get a canned responses
`get api/v2/tickets/cannedResponses/{id}` 
- Parameters 
    - id: int
- Response 
    - cannedResponse: [Canned responses](#canned-response) 

### Add a canned response 
`post api/v2/tickets/cannedResponses` 
- Parameters 
    - name: string
    - htmlContent: string
    - textContent: string
- Response 
    - cannedResponse: [Canned responses](#canned-response) 

### Update a canned response 
`put api/v2/tickets/cannedResponses/{id}` 
- Parameters 
    - [Canned responses](#canned-response)
- Response 
    - cannedResponse: [Canned responses](#canned-response) 

### Delete a canned response 
`delete api/v2/tickets/cannedResponses/{id}` 
- Parameters 
    - No parameter
- Response 
    - httpStatusCode

# Configs 
### Get site configs about ticket 
`get api/v2/ticket/configs` 
- Parameters 
    - no parameters
- Response 
    - configs
        - isEnabledDepartment: boolean 
        - recipientLimitPerEmail: integer 

# Departments 
## objects 
### department 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | department name | 
| `description` | string | department description | 
| `members` | [department member](#department-member)[] | department member array | 

### department member 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | member name | 
| `type` | string | agent or group | 

## endpoints 
### Get a department 
`get api/v2/ticket/departments/{id} ` 
- Response 
    - department: [department](#department) 

### List all departments 
`get api/v2/ticket/departments` 
- Response 
    - departments: [department](#department) List without department member. 

# EmailAccounts 
## objects 
### email account 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email address |  
| `type` | string | pop3 or exchange | 
| `agentAssigneeId` | integer | agent id | 
| `departmentAssigneeId` | integer | department id | 
| `isDefault` | boolean | if default email account | 


## endpoints 
### List all enabled email accounts 
`get api/v2/ticket/emailAccounts` 
- Response 
    - emailAccounts: [email account](#email-account) list 

# JunkEmails 
## objects 
### junk email 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `subject` | string | email subject | 
| `time` | datetime | received time | 
| `name` | string | sender name | 
| `from` | string | email from email address | 
| `to` | string | to email addresses | 
| `cc` | string | cc email addresses | 
| `htmlBody` | string | html body | 
| `plainBody` | string | plain text body | 
| `emailAccountId` | integer | receive email account id | 
| `isRead` | boolean | if is read | 
| `attachments` | [attachment](#attachment)[] | attachments | 

## endpoints 
### List junk emails
`get api/v2/ticket/junkEmails` 

- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime
    - timeTo: DateTime
- Response 
    - junkEmails: [junk email](#junk-email) list 
    - total: integer
    - previousPage: string, next page uri, the first page return null
    - nextPage: string, the last page return null
    - currentPage: string

### Get a junk email 
`get api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - id: integer, email id 
- Response 
    - junkEmail: [junk email](#junk-email) 

### Update a junk email 
`put api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - isRead: boolean, 
- Response 
    - junkEmail: [junk email](#junk-email) 

### Restore a junk email to a normal ticket 
`post api/v2/ticket/junkEmails/{id}/notJunk` 
- Parameters 
    - id: integer, email id 
- Response 
    - ticket: [ticket](#ticket) 

### Delete a junk email 
`delete api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - id: integer, junk email id 
- Response 
    - http status code 

# Tags 
## objects 
### tag 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | tag name | 
| `ticketCount` | integer | the number of tickets with the tag | 
## endpoints 
### List all tags 
`Get api/v2/ticket/tags` 
- Response 
    - tags: [tag](#tag) list 

### Add a tag 
`Post api/v2/ticket/tags` 
- Parameters 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Update One Tag 
`Put api/v2/ticket/tags/{id}` 
- Parameters 
    - id: integer, tag id 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Delete a tag 
`Delete api/v2/ticket/tags/{id}` 
- Parameters 
    - id: integer, tag id 
- Response 
    - http status code
