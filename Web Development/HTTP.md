

![[http_illustration.png]]


## What is HTTP (Hypertext Transfer Protocol) ?

HTTP (Hypertext Transfer Protocol) is the foundation of data communication for the World Wide Web. It's a protocol used for transmitting hypermedia documents, such as HTML. It follows a classical client-server model where the client opens a connection to make a request, and then waits until it receives a response from the server.

## How Does HTTP Work?

### [[HTTP Request | Request]]-[[HTTP Response | Response]] Cycle

1. **Client Request**: A client (usually a web browser) sends an HTTP request to the server. This request includes a method (like GET, POST), the path of the resource, headers containing additional information, and sometimes a body with data.
2. **Server Response**: The server processes the request, retrieves the necessary resources, and sends back an HTTP response. The response contains a status code indicating the outcome of the request, headers with additional information, and often a body with the requested content.



![[http_request_response_cycle.webp]]

### Methods

- **GET**: Requests data from a specified resource.
- **POST**: Submits data to be processed to a specified resource.
- **PUT**: Updates a specified resource.
- **DELETE**: Deletes a specified resource.

### [[Status Codes]]

- **200 OK**: The request has succeeded.
- **301 Moved Permanently**: The resource has been moved to a new URL.
- **404 Not Found**: The server cannot find the requested resource.
- **500 Internal Server Error**: A generic error occurred on the server.

## Components of an [[HTTP Request]]

1. **Method**: Indicates the action to be performed (GET, POST, etc.).
2. **URL**: The location of the resource on the server.
3. **HTTP Version**: Specifies the version of HTTP being used.
4. **Headers**: Provide additional information about the request (like User-Agent, Accept-Language).
5. **Body**: Contains data sent to the server (mainly used in POST and PUT requests).

## Components of an [[HTTP Response]]

1. **Status Line**: Includes the HTTP version, status code, and status text.
2. **Headers**: Provide additional information about the response (like Content-Type, Content-Length).
3. **Body**: Contains the requested content or data.

## Why is HTTP Important?

- **Ubiquity**: Almost every internet user relies on HTTP to browse the web.
- **Simplicity**: HTTP is simple enough for beginners to understand but powerful enough for complex applications.
- **Statelessness**: Each request is independent, which simplifies the server design.
- **Extensibility**: HTTP can be extended to support new features.

## Security Considerations

Since HTTP is not encrypted, data sent over HTTP is susceptible to interception and tampering. This is why HTTPS (HTTP Secure) was developed. It uses TLS/SSL encryption to protect the data transmitted between the client and the server.

## Conclusion

HTTP is a foundational protocol for the web, enabling the transfer of hypertext documents. It's the bridge between clients and servers, allowing them to communicate and exchange data. Understanding HTTP is crucial for web development, as it's involved in virtually every interaction on the web. Whether some is browsing a website, logging into an account, or posting on social media, HTTP is working behind the scenes to make it all happen.

## Resources
https://www.apinewbies.com/api-request/