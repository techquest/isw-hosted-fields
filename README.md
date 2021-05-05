# Hosted Fields

### Introduction
**Hosted fields** allows merchants who may not be PCIDSS compliant to have control over the look and feel of their checkout page.  
With **hosted fields**, you are able to present fields such as **card number**, **expiry date**, **CVV**, **pin** and **OTP** to the users in an iframe just the way you want.  
We have created two samples that you can work with and even modify to your taste.

### Sample 1
##### Payment Flow
In this sample, the payment flow is divided into three steps:
1. In the first step, users enter their card details.
2. Pin is entered.
3. Users enter OTP sent to their mobile device.


##### Usage   
###### 1. Create your HTML page like so: 
   
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hosted Fields Sample 1</title>
  </head>
  <body>
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
    <script src="https://nwp.qa.interswitchng.com/sdk.js"></script>
  </body>
</html>
```

###### 2. Add the following styles: 
```css
body {
    margin: 0px;
}

.payment-form-container {
    position: relative;
    background-color: white;
    margin: auto;
    margin-top: 5%;
    width: 400px;
    padding-bottom: 0px;
    box-sizing: border-box;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
}

.cardinal-container {
    position: absolute;
    width: 100%;
    height: 100%;
    background: white;
    left: 0;
    top: 0;
    z-index: 10;
}


.form-header {
    text-align: left;
    font-family: sans-serif;
    border-bottom: 2px solid #f0f0f0;
    padding: 15px;
    background-color: #fafafa;
}

.form-header>img {
    width: 30px;
    height: 30px;
    display: inline-block;
    vertical-align: middle;
    border: 2px solid #ececec;
    background: #ffffff;
    padding: 10px;
    border-radius: 5px;
}

.form-header .info {
    display: inline-block;
    vertical-align: middle;
    text-align: left;
    margin-left: 17px;
}

.form-header .info .title {
    font-size: 15px;
    margin-bottom: 5px;
}

.form-header .info .amount {
    color: #7c7c7c;
    font-size: 22px;
}

.form-page {
    display: none;
    padding: 25px;
    padding-top: 45px;
    padding-bottom: 45px;
    position: relative;
}

.form-page.show {
    display: block;
}

.form-page.cardinal {
    min-height: 350px;
}

.back-control {
    display: inline-block;
    margin-bottom: 0px;
    padding-top: 10px;
    padding-bottom: 10px;
}

.back-control * {
    vertical-align: middle;
    cursor: pointer;
}

.back-control img {
    width: 16px;
    height: 16px;
}

.back-control label {
    color: #0076ff;
    font-family: sans-serif;
    display: inline-block;
    font-size: 16px;
    padding-left: 3px;
}

.form-control {
    margin-top: 35px;
}

.form-control:first-of-type {
    margin-top: 0px;
}

.form-control label {
    display: inline-block;
    margin-bottom: 6px;
    font-size: 13px;
    font-family: sans-serif;
    color: #848484;
    font-weight: bold;
    letter-spacing: 2px;
}

.form-group {
    font-size: 0px;
}

.form-group .form-control {
    width: calc(50% - 10px);
    display: inline-block;
}

.form-group .form-control:last-child {
    margin-left: 20px;
}

.payment-field {
    display: inline-block;
    height: 45px;
    width: 100%;
    background-color: rgb(247, 247, 247);
    border-bottom: 3px solid #e4e4e4;
    transition: border-bottom-color 0.3s;
}

.form-text {
    font-size: 16px;
    line-height: 30px;
    font-family: monospace;
}

.button-container {
    margin-top: 40px;
}

.button-container button {
    width: 100%;
    height: 50px;
    color: white;
    font-size: 16px;
    border: none;
    cursor: pointer;
    outline: none;
}

.button-container button.blue {
    background: #5fabdc;
}

.button-container button.green {
    background: #5fdc91;
}

.button-container button.orange {
    background: #e3a83b;
}

.button-container button:hover {
    opacity: 0.8;
}

.button-container button:active {
    opacity: 0.9;
}

.alertSuccess{
    width: 300px;
    height: 0;
    overflow: hidden;
    background: #5fdc91;
    margin: 0 auto;
    transition: all 0.4s ease-in-out;
    margin-top: 15px;
    font-size: 20px;
}

#response-data{
    font-size: 20px;
    border: 5px 5px;
}

.showNotification{
    height: 200px;
}

.alertError{
    background: red !important;
}



.form-page.pin .back-control, .form-page.otp .back-control {
    margin-bottom: 40px;
}
```

###### 3. Add script.
The three steps involved in the payment flow have been included in the HTML code above. We need to be able to render these pages dynamically.  
a. Create the configuration object

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

The configuration object has three properties - **fields**, **cardinal** and **paymentParameters**.
A full description of the configuration object can be found [here](#the-configuration-object).

b. Create the hosted fields.
The 'create' method takes two parameters:
- The configuration obect
- A callback

The hosted fields creation snippet is represented below:
```js
isw.hostedFields.create(configuration, callback);
```

The callback will be like so:
```js
let instance;

let payButton = document.getElementById('pay-button');
let continueButton = document.getElementById('continue-button');
let validateButton = document.getElementById('validate-button');

let pinBackButton = document.getElementById('pin-back-button');
let otpBackButton = document.getElementById('otp-back-button');

pinBackButton.addEventListener('click', function () {
    setActivePage('card-details');
    instance.clearField('pin');
});

otpBackButton.addEventListener('click', function () {
    setActivePage('pin');
    instance.clearField('otp');
});


function createHandler(createError, hostedFieldsInstance) {
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

The callback method takes two parameters:
- createError: Returns an error (if any) during the creation process.
- hostedFieldInstance: Creates a new instance of the hosted fields. The instance of the hosted fields has the following methods on it:  
    a. makePayment()  
    b. validatePayment()  
    c. validatePayment()  
    d. binConfiguration()  
    e. getFieldsState()  
    f. on() 

The on() method handles events like focus, blur, validation, and cardinal-response. To check for the validation of each field you can call instance.getFieldsState().

###### The complete javascript code will be like so:
```js
var configuration = {
    fields: {
        cardNumber: {
            selector: '#cardNumber-container',
            placeholder: '****  ****  ****  ****',
            styles: {
                fontSize: '16px',
                padding: '0px 0px 0px 10px',
                backgroundColor: 'rgb(247, 247, 247)',
                padding: '0px 0px 0px 10px',
            }
        },
        expirationDate: {
            selector: '#expirationDate-container',
            placeholder: 'MM / YY',
            styles: {
                fontSize: '16px',
                padding: '0px 0px 0px 10px',
                backgroundColor: 'rgb(247, 247, 247)',
                padding: '0px 0px 0px 10px',
            }
        },
        cvv: {
            selector: '#cvv-container',
            placeholder: '***',
            styles: {
                fontSize: '16px',
                padding: '0px 0px 0px 10px',
                backgroundColor: 'rgb(247, 247, 247)',
                padding: '0px 0px 0px 10px',
            }
        },
        pin: {
            selector: '#pin-container',
            placeholder: '* * * *',
            styles: {
                fontSize: '16px',
                padding: '0px 0px 0px 10px',
                backgroundColor: 'rgb(247, 247, 247)',
                textAlign: 'center'
            }
        },
        otp: {
            selector: '#otp-container',
            placeholder: '* * * * * *',
            styles: {
                fontSize: '16px',
                padding: '0px 0px 0px 10px',
                backgroundColor: 'rgb(247, 247, 247)',
                textAlign: 'center'
            }
        }
    },
    cardinal: {
        containerSelector: '.cardinal-container',
        activeClass: 'show'
    },
    paymentParameters: {
        amount: 150000,
        currencyCode: "566",
        dateOfPayment: '2019-11-08T00:00:00',
        payableCode: "Default_Payable_MX26070",
        merchantCustomerName: "John Doe",
        merchantCode: 'MX26070',
        transactionReference: "isw_hosted_field_test:" + Date.now(),
    }

}


let instance;



let payButton = document.getElementById('pay-button');
let continueButton = document.getElementById('continue-button');
let validateButton = document.getElementById('validate-button');

let pinBackButton = document.getElementById('pin-back-button');
let otpBackButton = document.getElementById('otp-back-button');

pinBackButton.addEventListener('click', function () {
    setActivePage('card-details');
    instance.clearField('pin');
});

otpBackButton.addEventListener('click', function () {
    setActivePage('pin');
    instance.clearField('otp');
});


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

isw.hostedFields.create(configuration, callback);






function handleBinConfigResponse(err, response) {
    if (err != null && err.validationError === true) {
        let fieldContainer = document.getElementById('cardNumber-container');
        fieldContainer.style.borderBottomColor = '#FF0000';
        return;
    }

    if(err != null && err.networkError === true){
        showNotification(err, null);
        return;
    }

    if (err !== null) {
        showNotification(err, null)
        return;
    }

    if (response.supportsPin) {
        setActivePage('pin');
        return;
    }

    instance.makePayment(handlePayResponse);
}


function handlePayResponse(err, response) {
    if (err != null && err.validationError === true) {
        showNotification(err, null);
        return;
    }

    if(err != null && err.networkError === true){
        showNotification(err, null);
        return;
    }

    if (err != null) {
        showNotification(err, null)
        return;
    }

    if (response.requiresCentinelAuthorization === true) {
        setActivePage('cardinal');
        return;
    }

    if (response.responseCode === 'T0') {
        setActivePage('otp');
        showNotification(null, response);
        return;
    }
}

function handleValidateResponse(err, response) {

    if (err != null && err.validationError === true) {
        showNotification(err, null);
        return;
    }

    if(err != null && err.networkError === true){
        showNotification(err, null);
        return;
    }

    if (err != null) {
        showNotification(err, null);
        return;
    }

    if(response){
        showNotification(null, response)
        return;
    }
}

let alertDiv = document.getElementById('response')
let responseCodeDiv = document.getElementById('response-code') 
let responseMessageDiv = document.getElementById('response-message') 


function showNotification(error, response){
    if(response){
        console.log('response: ', response);
        let responseCode = response.responseCode
        let responseMessage = response.plainTextSupportMessage
        if(response.plainTextSupportMessage){
            setNotificationText(`${responseCode}`, `${responseMessage}`)
        } else {
            setNotificationText(`${responseCode}`, null)
        }
    } else if(error) {
        alertDiv.classList.add('alertError', true)
        let errorCode = error.responseCode;
        let validationError = error.validationError;
        let networkError = error.networkError;
        if(validationError){
            setNotificationText(null, `There's Validation Error`)
        }
        if(networkError){
            setNotificationText(null, `Network Error`)
        }
        if(errorCode){
            setNotificationText(`${errorCode}`, null)
        }
    }
    if (response.responseCode === '00') {
        setActivePage('card-details');
        return;
    }
}


function handleCardinalValidateResponse(err, response) {
    
    setActivePage('card-details');

}

function setNotificationText(responseCode, responseMessage){
    responseCodeDiv.innerHTML = responseCode;
    responseMessageDiv.innerHTML = responseMessage
    alertDiv.classList.add('showNotification')
        setTimeout(function(){
            alertDiv.classList.remove('showNotification', 'alertError')
        }, 7000)
}

function setActivePage(pageName) {

    let pages = document.querySelectorAll('.form-page');

    for (let i = 0; i < pages.length; i++) {
        let page = pages[i];
        page.classList.remove('show');
    }

    let activePage = document.querySelector('.form-page.' + pageName);
    activePage.classList.add('show');
}

window.onload = function(){
    //display the amount being processed on page
    document.getElementById('amount').innerHTML = configuration.paymentParameters.amount / 100;
}
```

### Sample 2
Below is another approach to the integration:

HTML
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Hosted Field Sample 2</title>
  <link rel="stylesheet" href="style.css" />
  <link href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet" integrity="sha384-wvfXpqpZZVQGK6TAh5PVlGOfQNHSoD2xbE+QkPxCAFlNEevoEH3Sl0sibVcOQVnN" crossorigin="anonymous" />
</head>

<body onload="showPaymentPage()">
  <div class="payment-form-container" style="display: none">
    <div class="notification-box">
      <div class="content"></div>
    </div>

    <div class="form-page card-details show">
      <div class="form-text" style="text-align: center;">PAY WITH CARD</div>
      <div class="payment-body">
        <div class="form-group">
          <div class="form-control c1">
            <div id="cardNumber-container" class="payment-field"></div>
          </div>
          <div class="form-control c2">
            <div id="expirationDate-container" class="payment-field"></div>
          </div>

          <div class="form-control c3">
            <div id="cvv-container" class="payment-field"></div>
          </div>
        </div>

        <div class="button-container">
          <button id="pay-button" class="purple">
            Pay (<span id="amount-placeholder" class="amount"></span>)
            <span id="payment-loading"><i class="fa fa-spinner"></i></span>
          </button>
        </div>
      </div>
    </div>

    <div class="form-page pin">
      <div class="back-control" id="pin-back-button">
        <label style="color: black; font-family: monospace;">
          << Back</label>
      </div>

      <div class="form-text" style="text-align: center;">
        Please provide your PIN
      </div>
      <div class="payment-body">
        <div class="form-control">
          <div id="pin-container" class="payment-field"></div>
        </div>

        <div class="button-container">
          <button id="continue-button" class="purple">
            Continue
            <span id="continue-loading"><i class="fa fa-spinner"></i></span>
          </button>
        </div>
      </div>
    </div>

    <div class="form-page otp">
      <div class="back-control" id="otp-back-button">
        <label style="color: black; font-family: monospace;">
          << Back</label>
      </div>

      <div class="form-text" style="text-align: center;">
        Please input the OTP sent to your mobile number
      </div>

      <div class="payment-body">
        <div class="form-control">
          <div id="otp-container" class="payment-field"></div>
        </div>

        <div class="button-container">
          <button id="validate-button" class="purple">
            Validate
            <span id="validate-loading"><i class="fa fa-spinner"></i></span>
          </button>
        </div>
      </div>
    </div>

    <div class="form-page cardinal">
      <div class="cardinal-container"></div>
    </div>
  </div>

  <script src="https://nwp.qa.interswitchng.com/sdk.js"></script>
  <script src="index.js"></script>
</body>

</html>
```

CSS
```css
body {
  margin: 0px;
  font-family: monospace;
}

.create-payment-container {
  position: relative;
  margin: auto;
  margin-top: 5%;
  width: 400px;
  padding-bottom: 0px;
  box-sizing: border-box;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  padding: 10px;
}

.create-payment-container div {
  margin-bottom: 20px;
}

.create-payment-container label {
  display: block;
  font-size: 12px;
  font-family: sans-serif;
  letter-spacing: 1px;
  margin-bottom: 5px;
}

.create-payment-container input {
  width: 100%;
  box-sizing: border-box;
  background-color: rgb(250, 250, 250);
  border: none;
  padding: 10px;
  padding-left: 10px;
  font-family: monospace;
}

.create-payment-container button {
  height: 30px;
}

.payment-form-container {
  position: relative;
  /* background-color: white; */
  margin: auto;
  margin-top: 5%;
  width: 500px;
  padding-bottom: 0px;
  box-sizing: border-box;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
}

.cardinal-container {
  position: absolute;
  width: 100%;
  height: 100%;
  background: white;
  left: 0;
  top: 0;
  z-index: 10;
}

.form-header {
  font-family: sans-serif;
  border-bottom: 2px solid #f0f0f0;
  padding: 15px;
  background-color: #fafafa;
  text-align: center;
}

.form-header > img {
  width: 30px;
  height: 30px;
  display: inline-block;
  vertical-align: middle;
  border: 2px solid #ececec;
  background: #ffffff;
  padding: 10px;
  border-radius: 5px;
}

.form-header .info {
  display: inline-block;
  vertical-align: middle;
  text-align: left;
  margin-left: 17px;
}

.form-header .info .title {
  font-size: 15px;
  margin-bottom: 5px;
}

.form-header .info .amount {
  color: black;
  font-size: 42px;
}

.form-page {
  display: none;
  padding: 25px;
  padding-top: 45px;
  padding-bottom: 45px;
  position: relative;
  /* background-color: #8898aa;   */
  background-color: rgba(18, 91, 152, 0.05);
}

.form-page.show {
  display: block;
}

.form-page.cardinal {
  min-height: 350px;
}

.back-control {
  display: inline-block;
  margin-bottom: 0px;
  padding-top: 10px;
  padding-bottom: 10px;
}

.back-control * {
  vertical-align: middle;
  cursor: pointer;
}

.back-control img {
  width: 16px;
  height: 16px;
}

.back-control label {
  color: #0076ff;
  font-family: sans-serif;
  display: inline-block;
  font-size: 16px;
  padding-left: 3px;
}

.form-control {
  margin-top: 35px;
  font-size: 0px;
  font-family: monospace;
}

.form-control:first-of-type {
  margin-top: 0px;
}

.form-group {
  font-size: 0px;
}

.form-group .form-control {
  /* width: calc(33%); */
  display: inline-block;
}

.c1 {
  width: 60%;
}
.c2 {
  width: 20%;
}
.c3 {
  width: 20%;
}

.payment-field {
  display: inline-block;
  height: 35px;
  width: 100%;
  background-color: rgb(247, 247, 247);
  transition: border-bottom-color 0.3s;
}

.form-text {
  font-size: 16px;
  line-height: 30px;
  font-family: monospace;
}

.button-container {
  margin-top: 10px;
  display: flex;
  justify-content: center;
}

.button-container button {
  width: 98%;
  height: 50px;
  color: white;
  font-size: 16px;
  border: none;
  cursor: pointer;
  outline: none;
  font-family: monospace;
  border-radius: 5px;
  margin-bottom: 5px;
}

.button-container button.blue {
  background: #5fabdc;
}

.button-container button.purple {
  background: #d782d9;
}

.button-container button.green {
  background: #5fdc91;
}

.button-container button.orange {
  background: #e3a83b;
}

.button-container button:hover {
  opacity: 0.8;
}

.button-container button:active {
  opacity: 0.9;
}

.form-page.pin .back-control,
.form-page.otp .back-control {
  margin-bottom: 40px;
}

.notification-box {
  display: none;
  width: 100%;
  position: absolute;
  left: 0px;
  top: 0px;
  padding: 10px;
  box-sizing: border-box;
}

.notification-box.show {
  display: block;
}

.notification-box .content {
  min-height: 20px;
  border-radius: 5px;
  padding: 6px;
  color: white;
  font-size: 16px;
  font-family: monospace;
}

.notification-box .content.error {
  background-color: #e97a7a;
}

.notification-box .content.success {
  background-color: #5cd796;
}

.payment-body {
  background-color: white;
  border-radius: 5px;
}

#validate-loading,
#payment-loading,
#continue-loading {
  display: none;
}
```

Javascript
```js
var timestampString = new Date().getTime().toString();

var configuration = {
  fields: {
    cardNumber: {
      selector: "#cardNumber-container",
      placeholder: "Card Number",
      styles: {
        fontSize: "16px",
        padding: "0px 0px 0px 10px",
        backgroundColor: "white",
        fontFamily: "monospace"
      }
    },
    expirationDate: {
      selector: "#expirationDate-container",
      placeholder: "MM / YY",
      styles: {
        fontSize: "16px",
        padding: "0px 0px 0px 10px",
        backgroundColor: "white",
        fontFamily: "monospace"
      }
    },
    cvv: {
      selector: "#cvv-container",
      placeholder: "CVV",
      styles: {
        fontSize: "16px",
        padding: "0px 0px 0px 10px",
        backgroundColor: "white",
        fontFamily: "monospace"
      }
    },
    pin: {
      selector: "#pin-container",
      placeholder: "PIN",
      styles: {
        fontSize: "16px",
        padding: "0px 0px 0px 10px",
        backgroundColor: "white",
        fontFamily: "monospace",
        textAlign: "center"
      }
    },
    otp: {
      selector: "#otp-container",
      placeholder: "OTP",
      styles: {
        fontSize: "16px",
        padding: "0px 0px 0px 10px",
        backgroundColor: "white",
        textAlign: "center",
        fontFamily: "monospace"
      }
    }
  },
  cardinal: {
    containerSelector: ".cardinal-container",
    activeClass: "show"
  },

  paymentParameters: {
    amount: 150000,
    currencyCode: "566",
    dateOfPayment: "2020-03-08T00:00:00",
    payableCode: "9405967",
    merchantCustomerName: "CIROMA CHUKUMA ADEKUNLE",
    merchantCode: "MX6072",
    transactionReference: "isw_hosted_field_test:" + timestampString
  }
};

var instance,
  showFormErrors = false,
  focusedField;

// Buttons and loader
var payButton = document.getElementById("pay-button");
var continueButton = document.getElementById("continue-button");
var validateButton = document.getElementById("validate-button");
var pinBackButton = document.getElementById("pin-back-button");
var otpBackButton = document.getElementById("otp-back-button");
var paymentLoading = document.getElementById("payment-loading");
var continueLoading = document.getElementById("continue-loading");
var validateLoading = document.getElementById("validate-loading");

function showPaymentPage() {
  isw.hostedFields.create(configuration, createHandler);
}

pinBackButton.addEventListener("click", function() {
  setActivePage("card-details");
  instance.clearField("pin");
});

otpBackButton.addEventListener("click", function() {
  setActivePage("pin");
  instance.clearField("otp");
});

//Create hosted field instance callback
function createHandler(createError, hostedFieldsInstance) {
  //handle create error
  if (createError != null) {
    var errorName = createError.name;
    var errorMessage = createError.message;

    alert(errorName + "\n" + errorMessage);
    return;
  }

  // set payment placeholder in payment form
  var selectedAmount = parseInt(configuration.paymentParameters.amount);
  var amountInMajor = selectedAmount / 100;
  document.querySelector("#amount-placeholder").innerHTML =
    "&#8358; " + amountInMajor;

  document.querySelector(".payment-form-container").style.display = "block";

  //expose instance to outer scope
  instance = hostedFieldsInstance;

  //Register focus handler to process event when a field gains focus
  instance.on("focus", function(event) {
    var fieldContainerEl = document.querySelector(event.selector);
    fieldContainerEl.style.borderBottomColor = "#a0c8e2";

    focusedField = event.fieldType;

    if (!showFormErrors) {
      return;
    }

    var validationState = instance.getFieldsState();

    checkFieldsValidation(
      ["cardNumber", "expirationDate", "cvv", "pin", "otp"],
      validationState
    );
  });

  //Register blur handler to process event when a field loses focus
  instance.on("blur", function(event) {
    var fieldName = event.fieldType;

    var fieldContainerEl = document.querySelector(event.selector);
    fieldContainerEl.style.borderBottomColor = "#e4e4e4";

    if (showFormErrors) {
      var validationState = instance.getFieldsState();

      if (!validationState[fieldName].valid) {
        fieldContainerEl.style.borderBottomColor = "red";
      }
    }
  });

  //Register validation handler to run some code when validation state updates
  instance.on("validation", function(validationState) {
    if (!showFormErrors) {
      return;
    }

    checkFieldsValidation(
      ["cardNumber", "expirationDate", "cvv", "pin", "otp"],
      validationState
    );
  });

  //Register cardinal-response handler to execute some code when cardinal paymnet completes
  instance.on("cardinal-response", handleCardinalValidateResponse);

  payButton.addEventListener("click", function() {
    paymentLoading.style.display = "inline";

    showFormErrors = true;

    var validationState = instance.getFieldsState();

    var fieldsValid = checkFieldsValidation(
      ["cardNumber", "expirationDate", "cvv"],
      validationState
    );

    if (!fieldsValid) {
      return;
    }

    instance.getBinConfiguration(handleBinConfigResponse);
  });

  continueButton.addEventListener("click", function() {
    continueLoading.style.display = "inline";

    var validationState = instance.getFieldsState();

    var fieldsValid = checkFieldsValidation(["pin"], validationState);

    if (!fieldsValid) {
      return;
    }

    instance.makePayment(handlePayResponse);
  });

  validateButton.addEventListener("click", function() {
    validateLoading.style.display = "inline";
    console.log("CLICKED VALIDATE");
    instance.validatePayment(handleValidateResponse);
  });
}

function handleBinConfigResponse(err, response) {
  paymentLoading.style.display = "none";

  if (err != null && err.validationError === true) {
    showNotification("Validation Error", true);
    return;
  }

  if (err != null && err.networkError === true) {
    showNotification("Network Error", true);
    return;
  }

  if (err !== null) {
    showNotification(
      "Could not process the request. " + err.responseCode,
      true
    );
    return;
  }

  if (response.supportsPin) {
    setActivePage("pin");
    return;
  }

  instance.makePayment(handlePayResponse);
}

function handlePayResponse(err, response) {
  continueLoading.style.display = "none";

  if (err != null && err.validationError === true) {
    showNotification("Validation Error", true);
    return;
  }

  if (err != null && err.networkError === true) {
    showNotification("Network Error", true);
    return;
  }

  if (err != null) {
    showNotification("Payment failed. " + err.responseCode, true);
    return;
  }

  if (response.responseCode === "00") {
    showNotification("Transaction successful", false);
    setActivePage("card-details");
    return;
  }

  if (
    response.responseCode === "T0" &&
    response.requiresCentinelAuthorization === true
  ) {
    setActivePage("cardinal");
    return;
  }

  if (response.responseCode === "T0") {
    setActivePage("otp");
    return;
  }

  showNotification("Payment failed. " + response.responseCode, true);
}

function handleValidateResponse(err, response) {
  console.log("VALIDATION RESPONSE: ", response);
  validateLoading.style.display = "none";

  if (err != null && err.validationError === true) {
    showNotification("Validation Error", true);
    return;
  }

  if (err != null && err.networkError === true) {
    showNotification("Network Error", true);
    return;
  }

  if (err != null) {
    showNotification("Payment validation failed. " + err.responseCode, true);
    return;
  }

  if (response.responseCode === "00") {
    showNotification("Transaction successful", false);
    setActivePage("card-details");
    return;
  }

  showNotification("Payment validation failed. " + response.responseCode, true);
}

function handleCardinalValidateResponse(err, response) {
  setActivePage("card-details");

  if (err != null && err.validationError === true) {
  }

  if (err != null) {
    showNotification("Something went wrong", true);
    return;
  }

  if (response.responseCode === "00") {
    showNotification("Transaction successful", false);
    return;
  }

  showNotification(
    "Cardinal payment validation failed. " + response.responseCode,
    true
  );
}

function setActivePage(pageName) {
  var pages = document.querySelectorAll(".form-page");

  for (var i = 0; i < pages.length; i++) {
    var page = pages[i];
    page.classList.remove("show");
  }

  var activePage = document.querySelector(".form-page." + pageName);
  activePage.classList.add("show");
}

function showNotification(message, isError) {
  var notificationBoxEl = document.querySelector(".notification-box");
  notificationBoxEl.classList.add("show");

  var contentEl = notificationBoxEl.querySelector(".content");
  contentEl.innerHTML = message;

  contentEl.classList.remove("error");
  contentEl.classList.remove("success");

  contentEl.classList.add(isError ? "error" : "success");

  setTimeout(function() {
    notificationBoxEl.classList.remove("show");
  }, 3000);
}

function checkFieldsValidation(fieldNames, validationState) {
  var formFieldsValid = true;

  for (var i = 0; i < fieldNames.length; i++) {
    var fieldName = fieldNames[i];
    var fieldValidationState = validationState[fieldName];

    var fieldSelector = fieldValidationState.selector;
    var fieldContainerEl = document.querySelector(fieldSelector);

    if (fieldValidationState.valid === false) {
      formFieldsValid = false;

      fieldContainerEl.style.borderBottomColor = "red";
      continue;
    }

    if (focusedField === fieldName) {
      fieldContainerEl.style.borderBottomColor = "#a0c8e2";
    } else {
      fieldContainerEl.style.borderBottomColor = "#e4e4e4";
    }
  }

  return formFieldsValid;
}
```


### References
#### The Configuration Object
1. fields: These are the input fields on the checkout page. They include cardNumber, expirationDate, cvv, pin and otp.  
Each of these fields have three properties, namely:
    a. selector: This is the ID selector of the field.
    b. placeholder: Initially displayed text on the field.
    c. styles: This is used to style each field.

2. cardinal: This has two properties, namely:
    a. containerSelector: The identifier for the container div.
    b. activeClass: The CSS class that displays the div.

3. paymentParameters. This has the following properties:
    a. amount: Total amount to be paid in kobo.
    b. currencyCode: The ISO code of the currency being used.
    c. dateOfPayment: The date the payment was made.
    d. payableCode: As provided on your Quickteller Business dashboard.
    e. merchantCode: As provided on your [Quickteller Business dashboard](https://business.quickteller.com/developertools).
    f. transactionReference: A unique reference for every transaction.

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

#### Response Codes
A list of all the possible response codes and their descriptions can be found [here](https://sandbox.interswitchng.com/docbase/docs/webpay/response-codes/)
