### Note
- When a agent reply a ticket, the ticket system will post a request to the webhook. 
- The re-post mechanism: It is the same as file uplaod webhook, If the event of sending webhook fails, or no response is received, we have a Timer function to repost the event periodically, but the interval gradually extends longer and longer, until 3 days later when the re-post will come to a stop.
    - Immediately send the second post after the first webhook post fails.
    - The third post will be sent 1 minute after the second one fails.
    - The fourth post will be sent 10 minutes after the third one fails.
    - The fifth post will be sent 1 hour after the fourth one fails.
    - The sixth post will be sent 24 hours after the fifth one fails.
    - The seventh post will be sent 24 hours after the sixth one fails.
    - The last post will be sent 24 hours after the seventh one fails.

### Agent Reply Webhook
##### Request Data Format
  | Name | Type  | Description |
  | - | - | - |
  | `event` | string  | `ticket.AgentReplied`, |
  | `eventId` | string  | Event id, unique for each event post |
  | `payload` | [payload](#Agent-Reply-Webhook-Payload)  | Payload data |
  | `eventTime` | datatime  | Event time |
  
##### Agent Reply Webhook Payload
  | Name | Type  | Description |
  | - | - | - |
  | `ticket`| object | ticket object | 
  | `message` | object | message object |
  | `contact` | object  | contact object |
