The Data Endpoint API represents the contract that the Glimpse Client expects the server to provide for data access. This contract is important as the Glimpse Client is designed to work with multiple server implementations. This document informs Glimpse server implementations how to appropriately respond to requests from the Glimpse Client.

The client expects a single server endpoint that responds to requests appropriately based on request parameters. This behaviour is designed to minimise framework-specific configuration for alternative server implementations, and help ensure the client can seamlessly interact with any server endpoint.

In the case of the default .NET implementation of the Glimpse Server, this endpoint is exposed via the `/glimpse.axd` handler, and this is used for examples throughout this document.

## Data Resource Endpoint API

### Get Glimpse Request

Returns a single Glimpse Request data type for the specified `requestId`. The response must conform to the [`Glimpse Request`](#glimpse-request) contract defined below.


**Parameters**
`requestId`

**Request**
```
GET /glimpse.axd?requestId=33fe61d1-cc8c-42b3-8ff4-1b8185c1ee98
```
**Response**
```
Status: 200 OK
```
```js
{ 
    method : "GET",
    duration : 123,
    browser : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/22.0.1201.0 Safari/537.1",
    clientName : "iPhone",
    requestTime : "2011/11/09 12:00:00",
    requestId : "33fe61d1-cc8c-42b3-8ff4-1b8185c1ee98",
    isAjax : false,
    url : "/Glimpse/Glimpse/wiki/Data-Endpoints/",
    metadata : { /* [Glimpse Metadata] */ } ,
    data: { /* [Glimpse Plugins] */ } 
}
```

### Get Glimpse Plugin

Returns data for a single Glimpse Plugin, for a single request. The response must conform to the [`Glimpse Plugin`](#glimpse-plugin) contract defined below.

**Parameters**
`requestId`
`pluginKey`

**Request**
```
GET /glimpse.axd?requestId=33fe61d1-cc8c-42b3-8ff4-1b8185c1ee98&pluginKey=Routes
```
**Response**
```
Status: 200 OK
```
```js
{
    "name" : "Routes",
    "data" : [Glimpse Simple Layout]
}
```
## Data Resource Contracts

This section provides formal definitions for the data contracts expected by the client.

### Glimpse Request

`[Glimpse Request]`
```js
{  
    ("method" : ("GET" | "POST" | "DELETE" | "PUT")),
    ("duration" : numeric), 
    ("browser" : (friendlyBrowserName | userAgent)),
    ("clientName" : alphanumeric),
    ("requestTime" : timestampAsString), 
    ("requestId" : alphanumeric),
    ("parentId" : alphanumeric)?,
    ("isAjax" : bool),
    ("url" : absolutePath),
    ("metadata" : [Glimpse Metadata])?,
    ("data" : [Glimpse Plugins])
}
```

**Remarks**

 * `method` - HTTP verb used
 * `duration` - The number of milliseconds that the request took to execute on the server
 * `browser` - Description that can be used to help identify which browser a request came from
 * `clientName` - The session name by which requests can be grouped
 * `requestId` - Unique Id of the request
 * `parentId` - The ID of the logical parent that "owns" the request
    * Mainly used in the case of Ajax requests and to help identify the originating "parent" request 
 * `isAjax` - Whether this request was an ajax request or not
 * `url` - Absolute path that corresponds with the request
 * `metadata` - Any request specific metadata that this request has
    * Request specific metadata is merged with the global metadata when processing the request. If there are any conflicts between this request specific metadata and the global metadata, the request specific metadata will be applied.
 * `data` - An key/value structure, representing the Glimpse Plugin's that where processed in this request

### Glimpse Plugin

`[Glimpse Plugin]`
```js
{
    ("name" : alphanumeric),
    ("data" : ("" | null | alphanumeric | [Glimpse Simple Layout]))?,
    ("isLazy" : bool)?
}
```

**Remarks**

 * `name` - Display name that the client will use to refer to this plugin
 * `data` - Actual data payload of the tab (typically an object in the form of the `Glimpse Simple Layout`)
    * Value = `null`; tab should be rendered, but appear disabled
    * Value = `""`; tab should be rendered, appear enabled and show the text `No data found for this plugin.`
    * Value = `alphanumeric`; tab should display the alphanumeric as a message
 * `isLazy` - Used to indicate if the client should lazily load plugin
    * If this is the case "data" should be set to "".


****
****

## Data Resource Models

The following models are what the client expects to receive when making requests against the Data Resource Endpoint. This is designed to be a formal definition and from this point on, data type described by [] will be defined throughout this section. Other "simple" data type will assume that the reader is able to derive the values via the names and examples. 

### [Glimpse Request]

Definition:

```js
{  
    ("method" : ("GET" | "POST" | "DELETE" | "PUT")),
    ("duration" : numeric), 
    ("browser" : (friendlyBrowserName | userAgent)),
    ("clientName" : alphanumeric),
    ("requestTime" : timestampAsString), 
    ("requestId" : alphanumeric),
    ("parentId" : alphanumeric)?,
    ("isAjax" : bool),
    ("url" : absolutePath),
    ("metadata" : [Glimpse Metadata])?,
    ("data" : [Glimpse Plugins])
}
```

Remarks:

 * `method` - Describes the http verb that was used 
 * `duration` - The number of milliseconds that the request took to execute on the server
 * `browser` - Description that can be used to help identify which browser a request came from
 * `clientName` - The session name by which requests can be grouped by
 * `requestId` - Unique ID that each request has
 * `parentId` - The ID of the logical parent that "owns" the request
    * Mainly used in the case of Ajax requests and to help identify the originating "parent" request 
 * `isAjax` - Whether this request was an ajax request or not
 * `url` - Absolute path that corresponds with the request
 * `metadata` - Any request specific metadata that this request has
    * Request specific metadata is merged with the global metadata when processing the request. If there are any conflicts between this request specific metadata and the global metadata, the request specific metadata will be applied.
 * `data` - An key/value structure, representing the Glimpse Plugin's that where processed in this request

Sample:
```js
{ 
    method : "GET",
    duration : 123,
    browser : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/22.0.1201.0 Safari/537.1",
    clientName : "iPhone",
    requestTime : "2011/11/09 12:00:00",
    requestId : "33fe61d1-cc8c-42b3-8ff4-1b8185c1ee98",
    isAjax : false,
    url : "/Glimpse/Glimpse/wiki/Data-Endpoints/",
    metadata : { /* [Glimpse Metadata] */ } ,
    data: { /* [Glimpse Plugins] */ } 
}
```

### [Glimpse Metadata]

Definition:

```js
{  
    ("version" : [Glimpse Metadata Versions]),
    ("paths" : [Glimpse Metadata Resources]),
    ("environmentUrls" : [Glimpse Metadata Environments])?,
    ("correlation" : [Glimpse Metadata Correlation])?,
    ("plugins" : [Glimpse Metadata Plugins])?
}

/* [Glimpse Metadata Versions] */
[
    ({ 
        ("name" : alphanumeric),
        ("current" : versionNumber),
        ("channel" : alphanumeric)
    })+
]

/* [Glimpse Metadata Resources] */
{  
    (resourceName : absolutePath)+ 
}

/* [Glimpse Metadata Environments] */
{  
    (environmentName : baseUrlPath)+ 
}

/* [Glimpse Metadata Correlation] */
{  
    ("title" : alphanumeric),
    ("legs" : [ (
            ("method" : ("GET" | "POST" | "DELETE" | "PUT")),
            ("url" : absolutePath),
            ("requestId" : alphanumeric)
         )+ ])
}
```

Remarks:

 * `version` - Describes the version numbers of the plugin packs in the system
    * `name` - Name of the plugin pack used to identify it 
    * `current` - Current version of the pack described semantically (i.e. 1.1.3.2)
    * `channel` - What channel the pack is a part of (i.e. Alpha, Beta, Release)
 * `paths` - Describes the paths at which the client can access various resources
    * `resourceName` - The key that the client is programmed to lookup for a given url
 * `environmentUrls` - Facilitates the environment switching features that glimpse supports
    * `environmentName` - Title that the developer selects for each given environment 
 * `correlation` - Used in cases where multiple requests are to be linked together (primarily in PRG cases)
    * `title` - Friendly title of the correlation that has been made
    * `legs` - A collection holding the description of each of the requests involved
    * `method` - Describes the http verb that was used 
    * `requestId` - The ID that the system can use identify the request
 * `plugins` - An key/value structure, the Glimpse Metadata Plugin that is to be applied to individual plugins 

Sample:
```js
{ 
    version : '1.1.3.2',
    paths : {
        "data" : "/Glimpse/Data",
        "paging" : "/Glimpse/Pager", 
        "config" : "/Glimpse/Config",
        "logo" : "/Glimpse/Logo",
        "sprite" : "/Glimpse/Sprite",
        "popup" : "/Glimpse/Popup"
    },
    environmentUrls : {
        "Dev" : "http://localhost/",
        "QA" : "http://qa.getglimpse.com/",
        "Prod" : "http://getglimpse.com/"
    },
    plugins : { /* [Glimpse Metadata Plugins] */ }
}
```

### [Glimpse Plugin]

Definition:

```js
{ 
    ("name" : alphanumeric),
    ("data" : ("" | null | alphanumeric | [Glimpse Simple Layout]))?, 
    ("isLazy" : bool)?
}
```

Remarks:

 * `name` - Display name that the client will use to refer to this plugin
 * `data` - Actual data payload of the tab (typically an object in the form of the `Glimpse Simple Layout`)
    * Value = `null`; tab should be rendered, but appear disabled
    * Value = `""`; tab should be rendered, appear enabled and show the text `No data found for this plugin.`
    * Value = `alphanumeric`; tab should display the alphanumeric as a message
 * `isLazy` - Used to indicate if the client should lazily load plugin
    * If this is the case "data" should be set to "".

Sample:
```js
{ 
    name : "Routes",
    data : { /* [Glimpse Simple Layout] */ }
}
```

### [Glimpse Metadata Plugin]

Definition: 

```js
{  
    ("pagingInfo" : [Glimpse Metadata Paging])?,
    ("documentationUri" : url)?,
    ("structured" : [Glimpse Structured Metadata])?
}

/* [Glimpse Metadata Paging] */
{  
    ("pagerType" : ("continuous" | "traditional" | "scrolling"), 
    ("pageSize" : numeric), 
    ("pageIndex" : numeric), 
    ("totalNumberOfRecords" : numeric) 
}
```

Remarks:

 * `pagingInfo` - Indicates that a plugin has a large volume of data that should be paged
    * `pagerType` - The type of paging that the system should use
        * `continuous` > As the user scrolls to the bottom of the page, the next page is loaded
        * `traditional` > System provides a next and previous buttons
        * `scrolling` > System provides a "more" button, which when clicked appends the next group of records
    * `pageSize` - The number of records that make up each page
    * `pageIndex` - Which page we are up to
    * `totalNumberOfRecords` - The total number of records the system has
 * `documentationUri` - The remote URL that provides help documentation for the plugin
 * `structured` - The Glimpse Structured Metadata that describes how the plugins data should be formatted

Sample:
```js
{
    pagingInfo : { 
        pagerType : 'continuous', 
        pageSize : 5, 
        pageIndex : 0, 
        totalNumberOfRecords : 31 
    },
    documentationUri : "http://getGlimpse.com/Help/Plugin/Timeline",
    structured : { /* [Glimpse Structured Metadata] */ }
}
```

### [Glimpse Simple Layout]

See [[Glimpse Simple Layout|Simple Layout]] documentation for the formal specification.


### [Glimpse Structured Layout]

See [[Glimpse Structured Layout|Structured Layout]] documentation for the formal specification.



## Data Resource Endpoint
The intent is for the below API to be serviced through a single endpoint. The server implantation should be able to determine what specific request is being make via the parameters supplied. This concession is intended to simply the Resource Endpoint implementation on the server side - as different frameworks might require more configuration for any increase in endpoints. 

The physical URL that the client will use to access this End Point is determined by by the Resource Endpoint API. This allows for the server implementation to provide a URL that is consistent with that platform. The exact structure of each of the models as described above.

The following describes what URL can be used to access the corresponding resources. Note the example URL used in this case is `/glimpse.axd` but depending on the implementation this would change. Also for exact model implantation's, note the defined type and find definition in the above model descriptions.

### Get Glimpse Request
Single `[Glimpse Request]` that matches the given `requestId`.

**/glimpse.axd?requestId=alphanumeric**
```js 
{
    [Glimpse Request]
}
```

The data returned represents a requests full response. Meaning it not only includes the "header", but it contains the request specific metadata and request data payload. 


### Get Glimpse Plugin
The `[Glimpse Plugin]` data for an individual plugin. Primarily used when lazy loading plugins. 

**/glimpse.axd?requestId=alphanumeric&pluginKey=alphanumeric**
```js 
{
    [Glimpse Plugin]
}
```

The data returned represents the payload for an individual plugin. Meaning it includes the payload which is normally embedded within one of the elements of the `[Glimpse Request]` responses `data` property.


### Get Child Request Headers
List of `[Glimpse Request Heads]` that represent the child ajax requests associated with a given origin `requestId`.

**/glimpse.axd?parentRequestId=alphanumeric&ajaxResults=bool**
```js 
[
    [Glimpse Request Heads]
]
```

The data returned represents the requests *header* response. Meaning it *only* includes the header.

*Enhancements*: A client should be able to indicate what previous ajax requests is knows about. Meaning, if the server was previously asked for a list of child requests and it returned 5 results, next time it is called, if there are 2 new child requests, only the 2 new results should be returned instead of all 7.


### Get All Request Headers
List of `[Glimpse Request Heads]` that represent all the requests that the glimpse storage provider knows about.

**/glimpse.axd**
```js 
{
    (sessionName : [
        [Glimpse Request Heads]
    ])+
]
```

The data returned represents a requests *header* response. Meaning it *only* includes the header. It is grouped by the *client* that the request originated from. 

*Enhancements:* A client should be able to indicate what previous requests is knows about. Meaning, if the server was previously asked for a list of requests and it returned 5 results, next time it is called, if there are 2 new requests, only the 2 new results should be returned instead of all 7.