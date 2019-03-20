## Upload File Webhook

### Note
- After user uploads an file to server, at this time the status of the file is unavailable for download. Ticketing/live chat system will pass this event with uploaded file information to the defined webhook URL and need a response to check whether the post is received successfully . 
- After your scan tool finished scanning the file,  you can return the webhook response directly, you can also  call the restful API to change the status of the file as the scanning result.
    - Live chat attachment API: put /api/v2/livechat/attachments/{guid}
    - Ticket attachment API: put /api/v2/ticket/attachments/{guid}
- Repost mechanism. If the sending webhook fails or no response is received,  we have a Timer program to repost periodically, but the interval will be logner and longer.


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
    "result": "passed",  //string, passed or failed or delay, delay means sent the result via API later.
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
 
