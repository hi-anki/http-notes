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
  - [MIME Types](#mime-types)
    - [Structure](#structure)
    - [Classification Of `type`](#classification-of-type)
      - [Discrete `type`](#discrete-type)
      - [Multipart `type`](#multipart-type)
    - [Important MIME TYpes](#important-mime-types)
    - [Note: Legacy JavaScript MIME Types](#note-legacy-javascript-mime-types)
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

## MIME Types
+ A media type (also known as a **Multipurpose Internet Mail Extensions or MIME type**) indicates the nature and format of a document, file, or assortment of bytes.
+ Browsers use MIME type, not the file extension, to determine how to process a URL, so it's important that web servers send the correct MIME type in the response's `Content-Type` header.

### Structure
```
type/sub_type

type/subtype;parameter=value
```

+ The type represents the general category in which the data type falls.
+ The subtype identifies the exact kind of data.
+ An optional parameter can be added to provide additional details:
  ```
  type/subtype;parameter=value
  ```
  > For example, for any MIME type whose main type is text, an optional `charset` parameter can be used to specify the character-set used for the characters in the data. If no `charset` is specified, the default is `ASCII (US-ASCII)` unless overridden by the user agent's settings. To specify a UTF-8 text file, the MIME type `text/plain;charset=UTF-8` is used.
----
+ MIME types are case-insensitive but are traditionally written in lowercase. The parameter values can be case-sensitive.

### Classification Of `type`
There are two classes of type: **discrete** and **multipart**

#### Discrete `type`
Discrete type includes types which represent a single file or medium, such as a single text or music file.

1. `application`
   + Any kind of binary data that doesn't fall explicitly into one of the other types.
   + Any data that requires specialized applications to interpret it.
   + For binary documents without a specific or known subtype, `application/octet-stream` should be used.
2. `audio`
3. `example`
   + Reserved for use as a placeholder in examples showing how to use MIME types.
   + It can also be used a sub_type.
4. `font`
5. `image`
6. `model`, includes model data for a 3D object or scene.
7. `text`
   + Includes any human-readable data.
   + For text documents without a specific subtype, `text/plain` should be used
8. `video`
9. `haptics`
10. `message`

#### Multipart `type`
+ A multipart type represent a composite document, a document that's comprised of multiple component parts, each of which may have its own individual MIME type; or, a multipart type may encapsulate multiple files being sent together in one transaction. 
  + For example, multipart MIME types are used when attaching multiple files to an email.
+ Except for `multipart/form-data`, which is used in `POST` requests to submit HTML Forms, and `multipart/byteranges`, used with `206 Partial Content` to send part of a document, HTTP doesn't handle multipart documents in a special way: the message is transmitted to the browser (which will likely show a "Save As" window if it doesn't know how to display the document).

+ It is further divided into two types:
  1. `message`
     + A message that encapsulates other messages. This can be used, for instance, to represent an email that includes a forwarded message as part of its data.
  2. `multipart`
     + Data that consists of multiple components which may individually have different MIME types.

### Important MIME TYpes
1. **application/octet-stream**
   + This is the default for binary files.
   + As it means unknown binary file, browsers usually don't execute.
   + They treat it as if the `Content-Disposition` header was set to `attachment`, and propose a "**Save As**" dialog.

2. `text/plain`
   + This is the default for textual files.
   + **Note:** `text/plain` does not mean "any kind of textual data." If they expect a specific kind of textual data, they will likely not consider it a match.

3. `text/css`
4. `text/html`
5. `text/javascript`: JavaScript content should be strictly served using this MIME type.
6. `image/jpg`
7. `image/jpeg`
8. `image/png`
9. `image/webp`
10. `image/gif`
11. `multipart/form-data`, submitting HTML Form data.

### Note: Legacy JavaScript MIME Types
The MIME Sniffing Standard also allows JavaScript to be served using any of the following legacy JavaScript MIME types:
+ `application/javascript` (Deprecated)
+ `application/ecmascript` (Deprecated)
+ `application/x-ecmascript` (Non-standard)
+ `application/x-javascript` (Non-standard)
+ `text/ecmascript` (Deprecated)
+ `text/javascript1.0` (Non-standard)
+ `text/javascript1.1` (Non-standard)
+ `text/javascript1.2` (Non-standard)
+ `text/javascript1.3` (Non-standard)
+ `text/javascript1.4` (Non-standard)
+ `text/javascript1.5` (Non-standard)
+ `text/jscript` (Non-standard)
+ `text/livescript` (Non-standard)
+ `text/x-ecmascript` (Non-standard)
+ `text/x-javascript` (Non-standard)

# Reference
+ [HTTP Reference, on MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP)
+ [Stateless Protocol, Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)
+ [Complete List Of MIME Types, IANA Registry](https://www.iana.org/assignments/media-types/media-types.xhtml)