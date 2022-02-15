# Active Directory

**Active Directory** or **AD** is a directory service developed by Microsoft for Windows domain networks. A *directory* is a hierarchical structure that stores information about objects on the network.

Active Directory is an umbrella title for a broad range of directory-based identity-related services.

Active Directory includes:

- A set of rules, **the schema**, that defines the classes of objects and attributes contained in the directory, the constraints and limits on instances of these objects, and the format of their names.

- A **global catalog** that contains information about every object in the directory. This allows users and administrators to find directory information regardless of which domain in the directory actually contains the data.

- A **query and index mechanism**, so that objects and their properties can be published and found by network users or applications.

- A **replication service** that distributes directory data across a network. All *domain controllers* in a domain participate in replication and contain a complete copy of all directory information for their domain. Any change to directory data is replicated to all domain controllers in the domain.

A server running the **Active Directory Domain Service** or **AD DS** role is called a *domain controller*. It authenticates and authorizes all users and computers in a Windows domain type network, assigning and enforcing security policies for all computers, and installing or updating software. For example, when a user logs into a computer that is part of a Windows domain, Active Directory checks the submitted username and password and determines whether the user is a system administrator or normal user. Also, it allows management and storage of information, provides authentication and authorization mechanisms, and establishes a framework to deploy other related services: **Certificate Services**, **Active Directory Federation Services** or **AD FS**, **Lightweight Directory Services**, and **Rights Management Services**.

Active Directory uses **Lightweight Directory Access Protocol** or **LDAP** versions 2 and 3, Microsoft's version of Kerberos and DNS.
