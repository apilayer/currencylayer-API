# Language Examples

## jQuery.ajax

Echange Rates can be easily accessed by using jQuery to make an AJAX call. Using this method is widely used and highly recommend due to ease of integration.

### Accessing Real-time Exchange Rates:

Find below an example for how to access the latest exchange rate data via jQuery.ajax:

```javascript
// set endpoint and your access key
endpoint = 'live'
access_key = 'YOUR_ACCESS_KEY';

// get the most recent exchange rates via the "live" endpoint:
$.ajax({
    url: 'https://apilayer.net/api/' + endpoint + '?access_key=' + access_key,   
    dataType: 'jsonp',
    success: function(json) {

        // exchange rata data is stored in json.quotes
        alert(json.quotes.USDGBP);
        
        // source currency is stored in json.source
        alert(json.source);
        
        // timestamp can be accessed in json.timestamp
        alert(json.timestamp);
        
    }
});
```

### Performing a Currency Conversion:

Converting one currency to another in jQuery.ajax is as simple as:

```javascript
// set endpoint and your access key
endpoint = 'convert';
access_key = 'YOUR_ACCESS_KEY';

// define from currency, to currency, and amount
from = 'EUR';
to = 'GBP';
amount = '10';

// execute the conversion using the "convert" endpoint:
$.ajax({
    url: 'https://apilayer.net/api/' + endpoint + '?access_key=' + access_key +'&from=' + from + '&to=' + to + '&amount=' + amount,   
    dataType: 'jsonp',
    success: function(json) {

        // access the conversion result in json.result
        alert(json.result);
                
    }
});
```
