- [HyperText Transfer Protocol (HTTP)](#hypertext-transfer-protocol-http)
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
      - [1. Discrete `type`](#1-discrete-type)
      - [2. Multipart `type`](#2-multipart-type)
    - [Important MIME TYpes](#important-mime-types)
    - [Note: Legacy JavaScript MIME Types](#note-legacy-javascript-mime-types)
    - [MIME Sniffing](#mime-sniffing)
    - [File Type Interpretation Methods](#file-type-interpretation-methods)
  - [Compression In HTTP](#compression-in-http)
    - [File Format Compression](#file-format-compression)
    - [End-to-End Compression](#end-to-end-compression)
  - [Caching](#caching)
    - [Cache Types](#cache-types)
    - [Stored HTTP Response State](#stored-http-response-state)
    - [Validation](#validation)
    - [Force Revalidation](#force-revalidation)
    - [Don't Cache](#dont-cache)
  - [Authentication](#authentication)
    - [Proxy Authentication](#proxy-authentication)
    - [Access Forbidden](#access-forbidden)
    - [Authentication Method](#authentication-method)
    - [Authentication Schemes](#authentication-schemes)
    - [Basic HTTP Authentication (RFC 7617)](#basic-http-authentication-rfc-7617)
  - [Cookies](#cookies)
    - [Creating Cookies](#creating-cookies)
    - [Updating Cookies](#updating-cookies)
    - [Types Of Cookies (On The Basis Of Their Life)](#types-of-cookies-on-the-basis-of-their-life)
    - [Cookie Recreation](#cookie-recreation)
    - [Security Aspect](#security-aspect)
      - [Some Knowledge](#some-knowledge)
  - [Redirections In HTTP](#redirections-in-http)
    - [Types Of Redirects](#types-of-redirects)
      - [1. Permanent Redirection](#1-permanent-redirection)
      - [2. Temporary Redirection](#2-temporary-redirection)
      - [3. Special Redirection](#3-special-redirection)
    - [Methods Of Redirection](#methods-of-redirection)
      - [1. HTTP Redirection](#1-http-redirection)
      - [2. HTML Redirection](#2-html-redirection)
      - [3. JavaScript Redirection](#3-javascript-redirection)
    - [Order Of Precedence](#order-of-precedence)
    - [Use Cases](#use-cases)
      - [1. Domain Aliasing](#1-domain-aliasing)
      - [2. Website Restructuring](#2-website-restructuring)
      - [3. Temporary Redirects To Unsafe Requests](#3-temporary-redirects-to-unsafe-requests)
  - [Proxy Servers](#proxy-servers)
    - [1. Forward Proxy](#1-forward-proxy)
    - [2. Reverse Proxy](#2-reverse-proxy)
    - [HTTP Tunneling](#http-tunneling)
    - [How It Is Done?](#how-it-is-done)
    - [Proxy Auto-Configuration (PAC)](#proxy-auto-configuration-pac)
    - [Common Use Cases](#common-use-cases)
  - [HTTP Cient Hints](#http-cient-hints)
    - [Low Entropy Client Hints](#low-entropy-client-hints)
    - [High Entropy Client Hints](#high-entropy-client-hints)
    - [Critical Client Hints](#critical-client-hints)
- [Reference](#reference)
- [Other Useful Resources](#other-useful-resources)

# HyperText Transfer Protocol (HTTP)
+ Application layer protocol.
+ Works on client-server model, where a user initiates a request using their browser (`user-agent`) and a server responds to that request. These messages are called **request** and **response**.
  > **Note:** Browsers aren't the only `user-agent`. Utilities like `curl`, `python-requests` have their own `user-agent`.

+ Between the Web browser and the server, numerous computers relay these HTTP messages.
  + Those operating at the application layer are generally called **proxies**.
  + These can be **transparent**, forwarding on the requests they receive without altering them in any way, or **non-transparent**, in which case they will change the request in some way before passing it along to the server.

+ It is the foundation of any data exchange happening on the Web.
+ HTTP is also extensible with the introduction of `headers`.

+ HTTP is stateless but not a sessionless protocol.
  + A stateless protocol means there is no link between the two requests being successively made on the same connection.
  + HTTP cookies allow the use of stateful sessions. Using HTTP headers, HTTP Cookies are added to the workflow, allowing session creation on each HTTP request to share the same context/state.

+ HTTP relies on a TCP connection.
  + Before a client and a server can exchange messages, they must establish a TCP connection. 
  + The default behavior of `HTTP/1.0` is to open a separate TCP connection for each HTTP request/response pair. This is less efficient than sharing a single TCP connection when multiple requests are sent in close succession.

+ HTTP connections before `HTTP/2` were human readable as they were in plain-text format. With HTTP/2, these simple messages are encapsulated in frames, making them impossible to read directly.

+ HTTP(over TCP):80
+ HTTPS(over TCP):443

## HTTP Request
+ An HTTP Request is made up of three blocks:
  1. The first line is **the status line**. It consists of the 3 things:
     1. The request method.
     2. Absolute path to the target, without the protocol or domain name.
     3. HTTP version being used.
  2. Subsequent lines represent HTTP headers, giving the server information about what type of data is appropriate. 
  3. An empty line indicating the end of HTTP headers.
  4. A data block. Only a POST, PATCH and PUT reuqest can have a data block.

+ The start-line and the headers are collectively known as the **request head**, while the rest is known as the **body**.

+ Request Structure:
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

+ A `POST` Request:
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
| `GET` | Retreive a resource from a web server |
| `HEAD` | Retreives only the headers for a request |
| `POST` | Send data to a server |
| `PUT` |
| `DELETE` | Delete a resource |
| `CONNECT` | Establishes a tunnel to the server |
| `OPTIONS` | Retreives the avalable methods to connect with the server |
| `TRACE` | Performs a message loop-back test along the path to the target resource |
| `PATCH` | Make partial modifications to a resource |

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
+ A server response is made up of 3 sections.
  1. The first line is **the status line**. It consists of 3 things:
     1. HTTP version being used, 
     2. Response status code,
     3. A brief description of the code (human-readable).
  2. Subsequent lines represent HTTP headers.
  3. An empty line indicating the end of HTTP headers.
  4. The response body. It is included in most of the response messages.

+ Response Structure:
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
+ They tell the status of an HTTP request.
+ Responses are grouped into 5 classes: 
  1. Informational Responses
  2. Successful Responses
  3. Redirects
  4. Client Errors
  5. Server Errors
+ Every class has a range of codes, denoting different meaning.

## HTTP Headers
+ Headers are the metadata sent with a request.
+ They are **case-insensitive**.
+ Some headers are exclusive to either request or response, while others can be used in both.

+ General Syntax:
  ```
  Header: value
  ```

+ Example:
  ```
  Content-Length: 56
  ```

## MIME Types
+ It stands for **Multipurpose Internet Mail Extensions**. In simple words, it represent media types.
+ Browsers use MIME type, not the file extension, to determine how to process a URL, so it's important that web servers send the correct MIME type in the response's `Content-Type` header.

### Structure
```
type/sub_type

type/subtype;parameter=value
```

+ The **type** represents the general category in which the data type falls.
+ The **subtype** identifies the exact kind of data.
+ An optional parameter can be added to provide additional details:
  ```
  type/subtype;parameter=value
  ```
  > For example, any media of type **text** can have an optional `charset` parameter, to specify the character-set used in the data. The default character-set is `ASCII (US-ASCII)` unless overridden by the user agent. To specify a UTF-8 text file, we can use `text/plain;charset=UTF-8`
+ MIME types are case-insensitive but are generally written in lowercase. The parameter values can be case-sensitive.

### Classification Of `type`
#### 1. Discrete `type`
Discrete type includes types which represent a single file or medium, such as, a single text.

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

#### 2. Multipart `type`
+ A multipart type represent a composite document, a document that's made of multiple component parts, each of which may have its own individual MIME type; or, a multipart type may encapsulate multiple files being sent together in one transaction. 
  + For example, multipart MIME types are used when attaching multiple files to an email.

+ Except for `multipart/form-data`, which is used in `POST` requests to submit HTML Forms, and `multipart/byteranges`, used with `206 Partial Content` to send part of a document, HTTP doesn't handle multipart documents in a special way: the message is transmitted to the browser (which will likely show a "Save As" window if it doesn't know how to display the document).

+ It is further divided into two types:
  1. `message` : A message that encapsulates other messages. This can be used to represent an email that includes a forwarded message as part of its data.
  2. `multipart` : Data that consists of multiple components which may individually have different MIME types.

### Important MIME TYpes
1. `application/octet-stream`
   + Default for binary files.
   + It means unknown binary file, which the browser *usually* don't execute.
   + They treat it as if the `Content-Disposition` header was set to `attachment`, and propose a "**Save As**" dialog.

2. `text/plain`
   + Default for textual files.
   + **Note:** `text/plain` does not mean "any kind of textual data." If the server expect a specific kind of textual data, it will likely not consider it a match.

3. `text/css`
4. `text/html`
5. `text/javascript`: JavaScript content should be strictly served using this MIME type.
6. `image/jpg`
7. `image/jpeg`
8. `image/png`
9. `image/webp`
10. `image/gif`

11. `multipart/form-data`: used to submit HTML Forms.
    - It is delimited by a boundary, a string starting with a double dash `--`. Each part is its own entity with its own HTTP headers, Content-Disposition, and Content-Type for file uploading fields.
    - Example
      ```
      POST / HTTP/1.1
      Host: localhost:8000
      User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
      Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
      Accept-Language: en-US,en;q=0.5
      Accept-Encoding: gzip, deflate
      Connection: keep-alive
      Upgrade-Insecure-Requests: 1
      Content-Type: multipart/form-data; boundary=---------------------------8721656041911415653955004498
      Content-Length: 465

      -----------------------------8721656041911415653955004498
      Content-Disposition: form-data; name="myTextField"

      Test
      -----------------------------8721656041911415653955004498
      Content-Disposition: form-data; name="myCheckBox"

      on
      -----------------------------8721656041911415653955004498
      Content-Disposition: form-data; name="myFile"; filename="test.txt"
      Content-Type: text/plain

      Simple file.
      -----------------------------8721656041911415653955004498--

      ```

12. `multipart/byteranges`: used to send partial responses to the browser.

### Note: Legacy JavaScript MIME Types
The MIME Sniffing Standard also allows JavaScript to be served using any of the following legacy JavaScript MIME types:

**DEPRECATED**
+ `application/javascript`
+ `application/ecmascript`
+ `text/ecmascript`

**NON-STANDARD**
+ `application/x-ecmascript`
+ `application/x-javascript`
+ `text/javascript1.0`
+ `text/javascript1.1`
+ `text/javascript1.2`
+ `text/javascript1.3`
+ `text/javascript1.4`
+ `text/javascript1.5`
+ `text/jscript`
+ `text/livescript`
+ `text/x-ecmascript`
+ `text/x-javascript`

### MIME Sniffing
+ In the absence of a MIME type, or in cases where the browser believe that the proposed MIME type is incorrect, the browser tries to guess the correct MIME type by looking at the bytes of the resource.
+ Each browser performs MIME sniffing differently and under different conditions.
+ There are security concerns as some MIME types represent executable content. Servers can prevent MIME sniffing by sending the `X-Content-Type-Options` header.

### File Type Interpretation Methods
1. MIME types are the most preferred option.
2. Filename suffixes can be used sometimes, especially on **Microsoft Windows**. But there is no guarantee they are correct.
3. Magic Numbers. 
   + The syntax of different formats allows file-type inference by looking at their byte structure. 
   + For example, GIF files start with the `47 49 46 38 39` hexadecimal value (GIF89), and PNG files with `89 50 4E 47` (.PNG). 
   + Not all file types have magic numbers, so this is not 100% reliable either.

## Compression In HTTP
It can increases a website's performance and significantly reduce the network bandwidth consumption.

### File Format Compression
+ Each data type has some redundancy, wasted space, basically.
+ Compression algorithms used for files can be grouped into two broad categories:
  1. **Loss-less Compression:** Here, the compression-decompression cycle doesn't alter the data. Ex: gif or png.
  2. **Lossy Compression:** Here, the compression-decompression cycle alters the original data in a (hopefully) imperceptible way for the user. Video formats on the Web are lossy; the jpeg image format is also lossy.
+ Some formats can be used for both loss-less or lossy compression, like `webp`.
+ Lossy compression algorithms are usually more efficient than loss-less ones.

### End-to-End Compression
+ It yields the largest performance improvements for a website. 
+ It refers to a compression of message body that is done by the server and will last unchanged until it reaches the client. The intermediate nodes leave the body untouched. 
+ All modern browsers and servers support it. The only thing to negotiate is the compression algorithm to use. These algorithms are optimized for text.
+ The 2 most used algorithms are: `gzip` and `br`.
+ To select the algorithm, browsers and servers use **proactive content negotiation**. 
  + The browser sends an `Accept-Encoding` header with the algorithms it supports and its order of preferrence, the server picks one, uses it to compress the body of the response and uses the `Content-Encoding` header to tell the browser the algorithm it has chosen. 
  + As content negotiation has been used to choose a representation based on its encoding, the server must send a `Vary` header containing at least `Accept-Encoding` alongside this header in the response; that way, **caches** will be able to cache the different representations of the resource.

## Caching
+ HTTP cache stores a response associated with a request and reuses the stored response for subsequent requests. It has various advantages, such as:
  1. The closer the client and cache are, the faster the response will be. Sometimes, even the browser itself stores a cache for browser requests.
  2. Reduces the load on the server.

### Cache Types
1. **Private Cache:**
   + It is tied to a specific client, typically a browser cache. 
   + It is not shared with other clients, and can be used to store a personalized response for that user. 
   + If a response contains personalized content and you want it to only in a private cache, you must specify the **private** directive, `Cache-Control: private`
   + Personalized content is usually controlled by cookies, but the presence of a cookie does not always indicate that it is private, and thus a cookie alone does not make a response private.

2. **Shared Cache:**
   + It is usually stored in proxy servers and can be shared among users. 
   + It is further classified into **proxy caches** and **managed caches**.
   + **Proxy Cache:** 
   + **Managed Cache:** Managed caches are explicitly deployed by service developers to offload the origin server and to deliver content efficiently. Examples include reverse proxies, CDNs, and service workers in combination with the Cache API.

3. **Heuristic Cache:** HTTP is designed to cache as much as possible, so even if no `Cache-Control` is given, responses will get stored and reused if certain conditions are met. This is called heuristic caching.

### Stored HTTP Response State
1. **Fresh:** Indicates that the cached response is still valid and can be reused.
2. **Stale:** Indicates that the cached response has been expired.
----

+ The criteria for determining a response's state is its **age**, which is the time elapsed since the response was generated.

### Validation
+ Stale responses are not immediately discarded. HTTP has a mechanism to transform a stale response into a fresh one by asking the origin server. This is called **validation**, or sometimes, **revalidation**.
+ It is done using a **conditional request**, that includes an `If-Modified-Since` or `If-None-Match` request header.

+ A client can send a request with an `If-Modified-Since` request header to ask the server if there have been any changes made since the specified time.
  + If the server respond with a `304 Not Modified`, it indicates **no change**. 
  + The client reverts the stored stale response back to being fresh and can reuse it for previously specified timeline for freshness.

+ A client can also use an `ETag` value to do this.
  + The client can take the `ETag` value for the response header for the cached response, and puts it into the `If-None-Match` request header, to ask the server if the resource has been modified.
  + If the server respond with a `304 Not Modified`, it indicates **no change**. 
  + But if the server determines the requested resource should now have a different ETag value, the server will instead respond with a `200 OK` and the latest version of the resource.

### Force Revalidation
+ If you want to fetch the latest content from the server everytime, you can use the `no-cache` directive to perform force validation.
+ A `max-age=0` means that the response is immediately stale, and `must-revalidate` means that it must not be reused without revalidation, once it is stale — so, in combination, it is the same as `no-cache`.

### Don't Cache
To prevent caching of a response, use `no-store`.

## Authentication
+ RFC 7235 provides a general framework for access control and authentication in HTTP.
+ The server responds to a client with a `401 Unauthorized` response status and provides information on how to authorize with a `WWW-Authenticate` response header containing at least one challenge.
+ A client that wants to authenticate itself with the server can do so by including an `Authorization` request header with the credentials.
+ `UTF-8` is the character encoding used in an HTTP Authentication.
+ Earlier Firefox used `ISO-8859-1` encoding.

### Proxy Authentication
+ The same challenge-response mechanism is used.
+ A `407 Proxy Authentication Required` is issued.
+ The `Proxy-Authenticate` response header contains at least one challenge applicable to the proxy.
+ The `Proxy-Authorization` request header is used for providing the credentials to the proxy server.

### Access Forbidden
+ If a (proxy) server receives invalid credentials, it should respond with a `401 Unauthorized` or with a `407 Proxy Authentication Required`, and the user may send a new request or replace the `Authorization` header field.
+ If a (proxy) server receives valid credentials that are inadequate to access a given resource, the server should respond with the `403 Forbidden` status code.
+ A `404 Not Found` status code can be used to hide the existence of the page to a user without adequate privileges or not correctly authenticated.

### Authentication Method
+ The `WWW-Authenticate` and `Proxy-Authenticate` response headers define the authentication method that should be used to gain access to a resource. The syntax:
  ```
  WWW-Authenticate: AUTH_SCHEME realm=<realm>
  Proxy-Authenticate: AUTH_SCHEME realm=<realm>
  ```
  > **realm** refers to the space the user is trying to get access to.

+ The `Authorization` and `Proxy-Authorization` request headers contain the credentials to authenticate a user agent with a (proxy) server. The syntax:
  ```
  Authorization: AUTH_SCHEME <credentials>
  Proxy-Authorization: AUTH_SCHEME <credentials>
  ```
  > The **credentials** can be encoded or encrypted depending on the authentication scheme.

### Authentication Schemes
IANA maintains a list of authentication schemes, but there are other schemes offered by different host services, such as Amazon AWS. 

IANA maintained authentication schemes include:
1. `Basic`
2. `Bearer` (bearer tokens to access OAuth 2.0-protected resources)
3. `Concealed`
4. `Digest`
5. `DPoP`
6. `GNAP`
7. `HOBA` (HTTP Origin-Bound Authentication, digital-signature-based)
8. `Mutual`
9. `Negotiate/NTLM`
10. `OAuth`
11. `Private Token`
12. `SCRAM-SHA-1`
13. `SCRAM-SHA-256`
14. `Vapid`

Amazon AWS Server Authentication (`AWS4-HMAC-SHA256`)

### Basic HTTP Authentication (RFC 7617)
The Basic authentication scheme sends the credentials as **userId/password** pairs, base64 encoded but not encrypted. It is completely insecure unless the exchange is done over a secure connection like HTTPS/TLS.

## Cookies
+ It is a small piece of data a server sends to a web browser.
+ Cookies enable web applications to store limited data and remember state information.
+ Typically, the server will use cookies to determine whether the different requests are coming from the same browser/user and then issue a personalized or generic response as appropriate.
+ Cookies are mainly used for three purposes:
  1. **Session management**: User sign-in status, shopping cart contents, game scores, or any other user session-related details that the server needs to remember.
  2. **Personalization**: User preferences such as display language and UI theme.
  3. **Tracking**: Recording and analyzing user behavior.
+ Browsers are generally limited to a maximum number of cookies per domain (varies by browser, generally in the 100s), and a maximum size per cookie (usually 4KB).
+ Cookies are sent with every request, so they can worsen performance on low speed connections, especially if a lot of cookies are set.

### Creating Cookies
+ Cookies are set using the `Set-Cookie` header. 
+ Syntax: `Set-Cookie: <cookie-name>=<cookie-value>`

+ Example:
  ```
  HTTP/2.0 200 OK
  Content-Type: text/html
  Set-Cookie: yummy_cookie=chocolate
  Set-Cookie: tasty_cookie=strawberry

  page-body

  ```

+ To set cookies using JavaScript:
  ```js
  document.cookie = "yummy_cookie=chocolate";
  ```

### Updating Cookies
+ To update a cookie via HTTP, the server can send a `Set-Cookie` header with the existing cookie's name and a new value. For example:
  ```
  Set-Cookie: id=new-value
  ```

+ To update a cookie via JavaScript:
  ```js
  document.cookie = "yummy_cookie=new_value";
  ```
  **Note:** To do this in browser console, it is important that the `HttpOnly` attribute isn't set on the cookie.

### Types Of Cookies (On The Basis Of Their Life)
1. **Permanent Cookies:** These cookies are deleted after the date specified in the `Expires` attribute or after the period specified in the `Max-Age` attribute. Example: 
   ```
   Set-Cookie: id=a3fWa; Expires=Day, Date Month Year HH:MM:SS TIME_ZONE;
   Set-Cookie: id=a3fWa; Expires=Thu, 31 Oct 2021 07:28:00 GMT;

   Set-Cookie: id=a3fWa; Max-Age=seconds
   Set-Cookie: id=a3fWa; Max-Age=2592000
   ```
   **Note:** `Max-Age` is less error-prone, and takes precedence when both are set.
   > The rationale behind this is that when you set an Expires date and time, they're relative to the client the cookie is being set on. If the server is set to a different time, this could cause errors.

2. **Session Cookies:** These are the cookies that contain NO `Expires` or `Max-Age` attributes and are deleted when the current session ends. The browser defines when the "current session" ends.
   + **Note 1:** Some browsers use session restoring when restarting. This can cause session cookies to last indefinitely.
   + **Note 2:** If your site authenticates users, it should regenerate and resend session cookies, even ones that already exist, whenever a user authenticates. This approach helps prevent **session fixation attacks**, where a third-party can reuse a user's session.

### Cookie Recreation
+ There are some techniques designed to recreate cookies after they're deleted. These are known as **zombie cookies**.
+ These techniques violate user privacy and control, data privacy regulations, and could expose a website using them to legal liability.

### Security Aspect
+ By default, all the cookie are visible to, and can be changed by, the end user.
+ A cookie with the `Secure` attribute is only sent via **HTTPS**.
  + A site with HTTP protocol can not send such cookies.
  + It increases the complexity for MITM attackers. But it is not an absolute win.
  + For example, someone with access to the client's hard disk (or JavaScript if the HttpOnly attribute isn't set) can read and modify the information.
+ A cookie with the `HttpOnly` attribute can't be accessed by JavaScript, meaning, `document.cookie` is not possible; it can only be accessed when it reaches the server. 
  + Cookies that persist user sessions should always have the `HttpOnly` attribute set. 
  + This helps mitigating **cross-site scripting (XSS) attacks**.
+ JWT is another authentication mechanism which we can explore.
+ The `Domain` and `Path` attributes define the scope of a cookie: what URLs the cookies are sent to.

----
#### Some Knowledge
+ A **cross-site request** is a request that changes our current domain to another domain. Example: Opening an affiliate link from the description of a **youtube.com** video that points to **amazon.com**. This is a cross-site request. While the requests that keeps us on the same domain are **same-site requests**.
+ A **top-level navigation** is when you directly click on a link.
+ A **sub-request** is a request made by your browser on the behalf of the current domain to fetch certain resources that are to be displayed on the current page.
+ Example:
  + You visited on `amazon.com` from `youtube.com` by clicking on an affiliate link in a youtube video's description is an example of a *top-level navigation*.
  + Amazon instructed your browser to get some info from `youtube.com` or `instagram.com` to personalize the content being displayed to you, these are *sub-requests*.
+ Cookie prefixes are specific strings added at the beginning of a cookie name to indicate certain special behaviors.
----

+ The `SameSite` attribute lets servers specify if the browser should send the cookies associated with this domain in cross-site requests.
  + It provides some protection against **cross-site request forgery (CSRF)** attacks.
  + It takes three possible values: Strict, Lax, and None:
    1. `Strict` won't send your cookies in cross-site requests.
    2. `Lax` will allow cookies to be sent in cross-site requests when it is **top-level navigation**. And it will not send any cookie for **sub-requests**.
    3. `None` means no restriction. The Secure attribute must be set here.
  + **Note:** If no SameSite attribute is set, the cookie is treated as `Lax` by default.

+ To confine the cookie to a certain domain(s) or sub-domain(s), use the `domain` and `path` attributes.
  + If the domain attribute is not specified, the cookie is only available to the domain that set it.
  + The path attribute specifies the URL path for which the cookie is valid. It controls which part of the website the cookie is accessible to. If you set the path attribute to /, the cookie is available for the entire domain. If you set it to a specific path (like /account), the cookie will only be sent for URLs that match that path or are under it.

+ Cookie Prefixes:
  + Because of the design of the cookies, a server can't confirm that a cookie was set from a secure origin or even tell where a cookie was originally set. 
  + A vulnerable application on a subdomain can set a cookie with the Domain attribute, which gives access to that cookie on all other subdomains. This mechanism can be abused in a session fixation attack.
  + A cookie with `__Secure-` prefix is only sent over HTTPS connections. This is a security measure to prevent cookies from being exposed over unencrypted connections (HTTP). For this, the cookie must have the `secure` attribute and should be sent from a secure origin. Example: `__Secure-SessionId`
  + A cookie with `__Host-` prefix must
    + be set with the `Secure` and `SameSite=Strict` attributes, 
    + come from a secure origin, 
    + not have the `domain` attribute, and 
    + have the path attribute set to `/`. 
    
    This prefix ensures that the cookie is protected from *cross-site request forgery (CSRF) attacks*. In other words, it makes the cookie **domain-locked**. Example: `__Host-UserId`

## Redirections In HTTP
+ URL redirection, also known as URL forwarding, is a technique to give more than one URL address to a webpage. HTTP has a special kind of response, called a HTTP redirect, for this.
+ Redirects have numerous advatanges, such as:
  1. Temporary redirects during site maintenance or downtime.
  2. Permanent redirects to preserve existing links after changing the site's URLs.
+ Redirection is triggered when a server responds with a **redirect response**. Such response has a `3XX` status code and a `Location` header holding the URL to redirect to.
+ When browsers receive a redirect, they immediately load the new URL provided in the Location header.

### Types Of Redirects
#### 1. Permanent Redirection
These are meant to last forever. They imply that the original URL should no longer be used, and replace it with the new one.

| Response | Description |
| - | - |
| `301 Moved Permanently` | `GET` method is preserved, others may or may not be changed to `GET`. |
| `308 Permanent Redirect` | Preserves the original HTTP method & request body. |

#### 2. Temporary Redirection
Sometimes, the requested resource can't be accessed from its canonical location, but it can be accessed from another place. In this case, a temporary redirect can be used.

| Response | Description | Use Case
| - | - | - |
| `302 Found` | `GET` method is preserved, others may or may not be changed to `GET`. | The Web page is temporarily unavailable for unforeseen reasons. |
| `303 See Other` | `GET` method is preserved, others are changed to `GET` & request body is lost. | Used to redirect after a PUT or a POST, operation so that refreshing the result page doesn't re-trigger the operation. |
| `307 Temporary Redirect` | Preserves the original HTTP method & request body. | Same as `302` but better for non-`GET` methods. |

#### 3. Special Redirection
| Response | Description |
| - | - |
| `300 Multiple Choices` | |
| `304 Not Modified` | Sent for revalidated conditional requests. Indicates that the cached response is still fresh and can be used. |

### Methods Of Redirection
#### 1. HTTP Redirection
HTTP redirects are the best way to create redirections.

#### 2. HTML Redirection
+ A `<meta>` element with its `http-equiv` attribute set to `Refresh` in the `<head>` of the page does the same thing. When displaying the page, the browser will go to the indicated URL. Example:
  ```html
  <head>
    <meta http-equiv="Refresh" content="0; URL=https://example.com/" />
  </head>
  ```
  The `content` attribute should start with a number indicating how many seconds the browser should wait before redirecting to the given URL. Always set it to 0 for accessibility compliance.

+ This method only works with HTML, and cannot be used for images or other types of content.

#### 3. JavaScript Redirection
Redirections in JavaScript are performed by setting a URL string to the `window.location` property, loading the new page. Example:
```js
window.location = "https://example.com/";
```

Like HTML redirections, this can't work on all resources. Also, this will only work on clients that can execute JavaScript.

### Order Of Precedence
1. HTTP redirects.
2. JavaScript redirects.
   > This is because the `<meta>` redirect happens after the page is completely loaded, which is after all the scripts have been executed.
4. HTML redirects.
5. If there is any JavaScript redirect that happens after the page is loaded (for example, on a button click), it will execute last if the page isn't already redirected by the previous methods.

### Use Cases
#### 1. Domain Aliasing
1. Expanding the reach of your site. 
   + A common case is when a site resides at www.example.com, but accessing it from example.com should also work. Redirections for example.com to www.example.com are thus set up. 
   + Common synonyms or frequent typos of your domains can be redirected to the main domain.
2. Moving to a new domain.
   + Your company was renamed, but you want existing links or bookmarks to still find you under the new name.
3. Forcing HTTPS.
   + Requests made to the `http://` version of the site will be redirected to the `https://` version.

#### 2. Website Restructuring
+ When restructuring your website, URLs change. Even if you update your site's links to match the new URLs, you have no control over the URLs used by external resources. 
+ To avoid breaking these links, we can set up redirects from the old URLs to the new ones.

#### 3. Temporary Redirects To Unsafe Requests
Unsafe requests modify the state of the server and the user shouldn't resend them unintentionally.

## Proxy Servers
### 1. Forward Proxy
+ A forward proxy, or gateway, or just "proxy" provides proxy services to a client or a group of clients.
+ They store and forward Internet services (like the DNS, or web pages) to reduce and control the bandwidth usage.
+ A forward proxy acts on behalf of clients.
+ Forward proxies can also be anonymous and allow users to hide their IP address while browsing the Web or using other Internet services. 
+ For example, `Tor` routes internet traffic through multiple proxies for anonymity.

### 2. Reverse Proxy
+ A reverse proxy acts on the behalf of a server.
+ Reverse proxies can hide the identities of servers.
+ Reverse proxies can be used to distribute the load to several web servers (Load Balancing).
+ Reverse proxies can be used to offload the web servers by caching static content like pictures (Cache Static Content).
+ Reverse proxies can be used to compress and optimize content to speed up load time (Compression).
----

+ `Forwarded` is the standardized header to convey original host information, that might get lost in the path of proxies.
+ Some non-standard ones are: 
  1. `X-Forwarded-For`: Identifies the originating IP addresses of a client connecting to a web server through an HTTP proxy or a load balancer.
  2. `X-Forwarded-Host`: Identifies the original host requested that a client used to connect to your proxy or load balancer.
  3. `X-Forwarded-Proto`: Identifies the protocol (HTTP or HTTPS) that a client used to connect to your proxy or load balancer.
  4. `Via`: To convey information about the proxy itself. It is added by the proxies itself.

### HTTP Tunneling
+ HTTP tunneling is a technique that allows you to send data or messages over a network using the HTTP protocol, even if the actual communication is intended for a different protocol.
+ It is the same relaying your message through someone else, who is trusted and respected by the person you want to deliver your message to.
+ HTTP tunneling is using a protocol of higher level (HTTP) to transport a lower level protocol (TCP).
+ It is used to create a network link between two computers in conditions of restricted network connectivity like firewalls, NATs and ACLs. The tunnel is created by an intermediary called a proxy server which is usually located in a DMZ.
+ The `CONNECT` method in HTTP starts a two-way communication with the requested resource and can be used to open a tunnel.
  + This is how a client behind an HTTP proxy can access websites using TLS.
  + However, not all proxy servers support it or restrict it to port 443 only.

### How It Is Done?
1. The data that has to be transmitted (which may use a different protocol like SSH) is wrapped inside HTTP packets. These HTTP packets are then sent over the network, just like any normal HTTP traffic.
2. To make this work, we need an **intermediary proxy server** or **tunnel endpoint**. 
   + This server is configured to recognize the encapsulated data and forward it to the actual destination where the original protocol can be processed. 
   + The server acts as a translator, extracting the original data from the HTTP packets and forwarding it to the correct destination.
3. Many firewalls and network security devices are configured to only allow HTTP/HTTPS traffic, as it's very common and often essential for web browsing. 
   + By tunneling other types of traffic inside HTTP, the traffic can bypass these restrictions. 
   + This is especially useful in restricted networks where protocols like SSH or FTP might be blocked.

### Proxy Auto-Configuration (PAC)
A Proxy Auto-Configuration (PAC) file is a JavaScript function that determines whether web browser requests (HTTP, HTTPS, and FTP) go directly to the destination or are forwarded to a web proxy server.

### Common Use Cases
1. Bypassing Network Restrictions.
2. Accessing Remote Services. 
3. Secure Communication (over untrusted networks).

## HTTP Cient Hints
+ Client hints are a set of HTTP request header fields that a server can proactively request from a client to get information about the device, network, user, and user-agent-specific preferences. 
+ The server can determine which resources to send, based on the information that the client chooses to provide.
+ The client will decide which info to send.
+ A server must announce that it supports client hints, using the `Accept-CH` header to specify the hints that it is interested in receiving. Example:

  ```
  Accept-CH: Width, Downlink, Sec-CH-UA
  ```

+ Client hints can also be specified in HTML using the `<meta>` element with the http-equiv attribute. Example:
  ```html
  <meta http-equiv="Accept-CH" content="Width, Downlink, Sec-CH-UA" />
  ```

### Low Entropy Client Hints
+ They don't give away much information that might be used to create a fingerprinting for a user.
+ They may be sent by default on every client request, irrespective of the server Accept-CH response header, depending on the permission policy.
+ They include:
  ```
  Save-Data,
  Sec-CH-UA,
  Sec-CH-UA-Mobile, and
  Sec-CH-UA-Platform
  ```

### High Entropy Client Hints
+ The high entropy hints are those that have the potential to give away more information that can be used for user fingerprinting.
+ They are gated in such a way that the user agent can make a decision whether to provide them.
+ All client hints that are not low entropy hints are considered high entropy hints.

### Critical Client Hints
+ A critical client hint is one where applying the response may significantly change the rendered page, potentially in a way that is jarring or will affect usability, and therefore which must be applied before the content is rendered.

# Reference
+ **Almighty ChatGPT**
+ [HTTP Reference, MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP)
+ [Stateless Protocol, Wikipedia](https://en.wikipedia.org/wiki/Stateless_protocol)
+ [A Complete List Of MIME Types, IANA Registry](https://www.iana.org/assignments/media-types/media-types.xhtml)
+ [Common MIME Types, MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME_types/Common_types)
+ [IANA HTTP Authentication Schemes Registry](https://www.iana.org/assignments/http-authschemes/http-authschemes.xhtml)
+ [HTTP Tunneling, Wikipedia](https://en.wikipedia.org/wiki/HTTP_tunnel)

# Other Useful Resources
+ [WebAppSec/Web Security Verification](https://wiki.mozilla.org/WebAppSec/Web_Security_Verification)
+ [Mozilla Security Review and Best Practices Guide](https://www-archive.mozilla.org/projects/security/components/reviewguide.html)
+ [Firefox security guidelines](https://developer.mozilla.org/en-US/docs/Web/Security/Firefox_Security_Guidelines)