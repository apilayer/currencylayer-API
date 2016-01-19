# Language Examples

## PHP (CURL)

### Capture GIFs in PHP:

Find below a simple PHP function that lets you generate a GIF and define all required and optional parameters:

```php
function giflayer($url, $args) {

  // set access key
  $access_key = "YOUR_ACCESS_KEY";
  
  // encode target URL
  $params['url'] = urlencode($url);

  $params += $args;

  // create the query string based on the options
  foreach($params as $key => $value) { $parts[] = "$key=$value"; }

  // compile query string
  $query = implode("&", $parts);

  return "http://apilayer.net/api/capture?access_key=$access_key&$query";

}

// set required parameters
$params['start']  = '';    
$params['end'] = '';      

// set optional parameters (leave blank if unused)
$params['duration']  = '';    
$params['size'] = '';      
$params['crop']  = '';  
$params['fps'] = '';      
$params['trailer'] = '';      
$params['export'] = '';      

// capture
$call = giflayer("https://www.youtube.com/watch?v=3W6hZR29l5o", $params);   
```
