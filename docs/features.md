# API Features

See the API's [Full Documentation](https://currencylayer.com/documentation).

## Historical Rates (available to all users)

The currencylayer API provides accurate *EOD (End of Day)* Exchange Rate data for every day since 1999.

Historical Rates may be accessed by simply appending the `date` parameter with a valid date (Format: `YYYY-MM-DD`) to the API's `historical` Endpoint.

**Syntax:**

```
http://apilayer.net/api/historical
    ? access_key = YOUR_ACCESS_KEY
    & date = YYYY-MM-DD      
```

**Example Query:**

```
http://apilayer.net/api/historical
    ? access_key = YOUR_ACCESS_KEY
    & date = 2005-02-01     
```

Please be aware that Exchange Rate data for certain currencies may not be available for each requested day (e.g. Bitcoin was only introduced in 2009).

## Requesting Specific Currencies (available to all users)

For both Live and Historical Rates, you may request specific currencies by appending the `currencies` parameter followed by any available 3-letter Currency Codes (divided by commas) of your choice.

The `currencies` parameter is supported by each of the API's Endpoints (except for Single-Conversion, more info below), may be combined with JSONP callbacks, and has no limits in size.

**Example Query:**

```
http://apilayer.net/api/live
    ? access_key = YOUR_ACCESS_KEY
    & currencies = AUD,CHF,EUR,GBP,PLN     
```

Using this query, the API will return a standard response with only your specified currencies in the `rates` object:

```json
{
  "success": true,
  "terms": "[...]",
  "privacy": "[...]",
  "timestamp": 1430068515,
  "base": "USD",
  "rates": {
    "AUD": 1.278384,
    "CHF": 0.953975,
    "EUR": 0.919677,
    "GBP": 0.658443,
    "PLN": 3.713873
  }
}
```

The currencies feature may be especially useful when working with Time-Frame Queries, as it allows you to reduce the API Response's file size drastically.

## Base Currency Switching (available to all paid users)

As a paid customer, you may request Exchange Rates relative to a different `base` currency than US Dollars, simply by appending the `base` parameter followed by the 3-letter Currency Code of your choice.

The `base` parameter is supported by each of the API's Endpoints (except for Single-Conversion) and may be used in combination with JSONP callbacks.

**Example Query:**

```
http://apilayer.net/api/live
    ? access_key = YOUR_ACCESS_KEY
    & base = GBP  
```

**Example Response:**

Using this query, the API will return the standard result set, but relative to the base currency you specified (in our example: GBP):

```json
{
  "success": true,
  "terms": "[...]",
  "privacy": "[...]",
  "timestamp": 1430068515,
  "base": "GBP",
  "rates": {
    "AED": 5.578448,
    "AFN": 87.869413,
    "ALL": 196.414724,
    "AMD": 719.087298,
    "ANG": 2.717836,
    "AOA": 165.601846,
    "ARS": 13.514458,
    "AUD": 1.941526,
    "[...]"
    }
}      
```

Please note that only currencies that are part of the respective result set can be set as `base` currency.

## Single-Conversion API (available to all paid users)

Using the `convert` Endpoint, you may request the currencylayer API to perform a Single Currency Conversion on your behalf.

Simply specify a `from` Currency Code, a `to` Currency Code, and the `amount` you would like to convert.

**Example Query:**

```
http://apilayer.net/api/convert
    ? access_key = YOUR_ACCESS_KEY
    & from = USD
    & to = GBP       
    & amount = 10     
```

As long as you don't specify an additional `date` along with these properties, the latest available Exchange Rates will be used for your Conversion.

**Example response:**

A Conversion API Response contains more information than standard result sets. Your requested query is returned in the `query` object. It contains the specified `from`, `to`, and `amount` properties. Additionally, the API will return an `info` object, which indicates how up-to-date the used Exchange Rates are (`timestamp`) and at which conversion rate (`rate`) your specified amount was converted.

At the end of the response, there will be a `result` property containing your successfully converted amount.

```json
{
  "success": true,
  "terms": "[...]",
  "privacy": "[...]",
  "query": {
    "from": "USD",
    "to": "GBP",
    "amount": 10
  },
  "info": {
    "timestamp": 1430068515,
    "rate": 0.658443
  },
  "result": 6.58443
}
```

### Currency Conversion using Historical Rates

The currencylayer API also supports Single-Currency Conversions using Historical Exchange Rates. Simply append an additional `date` property with a valid date (Format: `YYYY-MM-DD`) to your query.

**Example query:**

```
http://apilayer.net/api/convert
    ? access_key = YOUR_ACCESS_KEY
    & from = USD
    & to = GBP
    & amount = 10
    & date = 2005-01-01
```

**Example response:**

Additionally to the above mentioned properties and objects, the API will return `"historical" : true` to confirm your request, and the `date` you specified (also indicated by the `timestamp` property).

```json
{
  "success": true,
  "terms": "[...]",
  "privacy": "[...]",
  "query": {
    "from": "USD",
    "to": "GBP",
    "amount": 10
  },
  "info": {
    "timestamp": 1104623999,
    "rate": 0.51961
  },
  "historical": true,
  "date": "2005-01-01",
  "result": 5.1961
}
```

Please note that Historical Currency Conversions can only be performed if Exchange Rate data for the respective currencies are available at the specified `date`.

## Time-Frame Queries (only Professional & Enterprise users)

If your Subscription Plan features the `timeframe` API Endpoint, you may request Historical Exchange Rates for a time-period of your choice. (maximum range: 365 days)

Simply specify your preferred time-frame, consisting of a `start_date` and an `end_date`, both of the format `YYYY-MM-DD`.

**Example query:**

```
http://apilayer.net/api/convert
    ? access_key = YOUR_ACCESS_KEY
    & start_date = 2010-03-01
    & end_date = 2010-04-01
    & currencies = USD,GBP,EUR
```

This feature can also be used in combination with **Base Currency Switching** and **JSONP Callbacks**.

**Please note:** Since the currencylayer API allows time-frames of up to 365 days, queries containing all available currencies for each requested day may result in very large file sizes. For optimum performance and a lower server-load, it is highly recommended to use this feature combined with the `currencies` parameter, which allows you to request only specific currencies.

**Example response:**

Along with `"timeframe": true`, the `start_date`, and the `end_date`, the API Response's `rates` object will contain the specified Exchange Rates divided into "sub-objects" for each of the days within the requested time-frame.

```json
{
  "success": true,
  "terms": "[...]",
  "privacy": "[...]",
  "timeframe": true,
  "start_date": "2010-03-01",
  "end_date": "2010-04-01",
  "base": "USD",
  "rates": {
    "2010-03-01": {
      "USD": 1,
      "GBP": 0.668525,
      "EUR": 0.738541
    },
    "2010-03-02": {
      "USD": 1,
      "GBP": 0.668827,
      "EUR": 0.736145
    },
    "[...]"
  }
}    
```

### Date Correction Mechanisms

To enhance usability, automatic date-correction mechanisms have been implemented. If you request an invalid `start_date` or `end_date`, the API will automatically set it backwards to nearest available date - e.g. `2015-02-30` will be automatically corrected to `2015-02-28`. This enables you to sequentially request every month from the 1st to the 31st, disregarding the individual length of each month.

## Currency-Change Queries (only Enterprise users)

Using the API's `change` Endpoint, you may request the change (both margin and percentage) of one or more currencies, relative to a Base Currency, within a specific time-frame (optional).

Unlike with Time-Frame Queries, performing a **Currency-Change Query** does not require you to specify a time-frame. If you do not provide a time-frame, the API will automatically return the change values ranging from yesterday's *EOD (End Of Day)* data, to the latest available exchange rates of today.

However, if you decide to specify a `start_date` and and `end_date` (both of the format `YYYY-MM-DD`), the API will return change values for your preferred time-frame.

**Example query:**

```
http://apilayer.net/api/change
    ? access_key = YOUR_ACCESS_KEY
    & start_date = 2005-01-01
    & end_date = 2010-01-01
    & currencies = AUD,EUR,MXN
```

This feature can also be used in combination with **Base Currency Switching** and **JSONP Callbacks**.

**Example response:**

To indicate that you are performing a `change` query, the API will return `"change": true`. Right below, your specified `start_date` and `end_date` will be part of the API Response as well.

Just like when performing a Time-Frame Query, the API Response's `rates` object will be divided in sub-objects (one for each requested currency).

The four properties contained in these sub-objects are explained below in **Code View**:

```
start_rate     the respective currency's exchange rate at the  
               beginning of the specified period
               
end_rate       the respective currency's exchange rate at the  
               end of the specified period
               
change         the margin between the currency's start_rate
               end_rate
               
change_pct     the currency's percentage change within the 
               specified time-frame  
```

This is how the complete API Response will look like:

```json
{
  "success": true,
  "terms": "[...]",
  "privacy": "[...]",
  "change": true,
  "start_date": "2005-01-01",
  "end_date": "2010-01-01",
  "base": "USD",
  "rates": {
    "AUD": {
      "start_rate": 1.281236,
      "end_rate": 1.108609,
      "change": -0.1726,
      "change_pct": -13.4735
    },
    "EUR": {
      "start_rate": 0.73618,
      "end_rate": 0.697253,
      "change": -0.0389,
      "change_pct": -5.2877
    },
    "MXN":{
      "start_rate": 11.149362,
      "end_rate": 13.108757,
      "change": 1.9594,
      "change_pct": 17.5741
    }
  }
} 
```

If the decimal values contained both in the `change` and in the `change_pct` property are positive, the value of the respective currency increased over the course of the specified time-frame. (e.g. `"change_pct": 17.5741` would be a `+17.57% increase`). If the values are negative, it decreased. (e.g. `"change_pct": -5.2877` would be a `-5.29% decrease`)

### "change": 0

If the `change` and `change_pct` properties equal zero, either the specified currency's exchange rate value has not changed over the course of the entire time-frame, or an exchange rate value for the respective currency on at least one of the specified dates is "unavailable".

**Please note:** Since foreign exchange (forex) markets are only open from Sunday 5 PM ET to Friday 5 PM ET, change parameters may converge to zero during weekends.

[Sign up](https://currencylayer.com/product) for your personal API Access Key »
