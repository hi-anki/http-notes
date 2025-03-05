# HyperText Transfer Protocol (HTTP), An Introduction
- [HyperText Transfer Protocol (HTTP), An Introduction](#hypertext-transfer-protocol-http-an-introduction)
  - [HTTP Request](#http-request)
    - [Request Methods](#request-methods)
    - [Request Target Specification](#request-target-specification)
      - [1. Origin Form](#1-origin-form)
      - [2. Absolute Form](#2-absolute-form)
      - [3. Authority Form](#3-authority-form)
      - [4. Asterisk Form](#4-asterisk-form)
  - [HTTP Response](#http-response)
    - [HTTP Response Status Code](#http-response-status-code)
  - [HTTP Headers](#http-headers)
- [Reference](#reference)

+ An application layer protocol.
+ Works on client-server model, where a user initiates a request using their browser (`user-agent`) and a server responds to that request. These messages are called **request** and **response**.
  > **Note:** Browsers aren't the only `user-agent`. Utilities like `curl`, `python-requests` have their own `user-agent`.
+ Between the Web browser and the server, numerous computers and machines relay the HTTP messages.
  + Those operating at the application layers are generally called proxies.
  + These can be transparent, forwarding on the requests they receive without altering them in any way, or non-transparent, in which case they will change the request in some way before passing it along to the server.
+ HTTP is used for fetching resources such as HTML documents. It is the foundation of any data exchange on the Web
+ HTTP is also extensible with the introduction of `headers`.
+ HTTP is stateless but not a sessionless protocol.
  + A stateless protocol means there is no link between the two requests being successively carried out on the same connection.
  + HTTP cookies allow the use of stateful sessions. Using HTTP headers, HTTP Cookies are added to the workflow, allowing session creation on each HTTP request to share the same context, or the same state.
+ HTTP relies on a TCP connection.
  + Before a client and a server can exchange an HTTP request/response pair, they must establish a TCP connection, a process which requires several round-trips. 
  + The default behavior of `HTTP/1.0` is to open a separate TCP connection for each HTTP request/response pair. This is less efficient than sharing a single TCP connection when multiple requests are sent in close succession.
+ HTTP connections before `HTTP/2` were human readable as they were in plain-text format. With HTTP/2, these simple messages are encapsulated in frames, making them impossible to read directly.
+ With TCP, default HTTP port is 80.

## HTTP Request
+ An HTTP Request is made up of three blocks:
  1. The first line is **the status line**. It consists of the 3 things:
     1. Request method.
     2. Absolute path to the target, without the protocol or domain name.
     3. HTTP version being used.
  2. Subsequent lines represent an HTTP header, giving the server information about what type of data is appropriate. 
  3. An empty line indicating the end of HTTP headers.
  4. An optional data block. Only a POST, PATCH and PUT reuqest can have a request body.
+ The start-line and the headers are collectively known as the **head of the request**, and the part afterwards that contains its content is known as the **body**.

+ General Structure:
  ```
  METHOD PATH HTTP/version
  Headers

  Data Block
  ```

+ A `GET` Request:
  ```
  GET / HTTP/1.1
  Host: developer.mozilla.org
  Accept-Language: fr

  ```

+ a `POST` Request:
  ```
  POST /login.php HTTP/1.1
  Host: example.com
  Content-Length: 64
  Content-Type: application/x-www-form-urlencoded

  name=Xyz%20User&request=Send%20me%20one%20of%20your%20catalogue
  ```

### Request Methods
Also referred as HTTP Verbs.

| Method | Description |
| - | - |
| `GET` | Retreive a resource from a web server|
| `POST` | Send data to a server |
| `PUT` |
| `PATCH` |
| `OPTIONS` |
| `` |

### Request Target Specification
#### 1. Origin Form
+ The recipient combines an absolute path with the information in the Host header.
+ A query string can be appended to the path for additional information, in `key=value` pairs.

#### 2. Absolute Form
+ The complete URL is mentioned, including the scheme.

#### 3. Authority Form
+ It contains the authority and port separated by a colon. It is only used with the CONNECT method when setting up an HTTP tunnel.
+ Example: `CONNECT developer.mozilla.org:443 HTTP/1.1`

#### 4. Asterisk Form
+ It is only used with OPTIONS when you want to represent the server as a whole (*).
+ Example: `OPTIONS * HTTP/1.1`

## HTTP Response
+ A server response is also made up of 3 sections.
  1. The first line is **the status line**. It consists of 3 things:
     1. HTTP version being used, 
     2. Response status code,
     3. A brief description of the code (human-readable).
  2. Subsequent lines represent specific HTTP headers.
  3. An empty line indicating the end of HTTP headers.
  4. The response body. It is included in most of the messages when responding to a client.
+ General Syntax:
  ```
  HTTP/version STATUS_CODE DESCRIPTION
  Headers

  Data Block
  ```
+ Example:
  ```
  HTTP/1.1 200 OK
  Content-Type: text/html; charset=utf-8
  Content-Length: 55743
  Connection: keep-alive
  Cache-Control: s-maxage=300, public, max-age=0
  Content-Language: en-US
  Date: Thu, 06 Dec 2018 17:37:18 GMT
  ETag: "2e77ad1dc6ab0b53a2996dfd4653c1c3"
  Server: meinheld/0.6.1
  Strict-Transport-Security: max-age=63072000
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  X-XSS-Protection: 1; mode=block
  Vary: Accept-Encoding,Cookie
  Age: 7

  <!doctype html>
  <html lang="en">
  <head>
    <meta charset="utf-8">
    <title>A simple webpage</title>
  </head>
  <body>
    <h1>Simple HTML webpage</h1>
    <p>Hello, world!</p>
  </body>
  </html>

  ```

### HTTP Response Status Code
+ They indicate the status of an HTTP request.
+ Responses are grouped into five classes: 
  1. Informational Responses
  2. Successful Responses
  3. Redirects
  4. Client Errors
  5. Server Errors
+ Every class has a range of response codes, denoting different meaning.

## HTTP Headers
+ Headers are th metadata sent with a request.
+ Headers are **case-insensitive**.
+ General Syntax:
  ```
  Header: value
  ```
+ Example:
  ```
  Content-Length: 56
  ```
+ Some headers are exclusive to either requests or responses, while others can be used in both.

# Reference
+ [HTTP Reference, on MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP)
+ [Stateless Protocol, Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)