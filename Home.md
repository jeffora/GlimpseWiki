# Table of Contents

Glimpse allows you to take a never before seen look inside your server. Instead of forcing you to go line by line inside your code, Glimpse does the work for you. It tells you exactly what's going on with each request. It's able to summarize your server data in a way that facilitates ease of understanding. Glimpse is the tool that not only lets you follow your code, it combines and displays your data in such a way that the debugging process that once took hours can now take minutes. 

## Usage

## API

### Endpoints
The relationship between the client and server is facilitated by the client being able to depend on the server providing a fixed set of Resource Endpoints. These endpoints server not only content assets like images, scripts, etc but there is a Data Endpoint that's sole purpose is to provide the client with the request data it needs to operate. 

  * [[Resource|Resource Endpoints]] (draft)
  * [[Data|Data Endpoints]] (draft)

### Schema
Part of what makes Glimpse simple for Plugin authors to get their data rendering in Glimpse, is the simplicity of the Glimpse Schema. This Schema is very logical and easy to follow. By default, it will render a table of results that looks how the data is structured in Json. For the actual specification please see the below.

  * [[Simple|Glimpse Simple Layout]] (draft)
  * [[Structured|Glimpse Structured Layout]] (draft)