# API Specification & Properties

See the API's [Full Documentation](https://currencylayer.com/documentation).

The **currencylayer API** provides real-time (updates ranging from 60 seconds to 1 hour, depending on your Subscription Plan) Currency Exchange Rates, by default relative to US Dollars (USD) in `JSON` format.

It is - and will always be - free for personal use (up to 1.000 API Requests) and affordable for businesses. You can [sign up to get instant API Access here](https://currencylayer.com/product).

This Documentation intends to help you get the most out of the currencylayer API by explaining in detail integration, usage, features and more using different methods and programming languages.

## Preview API Response

Each currency is represented by its international ISO 4217 Currency Code (**3-letters**). This API provides Exchange Rates for 168 world currencies, updated in real-time and delivered in lightweight and highly portable JSON Format. Find below a Preview API Response.

```json
{
    "terms": "https://currencylayer.com/terms",
    "privacy": "https://currencylayer.com/privacy",
    "timestamp": 1430401802,
    "source": "USD",
    "quotes": {
        "USDAED": 3.672982,
        "USDAFN": 57.8936,
        "USDALL": 126.1652,
        "USDAMD": 475.306,
        "USDANG": 1.78952,
        "USDAOA": 109.216875,
        "USDARS": 8.901966,
        "USDAUD": 1.269072,
        "USDAWG": 1.792375,
        "USDAZN": 1.04945,
        "USDBAM": 1.757305,
    "[...]"
    }
}              
              
```

A [full list of supported currencies](https://currencylayer.com/currencies) can be accessed on the [official currencylayer website](https://currencylayer.com/currencies).

## API Access Key

After signing up, every user is assigned a personal API Access Key - a unique "password" that can be used to access the API's Endpoints. (see below)

To call the API, simply append your `access_key` as a parameter to the URL, like so:

```url
https://apilayer.net/api/live?access_key=YOUR_ACCESS_KEY  
```

You can [sign up for a free Access Key here](https://currencylayer.com/product). Our Free Plan offers up to 1.000 monthly API Requests and hourly updated Exchange Rates. Paid plans provide larger request allowances and more up-to-date exchange rates (reaching update frequencies of 60 seconds)

## API Endpoints (URLs)

The currencylayer API offers up to 6 customizable **Endpoints**, all of which providing different kinds of data and starting out with the following **Base URL**:

`https://apilayer.net/api/`

Take a look at the following two API Endpoints: (If you would like to try them out, [get a Free Plan](https://currencylayer.com/product) and don't forget to append your Access Key to the URL)

```url
// "live" - get the most recent exchange rate data
http://apilayer.net/api/live

// "historical" - get historical rates for a specific day  
http://apilayer.net/api/historical?date=YYYY-MM-DD     

```

## JSONP Callbacks

The currencylayer API also supports **JSONP** for cross-domain/AJAX requests. To use this feature, simply append:

`?callback=SOME_CALLBACK`

to any valid API Endpoint, and the result set will be returned **wrapped in the specified callback function**:

```json
http://apilayer.net/api/live?callback=SOME_CALLBACK 
```

**Note**: The API also supports `Access-Control (CORS)` headers.

## 256-bit HTTPS Encryption

Paid Customers may establish a **secure connection** (SSL, 256-bit HTTPS Encryption) to the currencylayer API and all data provided by and accessible through it, ensuring an encrypted communication between server and client.

To connect securely, simply append an `s` to the HTTP Protocol:

```json
https://apilayer.net/api/live?access_key=YOUR_ACCESS_KEY 
```

## API Properties

currencylayer API results are delivered in lightweight and portable **JSON format**. Find below descriptions for the API's JSON Object Properties:

### "success", "error"

The **"success"** property indicates if your query has been successful or not. (see **API Error Types** below).

```
// query successful
"success": "true"

// query failed
"success": "false"
```

### "terms"

[Terms & Conditions](https://currencylayer.com/terms): No guarantees are made and no warranties are offered regarding accuracy, validity, or availability, and all usage of this service is at your own risk.

```
"terms": "..."
```

### "privacy"

This property was included to prompt the customer to take a look at this service's [Privacy Policy](https://currencylayer.com/privacy), which defines the principles of how and for which purposes data provided by customers will or will not be used.

```
"privacy": "..."
```

### "timestamp"

The `timestamp` property represents the exact date and time (as a **UNIX** timestamp) the Exchange Rates in the respective API Response were collected, making it simple to determine how up-to-date your results are.


```
"timestamp": 1415543433
```

### "source"

This is the currency (by default: 'USD') to which all Exchange Rates in your respective API Response are relative.

The source currency can also be found in the JSON result set's `quotes` object (e.g. `"USD" : 1`).

**Changing the Source Currency:** In order to request Exchange Rates relative to a different Source Currency, simply specify it in your query using the source property. (e.g. `&source=PLN`).

Please be aware that only currencies which are part of the respective result set can be set as Source Currency. When requesting Historical Rates, some currencies may not be available for each day.

```
"source": "USD"
```

### "quotes"

The `quotes` object contains the Exchange Rates for the requested currencies in your query, consisting of their ISO 4217 Currency Codes (3-letters) and their respective Conversion Rates.

Each of these rates is relative to the selected `base` currency.

```json
"quotes": {
    "USDAED": 3.672982,
    "USDAFN": 57.8936,
    "USDALL": 126.1652,
    "USDAMD": 475.306,
    "[...]"
```

**Specifying output currencies**: In order to optimize your API result's file size, you may request a limited set of currencies to be computed, using the currencies parameter (e.g. `&currencies=GBP,EUR,CHF`).

## JSON Formatting

In order to enhance readability (i.e. for de-bugging purposes), the currencylayer API features a built-in JSON `format` function, which displays the API's Response in typically JSON-structured format.

To enable this function, simply append `format=1` to any valid API Endpoint URL:

```
http://apilayer.net/api/live
    ? access_key = YOUR_ACCESS_KEY
    & format = 1      
```

Please be aware that enabling `format` increases the API Response's file size, which might have a negative on your application's performance.

## API Error Codes

All specified API Error Codes have been documented in order to - if something goes wrong - help you debug your application in the shortest time possible. The currencylayer API will return a 3-digit error-type, an internal error-message, and a plain text error-description with suggestions for the user.

Find below an example error - triggered when a user's monthly API Request volume has been reached or exceeded:

```json
{
  "success": false,
  "error": {
    "code": 104,
    "info": "Your monthly usage limit has been reached. Please upgrade your Subscription Plan."    
  }
}
```

The following list should help you find the most commonly returned API Errors:

| Type | Description |
| :------------ | -----:|
| 404 | User requested a resource which does not exist. |
| 101 | User did not supply an Access Key. |
| 101 | User entered an invalid Access Key. |
| 103 | User requested a non-existend API Function. |
| 104 | User has reached or exceeded his Subscription Plan's monthly API Request Allowance. |
| 105 | The user's current Subscription Plan does not support the requested API Function. |
| 106 | The user's query did not return any results. |

[See all Error Codes](https://currencylayer.com/documentation#error_codes)

## HTTP ETags

In order to reduce your request bandwidth, the currencylayer API has built in support for `HTTP ETags`. These may be very useful optimizing the performance of your application.

**Definition:**
An Etag ("Entity Tag") is an HTTP response header used to determine whether the content stored in the browser cache still matches the content or entity on the server. As long as the content at that URL is not modified in any way, the Etag remains identical. If that content ever changes, a new and different ETag is assigned.

In our case, `ETags` allow you to check whether or not Exchange Rates have changed since your last API Request. If the rates have not been modified, your API Response will be considerably smaller in size than if they have. Practically, ETags provide a mechanism to cache exchange rate data as long as it is not updated.

[See a quick How-to guide for working with ETags](https://currencylayer.com/documentation#etags)


