The Resource Endpoints API represent the contract that the Glimpse client expects the server to provide to access resources. This contract is important as the Glimpse Client is designed to work with multiple server implementations. This definition allows anyone implementing a Glimpse Service, to know what resources are expected, when given Http `GET` is received by the "Resource Endpoint" manager.

## Assets
Most assets are static and relativity simple in nature. The exception is the [Data Endpoint|Data Endpoint] which provides the client with the data models it needs to present the user with renderings of the captured data.

### Config
Location of the [Config Page|Config Page]. Used so that the client can provide outbound links for the user to this resource in the rendered interface.

### Logo
Location of the [Logo|Resource Logo]. Used within the rendered interface for branding.

### Sprite
Location of the [Sprite|Resource Sprite]. Used within the rendered interface to provide a high fidelity experience.

### Paging (Draft)
Location of the [Paging Endpoint|Paging Endpoint]. This is used by plugins that gather more data records than what practically makes sense to send to the client in the initial payload. 

### Popup
Location of the [Popup Page|Popup Page]. Used so that the client can provide its popup functionality.

### Data
See [Data Endpoint|Data Endpoint] documentation for the formal specification.

## Client Integration
