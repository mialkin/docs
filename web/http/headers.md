# HTTP headers

**HTTP headers** let the client and the server pass additional information with an HTTP request or response. An HTTP header consists of its case-insensitive name followed by a colon (`:`), then by its value. Whitespace before the value is ignored.

Headers can be grouped according to their contexts.

## General headers

A general header is an HTTP header that can be used in both request and response messages but doesn't apply to the content itself. Depending on the context they are used in, general headers are either response or request headers. However, they are not entity headers.

The most common general headers are `Date`, `Cache-Control` or `Connection`.

## Request headers

Contain more information about the resource to be fetched, or about the client requesting the resource.

## Response headers

Hold additional information about the response, like its location or about the server providing it.

## Entity headers

An entity header is an HTTP header that can be used in an HTTP request or response, and describes the content of the body of the message. Headers like `Content-Length`, `Content-Language`, `Content-Encoding` are entities headers.

Even if they are neither request, nor response headers, entity headers are often included in such terms.

In the following example, `Content-Length` is an entity header, while `Host` and `User-Agent` are requests headers:

```text
POST /myform.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Content-Length: 128
```

## Links

[↑ HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
