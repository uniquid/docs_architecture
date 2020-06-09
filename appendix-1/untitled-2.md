# Permission Management Service HTTP APIs     Part 2

![](../.gitbook/assets/0%20%281%29.png)

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
      <td style="text-align:center">Part 2</td>
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
| POST | /api/v1/init/admin | Create admin |
| POST | /api/v1/users | Create new user |
| PUT | /api/v1/users/:userId | Update existing user |
| GET | /api/v1/users | Get users list of the tenant |
| DELETE | /api/v1/users/:userId | Delete user |
| POST | /api/v1/login | User login |
| GET | /api/v1/logout | User logout |
| POST | /api/v1/connectors | Create new connector \(AWS, Azure, Google\) |
| PUT | /api/v1/connectors/:id | Edit existing connector |
| GET | /api/v1/connectors | Get list of all connectors |
| GET | /api/v1/connectors | Get list of all connectors |
| DELETE | /api/v1/connectors/:id | Delete existing connector |
| GET | /api/v1/connectors/:id/status | Test orchestrator ↔ AWS connection |

#### Detailed API description

### Create admin

No special authentication requirements for this API call 

The API created first \(admin\) user. This admin user must be used to manage new users. The Init method can be called only once at the beginning. All other APIs couldn't be called before the Init call. 

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
      <td style="text-align:left">/api/v1/init/admin</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;userId&quot;: &quot;<a href="mailto:email@mail.com">email@mail.com</a>&quot;</p>
        <p>&quot;password&quot;: &quot;14f9un34nu34f9u34&quot;</p>
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
        <p>&quot;error&quot;: &quot;Admin user already initialized&quot;</p>
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

### Create new user

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API creates new user inside the current Orchestrator. User must be logged on. 

* If success – API returns http code 201 Created.  
* If request body is empty – API returns http code 400 Bad request, with error description 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body. 
* If user with given userId already exists – method fails and return http code 409 Conflict.  
* If any other exception throws during the user creation process – then return http code 500 Internal server error with the reason description of error. 

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
      <td style="text-align:left">/api/v1/users</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;userId&quot;: &quot;<a href="mailto:email@mail.com">email@mail.com</a>&quot;</p>
        <p>&quot;password&quot;: &quot;14f9un34nu34f9u34&quot;</p>
        <p>}</p>
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
      <td style="text-align:left">201 Created</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;userId&quot;: &quot;<a href="mailto:email@mail.com">email@mail.com</a>&quot;</p>
        <p>// no password in response!</p>
        <p>}</p>
      </td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">409 Conflict</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;User already exists&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Given userId already exists.</p>
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

### Update user

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API update user user's information. User must me logged on. In the request body should be only fields that user wants to modify, fields without changes must be skipped. 

* If success – API returns http code 200 OK.  
* If request body is empty – API returns http code 400 Bad request, with error description 
* If given userId is absent returns http code 404 Not found. 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If any other exception throws during the user update process – then return http code 500 Internal server error with the reason description of error. 

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
      <td style="text-align:left">PUT</td>
      <td style="text-align:left">/api/v1/users/:userId</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;userId&quot;: &quot;<a href="mailto:email@mail.com">new_mail@mail.com</a>&quot;</p>
        <p>&quot;password&quot;:&quot;u34ff9u4n4n39u31f4&quot;</p>
        <p>}</p>
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
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;userId&quot;: &quot;<a href="mailto:email@mail.com">new_email@mail.com</a>&quot;</p>
        <p>// no password in response!</p>
        <p>}</p>
      </td>
      <td style="text-align:left">Success!</td>
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
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Can&apos;t find user with id&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Given userId doesn&apos;t exists.</p>
        <p>error &#x2013; text message, describes the error reason</p>
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



### View users list

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API returns the list of all users. User must be logged on. 

* If success – API returns http code 200 OK and body with the array of user objects. User objects does not contain password \(for security purpose\). 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/users  | Cookie: JSESSIONID=298zf09hf012fh2;  |

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
        <p>[</p>
        <p>{</p>
        <p>&quot;userId&quot;: &quot;first@mail.com&quot;</p>
        <p>// no password in response!</p>
        <p>},</p>
        <p>{</p>
        <p>&quot;userId&quot;: &quot;second@mail.com&quot;</p>
        <p>// no password in response!</p>
        <p>},</p>
        <p>...</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>userId &#x2013; unique user&apos;s identifier, in our case email.</p>
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

### Delete user

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API delete user by given userId. User must be logged on. 

* If success - API returns http code 410 Gone. 
* If user with given userId not exist – then return http code 404 Not found. 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| DELETE  | /api/v1/users/:userId  | Cookie: JSESSIONID=298zf09hf012fh2;  |

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
      <td style="text-align:left">410 Gone</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Can&apos;t find user with id&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The user with given userId not found</p>
        <p>error &#x2013; text message, describes the error reason</p>
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

### Login

_No special authentication requirements for this API call_

The API performs user login with given userId and password.  

* If success - method returns JSESSIONID which must be used as a cookie in all session requests. 
* If request body is empty – API returns http code 400 Bad request, with error description 
* If user with given password is incorrect – method fails and return http code 401 Unauthorized.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

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
      <td style="text-align:left">/api/v1/login</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;userId&quot;: &quot;<a href="mailto:email@mail.com">email@mail.com</a>&quot;,</p>
        <p>&quot;password&quot;: &quot;14f9un34nu34f9u34&quot;</p>
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
      <td style="text-align:left">Set-Cookie: JSESSIONID=298zf09hf012fh2;</td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>JSESSIONID &#x2013; session key to perform the API calls from CLI/webApp</p>
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
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The userId, password combination is incorrect.</p>
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

### Logout

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API preforms user logout.  

* If success – API returns http code 200 OK.  
* If user with given JSESSIONID is incorrect or key is expired – method fails and return http code 401 Unauthorized.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/logout  | Cookie: JSESSIONID=298zf09hf012fh2;  |

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
      <td style="text-align:left">Error! The JSESSIONID is incorrect or expired</td>
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

### Create cloud connection

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API creates new cloud connection for current tenant \(AWS/Azure\). User must be logged on. In the request body all fields are mandatory. 

* If success – API returns http code 200 OK and object with the information about created cloud connection except secret\_access\_key \(for security purpose\). 
* If request body is empty – API returns http code 400 Bad request, with error description 
* If user with given JSESSIONID is incorrect or key is expired – method fails and return http code 401 Unauthorized.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

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
      <td style="text-align:left">/api/v1/connectors</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;name&quot;: &quot;My AWS Cloud&quot;,</p>
        <p>&quot;type&quot;: &quot;aws&quot;,</p>
        <p>// specific fields for given cloud type</p>
        <p>}</p>
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
      <td style="text-align:left">201 Created</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;id&quot;: 12,</p>
        <p>&quot;name&quot;: &quot;My AWS Cloud&quot;,</p>
        <p>&quot;type&quot;: &quot;aws&quot;,</p>
        <p>// specific fields for given cloud type</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>id &#x2013; unique cloud connector identifier</p>
        <p>name &#x2013; connector&apos;s alias</p>
        <p>type &#x2013; the cloud type (aws, azure, google)</p>
        <p>Depending on type &#x2013; object can contain different additional fields
          specific to selected cloud.</p>
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
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The JSESSIONID is incorrect or expired</p>
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

### Update cloud connection

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

The API updates the existing cloud connector for current tenant by given id. User must me logged on. In the request body should be only fields that user wants to modify. 

* If success - API returns http code 200 OK and object with the information about updated cloud connection except password fields like secret\_access\_key \(for security purpose\). 
* If request body is empty – API returns http code 400 Bad request, with error description. 
* If request URI doesn't contain id or id is string \(must be number only\) – return http code 400 Bad request 
* If cloud connection with given id doesn't exist – then return http code 404 Not found. 
* If user with given JSESSIONID is incorrect or key is expired – method fails and return http code 401 Unauthorized.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

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
      <td style="text-align:left">PUT</td>
      <td style="text-align:left">/api/v1/connectors/:id</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;name&quot;: &quot;New name&quot;,</p>
        <p>&quot;type&quot;: &quot;aws&quot;,</p>
        <p>// specific fields for given cloud type</p>
        <p>}</p>
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
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;id&quot;: 12,</p>
        <p>&quot;name&quot;: &quot;New name&quot;,</p>
        <p>&quot;type&quot;: &quot;aws&quot;,</p>
        <p>// specific fields for given cloud type</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>id &#x2013; unique cloud connector identifier</p>
        <p>name &#x2013; connector&apos;s alias</p>
        <p>type &#x2013; the cloud type (aws, azure, google)</p>
        <p>Depending on type &#x2013; object can contain different additional fields
          specific to selected cloud.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Body can&apos;t be empty&quot;</p>
        <p>}</p>
        <p>{</p>
        <p>&quot;error&quot;: &quot;Missed or incorrect id...</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Client provide incorrect request.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Can&apos;t find Cloud Connector with id&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The cloud connection with given id doesn&apos;t exist.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">401 Unauthorized</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Session expired or incorrect credentials&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">Error! The JSESSIONID is incorrect or expired</td>
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

### Delete cloud connector

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

This API deletes the existing cloud connector for current tenant by given id. User must me logged on. 

* If success - API returns http code 410 Gone.
* If connector with given id doesn’t exist – return http code 404 Not found
* If request URI doesn't contains id or id is string \(must be number only\) – return http code 400 Bad request
* If user with given JSESSIONID is incorrect or key is expired – method fails and return http code 401 Unauthorized.
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| DELETE  | /api/v1/connectors/:id  | Cookie: JSESSIONID=298zf09hf012fh2;  |

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
      <td style="text-align:left">410 Gone</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Success!</td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Can&apos;t find Cloud Connector with id&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The cloud connection with given id not found in current tenant.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Missed or incorrect id...</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Wrong id given. It must be the number only and it&apos;s mandatory.</p>
        <p>error &#x2013; text message, describes the error reason</p>
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
        <p>Error! The JSESSIONID is incorrect or expired</p>
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

### View connector

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

This API returns cloud connector by given id. User must me logged on. API returns cloud connector object with the fields specific to selected cloud. Selected cloud depends on field type which can be aws, azure or google. Connector objects does not contain password information like secretAccessKey. 

* If success – API returns http code 200 OK and body with information about cloud connector by given id.
* If cloud connector with given id not exist – then return http code 404 Not found.
* If request URI must have id as number, otherwise, if it's string – then return http code 400 Bad request. 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/connectors  | Cookie: JSESSIONID=298zf09hf012fh2;  |

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
        <p>&quot;id&quot;: 12,</p>
        <p>&quot;name&quot;: &quot;My AWS Cloud&quot;,</p>
        <p>&quot;type&quot;: &quot;aws&quot;,</p>
        <p>// specific fields for given cloud type</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>id &#x2013; unique cloud connector identifier</p>
        <p>name &#x2013; connector&apos;s alias</p>
        <p>type &#x2013; the cloud type (aws, azure, google)</p>
        <p>Depending on type &#x2013; object can contains different additional fields
          specific to selected cloud.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">404 Not found</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Can&apos;t find Cloud Connector with id&quot;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! The cloud connection with given id not found.</p>
        <p>error &#x2013; text message, describes the error reason</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 Bad request</td>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;error&quot;: &quot;Incorrect id. Must be number only!&quot;;</p>
        <p>}</p>
      </td>
      <td style="text-align:left">
        <p>Error! Wrong id given. It must be the number only.</p>
        <p>error &#x2013; text message, describes the error reason</p>
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

### View all connectors

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

This API returns the list of all existing connectors of current tenant. User must me logged on. API returns list of cloud connector objects with the fields specific to selected cloud. Selected cloud depends on field type which can be aws, azure or google. Connector objects does not contain password information like secretAccessKey. 

* If success – API returns http code 200 OK and body with the array of connector objects. Connector objects does not contain password information like secretAccessKey. 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/connectors  | Cookie: JSESSIONID=298zf09hf012fh2;  |

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
        <p>[</p>
        <p>{</p>
        <p>&quot;id&quot;: 12,</p>
        <p>&quot;name&quot;: &quot;My AWS Cloud&quot;,</p>
        <p>&quot;type&quot;: &quot;aws&quot;,</p>
        <p>// specific fields for AWS cloud type</p>
        <p>},</p>
        <p>{</p>
        <p>&quot;id&quot;: 18,</p>
        <p>&quot;name&quot;: &quot;Microsoft Cloud&quot;,</p>
        <p>&quot;type&quot;: &quot;azure&quot;,</p>
        <p>// specific fields for AZURE cloud type</p>
        <p>},</p>
        <p>{</p>
        <p>&quot;id&quot;: 19,</p>
        <p>&quot;name&quot;: &quot;My Test Google cloud&quot;,</p>
        <p>&quot;type&quot;: &quot;google&quot;,</p>
        <p>// specific fields for Google cloud type</p>
        <p>}</p>
        <p>...</p>
        <p>]</p>
      </td>
      <td style="text-align:left">
        <p>Success!</p>
        <p>id &#x2013; unique cloud connector identifier</p>
        <p>name &#x2013; connector&apos;s alias</p>
        <p>type &#x2013; the cloud type (aws, azure, google)</p>
        <p>Depending on type &#x2013; object can contains different additional fields
          specific to selected cloud.</p>
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

### Connector status

Required the JSESSIONID in the request cookies, user must be logged in \(see: Login API\) 

This API test credentials and connection of given connector by accessKeyId. User must me logged on. 

* If success – API returns http code 200 OK and body. 
* If the JSESSIONID is absent, incorrect or expired, then return http code 401 Unauthorized with the error description in the response body.  
* If any other exception throws during the process – then return http code 500 Internal server error with the reason description of error. 

Request:

| **METHOD** | **REQUEST** | **BODY/HEADERS** |
| :--- | :--- | :--- |
| GET  | /api/v1/connectors/:access\_key\_id/status  | Cookie: JSESSIONID=298zf09hf012fh2;  |

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
      <td style="text-align:left">503 Service Unavailable</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Error! Can&apos;t connect to AWS.</td>
    </tr>
    <tr>
      <td style="text-align:left">403 Forbidden</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Error! Incorrect credentials. Successfully connected to AWS but rejected
        in credentials verification.</td>
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

