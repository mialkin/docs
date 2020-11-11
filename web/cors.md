# CORS

## A Security Risk

If the user has valid session cookies in their browser, they will be used to authenticate on api.yoursebsite.com and that would lead to unwanted e-mail sending. In most cases, dangerous requests will be “preflighted,” which means the domain needs to be approved before they can even send a request. This will prevent any malicious activities from happening.

One of the reasons a request might be preflighted is because they used a method other than GET, HEAD or POST. Another reason is that a request uses a custom HTTP header, such as `My-Custom-Header: foo`.
