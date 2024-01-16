# cURL

The [↑ `curl`](https://curl.se) is a command line tool and library
for transferring data with URLs.

## Examples

| Command                                   | Description                                     |
| ----------------------------------------- | ----------------------------------------------- |
| curl `https://www.example.com`            | Get the main page from a web-server             |
| curl -i `https://www.example.com`         | Include protocol response headers in the output |
| curl -H "Host: sub.domain.com" 120.7.3.15 | Passing header value                            |
| curl -v `https://www.example.com`         | Get verbose fetching                            |

## Arguments

| Short option | Long option    | Argument(s)      | Purpose                                                                                                                                                      |
| ------------ | -------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| \-X          | \--requests    | METHOD_NAME      | Specify the request method to use when communicating with an HTTP server                                                                                     |
| \-b          | \--cookie      | DATA or FILENAME | Specify which file to write all cookies after completed operation                                                                                            |
| \-c          | \--cookie-jar  | FILENAME         |                                                                                                                                                              |
| \-d          | \--data        | DATA             | Send data in POST request                                                                                                                                    |
| \-f          | \--fail        |                  | Fail fast with no output on server errors                                                                                                                    |
| \-F          | \--form        | name=content     | Form data for content-type *application/x-www-form-urlencoded*                                                                                               |
| \-H          | \--header      | HEADER           | Header to include in the request                                                                                                                             |
| \-i          | \--include     |                  | Include HTTP headers in the output                                                                                                                           |
| \-l          | \--head        |                  | Fetch the headers only                                                                                                                                       |
| \-k          | \--insecure    |                  | Skip verification that connection is secure                                                                                                                  |
| \-L          | \--location    |                  | If a redirect is received (3XX), force curl to redo the request on the new address                                                                           |
| \-o          | \--output      | <file>           | Write output to file instead of stdout                                                                                                                       |
| \-O          | \--remote-name |                  | Write output to a local file named like the remote file                                                                                                      |
| \-s          | \--silent      |                  | Do not show progress meter or error messages                                                                                                                 |
| \-v          | \--verbose     |                  | Make curls verbose during operation. Useful for debugging                                                                                                    |
| \-w          | \--write-out   | <format>         | Make curl display information on stdout after a completed transfer. See possible variables to include in output [here](https://curl.se/docs/manpage.html#-w) |

[↑ curl Post Request](https://www.warp.dev/terminus/curl-post-request).
