# POST vs PUT vs PATCH

## POST

Use POST to create a new resource.

A POST request requires a body in which you define the data of the entity to be created.

In a weather app, we could use a POST method to add weather data about a new city.

POST request is not idempotent. If you execute a POST request multiple times, you'll create a new resource multiple times despite them having the same data being passed in.

## PUT

Use PUT to modify a resource.

PUT updates the entire resource with data that is passed in the body payload. If there is no resource that matches the request, it will create a new resource.

In a weather app, we could use PUT to update all weather data about a specific city.

PUT requests are idempotent, meaning that executing the same PUT request will always produce the same result.

With PUT you need to pass in data to update the entire resource, even if you only want to modify one field.

Using a restaurant analogy, POST-ing multiple times would create multiple separate orders, whereas multiple PUT requests will update the same existing order.

If you just want to update part of your resource, you still need to send in data for the entire resource when you make a PUT request. The better-suited option here would be PATCH.

## PATCH

Use PATCH to modify a part of a resource.

With PATCH, you only need to pass in the data of the field that you want to update.

In a weather app, we could use PATCH to update the rainfall for a specified day in a specified city.

A PATCH is not necessarily idempotent, although it can be. Contrast this with PUT; which is always idempotent. For example if an auto-incrementing counter field is an integral part of the resource, then a PUT will naturally overwrite it, since it overwrites everything, but not necessarily so for PATCH.

[↑ PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH).

## Idempotence

Idempotence is an important property of some HTTP methods, ensuring that executing the same request multiple times produces the same result as if it was performed only once. GET, HEAD, PUT, DELETE, OPTIONS, and TRACE are idempotent methods, meaning they are safe to be retried or executed multiple times without causing unintended side effects. In contrast, POST and PATCH are generally considered non-idempotent, as their outcomes may vary with each request.

## Links

[↑ HTTP Request Methods – Get vs Put vs Post Explained with Code Examples](https://www.freecodecamp.org/news/http-request-methods-explained/)
