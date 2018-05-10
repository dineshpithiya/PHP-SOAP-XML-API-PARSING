## SOAP – Simple Object Access Protocol – is probably the better known of the two models.
SOAP relies heavily on XML, and together with schemas, defines a very strongly typed messaging framework.

###### The server-side portion of the web API is a programmatic interface to a defined request-response message system, and is typically referred to as the Web Service. There are several design models for web services, but the two most dominant are SOAP and REST.

> SOAP provides the following advantages when compared to REST:

1. Language, platform, and transport independent (REST requires use of HTTP)     
2. Works well in distributed enterprise environments (REST assumes direct point-to-point communication)
3. Standardized
4. Provides significant pre-build extensibility in the form of the WS* standards
5. Built-in error handling
6. Automation when used with certain language products

> Example

```
$soap_request='<soapenv:Envelope 
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
xmlns="http://clients.mindbodyonline.com/api/0_5">
  <soapenv:Header/>
    <soapenv:Body>
     <GetClients>
         <Request>
            <SourceCredentials>
               <SourceName>xyz</SourceName>
               <Password>xyz</Password>
               <SiteIDs>
                  <int>-91</int>
               </SiteIDs>
            </SourceCredentials>
            <UserCredentials>
               <Username>xyz</Username>
               <Password>xyz</Password>
               <SiteIDs>
                  <int>-91</int>
               </SiteIDs>
               <LocationID>0</LocationID>
            </UserCredentials>
            <XMLDetail>Full</XMLDetail>
            <PageSize>1500</PageSize>
            <CurrentPageIndex>0</CurrentPageIndex>
            <SearchText></SearchText>
         </Request>
      </GetClients>
   </soapenv:Body>
</soapenv:Envelope>';
```

> PHP call SOAP API

```
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => "https://api.mindbodyonline.com/0_5/ClientService.asmx",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => $soap_request,
  CURLOPT_HTTPHEADER => array(
    "cache-control: no-cache",
    "charset: UTF-8",
    "content-type: text/xml",
    "postman-token: b0e9cc6d-9975-4daa-03a9-620d87a39370",
    "SOAPAction: http://clients.mindbodyonline.com/api/0_5/GetClients",
    "user-agent: Apache-HttpClient/4.1.1"
  ),
));

$response = curl_exec($curl);
$client=XmlToArray($response);
print_r($client);
```

> Parse XML to PHP array
```
function XmlToArray($result)
{
    $response = preg_replace("/(<\/?)(\w+):([^>]*>)/", "$1$2$3", $result);
    $xml = new SimpleXMLElement($response);
    $body = $xml->xpath('//soapBody')[0];
    return json_decode(json_encode((array)$body), TRUE); 
}
```