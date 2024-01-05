# [[HTTP]] Status Codes

## Introduction to Status Codes

HTTP status codes are standardized codes in the HTTP response message that indicate the status of the request. They provide a way for the server to tell the client whether the request was successful, if there was an error, or if something else happened. Understanding these codes is essential for both API developers and consumers to handle responses correctly.

## Categories of Status Codes

### 1xx: Informational
These status codes indicate a provisional response, requiring the requester to continue the action.

- **100 Continue**: The server has received the request headers, and the client should proceed to send the request body.
- **101 Switching Protocols**: The requester has asked the server to switch protocols, and the server acknowledges.

### 2xx: Success
These indicate that the client's request was received, understood, and accepted.

- **200 OK**: The request has succeeded. The meaning of success varies depending on the HTTP method.
- **201 Created**: The request has been fulfilled, resulting in the creation of a new resource.
- **202 Accepted**: The request has been accepted for processing, but the processing has not been completed.
- **204 No Content**: The server successfully processed the request, but is not returning any content.

### 3xx: Redirection
These indicate that further action needs to be taken by the user agent to fulfill the request.

- **301 Moved Permanently**: The requested resource has been assigned a new permanent URI.
- **302 Found**: The server is currently responding to the request with a different URI.
- **304 Not Modified**: Indicates that the resource has not been modified since the last request.

### 4xx: Client Error
These are intended for situations in which the client seems to have erred.

- **400 Bad Request**: The server cannot or will not process the request due to something perceived as a client error.
- **401 Unauthorized**: Authentication is required, and it has failed or not yet been provided.
- **403 Forbidden**: The server understood the request but refuses to authorize it.
- **404 Not Found**: The requested resource was not found on the server.
- **429 Too Many Requests**: The user has sent too many requests in a given amount of time ("rate limiting").

### 5xx: Server Error
These indicate that the server failed to fulfill an apparently valid request.

- **500 Internal Server Error**: A generic error message, given when an unexpected condition was encountered.
- **501 Not Implemented**: The server either does not recognize the request method or lacks the ability to fulfill it.
- **503 Service Unavailable**: The server is currently unable to handle the request due to temporary overloading or maintenance.

## Practical Application Example: E-commerce Platform

### Scenario: User Checkout
- **Request**: `POST /checkout`
- **200 OK**: The checkout process was successful.
- **400 Bad Request**: The request was malformed (e.g., missing required fields).
- **401 Unauthorized**: User needs to log in to proceed.
- **500 Internal Server Error**: Unexpected error occurred processing payment.

### Scenario: Product Search
- **Request**: `GET /products/search?q=shirt`
- **200 OK**: Search successful, products returned.
- **404 Not Found**: No products matched the search criteria.
- **429 Too Many Requests**: User has exceeded the rate limit for searches.

## Best Practices

- **Clear Communication**: Use the most specific status code available to convey the result of the request to the client.
- **Handle Errors Gracefully**: Client-side, use status codes to determine how to proceed and provide meaningful feedback to the user.
- **Document Your API**: Clearly document what status codes your API returns and under what conditions.

## Conclusion

HTTP status codes are a critical part of web communication, providing a quick and standardized way to understand the result of HTTP requests. By properly using these codes, developers can create more reliable and understandable applications. Whether you're building or consuming APIs, a good understanding of HTTP status codes will improve your web development skills.