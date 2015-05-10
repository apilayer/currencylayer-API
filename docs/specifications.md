# API Specification & Properties

See the API's [Full Documentation](https://currencylayer.com/documentation).

The **currencylayer API** provides real-time (updates ranging from 60 seconds to 1 hour, depending on your Subscription Plan) Currency Exchange Rates, by default relative to US Dollars (USD) in `JSON` format.

It is - and will always be - free for personal use (up to 1.000 API Requests) and affordable for businesses. You can [sign up to get instant API Access here](https://currencylayer.com/product).

This Documentation intends to help you get the most out of the currencylayer API by explaining in detail integration, usage, features and more using different methods and programming languages.

## Preview API Response

Each currency is represented by its international ISO 4217 Currency Code (**3-letters**). This API provides Exchange Rates for 170 world currencies, updated in real-time and delivered in lightweight and highly portable JSON Format. Find below a Preview API Response.

```json
{
    "terms": "[...]",
    "privacy": "[...]",
    "timestamp": 1430401802,
    "base": "USD",
    "rates": {
        "AED": 3.672982,
        "AFN": 57.8936,
        "ALL": 126.1652,
        "AMD": 475.306,
        "ANG": 1.78952,
        "AOA": 109.216875,
        "ARS": 8.901966,
        "AUD": 1.269072,
        "AWG": 1.792375,
        "AZN": 1.04945,
        "BAM": 1.757305,
    "[...]"
    }
}              
              
```

A [full list of supported currencies](https://currencylayer.com/currencies) can be accessed both in JSON Format (Access Key required) and on the [currencylayer.com/currencies](https://currencylayer.com/currencies).

```json
{
  "[...]"
  "currencies": {
    "AED": "United Arab Emirates Dirham",
    "AFN": "Afghan Afghani",
    "ALL": "Albanian Lek",
    "AMD": "Armenian Dram",
    "ANG": "Netherlands Antillean Guilder",  
    "[...]" 
    }
}              
```

## Access Keys

After signing up, every user is assigned a personal API Access Key, which can be used to access any of the API's Endpoints. (see below)

To call the API, simply append your `access_key` as a parameter to the URL, like so:

```url
https://apilayer.net/api/live?access_key=YOUR_ACCESS_KEY  
```

You can [sign up for a free Access Key here](https://currencylayer.com/product). Our Free Plan offers up to 1.000 monthly API Requests and hourly updated Exchange Rates. Paid plans provide larger request allowances and more up-to-date exchange rates (reaching update frequencies of 60 seconds!)

## API Endpoints (URLs)

The currencylayer API offers up to 6 customizable **Endpoints**, all of which providing different kinds of data and starting out with the following **Base URL**:

`https://apilayer.net/api/`

Take a look at the following three API Endpoints: (If you would like to try them out, [get a Free Plan](https://currencylayer.com/product) and don't forget to append your Access Key to the URL)

```url
// "live" - get the most recent exchange rate data
http://apilayer.net/api/live

// "list" - get a JSON list of all supported currencies
http://apilayer.net/api/list

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

## HTTPS - Secure Datastreams

Paid Customers may establish a **secure connection** (SSL, 256-bit HTTPS Encryption) to the currencylayer API and all data provided by and accessible through it, ensuring an encrypted communication between the server and the client.

To connect securely, simply append an `s` to the HTTP Protocol:

```json
https://apilayer.net/api/live?access_key=YOUR_ACCESS_KEY 
```

## API Properties

currencylayer API results are delivered in portable **JSON format**. Find below descriptions for this API's JSON Object Properties:

### "success", "error"

The **"success"** property indicates that your query has been successful. If your query failed, the API will return an `error` (see API Error Types below).

```
// query successful
"success": "true"

// query failed
"error": "true"
```

### "terms"

Linking the [Terms & Conditions](https://currencylayer.com/terms) to every API Result intends to remind anybody using or viewing currencylayer API Data that no guarantees are made and no warranties are offered regarding accuracy, validity, or availability, and that all usage is at your own risk.

```
"terms": "..."
```

### "privacy"

This property was included to prompt the customer to take a look at this service's [Privacy Policy](https://currencylayer.com/privacy), which defines the principles of how and for which purposes data provided by users will or will not be used.

```
"privacy": "..."
```

### "timestamp"

The `timestamp` property represents the exact time (as a **UNIX** timestamp) the Exchange Rates in the respective result set were collected, making it simple to determine how up-to-date your results are.

**Important**: Please keep in mind that when working with JavaScript, the timestamp value has to be multiplied by 1,000, as JavaScript uses milliseconds instead of seconds.

```
"timestamp": 1415543433
```

### "base"

This is the currency (by default: 'USD') to which all Exchange Rates are relative. Knowing the Base Currency makes it possible to calculate the rate between any other two currencies.

The base currency can also be found in the JSON result set's `rates` object (e.g. `"USD" : 1`).

**Changing the Base Currency:** In order to request Exchange Rates relative to a different Base Currency, simply specify it in your query using the base property. (e.g. `&base=PLN`).

Please be aware that only currencies which are part of the respective result set can be set as Base Currency. When requesting Historical Rates, some currencies may not be available for each day

```
"base": "USD"
```

### "rates"

The `rates` object contains the Exchange Rates for the requested currencies in your query, consisting of their ISO 4217 Currency Codes (3-letters) and their respective Conversion Rates.

Each of these rates is relative to the selected `base` currency.

```json
"rates": {
    "AED": 3.672982,
    "AFN": 57.8936,
    "ALL": 126.1652,
    "AMD": 475.306,
    "[...]"
```

**Requesting specific currencies**: In order to optimize your API result's file size, you may request only specific currencies to be computed, using the currencies parameter 
(e.g. `&currencies=GBP,EUR,CHF`).

## JSON Prettyprint

In order to enhance readability (i.e. for de-bugging purposes), the currencylayer API features a built-in JSON `prettyprint` function, which displays the API's Response in typically JSON-structured format.

To enable this function, simply append `prettyprint=1` to any valid API Endpoint URL:

```
http://apilayer.net/api/live
    ? access_key = YOUR_ACCESS_KEY
    & prettyprint = 1      
```

Please be aware that enabling `prettyprint` increases the API Response's file size, which might have a negative on your application's performance.

## API Error Types

An error-type system has been developed in order to - if something goes wrong - help you debug your application in the shortest time possible. The currencylayer API will return a 3-digit error-type, an internal error-message, and a plain text error-description with suggestions for the user.

Find below a very common error - triggered when the user supplies an invalid API Access Key:

```json
{
  "error": true,
  "type": 101,
  "message": "invalid_access_key",
  "description": "You have not supplied a valid API Access Key. [Technical Support: support@apilayer.net]"  
}
```

The following list should help you find the most commonly returned API Errors:

| Type | Message  | Description |
| :------------ |:---------------:| -----:|
| 404 | "404_not_found" | User requested a resource which does not exist. |
| 101 | "missing_access_key" | User did not supply an Access Key. |
| 101 | "invalid_access_key" | User entered an invalid Access Key. |
| 103 | "invalid_api_function" | User requested a non-existend API Function. |
| 104 | "usage_limit_reached" | User has reached or exceeded his Subscription Plan's monthly API Request Allowance. |
| 105 | "function_access_restricted" | The user's current Subscription Plan does not support the requested API Function. |
| 106 | "no_rates_available" | The user's query did not return any results. |

[See all Error Types](https://currencylayer.com/documentation#error_types)

## HTTP ETags

In order to reduce your request bandwitdth, the currencylayer API has built in support for `HTTP ETags`. These may be very useful to optimize the performance of your application.

**Definition:**
An Etag ("Entity Tag") is an HTTP response header used to determine whether the content stored in the browser cache still matches the content or entity on the server. As long as the content at that URL is not modified in any way, the Etag remains identical. If that content ever changes, a new and different ETag is assigned.

In our case, `ETags` allow you to check whether or not Exchange Rates have changed since your last API Request. If the rates have not been modified, your response size will be considerably smaller in file size than if they have. Practically, ETags provide a mechanism to cache exchange rate data as long as it is not updated.

[See a quick How-to guide for working with ETags](https://currencylayer.com/documentation#error_types)


