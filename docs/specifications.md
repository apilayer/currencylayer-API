# API Specification & Properties

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
    [...]
    }
}              
              
```

