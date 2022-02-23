# Open Web Interface for .NET (OWIN)

**OWIN** is a standard for an interface between .NET web applications and web servers. It is a community-owned open-source project. Prior to OWIN, Microsoft's ASP.NET technology was designed on top of IIS, and web applications could not easily be run on another web server. OWIN aims to decouple the relationship between ASP.NET applications and IIS by defining a standard interface. Developers of web servers can be sure that, if they implement OWIN correctly, ASP.NET applications will run on their server. Similarly, new web frameworks could be developed as an alternative to ASP.NET. So long as they target OWIN, they will run on any OWIN compatible web server, including IIS.

In this regard, OWIN aims to do for .NET what Java Servlet and Servlet containers do for the JVM.

## OWIN as middleware

In addition to decoupling web frameworks and web servers, OWIN allows chaining together middleware into a pipeline. A web framework can interact with OWIN without knowing whether it is interacting directly with the underlying web server, or with one or more layers of middleware (each implementing OWIN) on top of the web server. This allows infrastructure concerns, such as authentication, to be split out into separate modules. This is desirable as it decouples them from the application's own code, and makes them reusable across applications. In *Project Katana*, Microsoft has made into OWIN modules several ASP.NET features that were previously part of the core ASP.NET framework. This allows them to be reused in other web frameworks, and also ensures a cleaner separation from the application using them.

## Project Katana

**Project Katana** is a set of OWIN components built by Microsoft.
