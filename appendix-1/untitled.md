# Permission Management Service HTTP APIs  Part1

![](../.gitbook/assets/0%20%283%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:center">
        <p><b>System Architecture</b>
        </p>
        <p>Appendix</p>
        <p>Permission Management Service HTTP APIs</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">Part 1</td>
    </tr>
  </tbody>
</table>

| Document ID | uid-sys-arch |
| :--- | :--- |
| Version | 1.0 |
| Status | Release |

## Appendix

### API Documentation

The appendix contains reference information about UniquID’s API.

### Permission Management Service HTTP APIs

#### API Overview

The described API operated with structures that always in JSON format.

All responses have Content-Type: application/json

| **METHOD** | **URI** | **DESCRIPTION** |
| :--- | :--- | :--- |
| GET | /api/v1/devices | Get all managed devices |
| GET | /api/v1/devices/:xpub | Get info about specific device |
| GET | /api/v1/contracts | Get list of the access contracts |
| POST | /api/v1/contracts | Create new access contracts |
| DELETE | /api/v1/contracts/:txid | Revoke existing access contracts |
| GET | /api/v1/info | Returns information about orchestrator |
| GET | /api/v1/properties | Get orchestrator properties |
| POST | /api/v1/properties | Upload data and saves the file on disk |
| POST | /api/v1/init | Init orchestrator |

#### Detailed API description

### Get all managed devices

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\)

The API returns all managed devices. User must be logged on. 

* If success - API returns a JSON array of all the orchestrated devices in the response body.  
* If no device found – then returns http code 404 Not found, with error description in the body. 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET | /api/v1/devices | Cookie: JSESSIONID=298zf09hf012fh2;  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">[
        <br />{
        <br />&quot;name&quot;: &quot;device_name_1&quot;,
        <br />&quot;xpub&quot;: &quot;public_key_1&quot;,
        <br />&quot;recipe&quot;: &quot;test&quot;,
        <br />&quot;contracts&quot;: 1,
        <br />&quot;shared&quot;: false
        <br />}, {
        <br />&quot;name&quot;: &quot;device_name_2&quot;,
        <br />&quot;xpub&quot;: &quot;public_key_2&quot;,
        <br />&quot;recipe&quot;: &quot;test&quot;,
        <br />&quot;contracts&quot;: 1,
        <br />&quot;shared&quot;: false
        <br />},
        <br />...
        <br />]</td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the device
          <br />xpub &#x2013; public key of the device
          <br />recipe &#x2013; is always &quot;test&quot;
          <br />contracts &#x2013; number of contracts involving this device, are always
          1
          <br />shared &#x2013; flag indicates if this is Shared Device; provided for
          future evolution. Always false in this version</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not Found</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;No devices found&quot;
        <br />}</td>
      <td style="text-align:left">
        <p>Error! Orchestrator have no devices</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;
        <br
        />}</td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is absent, incorrect or expired.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">{
        <br />stack: [
        <br />{
        <br />&quot;error&quot;: &quot;some java exception&quot;
        <br />&quot;trace&quot;: [
        <br />&quot;com.uniquid.servlet.....
        <br />&quot;com.uniquid.dao.....
        <br />....
        <br />]
        <br />},
        <br />{ &#x2026;. },
        <br />&#x2026;.
        <br />]
        <br />}</td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions
          <br />error &#x2013; text message, describes the error reason
          <br />trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Get info about specific device

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API returns information about the specific device.. To use this API – User must be logged on. 

* If success – API returns a JSON array containing just one item in the response body.   
* If there is no device with given xpub – then return http code 404 Not found with error description 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body. 
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET | /api/v1/devices/:xpub  | Cookie: JSESSIONID=298zf09hf012fh2;  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">{
        <br />&quot;name&quot;:&quot; dev_name_1&quot;,
        <br />&quot;xpub&quot;: &quot;pub_key_1&quot;,
        <br />&quot;recipe&quot;: &quot;test&quot;,
        <br />&quot;contracts&quot;: 1,
        <br />&quot;shared&quot;: false
        <br />}</td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the device
          <br />xpub &#x2013; public key of the device
          <br />recipe - is always &quot;test&quot;
          <br />contracts &#x2013; number of contracts involving this device. Is always
          1
          <br />shared &#x2013; flag indicates if this is Shared Device; provided for
          future evolution. Always false in this version</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;Device with xpub: &apos;af3J42if4828c2&apos;
        not found&quot;
        <br />}</td>
      <td style="text-align:left">
        <p>Error! Orchestrator have no device with given xpub</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;
        <br
        />}</td>
      <td style="text-align:left">Error! The JSESSIONID is absent, incorrect or expired.
        <br /><em><br /></em>error &#x2013; text message, describes the error reason</td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">{
        <br />stack: [
        <br />{
        <br />&quot;error&quot;: &quot;some java exception&quot;
        <br />&quot;trace&quot;: [
        <br />&quot;com.uniquid.servlet.....
        <br />&quot;com.uniquid.dao.....
        <br />....
        <br />]
        <br />},
        <br />{ &#x2026;. },
        <br />&#x2026;.
        <br />]
        <br />}</td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions
          <br />error &#x2013; text message, describes the error reason
          <br />trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Get list of access contracts

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API returns a list of all the Access Contracts of current tenant. To use this API – User must be logged on. 

* If success – API returns a JSON array containing all the access contracts in the response body.  
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/contracts  | Cookie: JSESSIONID=298zf09hf012fh2;  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">[
        <br />{
        <br />&quot;provider&quot;:
        <br />{
        <br />&quot;name&quot;:&quot;device_name_1&quot;,
        <br />&quot;xpub&quot;:&quot;device_xpub&quot;,
        <br />&quot;address&quot;:&quot;device_address_1&quot;
        <br />},
        <br />&quot;user&quot;:
        <br />{
        <br />&quot;name&quot;:&quot;device_name_2&quot;,
        <br />&quot;xpub&quot;:&quot;device_xpub_2&quot;,
        <br />&quot;address&quot;:&quot;device_address_2&quot;
        <br />},
        <br />&quot;revoker&quot;:
        <br />{
        <br />&quot;name&quot;:&quot;device_name_3&quot;,
        <br />&quot;xpub&quot;:&quot;device_xpub_3&quot;,
        <br />&quot;address&quot;:&quot;device_address_3&quot;
        <br />},
        <br />&quot;txid&quot;:&quot;txid&quot;,
        <br />&quot;recipe&quot;:&quot;Access&quot;,
        <br />&quot;status&quot;:&quot;ACTIVE&quot;,
        <br />&quot;timestamp&quot;:123456789,
        <br />&quot;functions&quot;: [34,35],
        <br />&quot;shared&quot;: false
        <br />},
        <br />&#x2026;
        <br />]</td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>provider &#x2013; provider device information
          <br />user &#x2013; user device information
          <br />revoker &#x2013; revoker device information
          <br />txid &#x2013; unique identifier of the transaction
          <br />recipe &#x2013; is always &quot;Access&quot;
          <br />status &#x2013; status of the contract. It can be &apos;pending&apos;,
          &apos;active&apos; or &apos;revoked&apos;
          <br />timestamp &#x2013; creation time of the contract
          <br />functions &#x2013; functionality enabled by the &quot;provider&quot; to
          the &quot;user&quot;
          <br />shared &#x2013; flag indicates if this is Shared Contract; provided for
          future evolution. Always false in this version</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;
        <br
        />}</td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is absent, incorrect or expired.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">{
        <br />stack: [
        <br />{
        <br />&quot;error&quot;: &quot;some java exception&quot;
        <br />&quot;trace&quot;: [
        <br />&quot;com.uniquid.servlet.....
        <br />&quot;com.uniquid.dao.....
        <br />....
        <br />]
        <br />},
        <br />{ &#x2026;. },
        <br />&#x2026;.
        <br />]
        <br />}</td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions
          <br />error &#x2013; text message, describes the error reason
          <br />trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Create new Access Contract

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API creates a new Access Contract between devices provider and user. This API allows to create contract and if both devices belongs to current tenant. To use this API - User must be logged on. 

* If success – API returns http code 200 OK 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If request body is empty – API returns http code 400 Bad request, with error description.
* If provider does not exist in orchestrator database – API returns http code 404 Provider with given Xpub not found.
* If user does not exist in orchestrator database – API returns http code 404 User with given Xpub not found.
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>METHOD</b>
      </th>
      <th style="text-align:left"><b>REQUEST</b>
      </th>
      <th style="text-align:left"><b>BODY/HEADERS</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">POST</td>
      <td style="text-align:left">/api/v1/contracts</td>
      <td style="text-align:left">
        <p>{
          <br />&quot;provider&quot;:&quot;device_xpub&quot;,
          <br />&quot;user&quot;:&quot;device_xpub&quot;,
          <br />&quot;functions&quot;: [34,35]
          <br />}</p>
        <p>&quot;functions&quot; is optional. If not set, then the orchestrator will
          allow functions 33 to 37.</p>
        <p>Cookie: JSESSIONID=298zf09hf012fh2;</p>
      </td>
    </tr>
  </tbody>
</table>

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;
        <br
        />}</td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is absent, incorrect or expired.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad request</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;Body can&apos;t be empty&quot;
        <br />}</td>
      <td style="text-align:left">
        <p>Error! Client provide incorrect request.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;Provider with given Xpub not found&quot;
        <br />}</td>
      <td style="text-align:left">
        <p>Error! There is no provider with given Xpub in database of Orchestrator</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">{
        <br />&quot;error&quot;: &quot;User with given Xpub not found&quot;
        <br />}</td>
      <td style="text-align:left">
        <p>Error! There is no user with given Xpub in database of Orchestrator</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">{
        <br />stack: [
        <br />{
        <br />&quot;error&quot;: &quot;some java exception&quot;
        <br />&quot;trace&quot;: [
        <br />&quot;com.uniquid.servlet.....
        <br />&quot;com.uniquid.dao.....
        <br />....
        <br />]
        <br />},
        <br />{ &#x2026;. },
        <br />&#x2026;.
        <br />]
        <br />}</td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions
          <br />error &#x2013; text message, describes the error reason
          <br />trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Delete an Access Contract

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API deletes existing Access Contract. To use this API – User must be logged on. 

* If success – API returns http code 200 OK 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body. 
* If contract with given txid not exist – then return http code 404 Not found  
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| DELETE  | /api/v1/contracts/:txid  | Cookie: JSESSIONID=298zf09hf012fh2;  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is absent, incorrect or expired.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Contract not found&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Resource not found.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>stack: [</p>
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>&quot;trace&quot;: [</p>
        <p>&quot;com.uniquid.servlet.....</p>
        <p>&quot;com.uniquid.dao.....</p>
        <p>....</p>
        <p>]</p>
        <p>},</p>
        <p>{ &#x2026;. },</p>
        <p>&#x2026;.</p>
        <p>]</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions</p>
        <p>error &#x2013; text message, describes the error reason</p>
        <p>trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Get Orchestrator info

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API returns the information about the current Orchestrator. User must be logged on. 

* If success – API returns the information about the current Orchestrator in the content body and http code 200 OK 
* If the request has extra path – method returns http code 400 Bad request with "path not implemented" in the content body. 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/info  | Cookie: JSESSIONID=298zf09hf012fh2;  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;name&quot;:&quot;OrchestratorNJI95H28R4TA&quot;,</p>
        <p>&quot;publicKey&quot;: &quot;tpubDBYT1CEYNPzH9AveZd55....&quot;,</p>
        <p>&quot;providerBalance&quot;:0,</p>
        <p>&quot;userBalance&quot;:0,</p>
        <p>&quot;topupAddress&quot;:&quot;mkZusM4szmV7pkWYDvdVTHk28Jm...&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the Orchestrator</p>
        <p>publicKey &#x2013; Orchestrator&#x2019;s public key</p>
        <p>providerBalance &#x2013; coin balance of the &quot;provider&quot; bip32
          hierarchy</p>
        <p>userBalance &#x2013; coin balance of the &quot;user&quot; bip32 hierarchy</p>
        <p>topupAddress &#x2013; address to use to recharge the Orchestrator</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 bad request</td>
      <td style="text-align:left">Path not implemented</td>
      <td style="text-align:left">Error! Extra path information not permitted</td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is absent, incorrect or expired.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>stack: [</p>
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>&quot;trace&quot;: [</p>
        <p>&quot;com.uniquid.servlet.....</p>
        <p>&quot;com.uniquid.dao.....</p>
        <p>....</p>
        <p>]</p>
        <p>},</p>
        <p>{ &#x2026;. },</p>
        <p>&#x2026;.</p>
        <p>]</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions</p>
        <p>error &#x2013; text message, describes the error reason</p>
        <p>trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Get Orchestrator properties

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API allows to list the properties of the current Orchestrator. User must be logged on. 

* If success – API returns the properties of the current Orchestrator in the content body and http code 200 OK 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/properties  | Cookie: JSESSIONID=298zf09hf012fh2;  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>REQUEST</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;network&quot;:&quot;&quot;,</p>
        <p>&quot;mqttTopic&quot;:&quot;&quot;,</p>
        <p>&quot;mqttBroker&quot;:&quot;&quot;,</p>
        <p>&quot;httpPort&quot;:&quot;&quot;,</p>
        <p>&quot;insightApiUrl&quot;:&quot;&quot;,</p>
        <p>&quot;registryUrl&quot;:&quot;&quot;,</p>
        <p>&quot;imprinterUrl&quot;:&quot;&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>network &#x2013; blockchain network</p>
        <p>mqttTopic &#x2013; listening topic</p>
        <p>mqttBroker &#x2013; broker for MQTT communication</p>
        <p>insightApiUrl &#x2013; blockchain API service</p>
        <p>registryUrl &#x2013; URL of the registry service</p>
        <p>imprinterUrl &#x2013; URL of the imprinter service</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is absent, incorrect or expired.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>stack: [</p>
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>&quot;trace&quot;: [</p>
        <p>&quot;com.uniquid.servlet.....</p>
        <p>&quot;com.uniquid.dao.....</p>
        <p>....</p>
        <p>]</p>
        <p>},</p>
        <p>{ &#x2026;. },</p>
        <p>&#x2026;.</p>
        <p>]</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions</p>
        <p>error &#x2013; text message, describes the error reason</p>
        <p>trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Update Orchestrator properties

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API allows to upload a settings file to the current Orchestrator. User must be logged on. 

* If success – API returns the information about the current Orchestrator in the content body and http code 200 OK 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If request body is empty – API returns http code 400 Bad request, with error description 
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>METHOD</b>
      </th>
      <th style="text-align:left"><b>REQUEST</b>
      </th>
      <th style="text-align:left"><b>BODY/HEADERS</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">POST</td>
      <td style="text-align:left">/api/v1/properties</td>
      <td style="text-align:left">
        <p>File to upload</p>
        <p>Cookie: JSESSIONID=298zf09hf012fh2;</p>
      </td>
    </tr>
  </tbody>
</table>

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is absent, incorrect or expired.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Body can&apos;t be empty&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Client provide incorrect request.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>stack: [</p>
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>&quot;trace&quot;: [</p>
        <p>&quot;com.uniquid.servlet.....</p>
        <p>&quot;com.uniquid.dao.....</p>
        <p>....</p>
        <p>]</p>
        <p>},</p>
        <p>{ &#x2026;. },</p>
        <p>&#x2026;.</p>
        <p>]</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions</p>
        <p>error &#x2013; text message, describes the error reason</p>
        <p>trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

### Init Orchestrator

No special authentication requirements for this API call 

The API initialize orchestrator \(set organization\). This organization needs to start working orchestrator and use to communicate between another components of the system. The Init method can be called only once at the beginning. All other APIs couldn't be called before the Init call. 

* If success – API returns https code 200 OK. 
* If request body is empty – API returns http code 400 Bad request, with error description 
* If any other exception throws during the init orchestrator process – then return http code 500 Internal server error with the reason description of error. 

Request:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>METHOD</b>
      </th>
      <th style="text-align:left"><b>REQUEST</b>
      </th>
      <th style="text-align:left"><b>BODY/HEADERS</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">POST</td>
      <td style="text-align:left">/api/v1/init</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;orgId&quot;: &quot;pepsico&quot;</p>
        <p>}</p>
      </td>
    </tr>
  </tbody>
</table>

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>HTTP CODE</b>
      </th>
      <th style="text-align:left"><b>BODY</b>
      </th>
      <th style="text-align:left"><b>MEANING</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">403 Forbidden</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Settings already initialized&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The Orchestrator already initialized. The second call of Init not
          allowed.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Body can&apos;t be empty&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Client provide incorrect request.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>stack: [</p>
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>&quot;trace&quot;: [</p>
        <p>&quot;com.uniquid.servlet.....</p>
        <p>&quot;com.uniquid.dao.....</p>
        <p>....</p>
        <p>]</p>
        <p>},</p>
        <p>{ &#x2026;. },</p>
        <p>&#x2026;.</p>
        <p>]</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Unexpected error. Must not appear in release version.</p>
        <p>stack &#x2013; array of cascade of exceptions</p>
        <p>error &#x2013; text message, describes the error reason</p>
        <p>trace &#x2013; the array of strings contains java trace for error.</p>
      </td>
    </tr>
  </tbody>
</table>

