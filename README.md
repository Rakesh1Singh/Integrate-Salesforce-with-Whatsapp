# Integrate-Salesforce-with-Whatsapp

```javascript
String account='AC028c994aefbf204fc0cf647c5391885c';
String token='f65dc59738b99df42f04a70adb3591ed';

HttpRequest req = new HttpRequest();
 // be sure this is configured in "Remote Site Settings"
req.setEndpoint('https://api.twilio.com/2010-04-01/Accounts/'+account+'/Messages.json');
req.setMethod('POST'); 
// This tells the API that we are sending and receiving the data as a JSON object 
req.setHeader('Content-Type','application/json');
req.setHeader('Content-Type','application/x-www-form-urlencoded');


Blob headerValue = Blob.valueOf(account + ':' + token);
// Base 64 Encode the blob and prepend "Basic "
String authorizationHeader = 'BASIC ' + EncodingUtil.base64Encode(headerValue);
// Add the basic auth string to the Request Header
req.setHeader('Authorization', authorizationHeader);
String message='this is from twilio';
String mobileno = '+918871534882';
string jsonString='From='+EncodingUtil.urlEncode('whatsapp:+14155238886', 'UTF-8')+'&Body='+EncodingUtil.urlEncode(message, 'UTF-8')+'&To='+EncodingUtil.urlEncode('whatsapp:'+mobileno+'', 'UTF-8')+'';
req.setBody(jsonString);

Http http = new Http();
HTTPResponse res = http.send(req);
System.debug(res.getBody());
```
