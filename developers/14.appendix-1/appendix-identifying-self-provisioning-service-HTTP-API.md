# Identity Self-Provisioning Service HTTP API

![](../.gitbook/assets/0%20%282%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:center">
        <p><b>System Architecture</b>
        </p>
        <p>Appendix</p>
        <p>Identity Self-Provisioning Service HTTP APIs</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

| Document ID | uid-sys-arch |
| :--- | :--- |
| Version | 1.0 |
| Status | Release |

## Appendix

### API Documentation

The appendix contains reference information about UniquID’s API.

### Identity Self-Provisioning Service HTTP APIs

#### API Overview

The described API operated with structures that always in JSON format.

All responses have Content-Type: application/json

| **METHOD TYPE** | **URI** | **DESCRIPTION** |
| :--- | :--- | :--- |
| GET | /api/v1/devices | Get list of managed devices |
| GET | /api/v1/devices/:status | Get list of managed devices filtered by status |
| GET | /api/v1/enrollment | Get list of the orchestration contracts |
| GET | /api/v1/enrollment/:providerAddress | Get the orchestration contract for the specified Provider Address |
| POST | /api/v1/enrollment | Create a new orchestration contract |
| GET | /api/v1/orchestrators | Get list of Orchestrators |
| GET | /api/v1/orchestrators/:xpub | Get info about specific Orchestrator |
| POST | /api/v1/orchestrators | Create a new Orchestrator |
| DELETE | /api/v1/orchestrators/:xpub | Delete Orchestrator |
| GET | /api/v1/info | Get information about imprinter |
| GET | /api/v1/properties | Get settings information |
| POST | /api/v1/properties | Upload data and saves the file on disk |

#### Detailed APIs description

### Get managed devices

The method returns all the devices managed by the current Imprinter.

* If success - method returns a json array of all the devices in the response body.
* If an exception throws during the process – then return http code 500 internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/devices |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>[</p>
        <p>{</p>
        <p>&quot;name&quot;: &quot;device1&quot;,</p>
        <p>&quot;xpub&quot;: &quot;xpub_device_1&quot;,</p>
        <p>&quot;creationTime&quot;: 1234567890,</p>
        <p>&quot;recipe&quot;: &quot;test&quot;,</p>
        <p>&quot;retries&quot;: 0,</p>
        <p>&quot;status&quot;: &quot;ORCHESTRATED&quot;</p>
        <p>},</p>
        <p>{</p>
        <p>&quot;name&quot;: &quot;device2&quot;,</p>
        <p>&quot;xpub&quot;: &quot;xpub_device_2&quot;,</p>
        <p>&quot;creationTime&quot;: 1234567890,</p>
        <p>&quot;recipe&quot;: &quot;test&quot;,</p>
        <p>&quot;retries&quot;: 0,</p>
        <p>&quot;status&quot;: &quot;IMPRINTED&quot;</p>
        <p>}</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the device</p>
        <p>xpub &#x2013; public key of the device</p>
        <p>creationTime &#x2013; timestamp of device creation</p>
        <p>recipe &#x2013; is always &quot;test&quot;</p>
        <p>retries &#x2013; attempts to change status from *ING to *ED</p>
        <p>status &#x2013; can be CREATED, IMPRINTING, IMPRINTED, ORCHESTRATING,
          ORCHESTRATED</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Get managed devices filtered by status

The method returns all the devices managed by the current Imprinter filtered by status.

* If success - method returns in the response body a json array of all the devices with the specified status.
* If an exception throws during the process – then return http code 500 internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/devices/:status |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>[</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;device_name_1&quot;,</p>
        <p>&quot;xpub&quot;:&quot;public_key_1&quot;,</p>
        <p>&quot;creationTime&quot;:1234567890,</p>
        <p>&quot;recipe&quot;:&quot;test&quot;</p>
        <p>},</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;device_name_2&quot;,</p>
        <p>&quot;xpub&quot;:&quot;public_key_2&quot;,</p>
        <p>&quot;creationTime&quot;:1234567890,</p>
        <p>&quot;recipe&quot;:&quot;test&quot;</p>
        <p>}</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p><em>name</em> &#x2013; name of the device</p>
        <p><em>xpub </em>&#x2013; public key of the device</p>
        <p><em>creationTime</em> &#x2013; timestamp of device creation</p>
        <p><em>recipe </em>&#x2013; is always &quot;test&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p><em>error </em>&#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Get enrollment \(orchestration\) contracts

The method returns all the orchestration contracts created by the current Imprinter.

* If success - method returns in the response body a json array of all the devices with the specified status.
* If an exception throws during the process – then return http code 500 internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/enrollment |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>[</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;device_name_1&quot;,</p>
        <p>&quot;providerAddress&quot;:&quot;provider_address_1&quot;,</p>
        <p>&quot;userAddress&quot;:&quot;user_address_1&quot;,</p>
        <p>&quot;txid&quot;:&quot;transaction_id&quot;,</p>
        <p>&quot;orchestratorName&quot;:&quot;orchestrator_name_1&quot;,</p>
        <p>&quot;xpub&quot;:&quot;imprinter_public_key&quot;</p>
        <p>},</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;device_name_2&quot;,</p>
        <p>&quot;providerAddress&quot;:&quot;provider_address_2&quot;,</p>
        <p>&quot;userAddress&quot;:&quot;user_address_2&quot;,</p>
        <p>&quot;txid&quot;:&quot;transaction_id&quot;,</p>
        <p>&quot;orchestratorName&quot;:&quot;orchestrator_name_1&quot;,</p>
        <p>&quot;xpub&quot;:&quot;imprinter_public_key&quot;</p>
        <p>}</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the device</p>
        <p>providerAddress &#x2013; bip32 address of the provider</p>
        <p>userAddress &#x2013; bip32 address of the provider</p>
        <p>txid &#x2013; unique identifier of the transaction</p>
        <p>orchestratorName &#x2013; name of the orchestrator that manage the device</p>
        <p>xpub &#x2013; public key of the device</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Get enrollment \(orchestration\) contract for a specific provider

The method returns orchestration contracts created by the current Imprinter for the specified device address.

* If success - method returns in the response body a json array of all the devices with the specified provider address.
* If given providerAddress doesn't exists returns http code 404 Not Found.
* If an exception throws during the process – then return http code 500 internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/enrollment/:providerAddress |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>[</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;device_name_1&quot;,</p>
        <p>&quot;providerAddress&quot;:&quot;provider_address&quot;,</p>
        <p>&quot;userAddress&quot;:&quot;user_address_1&quot;,</p>
        <p>&quot;txid&quot;:&quot;transaction_id&quot;,</p>
        <p>&quot;orchestratorName&quot;:&quot;orchestrator_name_1&quot;,</p>
        <p>&quot;xpub&quot;:&quot;imprinter_public_key&quot;</p>
        <p>}</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the device</p>
        <p>providerAddress &#x2013; bip32 address of the provider</p>
        <p>userAddress &#x2013; bip32 address of the provider</p>
        <p>txid &#x2013; unique identifier of the transaction</p>
        <p>orchestratorName &#x2013; name of the orchestrator that manage the device</p>
        <p>xpub &#x2013; public key of the device</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not Found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Can&apos;t find contract with address&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Given providerAddress doesn&apos;t exists</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Create an enrollment \(orchestration\) contract

The method creates a new orchestration contract between an orchestrator and a device.

* If success - method returns http code 200 OK.
* If request body is empty – API returns http code 400 Bad request, with error description
* If an exception throws during the creation process – then return http code 500 internal server error with the error description in the response body.

Request:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>
      </th>
      <th style="text-align:left"><b>Request</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">POST</td>
      <td style="text-align:left">/api/v1/enrollment</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;orchestrator&quot;:&quot;xpub_orchestrator&quot;,</p>
        <p>&quot;device&quot;:&quot;xpub_device&quot;</p>
        <p>}</p>
      </td>
    </tr>
  </tbody>
</table>

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!<b> </b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad Request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### View orchestrators list

The method returns the list of all the orchestrators created by the current Imprinter.

* If success - method returns in the response body a json array of all the orchestrators.
* If an exception throws during the process – then return http code 500 internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/orchestrators |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>[</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;orchestrator_name_1&quot;,</p>
        <p>&quot;xpub&quot;:&quot;orchestrator_1_public_key&quot;,</p>
        <p>&quot;creationTime&quot;:1234567890</p>
        <p>},</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;orchestrator_name_2&quot;,</p>
        <p>&quot;xpub&quot;:&quot;orchestrator_2_public_key&quot;,</p>
        <p>&quot;creationTime&quot;:1234567890</p>
        <p>}</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the orchestrator</p>
        <p>xpub &#x2013; public key of the orchestrator</p>
        <p>creationTime &#x2013; timestamp of orchestrator creation</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### View orchestrator information

The method returns info about a specific orchestrator.

* If success - method returns in the response body a json array with the orchestrator information.
* If an exception throws during the process – then return http code 500 internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/orchestrators/:xpub |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>[</p>
        <p>{</p>
        <p>&quot;name&quot;:&quot;orchestrator_name_1&quot;,</p>
        <p>&quot;xpub&quot;:&quot;orchestrator_1_public_key&quot;,</p>
        <p>&quot;creationTime&quot;:1234567890</p>
        <p>}</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the orchestrator</p>
        <p>xpub &#x2013; public key of the orchestrator</p>
        <p>creationTime &#x2013; timestamp of orchestrator creation</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not Found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Can&apos;t find orchestrator with xpub&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Given xpub doesn&apos;t exists</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Create a new orchestrator

The method creates a new orchestrator inside the current imprinter.

* If success - method returns http code 201 created.
* If an exception throws during the process – then return http code 400 bad request with the error description in the response body.
* If the xpub already exist – then return http code 409 Conflict with the error description in the response body.
* If any other exception throws during the creation process – then return http code 500 internal server error with the error description in the response body.

Request:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>
      </th>
      <th style="text-align:left"><b>Request</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">POST</td>
      <td style="text-align:left">/api/v1/orchestrators</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;name&quot;:&quot;orchestrator_name&quot;,</p>
        <p>&quot;xpub&quot;:&quot;orchestrator_xpub&quot;</p>
        <p>}</p>
      </td>
    </tr>
  </tbody>
</table>

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">201 OK</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;name&quot;: &quot;orchestrator_name_4&quot;,</p>
        <p>&quot;xpub&quot;: &quot;xpub4&quot;,</p>
        <p>&quot;creationTime&quot;: 1534766128988</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the orchestrator</p>
        <p>xpub &#x2013; public key of the orchestrator</p>
        <p>creationTime &#x2013; timestamp of orchestrator creation</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad Request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">409 Conflict</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Resource already exists&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Delete an orchestrator

The method deletes orchestrator by given xpub.

* If success - method returns http code 410 gone.
* If the xpub doesn’t exist, then return http code 404 Not found error with the error description in the response body.
* If any other exception throws during the creation process – then return http code 500 internal server error with the error description in the response body.

Request:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>
      </th>
      <th style="text-align:left"><b>Request</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">DELETE</td>
      <td style="text-align:left">/api/v1/orchestrators/:xpub</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;name&quot;:&quot;orchestrator_name&quot;,</p>
        <p>&quot;xpub&quot;:&quot;orchestrator_xpub&quot;</p>
        <p>}</p>
      </td>
    </tr>
  </tbody>
</table>

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">410 Gone</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not Found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Resource not found&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Get imprinter info

The method returns the information about the current Imprinter.

* If success - method returns the information about the current Orchestrator in the content body and http code 200 OK
* If the request has extra path – method returns http code 400 Bad request with "path not implemented" in the content body.
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/info |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;name&quot;: &quot;ImprinterTest&quot;,</p>
        <p>&quot;xpub&quot;: &quot;tpubDBYesqrK7QKqb5zD2RowX...&quot;,</p>
        <p>&quot;topupAddress&quot;:&quot;mrXWQAJi4Fk8Zt2MLgni...&quot;,</p>
        <p>&quot;userBalance&quot;: 1000000000,</p>
        <p>&quot;providerBalance&quot;: 0,</p>
        <p>&quot;devicesCreated&quot;: 0,</p>
        <p>&quot;devicesImprinted&quot;: 1,</p>
        <p>&quot;devicesImprinting&quot;: 0,</p>
        <p>&quot;devicesOrchestrated&quot;: 1,</p>
        <p>&quot;devicesOrchestrating&quot;: 0</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>name &#x2013; name of the Imprinter</p>
        <p>xpub &#x2013; public key of the imprinter</p>
        <p>topupAddress &#x2013; address to use to recharge the Orchestrator</p>
        <p>userBalance &#x2013; coin balance of the &quot;user&quot; bip32 hierarchy</p>
        <p>providerBalance &#x2013; coin balance of the &quot;provider&quot; bip32
          hierarchy</p>
        <p>devicesCreated &#x2013; number of devices in Created status</p>
        <p>devicesImprinted &#x2013; number of devices in Imprinted status</p>
        <p>devicesImprinting &#x2013; number of devices in Imprinting status</p>
        <p>devicesOrchestrated &#x2013; number of devices in Orchestrated status</p>
        <p>devicesOrchestrating &#x2013; number of devices in Orchestrating status</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad Request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Path not implemented&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### View imprinter properties

The method allows to view the properties of the current Imprinter.

* If success - method returns the properties of the current Imprinter in the content body and http code 200 OK
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| GET | /api/v1/properties |  |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200 OK</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;network&quot;: &quot;com.uniquid.regtest&quot;,</p>
        <p>&quot;mqttBroker&quot;: &quot;tcp://ipaddress:port&quot;,</p>
        <p>&quot;mqttTopic&quot;: &quot;ImprinterTest&quot;,</p>
        <p>&quot;httpPort&quot;: 8090,</p>
        <p>&quot;insightApiUrl&quot;: &quot;http://ipaddress:port/insight-api&quot;,</p>
        <p>&quot;registryUrl&quot;: &quot;http://ipaddress:port&quot;,</p>
        <p>&quot;peers&quot;: &quot;ipaddress;ipaddress;ipaddress&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>network &#x2013; blockchain network (bitcoin, litecoin, &#x2026;)</p>
        <p>mqttBroker &#x2013; url of the mqtt broker</p>
        <p>httpPort &#x2013; server listening on this port</p>
        <p>insightApiUrl &#x2013; url of the insight api service</p>
        <p>registryUrl &#x2013; url of the registry service</p>
        <p>peers &#x2013; ip addresses of the peers</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

### Upload properties file

The method allows to upload a settings file to the current Imprinter.

* If success - method returns the information about the current Imprinter in the content body and http code 200 OK
* If request body is empty – API returns http code 400 Bad request, with error description
* If an exception throws during the process – then return http code 500 Internal server error with the reason description of error.

Request:

| **Method** | **Request** | **Body** |
| :--- | :--- | :--- |
| POST | /api/v1/properties | File to upload |

Response:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Http Code</b>
      </th>
      <th style="text-align:left"><b>Body</b>
      </th>
      <th style="text-align:left"><b>Meaning</b>
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
      <td style="text-align:left">400 Bad Request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">500 Internal Server Error</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;some java exception&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error!</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
  </tbody>
</table>

