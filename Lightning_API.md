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