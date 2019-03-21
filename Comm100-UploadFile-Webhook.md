## Upload File Webhook

### Note
- When a user uploads a file to the server, the status of the file does not support downloading yet. The Ticketing/Live Chat system will need to pass the event with the uploaded file information to the defined webhook URL and need a response to check whether the post has been received successfully. 
- After the scan tool on your end finishes scanning the uploaded file, you will be able to return the webhook response directly. Another option is to call the restful API to change the status of the file according to the scanning result, or delete the attachment directly.
    - Live chat attachment API: 
        - put /api/v2/livechat/attachments/{guid}
        - delete /api/v2/livechat/attachments/{guid}
    - Ticket attachment API: 
        - put /api/v2/ticket/attachments/{guid}
        - delete /api/v2/ticket/attachments/{guid}
- The re-post mechanism: If the event of sending webhook fails, or no response is received, we have a Timer function to repost the event periodically, but the interval gradually extends longer and longer, until 3 days later when the re-post will come to a stop.
    - Immediately send the second post after the first webhook post fails.
    - The third post will be sent 1 minute after the second one.
    - The forth post will be sent 10 minutes later.
    - Next post is 1 hour later.
    - Then 24 hours later.


### Ticket Webhook
##### Request Data Format
  | Name | Type  | Description |
  | - | - | - |
  | `event` | string  | `fileUploaded` |
  | `eventId` | string  | event id, unique for each event post |
  | `ticketId`| int | ticket id | 
  | `guid` | string  | file guid |
  | `fileName` | string  | file name |
  | `fileExtension` | string  | file extension |
  | `fileSize` | int | file size |
  | `fileURL` | string  | file download URL | 
  | `fileLocalPath` | string  | file local path | 
##### Response Data Format
- Sample repsonse json:
```javascript
{
    "guid": "", //file guid
    "result": "passed",  //string, passed or failed or delay, delay means the result will be sent via API later.
    "message": "", //string, optional
}
```

### Livechat Webhook
##### Request Data Format
  | Name | Type  | Description |
  | - | - | - |
  | `event` | string  | `fileUploaded` |
  | `eventId` | string  | event id |
  | `chatId`| int | chat id or offline message id| 
  | `guid` | string  | file guid |
  | `fileName` | string  | file name |
  | `fileExtension` | string  | file extension |
  | `fileSize` | int | file size |
  | `fileURL` | string  | file download URL | 
  | `fileLocalPath` | string  | file local path | 

- Sample repsonse json:
```javascript
{
    "guid": "", //file guid
    "result": "passed",  //string, passed or failed or delay
    "message": "", //string, optional
}
```

### Update Status of Live Chat Attachment
`Put /api/v2/livechat/attachments/{guid}`

#### Parameters:
- status - string `passed` or  `failed`

#### Response
- attachment object

#### Example
##### Sample request:
```shell
curl -H "Authorization: Bearer yP7Agz9nzzpgyPTxfM6ajBgIMhuaoz_p1XvLgKyULP7SzIbCRUb3Qscheh \ 
     74BceSrdZ61_LrJ4saBNJPP8NJdsrx5CbWSOfVlqHU9-dp7lVgBZbVg661SOcDM0dMYb8nOZ4rixC79j-lHw \ 
    4mWLEhJAtUzqsfkG3QamG0VklLNThmPvRttwyLGqzZFY3keXNw5ivxy1Mr5smAJDWPfzKKQZXJIkutNz4W \ 
    t3iC80BlOPFbAMnDdtvKjle6gf2V1WkHA-JW9W9QZc7A" \ 
     -x PUT  https://channel.comm100.com/api/v2/livechat/attachments/12b76589-6ba7-4011-aa23-8779594aab11 `
```
### Delete a Live Chat Attachment
`Delete /api/v2/livechat/attachments/{guid}`

#### Response
- http status

### Update Status of Ticket Attachment
`Put /api/v2/ticket/attachments/{guid}`

#### Parameters:
- status - string `passed` or  `failed`

#### Response
- attachment object

#### Example
##### Sample request:
```shell
curl -H "Authorization: Bearer yP7Agz9nzzpgyPTxfM6ajBgIMhuaoz_p1XvLgKyULP7SzIbCRUb3Qscheh \ 
     74BceSrdZ61_LrJ4saBNJPP8NJdsrx5CbWSOfVlqHU9-dp7lVgBZbVg661SOcDM0dMYb8nOZ4rixC79j-lHw \ 
    4mWLEhJAtUzqsfkG3QamG0VklLNThmPvRttwyLGqzZFY3keXNw5ivxy1Mr5smAJDWPfzKKQZXJIkutNz4W \ 
    t3iC80BlOPFbAMnDdtvKjle6gf2V1WkHA-JW9W9QZc7A" \ 
     -x PUT  https://channel.comm100.com/api/v2/ticket/attachments/12b76589-6ba7-4011-aa23-8779594aab11 `
```
 
 ### Delete a Ticket Attachment
`Delete /api/v2/ticket/attachments/{guid}`

#### Response
- http status
