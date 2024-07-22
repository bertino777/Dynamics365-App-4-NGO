# JavaScript codes

## Entity records count
This code helps the agency to easily count number of records under Account and Contact tables by clicking on a button from inside the CRM. When number of records of a table exceed 5000 counting it brings some challenges.

| - | - |
|---|---|
| ![](/images/rec_count1.png) | ![](/images/rec_count2.png)|
| ![](/images/rec_count3.png) | ![](/images/rec_count4.png)|
```
function entityRecordCount() {
    // getGlobalContext() method provides access to the global context
    var globalContext = Xrm.Utility.getGlobalContext();

    // getClientUrl() method returns the base URL that was used to access the application
    var serverURL = globalContext.getClientUrl();

    // Count total records under account entity
    var query = "RetrieveTotalRecordCount(EntityNames=['account'])";

    // XMLHttpRequest is a JavaScript class containing methods to asynchronously transmit HTTP requests from a web browser to a web server.
    // Constructor initializes an XMLHttpRequest
    var req = new XMLHttpRequest ();

    // open() method will prepare the HTTP request to be sent, it takes three arguments: 
       //- The HTTP method you want to use
       //- The URL where you will send that request
       //- A boolean value, true for asynchronous request
    req.open("GET", serverURL + "/api/data/v9.2/" + query, true);

    // setRequestHeader() method sets the value of an HTTP request header
    req.setRequestHeader("Accept", "application/json");
    req.setRequestHeader("Content-Type", "application/json; charset=utf-8");
    req.setRequestHeader("OData-MaxVersion", "4.0");
    req.setRequestHeader("OData-Version", "4.0");

    // onreadystatechange defines a function to be called when the readyState property changes
    req.onreadystatechange = function () {
        // readyState hold the status of the XMLHttpRequest
           // 0: request is not initialized
           // 1: Server connection established
           // 2: Request received
           // 3: Processing request
           // 4: Request finished and response is ready

       if (this.readyState == 4 && this.status == 200)
       {
            // The JSON.parse() method parses a JSON string
            result = JSON.parse(this.response);
            alert(result['EntityRecordCountCollection']['Values'])

       }
    };

    // send() method is used to send the request
    req.send();

}
```
