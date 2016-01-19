# Language Examples

## PHP

### Accessing Real-time Exchange Rates:

Find below an example for how to access the latest exchange rate data via PHP (CURL):

```php
// set API Endpoint and Access Key (and any options of your choice)
$endpoint = 'live';
$access_key = 'YOUR_ACCESS_KEY';

// Initialize CURL:
$ch = curl_init('http://apilayer.net/api/'.$endpoint.'?access_key='.$access_key.'');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

// Store the data:
$json = curl_exec($ch);
curl_close($ch);

// Decode JSON response:
$exchangeRates = json_decode($json, true);

// Access the exchange rate values, e.g. GBP:
echo $exchangeRates['rates']['USDGBP'];
```

### Performing a Currency Conversion:

Converting one currency to another in PHP (CURL) is as simple as:

```php
// set API Endpoint, Access Key, required parameters
$endpoint = 'convert';
$access_key = 'YOUR_ACCESS_KEY';

$from = 'USD';
$to = 'EUR';
$amount = 10;

// initialize CURL:
$ch = curl_init('http://apilayer.net/api/'.$endpoint.'?access_key='.$access_key.'&from='.$from.'&to='.$to.'&amount='.$amount.'');   
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

// get the (still encoded) JSON data:
$json = curl_exec($ch);
curl_close($ch);

// Decode JSON response:
$conversionResult = json_decode($json, true);

// access the conversion result
echo $conversionResult['result'];
```
