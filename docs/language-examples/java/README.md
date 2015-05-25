[Sign up](https://currencylayer.com/product) for your personal API Access Key.

# Code Examples

## Java 

Making use of a variety of `Apache HTTP Components`, there is a fairly simple way to query the API using Java. Based on the following integration methods for the API's `live` and `convert` Endpoints, we will try to provide a general idea of how **currencylayer** JSON data can be accessed and implemented in your application.

The entire Java demo source code folder, along with all necessary `components`, is available for download in the **java directory**.

### Accessing Real-time Exchange Rates:

Find below an example for how to access and display the latest exchange rate data using Java:

```java
package org.json.poc;

// necessary components are imported
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

import org.apache.http.HttpEntity;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.json.JSONObject;

public class LiveResponseDemo{
        
        // essential URL structure is built using constants 
	public static final String ACCESS_KEY = "YOUR_ACCESS_KEY";
	public static final String BASE_URL = "https://apilayer.net/api/";
	public static final String ENDPOINT = "live";

	// this object is used for executing requests to the (REST) API
	static CloseableHttpClient httpClient = HttpClients.createDefault();
	
	/**
	 * 
	 * Notes:<br><br>
	 * 
	 * A JSON response of the form {"key":"value"} is considered a simple Java JSONObject.<br>
	 * To get a simple value from the JSONObject, use: <JSONObject identifier>.get<Type>("key");<br>
	 * 
	 * A JSON response of the form {"key":{"key":"value"}} is considered a complex Java JSONObject.<br>
	 * To get a complex value like another JSONObject, use: <JSONObject identifier>.getJSONObject("key")
	 * 
	 * Values can also be JSONArray Objects. JSONArray objects are simple, consisting of multiple JSONObject Objects.
	 * 
	 * 
	 */
	
	// sendLiveRequest() function is created to request and retrieve the data
	public static void sendLiveRequest(){
		
		// The following line initializes the HttpGet Object with the URL in order to send a request
		HttpGet get = new HttpGet(BASE_URL + ENDPOINT + "?access_key=" + ACCESS_KEY);
		
		try {
			CloseableHttpResponse response =  httpClient.execute(get);
			HttpEntity entity = response.getEntity();
			
			// the following line converts the JSON Response to an equivalent Java Object
			JSONObject exchangeRates = new JSONObject(EntityUtils.toString(entity));
			
			System.out.println("Live Currency Exchange Rates");
			
			// Parsed JSON Objects are accessed according to the JSON resonse's hierarchy, output strings are built
			Date timeStampDate = new Date((long)(exchangeRates.getLong("timestamp")*1000)); 
			DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss a");
			String formattedDate = dateFormat.format(timeStampDate);
			System.out.println("1 " + exchangeRates.getString("source") + " in GBP : " + exchangeRates.getJSONObject("quotes").getDouble("USDGBP") + " (Date: " + formattedDate + ")");
			System.out.println("\n");
			response.close();
		} catch (ClientProtocolException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ParseException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (JSONException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

        // sendLiveRequest() function is executed
	public static void main(String[] args) throws IOException{
		sendLiveRequest();
		httpClient.close();
		new BufferedReader(new InputStreamReader(System.in)).readLine();
	}
}
```

The **Java Output** will look like this:

```
Live Currency Exchange Rates
1 USD in GBP : 0.66046 (Date: 2015-05-02 21:26:15 PM)
```

### Performing a Currency Conversion:

Based on what we have seen in the previous example, converting one currency to another in Java is as simple as:

```java
package org.json.poc;

// necessary components are imported
import java.io.IOException;

import org.apache.http.HttpEntity;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.json.JSONObject;

public class ConvertResponseDemo{
	public static final String ACCESS_KEY = "YOUR_ACCESS_KEY";
	public static final String BASE_URL = "https://apilayer.net/api/";
	public static final String ENDPOINT = "convert";

	// this object is used for executing requests to the (REST) API
	static CloseableHttpClient httpClient = HttpClients.createDefault();
	
	public static void sendConvertRequest(){

		// the "from", "to" and "amount" can be set as variables
		HttpGet get = new HttpGet(BASE_URL + ENDPOINT + "?access_key=" + ACCESS_KEY + "&from=GBP&to=INR&amount=2");
		try {
			CloseableHttpResponse response =  httpClient.execute(get);
			HttpEntity entity = response.getEntity();
			JSONObject jsonObject = new JSONObject(EntityUtils.toString(entity));
			
			System.out.println("Single-Currency Conversion");

			// access the parsed JSON objects
			System.out.println("From : " + jsonObject.getJSONObject("query").getString("from"));
			System.out.println("To : " + jsonObject.getJSONObject("query").getString("to"));
			System.out.println("Amount : " + jsonObject.getJSONObject("query").getLong("amount"));
			System.out.println("Conversion Result : " + jsonObject.getDouble("result"));
			System.out.println("\n");
			response.close();
		} catch (ClientProtocolException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ParseException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (JSONException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	
	}

	public static void main(String[] args) throws IOException{
		sendConvertRequest();
		httpClient.close();
	}
}
```

The **Java Output** will look like this:

```
Single-Currency Conversion
From : GBP
To : INR
Amount : 2
Conversion Result : 192.761348
```




### Troubleshooting

#### Import of `components` failed

If there are errors importing the `components` (contained in the **lib** directory), the paths are most likely broken. To fix this, find the `.classpath` file and add the following code just before the `</classpath>` closing tags:

```xml
<classpathentry kind="lib" path="lib/httpclient-4.4.1.jar"/> 
<classpathentry kind="lib" path="lib/httpcore-4.4.1.jar"/> 
<classpathentry kind="lib" path="lib/commons-logging-1.2.jar"/> 
<classpathentry kind="lib" path="lib/json.jar"/>
```

#### SSL Error in in JavaSe-1.7 

If you are using in `JavaSe-1.7`, the fact that SSL is disabled by default may cause some unexpected errors. To fix this, find your .java file's `VM Arguments` section and add the following code:

```
-Djsse.enableSNIExtension=false
```

Access `VM Arguments` section in **Eclipse**:

1. Right-click on the .java file
2. Click Run As
3. Click Run Configurations
4. Click Arguments 
5. Add the above code to VM Arguments
6. then click Run


