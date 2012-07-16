The Data Endpoint API represents the contract that the Glimpse Client expects the server to provide. This contract is important as the Glimpse Client is designed to work with multiple server implementations. This definition allows anyone implementing a Glimpse Service know what data it is expected to be able to send, when given Http `Post` is received.

## End Point 
The intent if for the below API to be serviced through a single endpoint and for the server implantation to determine what needs to happen via the parameters supplied. This is intended to make the work of who ever is implementing the server side of Glimpse easier, as different frameworks might require more configuration for any increase in endpoints.  

The physical URL that the client will use to access this End Point is determined by by the Resource Endpoint API. This allows for the server implementation to provide a URL that is consistent with that platform. The exact structure of each of the models will be described in the Model API.

## API
### Request 
Single entry that matches the given `requestId`.

_Input_:

    { requestId : '...' }

_Output_: Full Response

    {     
        method : '...',
        duration : 111,  
        browser : '...',
        clientName : '...',
        requestTime : '2011/11/09 12:00:00',
        requestId : '1234',
        parentId : '...',
        isAjax : false,
        url : '/',  
        metadata : { ... },
        data : {
                'TabA' : { name : 'Tab A', data : { ... } },
                ...,
                'TabX' : { name : 'Tab X', data : { ... } },
        }
    }

_Note_: The data returned represents a requests *full* response. Meaning it *not only* includes the header, but it adds on the *request specifc metadata* and *request data payload*. 


### Plugin
Lazy loading data for an individual plugin.

_Input_:

    { 'requestId' : '...', 'pluginKey' : 'TabA' }

_Output_: Indovidual Plugin Payload

    { name : 'Tab A', data : { ... } }
    
_Note_: The data returned represents the payload for an * individual plugin*. Meaning it *includes* the payload which is normally embedded within one of the elements of the *full* responses `data` property.


### Child Requests
List of child ajax requests associated with a given origin `requestId`.

_Input_:

    { requestId : '...', ajaxResults : true }

_Output_: Header Responses 

    [{ method : '...', duration : 111, browser : '...', clientName : '...', requestTime : '2011/11/09 12:00:00', requestId : '...', parentId : '...', isAjax : true, url : '...' },
        ...,
    { method : '...', duration : 111, browser : '...', clientName : '...', requestTime : '2011/11/09 12:00:00', requestId : '...', parentId : '...', isAjax : true, url : '...' }, ]

_Note_: The data returned represents the requests *header* response. Meaning it *only* includes the header.

_Enhancements_: A client should be able to indicate what previous ajax requests is knows about. Meaning, if the server was previously asked for a list of child requests and it returned 5 results, next time it is called, if there are 2 new child requests, only the 2 new results should be returned instead of all 7.


### History
List of all the requests that the glimpse storage provider knows about.

_Input_:

    {}

_Output_: Client Header Responses


    {
        '' :  [ ... ],
        'iPhone' :   [{ method : '...', duration : 111, browser : '...', clientName : '...', requestTime : '2011/11/09 12:00:00', requestId : '...', parentId : '...', isAjax : true, url : '...' },
                        ...,
                        { method : '...', duration : 111, browser : '...', clientName : '...', requestTime : '2011/11/09 12:00:00', requestId : '...', parentId : '...', isAjax : true, url : '...' }, ],
        'Remote' : [ ... ]
    }
      
_Note_: The data returned represents a requests *header* response. Meaning it *only* includes the header. It is grouped by the *client* that the request originated from. 

_Enhancements_: A client should be able to indicate what previous requests is knows about. Meaning, if the server was previously asked for a list of requests and it returned 5 results, next time it is called, if there are 2 new requests, only the 2 new results should be returned instead of all 7.