# Flux

[↑ Flux](https://fluxcd.io/flux) is a tool for keeping Kubernetes clusters in sync with sources of configuration, like Git repositories, and automating updates to configuration when there is new code to deploy.

## Table of contents

- [Flux](#flux)
  - [Table of contents](#table-of-contents)
  - [Install](#install)
  - [Uninstall](#uninstall)
  - [Commands](#commands)
  - [Core concepts](#core-concepts)
    - [Source](#source)
    - [Kustomization](#kustomization)
  - [GitLab](#gitlab)

## Install

```bash
brew install fluxcd/tap/flux
# brew upgrade fluxcd/tap/flux

# Check if your Kubernetes cluster has everything needed to run Flux
flux check --pre
```

## Uninstall

```bash
brew uninstall fluxcd/tap/flux
```

Note that the uninstall command will not remove any Kubernetes objects or Helm releases that were reconciled on the cluster by Flux. It is safe to uninstall Flux and rerun the bootstrap, any existing workloads will not be affected.

## Commands

| Command                                                | Description                                   |
| ------------------------------------------------------ | --------------------------------------------- |
| flux get all                                           |                                               |
| flux get image policy                                  |                                               |
| flux get image repository NAME                         |                                               |
| flux get images update                                 |                                               |
| flux get kustomizations                                | Get `Kustomization` statuses                  |
| flux get kustomizations --watch                        | Start watching kustomizations                 |
| flux get sources all                                   | Get all source statuses                       |
| flux get sources git                                   | List `GitRepository` sources and their status |
| flux suspend kustomization KUSTOMIZATION_NAME          | Suspend reconciliation of `Kustomization`     |
| flux delete kustomization KUSTOMIZATION_NAME           | Delete a `Kustomization` resource             |
| flux delete source git SOURCE_NAME                     | Delete a `GitRepository` source               |
| flux reconcile kustomization flux-system --with-source | Pull and apply the changes                    |
| flux stats                                             | Stats of Flux reconciles                      |
| flux version                                           |                                               |

## Core concepts

### Source

A **source** defines the origin of a repository containing the desired state of the system and the requirements to obtain it. The origin of the source is checked for changes on a defined interval, if there is a newer version available that matches the criteria, a new artifact is produced.

All sources are specified as [↑ custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) in a Kubernetes cluster, examples of sources are [`↑ GitRepository`](https://fluxcd.io/flux/components/source/gitrepositories), `OCIRepository`, `HelmRepository` and `Bucket` resources.

### Kustomization

A `Kustomization` custom resource represents a local set of Kubernetes resources that Flux is supposed to reconcile in the cluster. The reconciliation runs every five minutes by default, but this can be changed with `.spec.interval`.

## GitLab

Generate pair of keys for GitLab:

```bash
cd ~/.ssh
ssh-keygen -f gitlab

# Add public key in GitLab profile: https://gitlab.com/-/profile/keys
```

Bootstrap repository:

```bash
flux bootstrap git \
--components-extra=image-reflector-controller,image-automation-controller \
--url=ssh://git@gitlab.com/mialkin/flux \
--branch=main \
--private-key-file=/Users/aleksei/.ssh/gitlab \
--path=clusters/microk8s

# Clone bootstrapped repository
cd ~/Downloads
git clone git@gitlab.com:mialkin/flux.git
cd flux

# Add .gitignore file
cat <<EOF > .gitignore
.idea/
EOF
```

Create `Deployment` for [↑ podinfo](https://github.com/stefanprodan/podinfo) application:

```bash
cat <<EOF > ./clusters/microk8s/podinfo-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: podinfo
  namespace: default
spec:
  selector:
    matchLabels:
      app: podinfo
  template:
    metadata:
      labels:
        app: podinfo
    spec:
      containers:
        - name: podinfod
          image: ghcr.io/stefanprodan/podinfo:5.0.0
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 9898
              protocol: TCP
EOF

git add -A && \
git commit -m "Add podinfo deployment" && \
git push origin main

# Tell Flux to pull and apply the changes or wait one minute for Flux to detect the changes on its own:
flux reconcile kustomization flux-system --with-source

kubectl port-forward podinfo-5cdb854ffc-tl2mg 9898:9898
```

Create `ImageRepository` and `ImagePolicy`:

```bash
flux create image repository podinfo \
--image=ghcr.io/stefanprodan/podinfo \
--interval=1m \
--export > ./clusters/microk8s/podinfo-registry.yaml

flux create image policy podinfo \
--image-ref=podinfo \
--select-semver=5.0.x \
--export > ./clusters/microk8s/podinfo-policy.yaml
```

Edit the `podinfo-deployment.yaml` file and add a marker to tell Flux which policy to use when updating the container image:

```yaml
spec:
  containers:
    - name: podinfod
      image: ghcr.io/stefanprodan/podinfo:5.0.0 # {"$imagepolicy": "flux-system:podinfo"}
```

Push changes and make sure they have been applied:

```bash
git add -A && \
git commit -m "Add podinfo image scan" && \
git push origin main

flux reconcile kustomization flux-system --with-source

flux get image repository podinfo && \
flux get image policy podinfo
```

Create `ImageUpdateAutomation`:

```bash
flux create image update flux-system \
--interval=1m \
--git-repo-ref=flux-system \
--git-repo-path="./clusters/microk8s" \
--checkout-branch=main \
--push-branch=main \
--author-name=fluxcdbot \
--author-email=fluxcdbot@mialkin.ru \
--commit-template="{{range .Updated.Images}}{{println .}}{{end}}" \
--export > ./clusters/microk8s/flux-system-automation.yaml

git add -A && \
git commit -m "Add image updates automation" && \
git push origin main

flux reconcile kustomization flux-system --with-source
```
