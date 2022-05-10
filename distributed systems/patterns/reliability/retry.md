# Retry

**Retry** is a design pattern that enables an application to handle anticipated, temporary failures when application tries to connect to a service or network resource by transparently retrying an operation that's previously failed.

If an application detects a failure when it tries to send a request to a remote service, it can handle the failure using the following strategies:

- **Cancel**. If the fault indicates that the failure isn't transient or is unlikely to be successful if repeated, the application should cancel the operation and report an exception. For example, an authentication failure caused by providing invalid credentials is not likely to succeed no matter how many times it's attempted.
- **Retry**. If the specific fault reported is unusual or rare, it might have been caused by unusual circumstances such as a network packet becoming corrupted while it was being transmitted. In this case, the application could retry the failing request again immediately because the same failure is unlikely to be repeated and the request will probably be successful.
- **Retry after delay**. If the fault is caused by one of the more commonplace connectivity or busy failures, the network or service might need a short period while the connectivity issues are corrected or the backlog of work is cleared. The application should wait for a suitable time before retrying the request.

## Links

- C# implementation [example](../../../dotnet/libraries/polly.md#retry).
- [↑ Retry pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/retry)
