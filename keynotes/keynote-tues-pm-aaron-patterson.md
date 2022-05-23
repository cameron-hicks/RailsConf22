# Keynote: Aaron Patterson

a.k.a. Tenderlove

New features in Rails 7:

- got rid of Spring
- import maps and es modules
- hotwire default
- turbo 7 default
- stimulus 3 default
- js bundling
- css bundling
- ^^ all of these are intended to make frontend development easier for backend devs

Rack:

- an old library that defines an interface between a webserver and an application so you can easily swap out webservers
- Rack + HTTP and Rack + TCP
- timeline of when http, http2, rack, and websockets were released
- in HTTP 1: when you make a request, the client opens a TCP connection with the server, sends the HTTP request, receives the HTTP response, then tears down the TCP connection.
  - however, connections are expensive; so people started using `keep-alive` headers to keep the TCP connection open for multiple request/response cycles.
  - a client can keep open multiple connections; requests are still serial, but on parallel connections, so you can still do stuff in parallel (in http 1.1, the docs recommended 2 connections, but people hacked on more)
    - limits to the # of connections lead to the strategy of bundling static assets, storing images as sprites, etc.
- at this time Rack = "HTTP translation"; converts the http request into a ruby data structure and back
- Websockets: allowed for bidirectional communication btw client and server
- Rack becaome an http translator and a kind of "websocket getter"
- http2 encouraged the use of ssl and all browsers implement it that way (but you technically don't have to)
  - it's also a binary protocol now (meaning all the requests are compiled down to and executed in machine code?)
  - also introduced request/response multiplexing (concurrent requests on the same socket)
    - makes http2 good for import maps b/c don't have to wait for requests to finish anymore before making a new one
  - Rails 7 uses http2 by default
  - push promises (?) — [wikipedia](https://en.wikipedia.org/wiki/HTTP/2_Server_Push)
- http3 (2020)
