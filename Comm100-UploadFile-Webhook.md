## Upload File Webhook

### Note
- After user uploads an file to server via Ticket API, ticketing system will pass this event with uploaded file information to the defined webhook URL, and wait for the response of the scan tool.

### Ticket Webhook
##### Request Data Format
  | Name | Type  | Description |
  | - | - | - |
  | `event` | string  | `fileUploaded` |
  | `ticketId`| int | ticket id | 
  | `guid` | string  | file guid |
  | `fileName` | string  | file name |
  | `fileExtension` | string  | file extension |
  | `fileSize` | int | file sieze |
  | `fileURL` | string  | file download url | 

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
  | `chatId`| int | chat id | 
  | `guid` | string  | file guid |
  | `fileName` | string  | file name |
  | `fileExtension` | string  | file extension |
  | `fileSize` | int | file sieze |
  | `fileURL` | string  | file download url | 

##### Response Data Format
 - Sample repsonse json:
```javascript
{
    "guid": "", //file guid
    "result": "success",  //string, success or fail
    "message": "", //string, option
}
```




- 实现逻辑： 用户通过API将文件上传到服务器后， Ticket系统会发送webhook到指定的URL， 等待humana 文件扫描工具的扫描，然后把结果response回来， 如果返回success，API正常返回， 否则删除缓存中上传的文件，API返回异常。