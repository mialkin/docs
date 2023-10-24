# Egress vs ingress

An **egress** is a traffic that exits an entity or a network boundary.

Since traffic often is translated using NAT in and out of a private network like the cloud, a response back from a public endpoint to a request that was initiated inside the private network is not considered ingress. If a request is made from the private network out to a public IP, the public server/endpoint responds back to that request using a port number that was defined in the request, and firewall allows that connection since its aware of an initiated session based on that port number.

An **ingress** is a traffic that enters the boundary of a network.

Ingress more specifically refers to unsolicited traffic sent from an address in public internet to the private network — it is not a response to a request initiated by an inside system. In this case, firewalls are designed to decline this request unless there are specific policy and configuration that allows ingress connections.

## Links

[↑ The definitions of Egress and Ingress for the cloud](https://aviatrix.com/learn-center/cloud-security/egress-and-ingress/).
