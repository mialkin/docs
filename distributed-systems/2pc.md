# Two-phase commit protocol, 2PC

The **two-phase commit protocol** or **2PC** is a distributed protocol that ensures consistent termination of a transaction in a distributed environment.

The essence of two phase commit, unsurprisingly, is that it carries out an update in two phases:

1\. The prepare phase asks each node if it can promise to carry out the update.

2\. The commit phase actually carries it out.

As part of the prepare phase, each node participating in the transaction acquires whatever it needs to assure that it will be able to do the commit in the second phase, for instance any locks that are required.

Once each node is able to ensure it can commit in the second phase, it lets the coordinator know, effectively promising the coordinator that it can and will commit in the second phase. If any node is unable to make that promise, then the coordinator tells all nodes to rollback, releasing any locks they have, and the transaction is aborted. Only if all the participants agree to go ahead does the second phase commence, at which point it's expected they will all successfully update.

It is crucial for each participant to ensure the durability of their decisions using pattern like [↑ Write-Ahead Log](https://martinfowler.com/articles/patterns-of-distributed-systems/write-ahead-log.html). This means that even if a node crashes and subsequently restarts, it should be capable of completing the protocol without any issues.

## Links

[↑ Two Phase Commit](https://martinfowler.com/articles/patterns-of-distributed-systems/two-phase-commit.html).

[↑ Distributed Transactions – Don't use them for Microservices](https://thorben-janssen.com/distributed-transactions-microservices/).
