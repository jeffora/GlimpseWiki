The Resource Endpoints API represent the contract that the Glimpse client expects the server to provide to access resources. This contract is important as the Glimpse Client is designed to work with multiple server implementations. This definition allows anyone implementing a Glimpse Service, to know what resources are expected, when given Http `GET` is received by the "Resource Endpoint" manager.

## Assets
Most assets are static and relativity simple in nature. The exception is the [[Data Endpoint|Data Endpoint]] which provides the client with the data models it needs to present the user with renderings of the captured data.

### Config
Location of the [[Config Page|Config Page]]. Used so that the client can provide outbound links for the user to this resource in the rendered interface.

### Logo
Location of the [[Logo|Resource Logo]]. Used within the rendered interface for branding.

### Sprite
Location of the [[Sprite|Resource Sprite]]. Used within the rendered interface to provide a high fidelity experience.

### Paging
Location of the [[Paging Endpoint|Paging Endpoint]]. This is used by plugins that gather more data records than what practically makes sense to send to the client in the initial payload. *Note this is a Draft specification as it may end up being merged into the Data Endpoint.*

### Popup
Location of the [[Popup Page|Popup Page]]. Used so that the client can provide its popup functionality.

### Data
See [[Data Endpoint|Data Endpoint]] documentation for the formal specification.

## Client Integration
For the client to be able to leverage these assets/endpoint, the client needs to know where they exist. That said, depending on the server implementation the resources may reside at different Http addresses. To cater for this occurrence, the client determines where to retrieve resources from via the `paths` property in the initial [Glimpse Metadata] provided to the client (see the [Glimpse Metadata] model described in the [[Data Endpoint|Data Endpoint]]) API for more details).  