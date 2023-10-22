# Two-phase commit protocol, 2PC

The **two-phase commit protocol** or **2PC** is a distributed protocol that ensures consistent termination of a transaction in a distributed environment.

The essence of two phase commit, unsurprisingly, is that it carries out an update in two phases:

- the first, prepare, asks each node if it's able to promise to carry out the update
- the second, commit, actually carries it out.

As part of the prepare phase, each node participating in the transaction acquires whatever it needs to assure that it will be able to do the commit in the second phase, for instance any locks that are required. Once each node is able to ensure it can commit in the second phase, it lets the coordinator know, effectively promising the coordinator that it can and will commit in the second phase. If any node is unable to make that promise, then the coordinator tells all nodes to rollback, releasing any locks they have, and the transaction is aborted. Only if all the participants agree to go ahead does the second phase commence, at which point it's expected they will all successfully update.

## Links

[↑ Two Phase Commit](https://martinfowler.com/articles/patterns-of-distributed-systems/two-phase-commit.html).

[↑ Distributed Transactions – Don't use them for Microservices](https://thorben-janssen.com/distributed-transactions-microservices/).
