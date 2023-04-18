# Argo CD

[↑ Argo CD](https://argo-cd.readthedocs.io) is a declarative, GitOps continuous delivery tool for Kubernetes.

Argo CD is implemented as a Kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in the Git repo).

A deployed application whose live state deviates from the target state is considered `OutOfSync`. Argo CD reports & visualizes the differences, while providing facilities to automatically or manually sync the live state back to the desired target state. Any modifications made to the desired target state in the Git repo can be automatically applied and reflected in the specified target environments.

Project's [↑ GitHub repository](https://github.com/argoproj/argo-cd).