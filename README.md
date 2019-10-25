
**Version v1.0**

# Introduction to API

API functionality consists of the following resources: **Account, Customer, Device, Item, Nav, Sales**.
Each resource is controlled by its corresponding controller using various actions.
Each action can have four basic methods: **POST** (create resource), **GET** (read resource), **PATCH** (update resource), **DELETE** (delete resource).
The router provides access control to the controller action and the method of calling it.
To manage resources, the router uses the following template engine (route):

- **/controller**                 - define controller name;
- **/action**                     - define action name of the controller;
- **/id**                         - define resource identifier;
- **/:layout**                    - define layout (html, table, etc);
- **/+function**                  - define additional function name of the action;
- **/-search**                    - define search for some fields (eg. No, customerCode, documentPrimaryKey or fuzzy search);
- **/_prefix**                    - define prefix (short name) for used table;
- **/sort/key1,key2,..**          - define order by keys, default direction is ASC, **"-"** before key sets the DESC direction;
- **/group/key1,key2,..**         - define group by keys for the response data;
- **/fields/key1,key2,..**        - define fields for the response data, default is **"*"**;
- **/count/numeric**              - define limit of count the response data, default is null;
- **/join/table,condition,type**  - define joins, default type is **LEFT**;
- **/key/value,condition**        - define where filter for the response data, default condition is **"="**;

All keys used in the router must be the same as they are named in the database: **lowerCamelCase** format.
Format for array definition: **(value1; value2; ...)**.

### Request and Response Structure

Request:

```
[
  "route"   => [ $_GET, <route> ],
  "data"    => [ $_POST, <input> ],
  "method"  => <method>,
  "token"   => <token>,
  "user"    => [ ... ],
]
```

Response:

```
[
  "jsonapi"   => [ "version" => <version>, "timestamp" => <timestamp>, "runtime" => <runtime>, "memory" => <memory> ],
  "request"   => <request>,
  "datacount" => <datacount>,
  "data"      => [ ... ],
]
```

Errors:

```
[
  "jsonapi" => [ "version" => <version>, "timestamp" => <timestamp>, "runtime" => <runtime>, "memory" => <memory> ],
  "request" => <request>,
  "errors"  => [ ... ],
]
```

### Authorization process

To authorize the user, need to send a post request with an username and password in the message header:
```
Authorization: Basic base64_encode(<username>:<password>)
```
or in the body of the message:
```
userName=<username>, password=<password>
```
In the response, a session will be opened and a token will be issued for api authentication:
```
Authorization: Bearer <token>
```

## ACCOUNT

The AccountController has the following actions: statusAction, loginAction, logoutAction, registerAction, logsAction.

### statusAction

```
ROUTE:    account/status

METHOD:   GET
ACCESS:   manager

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/account/status" -i
JS:       $.ajax({url: "/api/build/version/account/status", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route account/status --method GET
```

### loginAction

```
ROUTE:    account/login

METHOD:   POST
ACCESS:   all

CURL:     curl -H "Authorization: Basic base64_encode(<username>:<password>)" -H "Accept: application/json" -d "" -X POST "https://domain.com/api/build/version/account/login" -i
JS:       $.ajax({url: "/api/build/version/account/login", method: "POST", headers: {Authorization: "Basic base64_encode(<username>:<password>)"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  -

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/account/login" -i
JS:       $.ajax({url: "/api/build/version/account/login", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  -
```

### logoutAction

```
ROUTE:    account/logout

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/account/logout" -i
JS:       $.ajax({url: "/api/build/version/account/logout", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  -
```

### registerAction

```
ROUTE:    account/register

METHOD:   POST
ACCESS:   manager

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"userName":"<userName>", "password":"<password>", "fullName":"<fullName>", "acl":"<acl>", "role":"<role>", "repCode":"<repCode>", "email":"<email>", "image":"<image>", "functionality":"<functionality>"}' -X POST "https://domain.com/api/build/version/account/register" -i
JS:       $.ajax({url: "/api/build/version/account/register", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {userName:"<userName>", password:"<password>", fullName:"<fullName>", acl:"<acl>", role:"<role>", repCode:"<repCode>", email:"<email>", image:"<image>", functionality:"<functionality>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route account/register --method POST --data '{"userName":"<userName>", "password":"<password>", "fullName":"<fullName>", "acl":"<acl>", "role":"<role>", "repCode":"<repCode>", "email":"<email>", "image":"<image>", "functionality":"<functionality>"}'
```

### logsAction

```
ROUTE:    account/logs

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/account/logs" -i
JS:       $.ajax({url: "/api/build/version/account/logs", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route account/logs --method GET
```

## CUSTOMER

The CustomerController has the following actions: callsAction, commentsAction, detailsAction, favouritesAction, schedulesAction, statusesAction, tasksAction.

### callsAction

```
ROUTE:    customer/calls

METHOD:   POST
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"call":"<call>", "customerCode":"<customerCode>", "checkinTime":"<checkinTime>", "checkoutTime":"<checkoutTime>", "latitude":"<latitude>", "longitude":"<longitude>"}' -X POST "https://domain.com/api/build/version/customer/calls" -i
JS:       $.ajax({url: "/api/build/version/customer/calls", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {call:"<call>", customerCode:"<customerCode>", checkinTime:"<checkinTime>", checkoutTime:"<checkoutTime>", latitude:"<latitude>", longitude:"<longitude>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/calls --method POST --data '{"call":"<call>", "customerCode":"<customerCode>", "checkinTime":"<checkinTime>", "checkoutTime":"<checkoutTime>", "latitude":"<latitude>", "longitude":"<longitude>"}'

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/customer/calls" -i
JS:       $.ajax({url: "/api/build/version/customer/calls", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/calls --method GET

METHOD:   PATCH
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"call":"<call>", "customerCode":"<customerCode>", "checkinTime":"<checkinTime>", "checkoutTime":"<checkoutTime>", "latitude":"<latitude>", "longitude":"<longitude>"}' -X PATCH "https://domain.com/api/build/version/customer/calls/id" -i
JS:       $.ajax({url: "/api/build/version/customer/calls/id", method: "PATCH", headers: {Authorization: "Bearer <token>"}, data: {call:"<call>", customerCode:"<customerCode>", checkinTime:"<checkinTime>", checkoutTime:"<checkoutTime>", latitude:"<latitude>", longitude:"<longitude>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/calls/id --method PATCH --data '{"call":"<call>", "customerCode":"<customerCode>", "checkinTime":"<checkinTime>", "checkoutTime":"<checkoutTime>", "latitude":"<latitude>", "longitude":"<longitude>"}'

METHOD:   DELETE
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X DELETE "https://domain.com/api/build/version/customer/calls/id" -i
JS:       $.ajax({url: "/api/build/version/customer/calls/id", method: "DELETE", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/calls/id --method DELETE --data '{"call":"<call>", "customerCode":"<customerCode>", "checkinTime":"<checkinTime>", "checkoutTime":"<checkoutTime>", "latitude":"<latitude>", "longitude":"<longitude>"}'
```

### commentsAction

```
ROUTE:    customer/comments

METHOD:   POST
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"customerCode":"<customerCode>", "comment":"<comment>", "image":"<image>"}' -X POST "https://domain.com/api/build/version/customer/comments" -i
JS:       $.ajax({url: "/api/build/version/customer/comments", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {customerCode:"<customerCode>", comment:"<comment>", image:"<image>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/comments --method POST --data '{"customerCode":"<customerCode>", "comment":"<comment>", "image":"<image>"}'

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/customer/comments" -i
JS:       $.ajax({url: "/api/build/version/customer/comments", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/comments --method GET

METHOD:   PATCH
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"customerCode":"<customerCode>", "comment":"<comment>", "image":"<image>"}' -X PATCH "https://domain.com/api/build/version/customer/comments/id" -i
JS:       $.ajax({url: "/api/build/version/customer/comments/id", method: "PATCH", headers: {Authorization: "Bearer <token>"}, data: {customerCode:"<customerCode>", comment:"<comment>", image:"<image>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/comments/id --method PATCH --data '{"customerCode":"<customerCode>", "comment":"<comment>", "image":"<image>"}'

METHOD:   DELETE
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X DELETE "https://domain.com/api/build/version/customer/comments/id" -i
JS:       $.ajax({url: "/api/build/version/customer/comments/id", method: "DELETE", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/comments/id --method DELETE
```

### detailsAction

```
ROUTE:    customer/details

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/customer/details" -i
JS:       $.ajax({url: "/api/build/version/customer/details", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/details --method GET
```

### favouritesAction

---

### schedulesAction

```
ROUTE:    customer/schedules

METHOD:   POST
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"rep":"<rep>", "customerCode":"<customerCode>", "callDate":"<callDate>", "callType":"<callType>", "callComment1":"<callComment1>", "callComment2":"<callComment2>"}' -X POST "https://domain.com/api/build/version/customer/schedules" -i
JS:       $.ajax({url: "/api/build/version/customer/schedules", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {rep:"<rep>", customerCode:"<customerCode>", callDate:"<callDate>", callType:"<callType>", callComment1:"<callComment1>", callComment2:"<callComment2>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/schedules --method POST --data '{"rep":"<rep>", "customerCode":"<customerCode>", "callDate":"<callDate>", "callType":"<callType>", "callComment1":"<callComment1>", "callComment2":"<callComment2>"}'

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/customer/schedules" -i
JS:       $.ajax({url: "/api/build/version/customer/schedules", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/schedules --method GET

METHOD:   PATCH
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"rep":"<rep>", "customerCode":"<customerCode>", "callDate":"<callDate>", "callType":"<callType>", "callComment1":"<callComment1>", "callComment2":"<callComment2>"}' -X PATCH "https://domain.com/api/build/version/customer/schedules/id" -i
JS:       $.ajax({url: "/api/build/version/customer/schedules/id", method: "PATCH", headers: {Authorization: "Bearer <token>"}, data: {rep:"<rep>", customerCode:"<customerCode>", callDate:"<callDate>", callType:"<callType>", callComment1:"<callComment1>", callComment2:"<callComment2>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/schedules/id --method PATCH --data '{"rep":"<rep>", "customerCode":"<customerCode>", "callDate":"<callDate>", "callType":"<callType>", "callComment1":"<callComment1>", "callComment2":"<callComment2>"}'

METHOD:   DELETE
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X DELETE "https://domain.com/api/build/version/customer/schedules/id" -i
JS:       $.ajax({url: "/api/build/version/customer/schedules/id", method: "DELETE", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/schedules/id --method DELETE
```

### statusesAction

```
ROUTE:    customer/statuses

METHOD:   POST
ACCESS:   manager

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X POST "https://domain.com/api/build/version/customer/statuses" -i
JS:       $.ajax({url: "/api/build/version/customer/statuses", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/statuses --method POST

METHOD:   GET
ACCESS:   all
FUNC:     active, inactive, redlight, top80, bottom20, schedule

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/customer/statuses/+func" -i
JS:       $.ajax({url: "/api/build/version/customer/statuses/+func", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/statuses/+func --method GET
```

### tasksAction

```
ROUTE:    customer/tasks

METHOD:   POST
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"task":"<task>", "customerCode":"<customerCode>", "image":"<image>", "taskType":"<taskType>", "description":"<description>", "status":"<status>", "dueDate":"<dueDate>", "requestedByPhone":"<requestedByPhone>"}' -X POST "https://domain.com/api/build/version/customer/tasks" -i
JS:       $.ajax({url: "/api/build/version/customer/tasks", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {task:"<task>", customerCode:"<customerCode>", image:"<image>", taskType:"<taskType>", description:"<description>", status:"<status>", dueDate:"<dueDate>", requestedByPhone:"<requestedByPhone>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/tasks --method POST --data '{"task":"<task>", "customerCode":"<customerCode>", "image":"<image>", "taskType":"<taskType>", "description":"<description>", "status":"<status>", "dueDate":"<dueDate>", "requestedByPhone":"<requestedByPhone>"}'

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/customer/tasks" -i
JS:       $.ajax({url: "/api/build/version/customer/tasks", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/tasks --method GET

METHOD:   PATCH
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"task":"<task>", "customerCode":"<customerCode>", "image":"<image>", "taskType":"<taskType>", "description":"<description>", "status":"<status>", "dueDate":"<dueDate>", "requestedByPhone":"<requestedByPhone>"}' -X PATCH "https://domain.com/api/build/version/customer/tasks/id" -i
JS:       $.ajax({url: "/api/build/version/customer/tasks/id", method: "PATCH", headers: {Authorization: "Bearer <token>"}, data: {task:"<task>", customerCode:"<customerCode>", image:"<image>", taskType:"<taskType>", description:"<description>", status:"<status>", dueDate:"<dueDate>", requestedByPhone:"<requestedByPhone>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/tasks/id --method PATCH --data '{"task":"<task>", "customerCode":"<customerCode>", "image":"<image>", "taskType":"<taskType>", "description":"<description>", "status":"<status>", "dueDate":"<dueDate>", "requestedByPhone":"<requestedByPhone>"}'

METHOD:   DELETE
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X DELETE "https://domain.com/api/build/version/customer/tasks/id" -i
JS:       $.ajax({url: "/api/build/version/customer/tasks/id", method: "DELETE", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route customer/tasks/id --method DELETE
```

## DEVICE

The DeviceController has the following actions: logsAction.

### logsAction

```
ROUTE:    device/logs

METHOD:   POST
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"action":"<action>", "latitude":"<latitude>", "longitude":"<longitude>", "accuracy":"<accuracy>", "note":"<note>"}' -X POST "https://domain.com/api/build/version/device/logs" -i
JS:       $.ajax({url: "/api/build/version/device/logs", method: "POST", headers: {Authorization: "Bearer <token>"}, data: '{action:"<action>", latitude:"<latitude>", longitude:"<longitude>", accuracy:"<accuracy>", note:"<note>"}', dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route device/logs --method POST --data '{"action":"<action>", "latitude":"<latitude>", "longitude":"<longitude>", "accuracy":"<accuracy>", "note":"<note>"}'

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/device/logs" -i
JS:       $.ajax({url: "/api/build/version/device/logs", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route device/logs --method GET
```

## ITEM

The ItemController has the following actions: availablesAction, detailsAction.

### availablesAction

---

### detailsAction

```
ROUTE:    item/details

METHOD:   GET
ACCESS:   all
FUNC:     description

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/item/details/+func" -i
JS:       $.ajax({url: "/api/build/version/item/details/+func", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route item/details/+func --method GET
```

## NAV

The NavController has the following actions: envelopeAction, exportAction, importAction.

### envelopeAction

```
ROUTE:    nav/envelope

METHOD:   GET
ACCESS:   manager
TABLE:    customer_details, sales_order, sales_posted_invoices, sales_posted_invoice_lines, sales_uploaded_orders_app

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/nav/envelope/-table" -i
JS:       $.ajax({url: "/api/build/version/nav/envelope/-table", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route nav/envelope/-table --method GET
```

### exportAction

```
ROUTE:    nav/export

METHOD:   POST
ACCESS:   manager
TABLE:    sales_uploaded_orders_app

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X POST "https://domain.com/api/build/version/nav/export/-table" -i
JS:       $.ajax({url: "/api/build/version/nav/export/-table", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route nav/export/-table --method POST
```

### importAction

```
ROUTE:    nav/import

METHOD:   POST
ACCESS:   manager
TABLE:    customer_details, sales_order, sales_posted_invoices, sales_posted_invoice_lines

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X POST "https://domain.com/api/build/version/nav/import/-table" -i
JS:       $.ajax({url: "/api/build/version/nav/import/-table", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route nav/import/-table --method POST
```

## SALES

The SalesController has the following actions: reportAction, ordersAction, invoicesAction, invoiceLinesAction, ordersAppAction.

### reportAction

```
ROUTE:    sales/report

METHOD:   GET
ACCESS:   manager

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/sales/report" -i
JS:       $.ajax({url: "/api/build/version/sales/report", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route sales/report --method GET
```

### ordersAction

```
ROUTE:    sales/orders

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/sales/orders" -i
JS:       $.ajax({url: "/api/build/version/sales/orders", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route sales/orders --method GET
```

### invoicesAction

```
ROUTE:    sales/invoices

METHOD:   GET
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/sales/invoices" -i
JS:       $.ajax({url: "/api/build/version/sales/invoices", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route sales/invoices --method GET
```

### invoiceLinesAction

---

### ordersAppAction

```
ROUTE:    sales/ordersApp

METHOD:   POST
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"debugFlag":"<debugFlag>", "documentPrimaryKey":"<documentPrimaryKey>", "customerCode":"<customerCode>", "customerName":"<customerName>", "repName":"<repName>", "latitude":"<latitude>", "longitude":"<longitude>", "orderBlob":"<orderBlob>", "orderDate":"<orderDate>", "poNumber":"<poNumber>", "orderValue":"<orderValue>", "howTaken":"<howTaken>"}' -X POST "https://domain.com/api/build/version/sales/ordersApp" -i
JS:       $.ajax({url: "/api/build/version/sales/ordersApp", method: "POST", headers: {Authorization: "Bearer <token>"}, data: {debugFlag:"<debugFlag>", documentPrimaryKey:"<documentPrimaryKey>", customerCode:"<customerCode>", customerName:"<customerName>", repName:"<repName>", latitude:"<latitude>", longitude:"<longitude>", orderBlob:"<orderBlob>", orderDate:"<orderDate>", poNumber:"<poNumber>", orderValue:"<orderValue>", howTaken:"<howTaken>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route sales/ordersApp --method POST --data '{"debugFlag":"<debugFlag>", "documentPrimaryKey":"<documentPrimaryKey>", "customerCode":"<customerCode>", "customerName":"<customerName>", "repName":"<repName>", "latitude":"<latitude>", "longitude":"<longitude>", "orderBlob":"<orderBlob>", "orderDate":"<orderDate>", "poNumber":"<poNumber>", "orderValue":"<orderValue>", "howTaken":"<howTaken>"}'

METHOD:   GET
ACCESS:   all
FUNC:     status, transfered, nodebugged

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X GET "https://domain.com/api/build/version/sales/ordersApp/+func" -i
JS:       $.ajax({url: "/api/build/version/sales/ordersApp/+func", method: "GET", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route sales/ordersApp/+func --method GET

METHOD:   PATCH
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d '{"debugFlag":"<debugFlag>", "documentPrimaryKey":"<documentPrimaryKey>", "customerCode":"<customerCode>", "customerName":"<customerName>", "repName":"<repName>", "latitude":"<latitude>", "longitude":"<longitude>", "orderBlob":"<orderBlob>", "orderDate":"<orderDate>", "poNumber":"<poNumber>", "orderValue":"<orderValue>", "howTaken":"<howTaken>"}' -X PATCH "https://domain.com/api/build/version/sales/ordersApp/id" -i
JS:       $.ajax({url: "/api/build/version/sales/ordersApp/id", method: "PATCH", headers: {Authorization: "Bearer <token>"}, data: {debugFlag:"<debugFlag>", documentPrimaryKey:"<documentPrimaryKey>", customerCode:"<customerCode>", customerName:"<customerName>", repName:"<repName>", latitude:"<latitude>", longitude:"<longitude>", orderBlob:"<orderBlob>", orderDate:"<orderDate>", poNumber:"<poNumber>", orderValue:"<orderValue>", howTaken:"<howTaken>"}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route sales/ordersApp/id --method PATCH --data '{"debugFlag":"<debugFlag>", "documentPrimaryKey":"<documentPrimaryKey>", "customerCode":"<customerCode>", "customerName":"<customerName>", "repName":"<repName>", "latitude":"<latitude>", "longitude":"<longitude>", "orderBlob":"<orderBlob>", "orderDate":"<orderDate>", "poNumber":"<poNumber>", "orderValue":"<orderValue>", "howTaken":"<howTaken>"}'

METHOD:   DELETE
ACCESS:   all

CURL:     curl -H "Authorization: Bearer <token>" -H "Accept: application/json" -d "" -X DELETE "https://domain.com/api/build/version/sales/ordersApp/id" -i
JS:       $.ajax({url: "/api/build/version/sales/ordersApp/id", method: "DELETE", headers: {Authorization: "Bearer <token>"}, data: {}, dataType: "json", success: function(response) {console.log("response:", response)}})
CONSOLE:  php console --route sales/ordersApp/id --method DELETE
```


## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/aarizhov/test_project/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/aarizhov/test_project/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
