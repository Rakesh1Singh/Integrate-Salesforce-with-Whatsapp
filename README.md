# Integrate-Salesforce-with-Whatsapp

```javascript
String account='';
String token='';

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
String mobileno = '';
string jsonString='From='+EncodingUtil.urlEncode('whatsapp:+14155238886', 'UTF-8')+'&Body='+EncodingUtil.urlEncode(message, 'UTF-8')+'&To='+EncodingUtil.urlEncode('whatsapp:'+mobileno+'', 'UTF-8')+'';
req.setBody(jsonString);

Http http = new Http();
HTTPResponse res = http.send(req);
System.debug(res.getBody());
```

# Complete registration process with https://www.twilio.com/try-twilio
# Verify Email and mobile number
# Create Sandbox environment https://www.twilio.com/console/sms/whatsapp/learn

Connect your whatsapp to sandbox
Image 

Once you send the message, you will link your WhatsApp number to sandbox environment.
Image





