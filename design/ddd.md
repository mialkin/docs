# Domain-Driven Design

Eric Evans coined the term "Domain-driven design" in his book published in 2004.

DDD provides principles and patterns to solve difficult problems.

DDD provides us with a clean representation of the problem in code that we can readily understand and verify through tests.

DDD places as much emphasis on not only comprehending  what your clients wants, but working with them as full partners through a project. The ultimate goal isn't write code, not event develop software, but to solve the problems.

With DDD you "divide and conquer". By separating a problem into separate subdomains each problem can be tackled intependently, making the problem much easier to solve.

> "While domain-driven design provides many technical benefits, such as maintainability, it should be applied only to complex domains where the model and the linguistic processes provide clear benefits in the communication of complex information, and in the formulation of a common understanding of the domain." Eric Evans, Domain-Driven design

DDD is not always a correct path for your application. Some scenarios in which DDD is going to be just an overkill:

- Data-driven applications which don't need much more than a lot of CRUD logic
- If your domain is simple, even if you have a lot of technical challanges to overcome

