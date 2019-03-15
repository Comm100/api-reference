## Upload File Webhook

### Note
- After user uploads an file to server via Ticket API, ticketing system will pass this event with uploaded file information to the defined webhook URL, and wait for the response.

### Ticket Webhook
##### Request Data Format
  | Name | Type  | Description |
  | - | - | - |
  | `event` | string  | `fileUploaded` |
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
    "result": "success",  //string, success or fail
    "message": "", //string, option
}
```
### Livechat Webhook
##### Request Data Format
  | Name | Type  | Description |
  | - | - | - |
  | `event` | string  | `fileUploaded` |
  | `chatId`| int | chat id or offline message id| 
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
    "result": "success",  //string, success or fail
    "message": "", //string, option
}
```
