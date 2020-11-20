# SignalR

ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps. It uses WebSockets whenever possible.

For most applications, we recommend SignalR over raw WebSockets. SignalR provides transport fallback for environments where WebSockets is not available. It also provides a basic remote procedure call app model. And in most scenarios, SignalR has no significant performance disadvantage compared to using raw WebSockets.

For some apps, gRPC on .NET provides an alternative to WebSockets.

## Hub

A **hub** is a class that serves as a high-level pipeline that handles client-server communication.

A hub is a high-level pipeline that allows a client and server to call methods on each other. SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa. You can pass strongly-typed parameters to methods, which enables model binding. SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on MessagePack. MessagePack generally creates smaller messages compared to JSON.

Hubs call client-side code by sending messages that contain the name and parameters of the client-side method. Objects sent as method parameters are deserialized using the configured protocol. The client tries to match the name to a method in the client-side code. When the client finds a match, it calls the method and passes to it the deserialized parameter data.

## Links

- [↑ SignalR](https://dotnet.microsoft.com/apps/aspnet/signalr)
- [↑ Real-time apps](https://docs.microsoft.com/en-us/aspnet/core/signalr/introduction)
