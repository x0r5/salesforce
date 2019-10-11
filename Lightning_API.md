# Salesforce Data APIs
to manipulate Salesforce data
- REST API
- SOAP API
- BULK API
- Streaming API

## REST API
consists of
- resource URI (Uniform Resource Identifier)
- HTTP method (HEAD, GET, POST, PATCH, DELETE)
- request headers
- request body


## SOAP API
- WSDL file: **Web Services Description Language**
    - Partner WSDL: use with many Salesforce orgs
    - Enterprise WSDL: use with a single Salesforce org
        - reflects the org`s specific config
        - regenerate on metadata changes !

- WSC: **Web Services Connector**

### SOAP
#### endpoint URI (Uniform Resource Identifier)
`login.salesforce.com/services/Soap/c/46.0/<package version number>`
- /c -> enterprise wsl
- /u -> partner wsdl

#### SOAP Message
Login:
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="urn:enterprise.soap.sforce.com">
   <soapenv:Header>
      <urn:LoginScopeHeader>
         <urn:organizationId>?</urn:organizationId>
         <!--Optional:-->
         <urn:portalId>?</urn:portalId>
      </urn:LoginScopeHeader>
   </soapenv:Header>
   <soapenv:Body>
      <urn:login>
         <urn:username>?</urn:username>
         <urn:password>?</urn:password>
      </urn:login>
   </soapenv:Body>
</soapenv:Envelope>
```

Create:

URI: `https://<mysalesforcedomain>.my.salesforce.com/services/Soap/c/46.0`
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="urn:enterprise.soap.sforce.com" xmlns:urn1="urn:sobject.enterprise.soap.sforce.com">
   <soapenv:Header>
      <urn:SessionHeader>
         <urn:sessionId>?</urn:sessionId>
      </urn:SessionHeader>
   </soapenv:Header>
   <soapenv:Body>
      <urn:create>
         <!--Zero or more repetitions:-->
       	 <urn:sObjects xsi:type="urn1:Account" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
       	 <Name>Sample Soap Account Name</Name>
            <!--Zero or more repetitions:-->
         </urn:sObjects>
      </urn:create>
   </soapenv:Body>
</soapenv:Envelope>
```



## Apex Callouts
URI: endpoint

### REST Callouts
HTTP methods
- GET
- PUT
- POST
- DELETE

#### example HTTP request in APEX
```java
Http http = new Http();
HttpRequest request = new HttpRequest();
request.setEndpoint('https://th-apex-http-callout.herokuapp.com/animals');
request.setMethod('GET');
HttpResponse response = http.send(request);
// If the request is successful, parse the JSON response.
if (response.getStatusCode() == 200) {
    // Deserialize the JSON string into collections of primitive data types.
    Map<String, Object> results = (Map<String, Object>) JSON.deserializeUntyped(response.getBody());
    // Cast the values in the 'animals' key as a list
    List<Object> animals = (List<Object>) results.get('animals');
    System.debug('Received the following animals:');
    for (Object animal: animals) {
        System.debug(animal);
    }
}
```
JSON data:
```json
{
    "animals":["majestic badger","fluffy bunny","scary bear","chicken"]
}
```

>Hint: For complex json files use [JSON to APEX](https://json2apex.herokuapp.com/)

#### Send data to the server
> respons code `201` means success, any other than that sent to the debug log

```java
Http http = new Http();
HttpRequest request = new HttpRequest();
request.setEndpoint('https://th-apex-http-callout.herokuapp.com/animals');
request.setMethod('POST');
request.setHeader('Content-Type', 'application/json;charset=UTF-8');
// Set the body as a JSON object
request.setBody('{"name":"mighty moose"}');
HttpResponse response = http.send(request);
// Parse the JSON response
if (response.getStatusCode() != 201) {
    System.debug('The status code returned was not expected: ' +
        response.getStatusCode() + ' ' + response.getStatus());
} else {
    System.debug(response.getBody());
}
```