# Integrate-Salesforce-with-Whatsapp


```javascript
  <template>
    <div style="margin-left: 35%;
    margin-right: 35%;">
        <lightning-card  title="Send Whatsapp Message" >
        
            <p class="slds-p-horizontal_x-small">
                <lightning-input type="text" label="Enter a registered mobile number" placeholder='Type a mobile number here...'></lightning-input>
            </p>
         <p class="slds-p-horizontal_x-small">
                <lightning-input type="text" label="Enter a message" placeholder='Type message here...' ></lightning-input>
            </p> 
            
            <p slot="footer">
                <lightning-button variant="success" label="Send Message" title="Successful action" onclick={handleClick} class="slds-m-right_x-small" ></lightning-button>
            </p>
            
        </lightning-card>
    </div>

</template>
```

```javascript
  import { LightningElement, track } from 'lwc';
import sendEmail from '@salesforce/apex/SendWhatsappMessage.send';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';

export default class SendMessageToWhatsapp extends LightningElement {
     mobileno;
     message;

    _title;
    _message;
    _variant;

    handleClick=()=>{
        
        this.mobileno = this.template.querySelectorAll('lightning-input')[0].value;
        this.message = this.template.querySelectorAll('lightning-input')[1].value;
        sendEmail({mobileno: this.mobileno, message: this.message}).then(result=>{
            if(result == '201'){
                this._message = 'Message sent successfully';
                this._variant = 'success';
                this.showNotification();
                this.template.querySelectorAll('lightning-input')[0].value = '';
                this.message = this.template.querySelectorAll('lightning-input')[1].value='';
            }else{
                this._message = 'Message failed to send';
                this._variant = 'Error';
                this.showNotification();
            }
        });
    };

    showNotification(_title, _message, variant) {
        const evt = new ShowToastEvent({
            title: this._title,
            message: this._message,
            variant: this.variant,
        });
        this.dispatchEvent(evt);
    }
}
```

```javascript
  public class SendWhatsappMessage {
    @AuraEnabled
    public static String send(String mobileno, String message){
        String account=Label.Twilio_username;
        String token=Label.Twilio_token;
        
        HttpRequest req = new HttpRequest();
        //add https://api.twilio.com/ in Remote Site Settings
        req.setEndpoint('https://api.twilio.com/2010-04-01/Accounts/'+account+'/Messages.json');
        req.setMethod('POST'); 
        req.setHeader('Content-Type','application/json');
        req.setHeader('Content-Type','application/x-www-form-urlencoded');
        
        
        Blob headerValue = Blob.valueOf(account + ':' + token);
        String authorizationHeader = 'BASIC ' + EncodingUtil.base64Encode(headerValue);
        req.setHeader('Authorization', authorizationHeader);
        string jsonString='From='+EncodingUtil.urlEncode('whatsapp:+14155238886', 'UTF-8')+'&Body='+EncodingUtil.urlEncode(message, 'UTF-8')+'&To='+EncodingUtil.urlEncode('whatsapp:'+mobileno+'', 'UTF-8')+'';
        req.setBody(jsonString);
        
        Http http = new Http();
        HTTPResponse res = http.send(req);
        System.debug('status'+res.getStatusCode());
        System.debug(res.getBody());
        
        return String.valueOf(res.getStatusCode());
        
    }
}
```





# Complete registration process with https://www.twilio.com/try-twilio
# Verify Email and mobile number
# Create Sandbox environment https://www.twilio.com/console/sms/whatsapp/learn

Connect your whatsapp to sandbox
Image 

Once you send the message, you will link your WhatsApp number to sandbox environment.
Image





