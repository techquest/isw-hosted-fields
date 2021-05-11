# Hosted Fields

### Introduction
**Hosted fields** allows merchants who may not be PCIDSS compliant to have control over the look and feel of their checkout page.  
With **hosted fields**, you are able to present fields such as **card number**, **expiry date**, **CVV**, **pin** and **OTP** to the users in an iframe just the way you want.  

### Setting up the SDK
Setting up the hosted fields is as simple as doing the following:
```js
isw.hostedFields.create(configuration, callback);
```
The configuration object should be as described below:
```js
let configuration = {
    fields: {
        cardNumber: {
            selector: '#cardNumber-container',
            placeholder: '****  ****  ****  ****',
            styles: {}
        },
        expirationDate: {
            selector: '#expirationDate-container',
            placeholder: 'MM / YY',
            styles: {}
        },
        cvv: {
            selector: '#cvv-container',
            placeholder: '***',
            styles: {}
        },
        pin: {
            selector: '#pin-container',
            placeholder: '* * * *',
            styles: {}
        },
        otp: {
            selector: '#otp-container',
            placeholder: '* * * * * *',
            styles: {}
        }
    },
    cardinal: {
        containerSelector: '.cardinal-container',
        activeClass: 'show'
    },
    paymentParameters: {
        amount: 150000,
        currencyCode: "566",
        dateOfPayment: '2021-11-08T00:00:00',
        payableCode: "Default_Payable_MX26070",
        merchantCustomerName: "John Doe",
        merchantCode: 'XXXXXXXX',
        transactionReference: "isw_hosted_field_test:" + Date.now(),
    }

}
```

Below is a detailed explanation to each property in the configuration object:
##### The Configuration Object
1. **fields:** These are references to the input elements on the checkout page. They include cardNumber, expirationDate, cvv, pin and otp.  
Each of these fields have three properties, namely:  
    a. **selector:** This is the ID selector of the field.  
    b. **placeholder:** Initially displayed text on the field.  
    c. **styles:** This is used to style each field.  

2. **cardinal:** When the safetoken OTP service is not used, it is considered a cardinal transaction. You might want to customize the OTP page. There are two properties that can be used, namely:  
    a. **containerSelector:** The identifier for the container div.  
    b. **activeClass:** The CSS class that displays the div.  

3. **paymentParameters:** This has the following properties:  
    a. **amount:** Total amount to be paid in kobo.  
    b. **currencyCode:** The ISO code of the currency being used.  
    c. **dateOfPayment:** The date the payment was made.  
    d. **payableCode:** As provided on your [Quickteller Business dashboard](https://business.quickteller.com/developertools).    
    e. **merchantCode:** As provided on your [Quickteller Business dashboard](https://business.quickteller.com/developertools).  
    f. **transactionReference:** A unique reference for every transaction.   


Using the configuration object above, the corresponding HTML should have elements with IDs set to the properties of the 'fields' property like so:  
```html
    <div class="payment-form-container" data-field-type="pan">
      <div class="form-header">
        <img
          src="https://mufasa-qa.interswitchng.com/p/webpay/logos/default.png"
        />
        <div class="info">
          <div class="title">ISW Hosted Fields Sample 1</div>
          <div>&#8358; <span id="amount"></span></div>
        </div>
      </div>

      <div class="alertSuccess" id="response">
        <p id="response-code" style="padding: 0px 10px"></p>
        <p id="response-message" style="padding: 0px 10px"></p>
      </div>

      <!-- First page of the payment flow -->
      <div class="form-page card-details show">
        <div class="form-control">
          <label>Card Number</label>
          <div id="cardNumber-container" class="payment-field"></div>
        </div>
        <div class="form-group">
          <div class="form-control">
            <label>EXP</label>
            <div id="expirationDate-container" class="payment-field"></div>
          </div>
          <div class="form-control">
            <label>CVV</label>
            <div id="cvv-container" class="payment-field"></div>
          </div>
        </div>
        <div class="button-container">
          <button id="pay-button" class="blue">Pay</button>
        </div>
      </div>

      <!-- Second page of the payment flow -->
      <div class="form-page pin">
        <div class="back-control" id="pin-back-button">
          <img src="./back-icon.png" />
          <label>Back</label>
        </div>
        <div class="form-text">Please provide your PIN</div>
        <div class="form-control">
          <label></label>
          <div id="pin-container" class="payment-field"></div>
        </div>
        <div class="button-container">
          <button id="continue-button" class="green">Continue</button>
        </div>
      </div>

      <!-- Third page of the payment flow -->
      <div class="form-page otp">
        <div class="back-control" id="otp-back-button">
          <img src="./back-icon.png" />
          <label>Back</label>
        </div>
        <div class="form-text">
          Please input the OTP sent to your mobile number
        </div>
        <div class="form-control">
          <label></label>
          <div id="otp-container" class="payment-field"></div>
        </div>
        <div class="button-container">
          <button id="validate-button" class="orange">Validate</button>
        </div>
      </div>
      <div class="form-page cardinal">
        <div class="cardinal-container"></div>
      </div>
    </div>
```

##### The callback function
The callback function takes two parameters:
- createError: This returns an error (if any) during the creation process.
- hostedFieldInstance: This creates a new instance of the hosted fields. The instance of the hosted fields has the following methods on it:  
     
    a. getBinConfiguration(): After the user enters their card details, this function checks to see the details are valid. If valid, it invokes the makePayment() function, otherwise, it returns an error.  
    b. makePayment(): When this function is called, an OTP is sent. The user is redirected to the OTP page where the sent value is entered.  
    c. validatePayment(): After entering the OTP, this method is called. An attempt to charge the card is made and a response is returned. A sample response is indicated below:
    ```js
    {
        responseCode: "00"
    }
    ```
    A list of all possible response codes and their meanings can be found [here](https://sandbox.interswitchng.com/docbase/docs/webpay/response-codes/)
     
    d. on(): This method handles events like focus, blur, validation, and cardinal-response.   
    e. getFieldsState(): To check for the validation of each field you can call instance.getFieldsState().


A sample callback function will be like so:

```js
let instance;

function callback(createError, hostedFieldsInstance) {
    if (createError != null) {
        throw createError;
    }

    instance = hostedFieldsInstance;


    instance.on('focus', function (event) {
        let fieldContainer = document.querySelector(event.selector);
        fieldContainer.style.borderBottomColor = '#a0c8e2';
    });

    instance.on('blur', function (event) {
        let fieldContainer = document.querySelector(event.selector);
        fieldContainer.style.borderBottomColor = '#e4e4e4';
    });

    instance.on('validation', function (event) {
    });

    instance.on('cardinal-response', handleCardinalValidateResponse);



    payButton.addEventListener('click', function () {
        instance.getBinConfiguration(handleBinConfigResponse);
    });

    continueButton.addEventListener('click', function () {
        instance.makePayment(handlePayResponse);
    });

    validateButton.addEventListener('click', function () {
        instance.validatePayment(handleValidateResponse);
    });

}
```

### Events
The 'on' method gives you the ability to hook into the focus, blur, validation and cardinal-response events. It allows you to subscribe to these events.
#### Focus and Blur Events
Below is a description of the object returned by the focus and blur events:
| Key       | Type   | Description                                                                               |
|-----------|--------|-------------------------------------------------------------------------------------------|
| fieldType | string | Displays the type of input field which can be cardNumber, expirationDate, cvv, pin or otp |
| selector  | string | Displays the id of the div container.                                                     |

#### Validate Event
Below is a description of the object returned by the validate event.
| Key            | Type   | Description                                                                                                                                                   |
|----------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| cardNumber     | object | <table width="100%"> <tr><td>cardType</td><td>string</td></tr> <tr><td>isEmpty</td><td>boolean</td></tr> <tr><td>valid</td><td>boolean</td></tr> </table>     |
| cvv            | object | <table width="100%"> <tr><td>isEmpty</td><td>boolean</td></tr>  <tr><td>valid</td><td>boolean</td></tr>  </table>                                             |
| expirationDate | object | <table width="100%">  <tr><td>expired</td><td>boolean</td></tr>  <tr><td>isEmpty</td><td>boolean</td></tr>  <tr><td>valid</td><td>boolean</td></tr>  </table> |
| otp            | object | <table width="100%"> <tr><td>isEmpty</td><td>boolean</td></tr>  <tr><td>valid</td><td>boolean</td></tr>  </table>                                             |
| pin            | object | <table width="100%">  <tr><td>isEmpty</td><td>boolean</td></tr>  <tr><td>valid</td><td>boolean</td></tr>  </table>                                            |


### Sample Code
A sample code of the complete integration can be found on [github](https://github.com/techquest/isw-hosted-fields).
