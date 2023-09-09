# Argo CD

[↑ Argo CD](https://argo-cd.readthedocs.io) is a declarative, GitOps continuous delivery tool for Kubernetes.

Argo CD is implemented as a Kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in the Git repo).

A deployed application whose live state deviates from the target state is considered `OutOfSync`. Argo CD reports & visualizes the differences, while providing facilities to automatically or manually sync the live state back to the desired target state. Any modifications made to the desired target state in the Git repo can be automatically applied and reflected in the specified target environments.

Project's [↑ GitHub repository](https://github.com/argoproj/argo-cd).

## Simplified Argo CD workflow

1. A developer creates a new feature in the source code Git repository.
2. The CI system picks the source code change, packages the code, and runs unit tests or security scans as needed.
3. The developer makes a pull request and automatically or manually changes the Kubernetes manifest, stored in a separate Git repository that holds only Kubernetes manifests.
4. The GitOps team reviews the pull request and merges approved changes to the main branch, triggering a webhook that notifies Argo CD of the change.
5. Argo CD copies the repository and compares the desired application’s state with the Kubernetes cluster’s current state, applying the necessary changes to the cluster configuration.
6. Kubernetes controllers reconcile the required changes to the cluster resources until it reaches the target configuration.
7. Argo CD tracks the change’s progress and reports the application as "in sync" when the cluster matches the desired state.
8. ArgoCD can also work in the opposite direction to monitor changes to the Kubernetes cluster, rolling them back if they do not match the current Git configuration.

[↑ What Is a GitOps Workflow?](https://codefresh.io/learn/gitops/gitops-workflow-vs-traditional-workflow-what-is-the-difference/)
