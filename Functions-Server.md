---
search:
   keywords: ['function', 'server', 'server-side']
---

# Server-side Functions

In OrientDB, you can replace the use of Servlets with server-side functions.  For more information on how to call server-side functions, see [Functions using the HTTP REST API](Functions-Use.md#calling-functions-from-the-http-rest-api)

When the HTTP REST protocol calls server-side functions, OrientDB embeds a few additional variables:

- [**Request Object**](#request-object) The HTTP request, implemented by the `OHttpRequestWrapper` class.
- [**Response Object**](#response-object) The HTTP request response, implemented by the `OHttpResponseWrapper` class.
- [**Util Object**](#util-object) The utility class with helper functions, to use inside the functions, implemented by the `OFunctionUtilWrapper` class.

## Request Object

OrientDB references this object as `request`.  For instance,

```javascript
var params = request.getParameters();
```


|Method signature|Description|Return type|
|----------------|-----------|-----------|
| [**`getContent()`**](Functions-Server-getContent.md) |Returns the request content.|String|
|[**`getUser()`**](Functions-Server-getUser.md)|Gets the request user name.|String|
|[**`getContentType()`**](Functions-Server-getContentType.md)|Returns the request content type.|String|
|[**`getHttpVersion()`**](Functions-Server-getHttpVersion.md)|Return the request HTTP version.|String|
|[**`getHttpMethod()`**](Functions-Server-getHttpMethod.md)|Return the request HTTP method called.|String|
|[**`getIfMatch()`**](Functions-Server-getIfMatch.md)|Return the request IF-MATCH header.|String|
|[**`isMultipart()`**](Functions-Server-isMultipart.md)|Returns if the request is multi-part. |boolean|
|[**`getArguments()`**](Functions-Server-getArguments.md)|Returns the request arguments passed in REST form. Example: `/2012/10/26`.|String[]|
|[**`getArgument(<position>)`**](Functions-Server-getArgument.md)|Returns the request argument by position, or `null` if not found.|String|
|[**`getParameters()`**](Functions-Server-getParameters.md)|Returns the request parameters.|String|
|[**`getParameter(<name>)`**](Functions-Server-getParameter.md)|Returns the request parameter by name or null if not found.|String|
|[**`hasParameters(<name>, ...)`**](Functions-Server-hasParameters.md)|Returns the number of parameters found between those passed.|Integer|
|[**`getSessionId()`**](Functions-Server-getSessionId.md)|Returns the session-id.|String|
|[**`getURL()`**](Functions-Server-getURL.md)|Returns the request URL.|String|




## Response Object

OrientDB references this object as `response`.  For instance,


```javascript
var db = orient.getDatabase();
var roles = db.query("select from ORole where name = ?", roleName);
if( roles == null || roles.length == 0 ){
  response.send(404, "Role name not found", "text/plain", "Error: role name not found" );
} else {

  db.begin();
  try{
    var result = db.save({ "@class" : "OUser", name : "Luca", password : "Luc4", "roles" : roles});
    db.commit();
    return result;
  }catch ( err ){
    db.rollback();
    response.send(500, "Error on creating new user", "text/plain", err.toString() );
  }
}
```



|Method signature|Description|Return type|
|----------------|-----------|-----------|
| [**`getHeader()`**](Functions-Server-getHeader.md) |Returns the response additional headers.|String|
| [**`setHeader(String header)`**](Functions-Server-setHeader.md) |Sets the response additional headers to send back. To specify multiple headers use the line breaks. |Request object|
|[**`getContentType()`**](Functions-Server-getContentType.md)|Returns the response content type. If null will be automatically detected.|String|
|[**`setContentType(String contentType)`**](Functions-Server-setContentType.md)|Sets the response content type.  If null will be automatically detected.|Request object|
|[**`getCharacterSet()`**](Functions-Server-getCharacterSet.md)|Returns the response character set used.|String|
|[**`setCharacterSet(String characterSet)`**](Functions-Server-setCharacterSet.md)|Sets the response character set.|Request object|
|[**`getHttpVersion()`**](Functions-Server-getHttpVersion.md)| Returns the HTTP version. |String|
|[**`writeStatus(int httpCode, String reason)`**](Functions-Server-writeStatus.md)|Sets the response status as HTTP code and reason.|Request object|
|[**`writeHeaders(String contentType, boolean keepAlive)`**](Functions-Server-writeHeaders.md)|Sets the response headers specifying when using the keep-alive or not.|Request object|
|[**`writeLine(String content)`**](Functions-Server-writeLine.md) |Writes a line in the response. A line feed will be appended at the end of the content.|Request object|
|[**`writeContent(String content)`**](Functions-Server-writeContent.md) |Writes content directly to the response. |Request object|
|[**`writeRecords(List<OIdentifiable> records)`**](Functions-Server-writeRecords.md) |Writes records as response. The records are serialized in JSON format. |Request object|
|[**`writeRecords(List<OIdentifiable> records, String fetchPlan)`**](Functions-Server-writeRecords.md)|Writes records as response specifying a fetch-plan to serialize nested records. The records are serialized in JSON format. |Request object|
|[**`writeRecord(ORecord record)`**](Functions-Server-writeRecord.md) | Writes a record as response. The record is serialized in JSON format.|Request object|
|[**`writeRecord(ORecord record, String fetchPlan)`**](Functions-Server-writeRecord.md) | Writes a record as response. The record is serialized in JSON format.|Request object|
|[**`send(int code, String reason, String contentType, Object content)`**](Functions-Server-send.md) | Sends the complete HTTP response in one call.|Request object|
|[**`send(int code, String reason, String contentType, Object content, String headers)`**](Functions-Server-send.md) |Sends the complete HTTP response in one call specifying additional headers. Keep-alive is set. |Request object|
|[**`send(int code, String reason, String contentType, Object content, String headers, boolean keepAlive)`**](Functions-Server-send.md) | Sends the complete HTTP response in one call specifying additional headers. |Request object|
|[**`sendStream(int code, String reason, String contentType, InputStream content, long size)`**](Functions-Server-sendStream.md) | Sends the complete HTTP response in one call specifying a stream as content.|Request object |
|[**`flush()`**](Functions-Server-flush.md)| Flushes the content to the TCP/IP socket. |Request object|



## Util Object

OrientDB references this object as `util`.  For instance,

```javascript
if( util.exists(year) ){
  print("\nYes, the year was passed!");
}
```


|Method signature|Description|Return type|
|----------------|-----------|-----------|
|[**`exists(<variable>)`**](Functions-Server-exists.md)|Returns trues if any of the passed variables are defined. In JS, for example, a variable is defined if it's not null and not equal to `undefined`.|Boolean|


